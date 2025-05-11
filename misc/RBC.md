
# RBCs can correct us!


### Understanding the encoding
The encoder takes the original bit-array b[0..n-1] (MSB to LSB) and produces an output bit-string e[0..n-1] such that:

1. `e[0] = b[0]`
2. For each j from 1 to n-1: `e[j] = b[j] XOR b[j-1]`

This is because the code builds adjacent XORs and then reverses the result.


## Solution Approach

To invert it back, we use - 
```
b[0] = e[0]
for j in range(1, n):
    b[j] = e[j] XOR b[j-1]
```

```python
encoded = "1100010 1111010 1100001 1110101 1000110 1001011 101010 1010110 1110000 1010011 1011010 101000 101000 1010110 1110000 1010010 101010 1011010 1011010 101111 1110000 1001100 101100 1010101 1000011"
flag = ""

for chunk in encoded.split():
    e_bits = [int(b) for b in chunk]
    # reconstruct original bits
    b = [0] * len(e_bits)
    b[0] = e_bits[0]
    for j in range(1, len(e_bits)):
        b[j] = b[j-1] ^ e_bits[j]
    # turn bit-list into an integer, then to a char
    val = int("".join(str(bit) for bit in b), 2)
    flag += chr(val)

print(flag)

```

`CSAY{r3d_bl00d_c3ll5_w7f}`