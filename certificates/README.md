# Local Certificate Authority

The real certificates are hosted on load-balancer. However GOC requires all
network connections to be ciphered.

This local authority permits to simply generate server certificates. Just to
launch the following command with all names you want in the certificate, and
**commit your changes**.

```shell
$ ./generateServerCertificate main-name.example.com altName1.example.com altName2.example.com

$ git add <path to cert, no need to commit priv key>
$ git commit . -m 'Add certificate for myservice.gov.mu'
```

## Install authority

Simply copy [govmu-local-ca.crt](./govmu-local-ca.crt) **and** the "rehash" link
[7d23600c.0](./7d23600c.0) into `/etc/ssl/certs`.
