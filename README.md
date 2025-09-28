# Burp-Suite---Modifying-and-Reissuing-Requests

> A short, hands-on lab modifying intercepted requests with **Burp Suite (Community Edition)** and using **Burp Repeater** to re-issue interesting requests. This uses intentionally vulnerable labs provided by PortSwigger.

---

## Safety & Legal Notice

**Important:** Only perform testing on systems you have explicit permission to test. The examples below use PortSwigger training labs (safe and legal). Do **not** attempt these techniques against production or third-party systems without written authorization.

---

## Table of contents
1. [Burp Proxy — Modifying HTTP Requests](#modifying-requests)
2. [Burp Proxy — Reissuing Requests (Repeater)](#reissuing-requests)

<a id="modifying-requests"></a>
## Burp Proxy — Modifying HTTP Requests

**Goal:** Intercept the "Add to cart" request and change the `price` parameter before it reaches the server.

### Steps

1. Download and open **Burp Suite (Community Edition)**.
2. Click `Next` on the first two startup screens and then **Start Burp**.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/690942e0-2aea-4c90-9299-3a1a65afc5a7">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/2422cdab-0938-477f-8ff9-c02428032f1f">


3. Click the **Proxy** tab and then **Open browser** (or configure your normal browser to proxy via Burp). 

4. Arrange your windows side-by-side (browser and Burp).
5. <img width="1414" alt="image" src="https://github.com/user-attachments/assets/7459f395-806a-4358-b64b-5292299fc44a">
6. Visit the lab: [PortSwigger lab — Logic vulnerabilities](https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-excessive-trust-in-client-side-controls)

<img width="600" alt="image" src="https://github.com/user-attachments/assets/5ffa1933-2a8a-4c04-b280-e3189844495f">

8. Create a free account on PortSwigger if you don't already have one.
9. Click **Access The Lab**. 
10. Log in to the demo shopping site. Lab credentials used here:
```yaml
username: wiener
password: peter
```
10. On the shop, click View details for Lightweight "l33t" Leather Jacket.
<img width="1414" alt="image" src="https://github.com/user-attachments/assets/7a03c0a8-c4fc-4831-96a7-abf7377b8e23">

11. In Burp, toggle Intercept off to turn interception on (The order of this is really important). 

12. Back in the browser, click Add to cart.
<img width="1414" alt="image" src="https://github.com/user-attachments/assets/98aa7d89-b901-410b-9091-a2764cecabea">

13. In Burp’s Proxy → Intercept panel you will see the intercepted request. Scroll the request body until you see a price parameter (example showed 133700, i.e. $1337.00).
<img width="800" alt="image" src="https://github.com/user-attachments/assets/4ea9a381-6f7e-4bc1-a906-e0cdc6a2c9b8">

14. Edit the parameter in the request (or use the Inspector → Request body parameters editor). Change 133700 → 1.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/f99b7685-2f51-42fd-92f1-c2a74b83d756">

15. Click the dropdown next to Forward on the toolbar and choose Forward all. 
16. Turn Intercept on back off (so further requests won’t be paused).
17. In the browser, open your cart — the item now shows the modified price!
<img width="1414" alt="image" src="https://github.com/user-attachments/assets/3d06dd0c-063d-4c3a-86b9-5ea37afcff98">




<a id="reissuing-requests"></a>
## Burp Proxy — Reissuing Requests (Repeater)

**Goal:** Use Burp Repeater to send a product request repeatedly with different inputs and observe errors/info leaks.

### Steps

1. Visit the lab: [PortSwigger lab — Infoleak in error messages](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-error-messages)
2. Click **Access The Lab**.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/e7b9972e-903e-4743-b3f1-7758c186301f">

3. Browse several product detail pages to generate traffic.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/a297918a-a083-4d20-a14a-8805e544cc42">


4. In Burp's **Proxy → HTTP history**, click the column header `#` to sort requests by time (descending).
5. Right-click a product page URL and choose **Add to scope**.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/aa6c4194-1d6e-4d2c-b9de-7f112524d9aa">

6. Click the filter icon and enable **Show only in-scope items** → **Apply & close**.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/448820a7-b8e9-4f54-b853-0b330bd4db3b">

7. Select a product request (right-click) → **Send to Repeater**.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/d133af24-fede-4411-85fd-8c475c2d7b38">

8. Open **Repeater**. The request appears in the left pane. Edit the product ID in the first line and click **Send**.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/77778f4c-d116-4ef1-81db-b3d32ff1799f">


**Examples:**

- Try a very large integer → you might get `HTTP/1.1 404 Not Found`.
- Try a string (non-numeric) → you might get `HTTP/1.1 500 Internal Server Error` and a server error message that discloses implementation details (e.g., `Apache Struts 2.2.3.31`).

I get a response of HTTP 500 Internal Server Error response and get some information that could be helpful for an attacker.  At the end of the Response message, there is a line reading: "Apache Struts 2.2.3.31".  An attacker would now know the web application and version being used here and can use that information for an attack!

<img width="800" alt="image" src="https://github.com/user-attachments/assets/3769372e-9a08-498f-8768-1c5bea4f416c">






