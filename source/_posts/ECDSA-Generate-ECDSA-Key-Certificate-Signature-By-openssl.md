---
title: 'ECDSA: Generate ECDSA Key/Certificate/Signature By openssl'
top: false
tags:
  - Crypto
date: 2021-06-20 09:35:17
categories: 随便写写
---

<!--more-->

## Create ECDSA private key
Create file `cert_config.txt`:

```
[ req ]
prompt             = no
distinguished_name = my_dn
                    
[ my_dn ]
commonName = xxxxxxxxx@xxxxxx.com
                    
[ my_exts ]
keyUsage         = digitalSignature
extendedKeyUsage = codeSigning
```

Then create ECDSA private key base on it:

```bash
openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:P-256 -pkeyopt ec_param_enc:named_curve -outform PEM -out ecdsasigner.key
```

## Create ECDSA certificate/public key

If you need certificate:

```bash
openssl req -new -x509 -config cert_config.txt -extensions my_exts -nodes -days 365 -key ecdsasigner.key -out ecdsasigner.crt
```

If you need public key:

```bash
openssl x509 -pubkey -noout -in ecdsasigner.crt > ecdsasigner-pub.key
```

## Create SHA256 Digital Signature

```bash
openssl dgst -sha256 -sign ecdsasigner.key <file_need_to_sign> > signature
base64 signature
```

## Verify

[ECDSA: Verify Signature](/2021-06-20/ECDSA-Verify-Signature/)
