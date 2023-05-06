---
title: "How JWT authentication works"
datePublished: Sat May 06 2023 10:56:59 GMT+0000 (Coordinated Universal Time)
cuid: clhbvejbm001p09mfgulofaei
slug: how-jwt-authentication-works
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ybtUqjybcjE/upload/2ac89672112d1c81ce7df7fd18d8b0f1.jpeg
tags: jwt, jsonwebtoken, json-web-tokens-jwt

---

JWT, or JSON Web Token, is a popular method of authentication used by many web applications. In this blog, we'll explore how JWT authentication works and why it's become so widely used.

A JWT is a compact, URL-safe means of representing claims to be transferred between two parties. It consists of three parts: a header, a payload, and a signature. The header and payload are base64 encoded and separated by a period, and the signature is created using a secret key.

```yaml
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.9Rytq70s3ILjIwGrU-3lEBqBxl5ho7Kk3e1iSHs3Yn0
```

You can check more information about JWT at [JSON Web Tokens -](https://jwt.io/) [jwt.io](http://jwt.io)

So, how does JWT authentication work?

### Client side

Let's say a user wants to log in to a web application. The user enters their username and password, and the server verifies that the credentials are correct. If they are, the server generates a JWT for the user and sends it back to the client. The client can then include the JWT in future requests to the server to authenticate themselves.

### Server Side

When the client includes the JWT in a request, the server first verifies the signature to ensure that the token is legitimate. If the signature is valid, the server decodes the payload to extract information about the user and their permissions. This information can be used to determine whether the user is authorized to perform the requested action.

### Benefits

One of the main benefits of JWT authentication is that it's stateless. This means that the server doesn't need to keep track of any session information for each user, which can be especially useful in applications with high traffic.

Additionally, because the token is self-contained and includes all the necessary information, it can be easily shared between different services and APIs.

JWTs are widely supported by different programming languages and frameworks, making them easy to implement and integrate into existing systems.

These are some of the reasons why JWT authentication has become so widely used.

### Best practices

There are a few best practices to keep in mind when using JWT authentication. First, make sure to use a strong secret key to sign the tokens. This key should be kept secret and only known to the server.

It's a good idea to include an expiration time in the payload to limit the lifespan of each token and prevent potential security issues.

Always use HTTPS to ensure that the tokens are transmitted securely.

### Access tokens and refresh tokens

Access tokens and refresh tokens are two important components of JWT authentication.

* **Access tokens** are short-lived tokens that are used to grant access to protected resources on a server. It carries the necessary information to access a resource directly. In other words, when a client passes an access token to a server managing a resource, that server can use the information contained in the token to decide whether the client is authorized or not. **Access tokens usually have an expiration date and are short-lived**.
    
* **Refresh tokens** are longer-lived tokens that are used to obtain new access tokens when the current one expires. It carries the information necessary to get a new access token. In other words, whenever an access token is required to access a specific resource, a client may use a refresh token to get a new access token issued by the authentication server.
    

Here's how the flow typically works:

1. The user logs in to the web application, and the server generates an access token and a refresh token.
    
2. The server sends the access token and refresh token to the client.
    
3. The client includes the access token in requests to the server to access protected resources.
    
4. When the access token expires, the client sends a request to the server with the refresh token.
    
5. The server verifies the refresh token and generates a new access token, which is sent back to the client.
    
6. The client can continue to use the new access token to access protected resources.
    

One of the main benefits of using refresh tokens is that it reduces the number of times a user needs to log in to the web application. This can provide a better user experience and also reduce the load on the server.

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.