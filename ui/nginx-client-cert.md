# Nginx client certificate authentication and reverse proxy

This document shows how to create a PFX archive from a user key/cert and a minimal nginx configuration that enforces client certificate authentication while reverse-proxying to an unauthenticated backend.

## Create a PFX archive

Use the following OpenSSL command to create a PFX (PKCS#12) archive containing the user's certificate and key:

```
openssl pkcs12 -export -out user.pfx -inkey user.key -in user.crt -certfile ca.crt
```

You will be asked to supply an "export password". It is strongly recommended to set one because you’ll often need to transfer the PFX archive to a device (for example, your phone). You do not want an unprotected PFX file sitting in email or cloud storage without a password.

The PFX archive can be imported into web browsers (Chrome, Firefox, Edge, etc.). This lets the client present a certificate to the server for mutual TLS, proving identity without the CA or administrator ever seeing the user's private key.

## nginx Setup

Below is a minimal `nginx.conf` that supports client certificate authentication, redirects HTTP to HTTPS, and reverse-proxies requests to an unauthenticated backend. The HTTPS certificate in this example is provided by Let's Encrypt; obtaining that certificate is not covered here.

Replace `example.com` with your actual domain and ensure the certificate paths are correct on your server.

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
  worker_connections 768;
}

http {
  # some HTTP boilerplate
  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 65;
  types_hash_max_size 2048;
  server_tokens off;

  include /etc/nginx/mime.types;
  default_type application/octet-stream;

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;

  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;

  gzip on;
  gzip_disable "msie6";

  map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
  }

  # server on port 80 for HTTP -> HTTPS redirect
  server {
    listen 80;
    server_name example.com;
    return 301 https://example.com$request_uri;
  }

  # The letsencrypt-secured HTTPS server, which proxies our requests
  server {
    listen 443 ssl;
    server_name example.com;

    ssl_protocols TLSv1.1 TLSv1.2;
    # letsencrypt certificate
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # client certificate
    ssl_client_certificate /etc/nginx/client_certs/ca.crt;
    # make verification optional, so we can display a 403 message to those
    # who fail authentication
    ssl_verify_client optional;

    access_log /var/log/nginx/example.com;

    location / {
      # if the client-side certificate failed to authenticate, show a 403
      # message to the client
      if ($ssl_client_verify != SUCCESS) {
        return 403;
      }

      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;

      # Fix the "It appears that your reverse proxy set up is broken" error.
      proxy_pass          http://localhost:8080;
      proxy_read_timeout  90;

      # web sockets
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;

      proxy_redirect      http://localhost:8080 https://example.com;
    }
  }
}
```

Notes:
- `ssl_verify_client optional` is used so the server can return a custom 403 to clients who fail certificate verification. If you want nginx to perform strict verification and immediately terminate the TLS handshake for unverified clients, set `ssl_verify_client on`.
- Ensure the CA certificate(s) used to validate client certificates are stored at `/etc/nginx/client_certs/ca.crt` (or update the path accordingly).
- Adjust `ssl_protocols` to meet your security requirements; the example includes older TLS versions for compatibility. Prefer modern TLS (e.g., TLSv1.2 and TLSv1.3) in production.

--

Generated and added by the GitHub Cl
