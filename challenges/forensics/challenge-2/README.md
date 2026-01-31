# picture-this-1 - Base64 Image Decoding

## Challenge Description
No description provided - just a file to download.

<img width="600" height="1387" alt="Screenshot 2026-01-22 222143" src="https://github.com/user-attachments/assets/6bd29e82-f1a3-4344-a5cb-c6001c22d878" />


## Solution

### Initial Analysis
Upon viewing the challenge, I was presented with a downloadable file named `pcf_picture-this-1.tar.gz`. The challenge title "picture-this-1" suggested that the solution involved an image, but no image was immediately visible.

After downloading and extracting the archive, I found a text file containing what appeared to be a large Base64-encoded string.

### Step 1: Examining the File Contents
Opening the extracted file in a text editor revealed a massive Base64-encoded string:

<img width="600" height="1461" alt="Screenshot 2026-01-30 182659" src="https://github.com/user-attachments/assets/9be68255-5d65-4559-8c07-28e0c4f73ba6" />

The format and characters (A-Z, a-z, 0-9, +, /, =) confirmed this was Base64 encoding.

### Step 2: Recognizing the Image Format
Given the challenge name "picture-this-1", I realized that this Base64 string likely encoded an image file. Base64 is commonly used to encode binary data (like images) into ASCII text format.

### Step 3: Converting Base64 to Image
I used a Base64 to Image decoder tool at [codebeautify.org](https://codebeautify.org/base64-to-image-converter) to convert the Base64 string back into an image:

<img width="600" height="1393" alt="Screenshot 2026-01-22 222506" src="https://github.com/user-attachments/assets/bfdda1b6-f97e-4b29-a77a-ca4eeb906922" />

### Capturing the Flag
The decoded image revealed a picture of a cute blob character with the flag text visible on it.


