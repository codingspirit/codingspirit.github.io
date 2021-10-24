---
title: 'ECDSA: Verify Signature'
top: false
tags:
  - Crypto
date: 2021-06-20 09:38:34
categories:
---

To generate ECDSA Digital Signature: [ECDSA: Generate ECDSA Key/Certificate/Signature By openssl](/2021-06-20/ECDSA-Generate-ECDSA-Key-Certificate-Signature-By-openssl/)

<!--more-->

## Using openssl

```bash
openssl dgst -sha256 -verify ecdsasigner-pub.key -signature signature <file_need_to_verify>
```

## Using Java

```java
public static boolean isVerifyPassed(InputStream certIs, byte[] data, String signature) {
    boolean res = false;
    try {
        Certificate certificate = CertificateFactory.getInstance("X.509").generateCertificate(certIs);
        Signature verifier = Signature.getInstance("SHA256withECDSA");
        verifier.initVerify(certificate);
        verifier.update(data);
        res = verifier.verify(Base64.decode(signature, Base64.NO_WRAP));
    } catch (Exception ignored) {
    }
    return res;
}
```
