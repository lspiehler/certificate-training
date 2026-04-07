# Download and install Caddy
- [Caddy Download Page](https://caddyserver.com/download)
- Drop the binary in your `pki` directory
- Create caddy.json

# Launch PowerShell
```
cd pki
```

# generate a private key
openssl genpkey -outform PEM -algorithm RSA -pkeyopt rsa_keygen_bits:1024 -out caddy.key

# generate a CSR using the private key we created earlier
```
openssl req -new -nodes -key caddy.key `
  -subj "/C=US/ST=Louisiana/L=New Orleans/O=Sapphire Health/OU=IT/CN=kuiper.sapphirehealth.org" `
  -addext "subjectAltName=DNS:kuiper.sapphirehealth.org,DNS:kuiper01.sapphirehealth.org,DNS:kuiper02.sapphirehealth.org,IP:10.0.0.10" -out caddy.csr
```

# generate a self-signed certificate using private key we created earlier
```
openssl x509 -req -days 365 -in caddy.csr -signkey caddy.key -out caddy.crt
```

# Start Caddy with the self-signed certificate
```
.\caddy_windows_amd64.exe run --config .\caddy.json
```

# Navigate to https://localhost:9443