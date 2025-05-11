# Challenge: Lost Evidence

**Category:** Forensics  
**Description:**  
During a cybercrime investigation, authorities secured a suspect’s device. In a last-ditch effort before capture, the suspect attempted to eliminate key data. Your objective is to decipher the concealed remnants within a fragmented and compromised disk image.

**Download:** `disk.img`

---

## Steps to Solve

1. **Search for Potential Flag Artifacts**  
    Since we're analyzing a raw disk image, we can use the `grep` utility to look for readable strings, even in binary data. Run:

    ```bash
    grep -a -b 'CSAY' disk.img
    ```
    * -a: Treats the binary file as text
    * -b: Displays byte offset of matches

2. **Locate the Flag**  
This command reveals a line fetches the matching words:

    ```css
    CSAY{D3t3ct1v3_m0d3_0n}
    ```
    It was hidden in raw data but still recoverable through direct search.

---

## Flag
```css
CSAY{D3t3ct1v3_m0d3_0n}
```