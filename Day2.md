## CSRs

```
mkdir pki
cd pki

# generate a private key
openssl genpkey -outform PEM -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out priv.key

# generate a private key and a CSR
openssl req -new -nodes -newkey rsa:2048 `
  -keyout new.key `
  -subj "/C=US/ST=Louisiana/L=New Orleans/O=Sapphire Health/OU=IT/CN=kuiper.sapphirehealth.org" `
  -addext "subjectAltName=DNS:kuiper.sapphirehealth.org,DNS:kuiper01.sapphirehealth.org,DNS:kuiper02.sapphirehealth.org,IP:10.0.0.10"

# generate a CSR using the private key we created earlier
openssl req -new -nodes -key priv.key `
  -subj "/C=US/ST=Louisiana/L=New Orleans/O=Sapphire Health/OU=IT/CN=kuiper.sapphirehealth.org" `
  -addext "subjectAltName=DNS:kuiper.sapphirehealth.org,DNS:kuiper01.sapphirehealth.org,DNS:kuiper02.sapphirehealth.org,IP:10.0.0.10" -out kuiper.sapphirehealth.org.csr
```

## Self signed certificates
```
# generate a self-signed certificate
openssl req -newkey rsa:2048 -x509 -keyout selfsigned.key -out selfsigned.crt -days 365 -nodes -subj "/CN=localhost"

# generate a self-signed certificate using private key we created earlier
openssl x509 -req -days 365 -in kuiper.sapphirehealth.org.csr -signkey priv.key -out kuiper.sapphirehealth.org.crt
```

## sign with a CA on PKIaaS.io

## Export PKCS#12
```
openssl pkcs12 -export -out kuiper.pfx -inkey priv.key -in kuiper.sapphirehealth.org.crt
```