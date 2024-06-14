# Generate TLS Certificates

Generate TLS certificates for the application. For simplicity, we'll use openssl to generate self-signed certificates.

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=demo-app.local"
```
