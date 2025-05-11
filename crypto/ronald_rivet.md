# Challenge: Ronald Rivest

**Category:** Crypto  
**Description:**  
RSA is often considered unbreakable, but even the strongest locks can have hidden weaknesses. In this challenge, you're handed an RSA-encrypted message, but something’s not quite right. The key to unlocking it is within your reach—if you know where to look.

**Download:** `Challenge.txt`

---

## Provided Information

The file includes the RSA parameters:

- **Private exponent (d):**

    ```markdown
    1321059730692770657701688489269641637198996399474818318514163788431525598670314146660587724026898506385915390563226195114137969337047528261384908866928241
    ```

- **Public Key (e, N):**

    ```markdown
    e = 65537
    N = 6486722976729760290237173785964224468203388554160573023185940825836284795089430404997102107331067345871089220965914645805857321154755826022114992860905353
    ```

- **Ciphertext (C):**

    ```markdown
    2611660527808078979755799564993592748652216704719912737093634620336756163200559277118791197490834742910074658056394461278001107857542243968852689008042939
    ```

---

## Steps to Solve

1. **Recognize the RSA Components**  
 Since the private key `d` is provided, decryption is straightforward using the formula:

    ```math
    M = C^d mod N
    ```

2. **Implement the Decryption in Python**

```python
# RSA parameters
d = 1321059730692770657701688489269641637198996399474818318514163788431525598670314146660587724026898506385915390563226195114137969337047528261384908866928241
N = 6486722976729760290237173785964224468203388554160573023185940825836284795089430404997102107331067345871089220965914645805857321154755826022114992860905353
C = 2611660527808078979755799564993592748652216704719912737093634620336756163200559277118791197490834742910074658056394461278001107857542243968852689008042939
# Decryption
M = pow(C, d, N)
# Convert integer to bytes and decode
flag = M.to_bytes((M.bit_length() + 7) // 8, byteorder='big').decode()
print("Decrypted flag:", flag)
```

---

## Run the Script
Executing the code gives:

```css
Decrypted flag: csay{crYpt0_1s_n0t_s0_s3cur3}
```

---

## Flag
```css
csay{crYpt0_1s_n0t_s0_s3cur3}
```