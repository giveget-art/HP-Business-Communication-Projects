# Export PKCS#12 (PFX)

Run the following command to export a certificate and private key into a PFX (PKCS#12) archive:

```
openssl pkcs12 -export -out user.pfx -inkey user.key -in user.crt -certfile ca.crt
```

You will be asked to supply an “export password”, and it’s very recommended that one is set, since often you’ll need to transfer the PFX archive to a device such as your phone; you don’t want this sitting in your email without a password on.
