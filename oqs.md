# oqs


Start oqs docker image:
```bash
docker run --rm -it openquantumsafe/openssl3 sh
```

## Classical CA 
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

## Post-Quantum CA
Generate a Post-Quantum CA (one which uses a PQ signature schemes for its own certificate i.e. CA Certificate):
```bash
openssl req -x509 -new -newkey mldsa87_ed448 -keyout mldsa87_ed448_CA.key -out mldsa87_ed448_CA.crt -nodes -subj "/CN= PQ test CA" -days 365 
```
Generate Post-Quantum certificate signing request:
```bash
openssl req -new -newkey mldsa87_ed448 -keyout mldsa87_ed448_srv.key -out mldsa87_ed448_srv.csr -nodes -subj "/CN=server"
```
Verify the signature on the CSR and view it in plain-text format:
```bash
 openssl req -in mldsa87_ed448_srv.csr -noout -verify -text
```
Approve the CSR and issue a PQ X.509 (hybrid) certificate:
```bash
openssl x509 -req -in mldsa87_ed448_srv.csr -out mldsa87_ed448_srv.crt -CA mldsa87_ed448_CA.crt -CAkey mldsa87_ed448_CA.key -CAcreateserial -days 365
```
Verify whether the public key in the certificate matches with the corresponding private key (No diff indicates that both the keys are consistent with each other):
```bash
openssl x509 -in mldsa87_ed448_srv.crt -pubkey -noout > pubkey.pem
openssl pkey -in mldsa87_ed448_srv.key -pubout -out privkey_pub.pem
diff pubkey.pem privkey_pub.pem
```
View the certificate:
```bash
openssl x509 -in mldsa87_ed448_srv.crt -text -noout
```




### Google Chrome

TLS 1.3 hybridized Kyber support \
This option enables a combination of X25519 and Kyber in TLS 1.3. – Mac, Windows, Linux, ChromeOS, Android, Fuchsia, Lacros

chrome://flags/#enable-tls13-kyber

https://github.com/open-quantum-safe/oqs-demos/releases

```bash
kubectl create deployment quantum-nginx --image=openquantumsafe/nginx
kubectl expose deployment quantum-nginx --port=4433 --name=quantum-nginx
```


