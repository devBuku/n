# HTTP (Hypertext Transfer Protocol)

## What is HTTP?

- **HTTP** stands for **Hypertext Transfer Protocol**.
- A protocol is a set of rules that defines how computers communicate and exchange information.
- A protocol tells communicating systems how to structure, send, receive, and interpret information.

## HTTP Request-Response Model

At the heart of HTTP is a simple **request-response system**.

1. The **client** is the computer or application requesting information.
2. The **server** is the system that receives the request and sends back a response.
3. The server processes the request and returns a response containing the requested information or an appropriate status.

For example:

- Browser → HTTP request → Server
- Browser ← HTTP response ← Server

## URLs and Protocols

- URLs are not specific to HTTP. They can be used with different schemes, such as `https://`, `ftp://`, and others.
- The scheme at the beginning of a URL indicates which protocol or resource-access mechanism is being used.

Example:

```text
https://example.com
```


1. **HTTP does not necessarily mean communication over the Internet.** It can also be used within a local network.
2. `fetch()` does not return the response body directly. It returns a Promise that resolves to a `Response` object.
3. `response.json()` does not simply convert text into an object; it reads the response body and parses it as JSON. The result can be an object, array, string, number, boolean, or `null`, depending on the JSON.
4. **Important interview point:** `fetch()` generally does not reject its Promise for HTTP errors such as `404` or `500`. Check `response.ok` or `response.status` yourself.

Example:

```ts
const response = await fetch(url);

if (!response.ok) {
    throw new Error(`HTTP Error: ${response.status}`);
}

const data = await response.json();
```

# Web Addresses, IP Addresses & DNS

## 1. Web Addresses and IP Addresses

**Question:** How can we tell the Internet's routing system that we want
to communicate with a specific server?

This is where **IP addresses** come into play.

-   An IP address identifies a network interface so that it can be
    addressed over an IP network.
-   When we access a server using a domain name, DNS helps us find the
    IP address needed to communicate with it.
-   IP routing uses destination IP addresses to forward packets towards
    their destination.

### IPv4

-   IPv4 addresses consist of **4 sections**, called *octets*, separated
    by dots.
-   Each octet contains 8 bits and has a decimal value between `0` and
    `255`.
-   An IPv4 address is 32 bits long.
-   Example: `192.168.1.1`

### IPv6

IPv6 was introduced to provide a much larger address space.

-   IPv6 addresses are **128 bits** long.
-   They are written as **8 groups** of hexadecimal digits separated by
    colons (`:`).
-   Each group represents 16 bits.
-   Example: `2001:db8:85a3:0000:0000:8a2e:0370:7334`
-   IPv6 also supports shortened notation, such as omitting leading
    zeros and replacing one consecutive run of zero groups with `::`.

## 2. DNS (Domain Name System)

**DNS = Domain Name System**

-   DNS is a distributed naming system that maps domain names to
    information such as IP addresses.
-   It is commonly described as the **phonebook of the Internet**.

For example:

``` text
example.com → IP address
```

### How DNS Resolution Works

When we want to communicate with a server using a domain name, the
domain usually needs to be resolved to an IP address.

A simplified process:

1.  Our computer checks its local DNS cache for an existing result.
2.  If the result isn't available, it sends a query to a configured DNS
    resolver (often called a recursive resolver).
3.  If the resolver doesn't already have the answer cached, it can query
    the DNS hierarchy:
    -   **Root name server:** directs the resolver towards the relevant
        Top-Level Domain (TLD) name servers.
    -   **TLD name server:** directs the resolver towards the domain's
        authoritative name servers.
    -   **Authoritative name server:** provides the DNS record for the
        requested domain, such as its IP address.
4.  The resolver returns the answer to our computer and may cache it.
5.  Our computer can then use the IP address to communicate with the
    server.

> **Important:** Our computer does not normally contact an ICANN root
> name server directly for every request. It usually contacts a
> configured DNS resolver, which performs the lookup when necessary.

### DNS Is Distributed

DNS is **not one giant phonebook stored in one place**. It is a
distributed, hierarchical system made up of many servers and
organizations.

Think of DNS as a phonebook:

-   **Domain name** → A person's name.
-   **IP address** → Their phone number.
-   **DNS resolver** → The service that looks up the number.
-   **DNS hierarchy** → The system of directories that helps find the
    authoritative answer.

### What Is ICANN?

**ICANN = Internet Corporation for Assigned Names and Numbers**

- ICANN is a nonprofit organization that coordinates important parts of the Internet's unique identifier systems.
- Its responsibilities include coordinating the DNS root system,
   global IP address allocation policy (through the regional Internet
   registries), and the domain name system's top-level coordination.
- ICANN does **not** operate every DNS server or maintain a single
   database containing every domain name and its IP address.
- The DNS root zone contains information about top-level domains such as `.com`, `.org`, and `.in`.

> **Remember:** ICANN coordinates the global system; DNS resolvers and
> authoritative name servers perform the actual domain lookups.

## 3. Subdomains

A subdomain is a section added before a registered domain name.

For example:

``` text
example.com
api.example.com
blog.example.com
```

-   `example.com` → Domain name.
-   `api.example.com` → Subdomain.
-   `blog.example.com` → Subdomain.

Subdomains are useful for organizing different services or resources
under the same domain without purchasing separate domain names.

For example:

-   `api.example.com` could be used for an API.
-   `blog.example.com` could be used for a blog.
-   `www.example.com` could be used for the main website.

> A subdomain does not necessarily correspond to a separate physical server. Multiple subdomains can point to the same server, and one subdomain can use multiple servers.

## 4. URI vs. URL

### URI (Uniform Resource Identifier)

A URI is a string used to identify a resource.

### URL (Uniform Resource Locator)

A URL is a type of URI that provides a way to locate a resource, including information about how and where to access it.

**Every URL is a URI, but not every URI is a URL.**

## 5. Understanding URL Components Using JavaScript

JavaScript provides a built-in `URL` class that allows us to parse and
inspect different parts of a URL.

``` js

const urlObj = new URL("https://api.example.com:8080/users?id=10#profile");

console.log(urlObj.protocol); // https:
console.log(urlObj.hostname); // api.example.com
console.log(urlObj.port);     // 8080
console.log(urlObj.pathname); // /users
console.log(urlObj.search);   // ?id=10
console.log(urlObj.hash);     // #profile
console.log(urlObj.origin);   // https://api.example.com:8080
```

# Ports
- **If a port isn't present in a URL the default is used based on the Protocol**
- Ports are virtual little hubs managed by the operating system that help us segment incoming request and incoming data stream
- we cannot bind two pieces of software to the same port(then the operating system wouldn't know which software would need to handle which incoming request)
### Query Parameters
- query paramneters are not always present in the URL
- they often will change the page's content

### I/O Timings
- Getting data from RAM = nanosecond (we can do it synchronously)
- Getting data from Disk = 1ms (slow enough which can be problem to do it synchronously) (async / sync) (suppose loading a file from the disk on your vscode it will really be bad if the file is very large and my entire vscode froze therfore mostly these operations are done asynchronously)
- Getting data on network  (like making a http request / working on the internet) = 100ms to 2000ms (we typically work over the internet asynchronously)

[[freecodecamp]]
[[boot.dev]]
