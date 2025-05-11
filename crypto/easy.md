# Challenge: Easy

**Category:** Crypto  
**Description:**  
Can you find the flag with this string?

VVRGT1FsZFlkR2xaV0U1c1RtcFNaazFZVG1aa1IyZDZXREp6ZW1WV09UQmlNVGd6WVVkV1ptTXpWbXBSZWs1NlZUTXdQUT09

---

## Steps to Solve

1. **Recognize the Encoding**  
   The provided string clearly resembles Base64-encoded data.

2. **Decode the String Multiple Times**  
   Use a tool like [CyberChef](https://gchq.github.io/CyberChef/) or any Base64 decoder. Perform the **"From Base64"** operation **three times** consecutively.

3. **Extract the Flag**  
   After decoding it three times, the final output reveals:

```yaml
CSAY{base64_1s_th3_k3y_to_7he_sucC3sS}
```

---

## Flag
```css
CSAY{base64_1s_th3_k3y_to_7he_sucC3sS}
```