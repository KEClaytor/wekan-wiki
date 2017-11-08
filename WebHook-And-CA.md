Because NodeJS uses an [hardcoded list of certificates](https://github.com/nodejs/node/issues/4175) if you deploy a site (receiving the webhook from wekan) behind a reverse proxy on https with a Let's Encrypt certificate you may have a problem: Let's Encrypt's CA is not recognized.

* Download the Let’s Encrypt Authority X3 (IdenTrust cross-signed) from [here](https://letsencrypt.org/certificates/)
```sh
cd /etc/ssl/certs
wget https://letsencrypt.org/certs/lets-encrypt-x3-cross-signed.pem.txt -O lets-encrypt-x3-cross-signed.pem
```

* Now start the application with 
```sh
NODE_EXTRA_CA_CERTS=/etc/ssl/certs/lets-encrypt-x3-cross-signed.pem node main.js
```