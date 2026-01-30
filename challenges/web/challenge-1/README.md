# ryclic - Client-Side Validation Bypass

## Challenge Description
Some guy keeps talking sh*t about PCF. I need you to dislike bomb his post to ensure he learns his lesson.

Challenge URL: `http://ryclic-web.chals.grayhats.club`

<img width="600" alt="Screenshot 2026-01-22 212703" src="https://github.com/user-attachments/assets/31017d94-0c22-486b-91d7-12f0e642459b" />


## Solution

### Initial Analysis
Upon accessing the challenge instance, I was presented with a simple web interface displaying a post by user "kroot" with the text "F**k...". The page had three buttons at the top: "Get Dislikes", "Get Likes", and "Get Flag".

<img width="600" alt="Screenshot 2026-01-22 212714" src="https://github.com/user-attachments/assets/39cb512c-b695-43e2-b95c-403da1f90b21" />


The interface showed two interaction buttons below the post:
- A green thumbs-up (like) button
- A red thumbs-down (dislike) button

Clicking on the "Get Dislikes", "Get Likes", or "Get Flag" buttons at the top didn't lead anywhere, suggesting the flag mechanism was somewhere else.

### Finding the Vulnerability
I decided to examine the page source code to understand how the application worked. Using the browser's Developer Tools, I inspected the JavaScript code and found the following logic:

<img width="600" alt="Screenshot 2026-01-22 212747" src="https://github.com/user-attachments/assets/f2a331a3-5268-491d-9493-6e754ca29b6a" />

The key code revealed:
```javascript
if (dislikes >= 100000000) {
    const flagRes = await fetch('/getFlag');
    if (!flagRes.ok) return;
    const flagData = await flagRes.json();
    alert(flagData.flag);
}
```

This showed that the flag would be displayed when the dislike count reached **100,000,000** (100 million). The application was performing a client-side check, making it vulnerable to manipulation through the browser console.

### Exploitation
Since the validation was happening client-side, I could bypass the need to actually click the dislike button 100 million times. Instead, I directly called the `/dislike` endpoint with the required dislike count.

I opened the browser console and executed the following JavaScript:

```javascript
fetch('/dislike', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({dislikes: 100000000})
}).then(res => res.json()).then(data => alert(data.flag));
```

This sent a POST request directly to the `/dislike` endpoint with a dislike count of 100,000,000, bypassing the client-side button clicks entirely.

<img width="600" alt="Screenshot 2026-01-22 212846" src="https://github.com/user-attachments/assets/96904655-bc35-4000-b1b6-04cfa288b078" />

### Capturing the Flag
After executing the exploit script, the application immediately fetched the flag and displayed it in an alert box:

```
pcf{R7CI1C_is_done}
```
