---
title: "Understanding RESP"
datePublished: Sat Dec 30 2023 14:16:33 GMT+0000 (Coordinated Universal Time)
cuid: clqs5bx6p000208jm03p3gz2w
slug: understanding-resp
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xbEVM6oJ1Fs/upload/da068ac1dc88c43f2f9daca1406615aa.jpeg
tags: redis, resp

---

REdis Serialization Protocol

RESP (Redis Serialization Protocol) is a simple protocol used by Redis to communicate between the client and server. It's a text-based protocol that is easy to understand and implement.

Here's a brief overview of how it works:

1. **Simple Strings**: Strings are sent as lines terminated by `\r\n`. For example, `+OK\r\n` represents the string "OK".
    
    These are used to transmit non-binary safe strings and are prefixed with a `+` sign. They cannot contain a CR or LF character (basically, newlines are not allowed). Simple Strings are typically used for sending status replies. For example, `+OK\r\n` is a Simple String representing the string "OK".
    
2. **Errors**: Errors are also sent as strings, but start with a `-`. For example, `-Error message\r\n`.
    
3. **Integers**: Integers are sent as a string of digits, prefixed by a `:`. For example, `:1000\r\n` represents the integer 1000.
    
4. **Bulk Strings**: Bulk strings are binary safe and have a length prefix to specify their length. For example, `$6\r\nfoobar\r\n` represents the string "foobar".
    
    These are used to transmit binary-safe strings and are prefixed with a `$` sign followed by the length of the string. Bulk Strings can contain any kind of data, including binary data, and can be as long as 512MB.
    
5. **Arrays**: Arrays are sent as a prefix with the number of elements, followed by the elements themselves. For example, `*2\r\n$3\r\nfoo\r\n$3\r\nbar\r\n` represents the array \["foo", "bar"\].
    
    arrays are prefixed with an asterisk (`*`) followed by the number of elements in the array.
    
6. **Null Values**: RESP has special representations for null values. A null Bulk String is represented as `"$-1\r\n"`, and a null Array is represented as `"*-1\r\n"`
    

**Request-Response Model**: RESP is used in a request-response model. A client sends a request to the server, the server processes the request and then sends a response back to the client.

**Pipelining**: RESP supports pipelining. This means a client can send multiple requests to the server without waiting for the responses. The server will then send the responses back in the same order as the requests.

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.