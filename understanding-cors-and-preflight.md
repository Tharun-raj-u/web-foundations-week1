
# Understanding CORS and Preflight

## Concepts

### What is CORS and Why It Exists
CORS (Cross-Origin Resource Sharing) is a browser security feature that controls how web pages can make requests to a different origin.  
It exists to prevent malicious websites from reading sensitive data from another site without permission.

### Security Perspective
From a security angle, CORS protects users from **Cross-Site Request Forgery (CSRF)** and **data theft** by requiring servers to explicitly declare which origins can access their resources. Without CORS, any site could make hidden requests to another site on behalf of a logged-in user and read the response.

### What is an Origin
An origin is the combination of **scheme (protocol) + host (domain) + port**.  
Example:  
- `https://example.com:443` → scheme: `https`, host: `example.com`, port: `443`.

### Same-Origin vs Cross-Origin
- **Same-origin**: `https://example.com/page1` and `https://example.com/page2` → same scheme, host, and port.  
- **Cross-origin**: `https://example.com` and `https://api.example.com` → different subdomain.

### What is a Cross-Origin Request
A request is cross-origin when the requesting page’s origin is different from the resource’s origin. This includes differences in scheme, host, or port.

### Simple Requests vs Preflighted Requests
- **Simple request**: GET, POST (form-encoded), or HEAD with no custom headers and content types limited to `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`.  
- **Preflighted request**: Any request that does not meet “simple” criteria triggers a preflight OPTIONS request.

### What is a Preflight OPTIONS Request
A preflight is a browser-sent OPTIONS request made before the actual request to check if the server allows the cross-origin operation.  
The server must respond with CORS headers for the actual request to proceed.

### What Triggers a Preflight
- Custom request headers (e.g., `X-Auth-Token`).
- HTTP methods other than GET, POST, HEAD (e.g., PUT, DELETE, PATCH).
- Content types outside of the simple request set (e.g., `application/json`).

### Key CORS Response Headers
- **Access-Control-Allow-Origin**: Specifies which origin is allowed (cannot be `*` when credentials are included).  
- **Access-Control-Allow-Methods**: Lists allowed HTTP methods (e.g., `GET, POST, PUT`).  
- **Access-Control-Allow-Headers**: Lists allowed custom headers.  
- **Access-Control-Allow-Credentials**: If `true`, allows cookies/auth headers to be sent; must be used with a specific origin, not `*`.

### Why It Works in Postman or curl but Not in the Browser
Postman and curl are not browsers, so they don’t enforce CORS rules. CORS is enforced by browsers as a client-side security measure, not by the server or API itself.

---

## Practical Investigation

**Setup:**  
Tested with `index.html` containing buttons for different fetch calls.

### Observations
In the first case, a simple GET request was sent and worked without any issues. Since it was a simple request, the browser did not send a preflight OPTIONS request, and no CORS changes were needed.  

In the second case, a simple POST request with form-encoded data was tested. This also worked fine without triggering a preflight request. No adjustments were required here either.  

In the third case, a POST request with a JSON body was made. Because this is considered a non-simple request, the browser first sent a preflight OPTIONS request. The request still succeeded because the server responded correctly with the required CORS headers.  

The fourth case involved a preflight request with credentials (cookies or authentication headers). This request failed because the server responded without a specific `Access-Control-Allow-Origin` header that matches the requesting site. Instead, it was using `*`, which is not allowed when sending credentials. The fix would be to configure the server to return the exact origin instead of a wildcard.

---

## Real-World CORS Problem – Learning from a Developer

**Problem:**  
While learning, she tried to integrate her frontend with the backend API and send requests with cookies (credentials). But the browser threw a CORS error because the backend was using Access-Control-Allow-Origin: *, which is not allowed when sending credentials.

**Solution:**  
The backend was updated to return the exact frontend origin instead of * and added Access-Control-Allow-Credentials: true. After that, the browser allowed the requests and the integration worked fine.

**My Takeaway:**  
When working with cookies or authentication, you must always specify the exact allowed origin instead of `*`. I will also remember to check the OPTIONS preflight response first when debugging CORS issues.
