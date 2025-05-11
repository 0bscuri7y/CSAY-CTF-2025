# Challenge: Certified to PWN

**Category:** Crypto  

## Steps to Solve

1. **Understanding Challenge**  
   We are given a pfx file. The challenge focuses on some encoding and password. 
   Trying to open pfx file with `openssl pkcs12 -info -in cr4ck_me.pfx -nodes` asks for a passowrd.

2. **Strategy**  
   First guess was we had to bruteforce for a password, but password was not normal, but encoded. And most common encoding is base64. So I quickly wrote a bruteforcer, that reads a word from rockyou, encodes it with base64 and tries to bruteforce pfx file.

3. **Script**  
```python
import base64
from cryptography.hazmat.primitives.serialization import pkcs12
from cryptography.hazmat.backends import default_backend
from multiprocessing import Pool, Value, Lock, cpu_count
from tqdm import tqdm
import signal
import sys

# Load the PFX file globally to share across workers
with open("cr4ck_me.pfx", "rb") as f:
    PFX_DATA = f.read()

# Shared counter and lock for progress tracking
counter = Value('i', 0)
lock = Lock()

# Graceful exit on Ctrl+C
def init_worker():
    signal.signal(signal.SIGINT, signal.SIG_IGN)

# Worker function
def try_password(word):
    word = word.strip()
    if not word:
        return None

    b64_password = base64.b64encode(word.encode()).decode()
    try:
        pkcs12.load_key_and_certificates(PFX_DATA, b64_password.encode(), backend=default_backend())
        return (word, b64_password)  # Found
    except Exception:
        with lock:
            counter.value += 1
        return None

def main():
    try:
        with open("rockyou.txt", "r", encoding="latin-1") as f:
            words = f.readlines()

        print(f"[*] Loaded {len(words)} passwords. Starting brute-force with {cpu_count()} workers...")

        with Pool(processes=cpu_count(), initializer=init_worker) as pool:
            result_iter = pool.imap_unordered(try_password, words, chunksize=100)

            with tqdm(total=len(words)) as pbar:
                for result in result_iter:
                    pbar.update(counter.value - pbar.n)
                    if result:
                        word, b64 = result
                        print(f"\n[+] Password FOUND!")
                        print(f"    Original: {word}")
                        print(f"    Base64:   {b64}")
                        pool.terminate()
                        return
                print("\n[-] Password not found.")

    except KeyboardInterrupt:
        print("\n[!] Interrupted by user. Exiting.")
        sys.exit(1)

if __name__ == "__main__":
    main()

```

---

## Flag
Upon getting the correct password, we use it with `openssl pkcs12 -info -in cr4ck_me.pfx -nodes` command and get the flag.