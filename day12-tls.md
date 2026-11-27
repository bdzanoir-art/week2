# Day 12: HTTPS and TLS Certificates

## Key ideas
- HTTPS = HTTP + TLS. Default port: 443.
- TLS gives encryption, integrity and authentication.
- A certificate contains the domain name, a public key, dates and a CA signature.
- Chain of trust: server certificate -> intermediate CA -> root CA.

## Commands
```bash
openssl s_client -connect google.com:443 -showcerts </dev/null

openssl s_client -connect google.com:443 </dev/null 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates -ext subjectAltName
```

## My results (google.com)
- Issuer:
- Expiry date:
- TLS version:
- Verify return code:

## Self-signed vs expired
- Self-signed: the owner signed it, no trusted CA confirms it.
- Expired: the end date has passed.

## Public key and private key
- Public key: shared in the certificate. In TLS 1.3, it verifies the server signature.
- Private key: secret on the server. It signs to prove the server identity.
