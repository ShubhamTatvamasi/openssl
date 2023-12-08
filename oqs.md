# oqs


Start oqs docker image:
```bash
docker run --rm -it openquantumsafe/curl sh
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
