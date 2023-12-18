# oqs


Start oqs docker image:
```bash
docker run --rm -it openquantumsafe/openssl3 sh
```

Generate CA:
```bash
openssl req -x509 -new -newkey rsa -keyout CA.key -out CA.crt -nodes -subj "/CN=Post-Quantum CA" -days 365
```

Generate certificate signing request:
```bash
openssl req -new -newkey dilithium3 -keyout server.key -out server.csr -nodes -subj "/CA=localhost" -addext "subjectAltName = DNS:localhost"
```

Generate certificate using CA:
```bash
openssl req -x509 -in server.csr -out server.crt -CA CA.crt -CAkey CA.key -days 365 -copy_extensions copy
```

Verify certificate:
```bash
openssl x509 -text -noout -in server.crt
```

Generate self self-signed certificate:
```bash
openssl req -x509 -new -newkey dilithium5 -keyout server.key -out server.crt -nodes -days 365 -subj "/CN=Post-Quantum CA"
```


### Google Chrome

TLS 1.3 hybridized Kyber support \
This option enables a combination of X25519 and Kyber in TLS 1.3. – Mac, Windows, Linux, ChromeOS, Android, Fuchsia, Lacros

chrome://flags/#enable-tls13-kyber




