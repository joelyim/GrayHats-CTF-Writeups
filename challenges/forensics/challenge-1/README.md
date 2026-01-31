# decode - Base64 and ROT13 Decoding

## Challenge Description
Y3Bze3ExcV9sMGhfM2U0NXJfejN9

<img width="600" height="1400" alt="Screenshot 2026-01-22 212920" src="https://github.com/user-attachments/assets/eda486d2-4ae7-41da-91d1-7e77c25e9914" />

## Solution

### Initial Analysis
Upon viewing the challenge, I was presented with a simple description consisting of what appeared to be a Base64-encoded string:

```
Y3Bze3ExcV9sMGhfM2U0NXJfejN9
```

The format strongly suggested this was Base64 encoding, as it used the typical Base64 character set (A-Z, a-z, 0-9, +, /).

### Step 1: Base64 Decoding
I used [CyberChef](https://gchq.github.io/CyberChef/) to decode the Base64 string using the "From Base64" operation:

<img width="600" height="1309" alt="Screenshot 2026-01-22 212952" src="https://github.com/user-attachments/assets/fcbfa4ce-ad67-4298-81da-89a0f018080d" />


**Input:**
```
Y3Bze3ExcV9sMGhfM2U0NXJfejN9
```

**Output:**
```
cps{q1q_l0h_3e45r_z3}
```

The output looked like a flag format (`cps{...}`), but the content was scrambled and didn't make sense. This suggested an additional layer of encoding.

### Step 2: ROT13 Decoding
Recognizing that the text appeared to be obfuscated with a simple substitution cipher, I tried ROT13, a common encoding scheme that shifts each letter by 13 positions in the alphabet.

Using "ROT13" operation on the decoded Base64 output:

<img width="600" height="1105" alt="Screenshot 2026-01-22 213012" src="https://github.com/user-attachments/assets/06d6fc30-87d1-41d6-b9cc-e69187941bff" />

**Input:**
```
cps{q1q_l0h_3e45r_z3}
```

**ROT13 Output:**
```
pcf{d1d_y0u_3r45e_m3}
```

### Capturing the Flag
The flag was revealed after applying both Base64 and ROT13 decoding:

```
pcf{d1d_y0u_3r45e_m3}
```


