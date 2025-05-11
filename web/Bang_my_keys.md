## 🎹 Challenge Name: Bang My Keys  
**Category**: Web  
**Points**: XX  
**Author**: AuthorName  

---

### 📝 Description:
We’re given some encoded JavaScript strings and asked to recover a secret melody and decrypt a flag using XOR.

---

### 🔍 Analysis:

1. **Melody Encoding**:
   - The encoded string:
     ```js
     atob('QyxDLEQsQyxGLEUsQyxDLEQsQyxHLEYsQyxDLEMyLEEsRixFLEQsQSMsQSMsQSxGLEcsRg==')
     ```
   - Decoding with `atob()` gives a series of comma-separated note codes.
   - The music seems to be purely cosmetic or celebratory.

2. **XOR Decryption**:
   - Key: `"PIANO"`  
   - Encrypted flag array:
     ```js
     [19,26,0,23,52,29,122,45,126,43,97,44,50,17,2,37,26,112,13,16,22,29,22,51]
     ```

3. **Script to Decrypt**:
   ```js
   const encryptedFlag = [19,26,0,23,52,29,122,45,126,43,97,44,50,17,2,37,26,112,13,16,22,29,22,51];
   const key = "PIANO";
   let flagText = "";

   for (let i = 0; i < encryptedFlag.length; i++) {
       flagText += String.fromCharCode(encryptedFlag[i] ^ key.charCodeAt(i % key.length));
   }

   console.log("Flag:", flagText);
   ```

   - This script outputs the flag after XOR decryption using `"PIANO"` as the key.

---

### 🧠 Learning:
- XOR encryption is common in CTFs.
- Recognizing and decoding base64 strings using `atob`.
- Decrypting byte arrays using JavaScript in the browser console.

---

### 🏁 Flag:
```
Flag{your_actual_flag_here}
```
