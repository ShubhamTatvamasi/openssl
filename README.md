# openssl

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/shubhamtatvamasi/openssl)](https://hub.docker.com/r/shubhamtatvamasi/openssl)
[![Docker Image Size (tag)](https://img.shields.io/docker/image-size/shubhamtatvamasi/openssl/latest)](https://hub.docker.com/r/shubhamtatvamasi/openssl)
[![Docker Pulls](https://img.shields.io/docker/pulls/shubhamtatvamasi/openssl)](https://hub.docker.com/r/shubhamtatvamasi/openssl)
[![MicroBadger Layers (tag)](https://img.shields.io/microbadger/layers/shubhamtatvamasi/openssl/latest)](https://hub.docker.com/r/shubhamtatvamasi/openssl)
[![Docker Cloud Automated build](https://img.shields.io/docker/cloud/automated/shubhamtatvamasi/openssl)](https://hub.docker.com/r/shubhamtatvamasi/openssl)

Generate RSA private key, 4096 bit
```bash
openssl genrsa 4096
```

Generate RSA private key, 2048 bit with password
```bash
openssl genrsa -aes256
```

Save the generated RSA private key
```bash
openssl genrsa -aes256 -out private.pem
```

Generate and save public key from private key
```bash
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
```
---

### Standard RSA SSL key used by most websites

RSA 2048 bit key
```bash
# Generate Certifcate Request
openssl req -new -newkey rsa:2048 -nodes -keyout shubhamtatvamasi.key -out shubhamtatvamasi.csr

# Self-Sign Certificate
openssl x509 -req -in shubhamtatvamasi.csr -signkey shubhamtatvamasi.key -out shubhamtatvamasi.crt -days 365
```
---

### Self-Signed Certificate

Get the list of curves available
```bash
openssl ecparam -list_curves
```
> Only `secp384r1` and `prime256v1` keys are compatible with modern browsers

Generate new private key
```bash
openssl ecparam -name secp384r1 -genkey -out ec_key.pem
```

Generate certificate
```bash
openssl req -new -key ec_key.pem -x509 -nodes -days 3650 -out cert.pem
```
---

### OpenSSL DH Parameters

Generate Diffie-Hellman key exchange
```bash
openssl dhparam -out dhparam.pem 4096
```
> add `-dsaparam` flag for faster key generation
---

### Speed Test

ecdhp256 vs ecdhp384, operations per second
```bash
openssl speed ecdhp256 ecdhp384
```
---

### Read information of your SSL certificate

```bash
openssl x509 -text -noout -in certificate.crt
```
---

### Check whether a certificate is self-signed

```bash
openssl x509 -noout -issuer -subject -in cert.pem
openssl verify -CAfile cert.pem cert.pem
```

### Convert .cer → .pem

```bash
openssl x509 -inform DER -in certificate.cer -out certificate.pem
```

---

### Convert a PEM based file to JSON

```bash
awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' cert-name.pem
```

---

Extract the web server’s certificate:
```bash
openssl s_client -connect example.com:443 -showcerts < /dev/null
```

Copy certificate into the RedHat trust anchor directory:
```bash
sudo cp server.crt /etc/pki/ca-trust/source/anchors/
```
> File must end with `.crt` or it will not be loaded.

Update CA trust:
```bash
sudo update-ca-trust
```



