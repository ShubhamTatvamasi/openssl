# openssl

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/shubhamtatvamasi/openssl)](https://hub.docker.com/r/shubhamtatvamasi/openssl)
[![Docker Pulls](https://img.shields.io/docker/pulls/shubhamtatvamasi/openssl)](https://hub.docker.com/r/shubhamtatvamasi/openssl)
[![MicroBadger Layers (tag)](https://img.shields.io/microbadger/layers/shubhamtatvamasi/openssl/latest)](https://hub.docker.com/r/shubhamtatvamasi/openssl)
[![Docker Cloud Automated build](https://img.shields.io/docker/cloud/automated/shubhamtatvamasi/openssl)](https://hub.docker.com/r/shubhamtatvamasi/openssl)


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
openssl x509 -in ca.crt -text -noout
```
