```bash
# Create private key for CA
docker run --rm shubhamtatvamasi/openssl genpkey -algorithm ED25519 > ca.key

# Create CSR using the private key
docker run --rm -w /root -v $PWD:/root shubhamtatvamasi/openssl \
req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr

# Self sign the csr using its own private key
docker run --rm -w /root -v $PWD:/root shubhamtatvamasi/openssl \
x509 -req -in ca.csr -signkey ca.key -CAcreateserial -out ca.crt -days 1000

# check the certificate info
docker run --rm -w /root -v $PWD:/root shubhamtatvamasi/openssl \
x509 -text -noout -in ca.crt
```
