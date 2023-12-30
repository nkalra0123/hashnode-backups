---
title: "Demystifying CORS: Understanding Access-Control-Allow-Origin"
seoDescription: "Understanding Cross-Origin Resource Sharing (CORS) and the error related to Access-Control-Allow-Origin"
datePublished: Sat Dec 30 2023 14:14:25 GMT+0000 (Coordinated Universal Time)
cuid: clqs596dk000008lc2sxb8boa
slug: demystifying-cors-understanding-access-control-allow-origin
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/VUoMzpSFMrY/upload/295d9f8dcc114cdd7cd4966703f28ffe.jpeg
tags: cors, cors-errors

---

Cross-Origin Resource Sharing (CORS) is a security feature implemented by web browsers to control how web pages in one domain can request and interact with resources from another domain. CORS is crucial for maintaining a secure and controlled web environment.

In this blog post, we'll delve into one of the common CORS-related issues we encounter—the "Access-Control-Allow-Origin" error—and explore ways to address it.

Understanding the Error:

Imagine you're developing a web application on your local machine, running at '[http://localhost:3000](http://localhost:3000/)', and you attempt to make an XMLHttpRequest to fetch data from '[https://example.com](http://localhost:3000/)'. If the server at '[https://example.com](http://localhost:3000/)' lacks the appropriate CORS headers, the browser will block the request, triggering an error like:

```javascript
Access to XMLHttpRequest at 'https://example.com' from origin 'http://localhost:3000' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

Breaking Down CORS:

CORS operates on the principle of restricting web pages from making requests to a domain different from the one that served the web page. The 'Access-Control-Allow-Origin' header plays a pivotal role in CORS by indicating which origins are permitted to access the resources on the server.

![CORS_principle](https://www.freecodecamp.org/news/content/images/2020/07/CORS_principle.png align="left")

To send an OPTIONS request to a server using CURL, we can use the following command:

```bash
curl -i 'https://sessions.bugsnag.com/' \
  -X 'OPTIONS' \
  -H 'authority: sessions.bugsnag.com' \
  -H 'accept: */*' \
  -H 'accept-language: en-IN,en-GB;q=0.9,en;q=0.8,en-US;q=0.7' \
  -H 'access-control-request-headers: bugsnag-api-key,bugsnag-payload-version,bugsnag-sent-at,content-type' \
  -H 'access-control-request-method: POST' \
  -H 'origin: https://example.com' \
  -H 'referer: https://example.com/' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: cross-site' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36 Edg/121.0.0.0' \
  --compressed
```

Explanation of the flags and headers:

* `-i`: Includes the HTTP headers in the output.
    
* `-X 'OPTIONS'`: Specifies the HTTP method as OPTIONS.
    
* Various `-H` flags: Set headers for the request, including authority, accept, accept-language, access-control-request-headers, access-control-request-method, origin, referer, sec-fetch-dest, sec-fetch-mode, sec-fetch-site, and user-agent.
    
* `--compressed`: Requests compressed response.
    

We get a response like this

```bash

HTTP/1.1 200 OK
Access-Control-Allow-Headers: Origin, Content-Type, Accept, Authorization, User-Agent, Referer, X-Forwarded-For, Bugsnag-Api-Key, Bugsnag-Payload-Version, Bugsnag-Sent-At
Access-Control-Allow-Methods: POST
Access-Control-Allow-Origin: *
Date: Fri, 29 Dec 2023 14:09:28 GMT
Content-Length: 0
Via: 1.1 google
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
```

Let's break down the response we received:

**Explanation:**

* `HTTP/1.1 200 OK`: Indicates that the server successfully processed the OPTIONS request, and the response status is "OK" (HTTP status code 200).
    
* `Access-Control-Allow-Headers`: Specifies the headers that are allowed when making the actual request. The headers include Origin, Content-Type, Accept, Authorization, User-Agent, Referer, X-Forwarded-For, Bugsnag-Api-Key, Bugsnag-Payload-Version, Bugsnag-Sent-At.
    
* `Access-Control-Allow-Methods: POST`: Specifies that the server allows the POST method when accessing the resource.
    
* `Access-Control-Allow-Origin:` *: Indicates that any origin is allowed to access the resource. The wildcard '*' means any origin is permitted.
    
* `Date: Fri, 29 Dec 2023 14:09:28 GMT`: Provides the date and time when the response was generated.
    
* `Content-Length: 0`: Specifies the length of the response body in bytes. In this case, the response body is empty.
    
* `Via: 1.1 google`: Indicates that the response passed through a Google proxy server.
    
* `Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000`: Specifies alternative services, including HTTP/3 and their parameters.
    

In summary, the server has responded with a 200 OK status, allowing the specified headers and methods for the main request. The server is configured to accept requests from any origin (`Access-Control-Allow-Origin: *`).

**What are some HTTP Response Headers that we get in Options Call.**

This section lists the HTTP response headers that servers return for access control requests as defined by the Cross-Origin Resource Sharing specification. The 'Access-Control-Allow-Origin' header, for instance, specifies either a single origin or uses the '\*' wildcard to allow any origin to access the resource.

```javascript
Access-Control-Allow-Origin: <origin> | *
```

For example:

```javascript
Access-Control-Allow-Origin: https://mozilla.org
```

---

**Vary: Origin:**

The `Vary` HTTP response header describes the parts of the request message aside from the method and URL that influenced the content of the response it occurs in. Most often, this is used to create a cache key

If the server specifies a single origin, it should include 'Origin' in the 'Vary' response header to indicate to clients that server responses will differ based on the value of the 'Origin' request header.

```javascript
Vary: Origin
```

---

**Access-Control-Expose-Headers:**

The 'Access-Control-Expose-Headers' header adds specified headers to the allowlist that JavaScript in browsers is allowed to access.

```javascript
Access-Control-Expose-Headers: <header-name>[, <header-name>]*
```

For example:

```javascript
Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header
```

---

**Access-Control-Max-Age:**

The 'Access-Control-Max-Age' header indicates how long the results of a preflight request can be cached.

```javascript
Access-Control-Max-Age: <delta-seconds>
```

The delta-seconds parameter indicates the number of seconds the results can be cached.

---

**Access-Control-Allow-Credentials:**

The 'Access-Control-Allow-Credentials' header indicates whether or not the response to the request can be exposed when the credentials flag is true.

```javascript
Access-Control-Allow-Credentials: true
```

---

**Access-Control-Allow-Methods:**

The 'Access-Control-Allow-Methods' header specifies the method or methods allowed when accessing the resource, used in response to a preflight request.

```javascript
Access-Control-Allow-Methods: <method>[, <method>]*
```

---

**Access-Control-Allow-Headers:**

The 'Access-Control-Allow-Headers' header is used in response to a preflight request to indicate which HTTP headers can be used when making the actual request.

```javascript
Access-Control-Allow-Headers: <header-name>[, <header-name>]*
```

This header is the server-side response to the browser's 'Access-Control-Request-Headers' header.

**The origin responsible for serving resources need to set Access-Control-Allow-Origin header.**

**Server-Side Configuration:**

* **Specify Allowed Origins:**
    
    * Configure your server to include the "Access-Control-Allow-Origin" header in its responses, specifying the allowed origins.
        

**Server-Side Frameworks:**

Many server-side frameworks provide built-in support for CORS. For example, in Node.js with Express, you can use the 'cors' middleware

**Local Development Configuration:**

* During local development, you can disable CORS in your browser or use browser extensions to bypass it temporarily. However, this is not a recommended solution for production.
    

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.