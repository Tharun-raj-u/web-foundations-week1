

<div align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&duration=3000&pause=1000&color=2F81F7&center=true&vCenter=true&width=800&lines=How+the+Browser+Works;What+Happens+When+a+User+Types+a+URL+and+Presses+Enter" alt="Typing SVG" />
</div>



When a user enters a URL  `https://tharun.com` into the browser's address bar and presses Enter, the browser begins a complex process to retrieve and render the requested web page. This involves parsing the URL, resolving the domain, establishing a secure connection, fetching the response, and rendering the content.

## How the Domain Name is Resolved to an IP Address (DNS)

The browser cannot directly understand human-readable domain names. It needs the corresponding IP address to locate the server. This translation is done via the Domain Name System (DNS). The steps include:

1. **Cache Check**: The browser checks its own and the OS cache for a cached IP.
2. **DNS Query**: If not cached, a DNS query is sent to a DNS resolver.
3. **Recursive Lookup**: The resolver contacts root, TLD, and authoritative DNS servers.
4. **IP Return**: The final IP address of the domain is returned to the browser.

## How the Browser Connects to the Server Securely (HTTPS)

With the IP address known, the browser initiates a secure connection using HTTPS. This includes:

1. **TCP Handshake**: A TCP connection is established on port 443.
2. **TLS Handshake**:
   - The browser and server exchange supported encryption methods.
   - The server sends its SSL/TLS certificate.
   - A shared encryption key is generated securely.
3. **Secure Session**: Communication is now encrypted using symmetric encryption.

## How the Response is Received and Parsed

Once the secure connection is established:

1. The browser sends an HTTP GET request to the server.
2. The server responds with an HTTP response, typically containing HTML.
3. The browser begins parsing the response immediately, even before the entire document is received.
4. Linked resources (CSS, JS, images) are discovered and downloaded asynchronously.

## How the Browser Renders the Page

The browser rendering pipeline includes several stages:

1. **HTML Parsing** → DOM (Document Object Model) construction.
2. **CSS Parsing** → CSSOM (CSS Object Model) construction.
3. **JS Execution**: JavaScript may modify the DOM and CSSOM.
4. **Render Tree**: Built from DOM and CSSOM.
5. **Layout**: Calculates position and size of elements.
6. **Painting**: Converts elements into actual pixels.
7. **Compositing**: Combines painted layers for display.



