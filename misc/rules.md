# Challenge: Rules

**Category:** Misc  
**Description:** Read rules  

---

## Steps to Solve

1. **Go to the Rules Page**  
   Navigate to the challenge's **rules** page.

2. **View the Source Code**  
   Press `Ctrl + U` to open the page source code in your browser.

3. **Inspect for Hidden Data**  
   Inside the comments of the source code, you'll find a base64-encoded string:

```css
QnJvIGt5YSBjaGFpeWUgcnVsZXMgbmFoaSBwYWRlIGt5YSBqbyBjb21tZW50IGRla2ggcmFoYSBjaGFsIGxlamEgZmxhZzogQ1NBWXsxX2g0djNfcmVhRF9SdWwzcyQkfQ==
```


4. **Decode the Base64 String**  
Go to [CyberChef](https://gchq.github.io/CyberChef/) and paste the string. Use the "From Base64" operation to decode it.

5. **Get the Flag**  
The decoded string reveals the following message:

```
Bro kya chaiye rules nahi pade kya jo comment dekh raha chal leja flag: CSAY{1_h4v3_reaD_Rul3s$$}
```

---

## 🔐 Flag

```css
CSAY{1_h4v3_reaD_Rul3s$$}
```