# a-tragic-story - Information Disclosure & HTTP Method Manipulation

## Challenge Description
A tragic dystopian story on the surface. Can you unravel the secrets underneath?

Challenge URL: `https://a-tragic-story-80056d04a1f5.instancer.grayhats.club`

<img width="600" height="1371" alt="Screenshot 2026-01-22 215512" src="https://github.com/user-attachments/assets/2afacab9-3fc1-4d1d-a64b-3f6d3fd8bc1e" />


## Solution

### Initial Analysis
Upon accessing the challenge instance, I was presented with an "Article Dashboard" containing four article cards with camel-themed titles and descriptions:

1. "Found this cool camel on the shelf"
2. "I understand it now."
3. "DROMEDARY ON TOP!!!!" (this article's button was unclickable)
4. "American dream huh"

<img width="600" height="1386" alt="Screenshot 2026-01-22 214048" src="https://github.com/user-attachments/assets/ba48323d-adec-40f0-a57c-de92a2483182" />

### Finding Flag Fragment 1/3 - Hidden HTML Comments

I clicked on the first article "Found this cool camel on the shelf" which displayed the full article content.

<img width="600" height="1402" alt="Screenshot 2026-01-22 215043" src="https://github.com/user-attachments/assets/7b59e3a0-0f41-4a44-9bac-c95ec90b5e72" />

Using the browser's "View Page Source" feature, I examined the HTML and discovered a hidden comment containing the first fragment of the flag:

<img width="600" height="1384" alt="Screenshot 2026-01-22 215058" src="https://github.com/user-attachments/assets/eef404e8-07d6-4f4b-8ad2-73665d74015b" />

```html
<p hidden>PCF{1t_w</p>
```

**First fragment found: `PCF{1t_w`**

### Finding Flag Fragment 2/3 - robots.txt Discovery

Next, I examined the fourth article "American dream huh" which showed a camel holding a sign saying "free flags please".

<img width="600" height="1367" alt="Screenshot 2026-01-22 215152" src="https://github.com/user-attachments/assets/23e15708-d539-4527-8e61-08436881e790" />


Inspecting the page source revealed another hidden clue:

<img width="600" height="1357" alt="Screenshot 2026-01-22 215203" src="https://github.com/user-attachments/assets/5dc32679-1fb4-43d7-8f6f-6bd7d5b24b4d" />

```html
<p hidden>
  I AI generated these three images, the robots must be 
  eating good. I wonder where they are?
</p>
```

This hint about "robots" suggested checking the `robots.txt` file, which is a standard file used to instruct web crawlers about which parts of a site to access.

I navigated to `https://a-tragic-story-80056d04a1f5.instancer.grayhats.club/robots.txt` and found:

<img width="600" height="1395" alt="Screenshot 2026-01-22 215331" src="https://github.com/user-attachments/assets/988eb497-7d4a-46f3-91ba-fc9296be3129" />


```
User-agent: * Disallow: /secret 45_51ddh4
```

**Second fragment found: `45_51ddh4`**

This also revealed a `/secret` endpoint that was disallowed for web crawlers.

### Finding Flag Fragment 3/3 - HTTP Method Exploitation

Armed with knowledge of the `/secret` endpoint, I attempted to access it directly by navigating to:
`https://a-tragic-story-80056d04a1f5.instancer.grayhats.club/secret`

The page returned a message:

<img width="600" height="1384" alt="Screenshot 2026-01-22 215407" src="https://github.com/user-attachments/assets/43f129c5-460c-4a2d-9619-cb88bf023116" />


```
No Gets!
```

This indicated that GET requests were not allowed on this endpoint. The message suggested trying a different HTTP method. I opened the browser console and used JavaScript to send a POST request instead:

<img width="600" height="1371" alt="Screenshot 2026-01-22 215512" src="https://github.com/user-attachments/assets/99782276-79af-41e2-a68d-2e910e9e7139" />


```javascript
fetch('/secret', {
    method: 'POST'
})
.then(res => res.text())
.then(data => console.log(data));
```

The response contained the final fragment:

```
5_f4u1t}
```

**Third fragment found: `5_f4u1t}`**

### Assembling the Flag

Combining all three fragments discovered through different methods:

<img width="360" height="27" alt="Screenshot 2026-01-22 215630" src="https://github.com/user-attachments/assets/3c96d9be-0602-4b9f-8796-a69c447adac6" />


