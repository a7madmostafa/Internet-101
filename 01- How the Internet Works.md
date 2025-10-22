
# 🌐 How the Internet Works --- Summary {#-how-the-internet-works--summary}

## 1. The Internet Itself {#1-the-internet-itself}

The **Internet** is a huge network of computers all connected together.\
Every computer (or "device") that connects to the Internet has an **IP
address**, like a home address, so data knows where to go.



## 2. How Communication Happens: Request → Response {#2-how-communication-happens-request--response}

When you visit a website (e.g. `www.google.com`), your computer:

1.  **Sends a Request** asking for a page.
2.  The web server **sends a Response** containing the page's data
    (HTML, images, etc.).

This "request--response" cycle is the foundation of web communication.


## 3. DNS: Turning Names into Addresses {#3-dns-turning-names-into-addresses}

Humans use names (`www.google.com`),\
but the Internet uses IP addresses (`142.250.190.68`).

**DNS (Domain Name System)** is like a phonebook --- it translates
domain names into IP addresses before your browser can connect.


## 4. TCP/IP: The Internet's Core Protocols {#4-tcpip-the-internets-core-protocols}

Communication happens using two key layers:

### **IP (Internet Protocol)**

-   Handles **addressing and routing**.\
-   Decides *where* data packets go (like envelopes traveling through
    post offices).

### **TCP (Transmission Control Protocol)**

-   Ensures **reliable delivery**.\
-   Breaks data into **packets**, numbers them, sends them, and
    reassembles them at the destination.\
-   Resends any lost packets.

Together, they form **TCP/IP**, the foundation of Internet
communication.


## 5. HTTP: How the Web Works {#5-http-how-the-web-works}

**HTTP (HyperText Transfer Protocol)** is the **language** browsers and
web servers use to communicate over TCP/IP.

Example flow: Browser: GET /index.html HTTP/1.1 Server: 200 OK (returns
HTML content)

-   **HTTP request**: asks for a resource (page, image, etc.)
-   **HTTP response**: sends back the data + status code (200 OK, 404
    Not Found, etc.)

Modern sites often use **HTTPS**, which adds encryption (SSL/TLS) for
security.


## 6. Ports: Entry Points for Communication {#6-ports-entry-points-for-communication}

A **port** is like a specific "door" on a computer for a certain type of
service.

  Protocol   Default Port   Purpose
  ---------- -------------- ---------------------
  HTTP       80             Web traffic
  HTTPS      443            Secure web traffic
  FTP        21             File transfer
  SSH        22             Secure shell access

A full address looks like:\
`192.168.1.5:80` → IP address + port number.


## 7. Putting It All Together (Example) {#7-putting-it-all-together-example}

You type `https://example.com`:

1.  **DNS lookup** → find IP address of `example.com`.
2.  Browser connects to **server's IP via TCP**, using port **443**.
3.  Sends an **HTTP request** (GET `/`).
4.  Server sends **HTTP response** (HTML data).
5.  Browser renders the webpage.


## 🧠 TL;DR Summary {#-tldr-summary}

  Concept                Role
  ---------------------- ----------------------------------------------
  **IP**                 Finds the right computer
  **TCP**                Ensures data arrives correctly
  **Port**               Specifies which service to talk to
  **HTTP**               Defines the format of web requests/responses
  **Request/Response**   The browser asks, the server answers
  **DNS**                Translates names into IP addresses
  **HTTPS**              Adds encryption to HTTP


# 🔧 HTTP, URLs, Sockets & Standards --- Deeper Notes {#-http-urls-sockets--standards--deeper-notes}


## 8) URL = Protocol + Host + Resource (the big idea) {#8-url--protocol--host--resource-the-big-idea}

A **URL (Uniform Resource Locator)** captures three separate things in
one string:
`<scheme>`{=html}://`<host>`{=html}\[:`<port>`{=html}\]/`<path>`{=html}?`<query>`{=html}\#`<fragment>`{=html}

-   **Scheme**: e.g., `http`, `https`, `ftp`, `mailto`. This lets one
    *browser* be **multiprotocol**.
-   **Host**: typically a **domain name** (resolved to an IP).
-   **Resource**: path to the document or API (`/page1.htm`,
    `/api/items`), optional query/fragment.

This generalization (1990, CERN) replaced the "use a specific client for
a specific server/port" era.


## 9) Sockets vs. HTTP (phone line vs. conversation) {#9-sockets-vs-http-phone-line-vs-conversation}

-   A **TCP socket** is the *connection* (the "phone call") to
    `host:port` (e.g., `example.com:80` or `:443`).
-   **HTTP** is the *protocol conversation* **over** that connection
    (the words spoken after the call connects).
-   Typical ports: **80** (HTTP), **443** (HTTPS). HTTPS = HTTP over
    **TLS** (encryption + integrity).


## 10) The minimal HTTP/1.x exchange (by hand) {#10-the-minimal-http1x-exchange-by-hand}

HTTP/1.x is simple enough to type manually (e.g., with `telnet` or
`nc`):

**Client → Server**

    GET /page1.htm HTTP/1.1 
    Host: data.pr4e.org

-   Request line = **METHOD SP PATH SP PROTOCOL** + `\r\n`
-   **Headers** (optional but `Host` is required in HTTP/1.1)
-   **Blank line** = end of headers
-   (Optional) **body** for methods like `POST`

**Server → Client**

    HTTP/1.1 200 OK
    Date: Tue, 22 Oct 2025 09:15:00 GMT
    Server: Example
    Content-Type: text/html
    Content-Length: 1234

    <html> ...body... </html>

**Rules**:

-   Always end headers with a blank line.
-   The server's first line shows the status code.
-   The Content-Type tells the browser what the data represents.

### 🧩 Key HTTP Headers {#-key-http-headers}

  -----------------------------------------------------------------------------
  Header                         Direction   Meaning
  ------------------------------ ----------- ----------------------------------
  **Host**                       Request     Which website (domain) to reach

  **User-Agent**                 Request     Identifies the browser

  **Accept** /                   Request     What formats or languages are
  **Accept-Language**                        preferred

  **Cookie** / **Set-Cookie**    Both        User/session tracking

  **Content-Type**               Both        Format of body data (`text/html`,
                                             `application/json`)

  **Content-Length**             Both        Size of message body

  **Cache-Control**, **ETag**,   Response    Helps caching and freshness
  **Last-Modified**                          

  **Connection: keep-alive**     Both        Keeps TCP connection open for
                                             reuse

### 🚀 HTTP Methods {#-http-methods}

  Method       Safe   Idempotent   Use
  ------------ ------ ------------ ------------------------
  **GET**      ✅     ✅           Retrieve resource
  **HEAD**     ✅     ✅           Metadata only
  **POST**     ❌     ❌           Create or process data
  **PUT**      ❌     ✅           Replace resource
  **PATCH**    ❌     ❌           Partial update
  **DELETE**   ❌     ✅           Delete resource

### 📡 Status Codes (Fast Map) {#-status-codes-fast-map}

  ---------------------------------------------------------------------------
  Class     Meaning      Examples
  --------- ------------ ----------------------------------------------------
  **2xx**   Success      `200 OK`, `201 Created`

  **3xx**   Redirect     `301 Moved`, `302 Found`, `304 Not Modified`

  **4xx**   Client Error `400 Bad Request`, `403 Forbidden`, `404 Not Found`

  **5xx**   Server Error `500 Internal`, `502 Bad Gateway`, `503 Unavailable`
  ---------------------------------------------------------------------------

### 🔒 HTTPS: Security Layer on Top {#-https-security-layer-on-top}

-   Adds TLS (Transport Layer Security) on top of TCP.
-   Encrypts all data --- prevents eavesdropping & tampering.
-   Uses certificates to verify authenticity.
-   Still uses HTTP logic, just over a secure tunnel.
-   Default port: 443.

### ⚡ Modern Versions {#-modern-versions}

-   HTTP/2: Binary format, header compression, multiplexing (many
    requests over one connection).
-   HTTP/3: Uses QUIC (UDP-based) for faster connections, zero
    head-of-line blocking, and built-in encryption.

