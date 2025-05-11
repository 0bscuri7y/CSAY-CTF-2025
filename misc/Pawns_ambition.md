# Pawn's Ambition - Misc Challenge Writeup

## Challenge Description
**Pawn's Ambition** (150 points)

Even the humblest piece dreams of glory. A lowly pawn aspires to become a queen, but her journey is encrypted on a chessboard. Decode her path through the ranks by unraveling this unique blend of chess logic.

Flag Format: CSAY{}

## Files Provided
1. `encrypt.py` - Python script used for encrypting the flag
2. `output.txt` - Contains the encrypted message and the key

## Understanding the Challenge

The challenge involves a cryptographic scheme that uses chess coordinates on an 8x8 board (like a chessboard) to encode characters. Let's analyze the encryption algorithm:

1. A character board is created with 64 characters: `ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!_abcdefghijklmnopqrstuvwxyz`
2. Each character in the flag is mapped to a position on this 8x8 board
3. These positions are represented as chess coordinates (e.g., "a1", "h8")
4. The flag coordinates are XORed with a repeating key's coordinates to create the encrypted message

From `output.txt`, we have:
```
Encrypted message: f5a8e8c7e6d8c6d6d7a5f5f6c5a5b7c6c6e7c6c7f6
Key: a3g4a1b1b2c3f4b1f3c3f4a2g3d2e3e4a3g4a1b1b2
```

## Solution Approach

To decrypt the message, we need to:
1. Convert the encrypted message and key from chess notation to board indices
2. XOR these indices to get the original flag indices
3. Convert the flag indices back to characters

Let's implement the solution:

```python
def generate_board():
    board_chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!_abcdefghijklmnopqrstuvwxyz"
    char_to_index = {ch: idx for idx, ch in enumerate(board_chars)}
    index_to_char = {idx: ch for idx, ch in enumerate(board_chars)}
    return char_to_index, index_to_char

def index_to_chess_coord(index):
    row = 8 - (index // 8)
    col = chr(ord('a') + (index % 8))
    return f"{col}{row}"

def chess_coord_to_index(coord):
    col = ord(coord[0]) - ord('a')
    row = 8 - int(coord[1])
    return row * 8 + col

def decrypt_message(encrypted_coords, key_coords):
    flag_coords = []
    for enc_coord, key_coord in zip(encrypted_coords, key_coords):
        enc_index = chess_coord_to_index(enc_coord)
        key_index = chess_coord_to_index(key_coord)
        original_index = enc_index ^ key_index
        flag_coords.append(index_to_chess_coord(original_index))
    return flag_coords

def coords_to_text(coords):
    _, index_to_char = generate_board()
    text = ""
    for coord in coords:
        idx = chess_coord_to_index(coord)
        text += index_to_char[idx]
    return text

# Parse the encrypted message and key
encrypted_message = "f5a8e8c7e6d8c6d6d7a5f5f6c5a5b7c6c6e7c6c7f6"
key = "a3g4a1b1b2c3f4b1f3c3f4a2g3d2e3e4a3g4a1b1b2"

# Split into individual chess coordinates (2 chars each)
encrypted_coords = [encrypted_message[i:i+2] for i in range(0, len(encrypted_message), 2)]
key_coords = [key[i:i+2] for i in range(0, len(key), 2)]

# Decrypt the message
flag_coords = decrypt_message(encrypted_coords, key_coords)
flag_text = coords_to_text(flag_coords)

print("Decrypted Flag:", flag_text)
```

## Execution Results

When we run our decryption script, we get the following output:

```
Encrypted coordinates (21): ['f5', 'a8', 'e8', 'c7', 'e6', 'd8', 'c6', 'd6', 'd7', 'a5', 'f5', 'f6', 'c5', 'a5', 'b7', 'c6', 'c6', 'e7', 'c6', 'c7', 'f6']
Key coordinates (21): ['a3', 'g4', 'a1', 'b1', 'b2', 'c3', 'f4', 'b1', 'f3', 'c3', 'f4', 'a2', 'g3', 'd2', 'e3', 'e4', 'a3', 'g4', 'a1', 'b1', 'b2']
Decrypted flag coordinates: ['f2', 'g4', 'e1', 'd2', 'f4', 'b3', 'h2', 'c3', 'g4', 'c2', 'a1', 'f4', 'e2', 'd3', 'f4', 'g2', 'c1', 'c3', 'c3', 'd2', 'e4']
Decrypted Flag: pawn_dreams_of_queen!
```

Let's verify the first few coordinates to confirm our approach:

- Encrypted: f5 (index 29) XOR Key: a3 (index 40) = f2 (index 53) = 'p'
- Encrypted: a8 (index 0) XOR Key: g4 (index 38) = g4 (index 38) = 'a'
- Encrypted: e8 (index 4) XOR Key: a1 (index 56) = e1 (index 60) = 'w'

## The Flag

The decrypted message is: `pawn_dreams_of_queen!`

Therefore, the flag is: **CSAY{pawn_dreams_of_queen!}**

## Analysis

This challenge beautifully combines cryptography with the theme of chess. The name "Pawn's Ambition" refers to the chess concept where a pawn can be promoted to a queen when it reaches the opposite end of the board - exactly what our flag is referring to with "pawn_dreams_of_queen!".

The encryption scheme uses XOR with chess coordinates, which is clever because:
1. XOR is reversible (applying the same key twice gets you back to the original)
2. Chess coordinates are intuitive for mapping a 2D space (the 8×8 board)
3. The theme ties perfectly with the flag content

## Key Takeaways

1. Always analyze the provided files thoroughly to understand the encryption method
2. XOR ciphers can be broken by XORing the ciphertext with the key
3. Pay attention to the theme of the challenge - it often provides hints to the solution or flag format
4. Breaking the problem down into smaller steps makes complex challenges more manageable
