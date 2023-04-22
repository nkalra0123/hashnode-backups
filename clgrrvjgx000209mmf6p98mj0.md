---
title: "How HTTP server works"
seoDescription: "how http server works, learn how socket accept and read is used in servers"
datePublished: Sat Apr 22 2023 09:22:51 GMT+0000 (Coordinated Universal Time)
cuid: clgrrvjgx000209mmf6p98mj0
slug: how-http-server-works
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/40XgDxBfYXM/upload/0d52ea042b7be6aceae49e826a55d92f.jpeg
tags: java, server, tcp

---

An HTTP server is a piece of software that listens for incoming requests from web browsers and other HTTP clients and responds to them accordingly. When you enter a website URL in your browser, it sends a request to the server hosting that website, which in turn sends back the necessary files and data to display the webpage.

```java
public void run() {
        try {
            httpd.getMyServerSocket().bind(httpd.hostname != null ? new InetSocketAddress(httpd.hostname, httpd.myPort) : new InetSocketAddress(httpd.myPort));
            hasBinded = true;
        } catch (IOException e) {
            this.bindException = e;
            return;
        }
        do {
            try {
                final Socket finalAccept = httpd.getMyServerSocket().accept();
                if (this.timeout > 0) {
                    finalAccept.setSoTimeout(this.timeout);
                }
                final InputStream inputStream = finalAccept.getInputStream();
                httpd.asyncRunner.exec(httpd.createClientHandler(finalAccept, inputStream));
            } catch (IOException e) {
                NanoHTTPD.LOG.log(Level.FINE, "Communication with the client broken", e);
            }
        } while (!httpd.getMyServerSocket().isClosed());
    }
```

The above code snippet is an example of how an HTTP server works in Java. Let's break it down step by step.

The server starts by binding to a specific port using the `ServerSocket` class. This port number is the entry point for incoming requests. In this case, the server is binding to a specific hostname and port number, but it can also bind to all available interfaces (using the wildcard `0.0.0.0`) to listen for requests from any source.

Once the server has bound to the port, it enters into a loop to listen for incoming connections. When a client connects to the server, the server accepts the connection using `ServerSocket.accept()`, which creates a new socket object that represents the connection to the client. This socket is then used to communicate with the client over the network.

After the server accepts the connection, it sets a timeout for the socket if one has been specified. This timeout limits the amount of time the server will wait for the client to send data. If no data is received within the specified timeout period, the connection will be closed.

Next, the server creates an input stream from the socket's input stream. This input stream is used to read data from the client. The server then uses an `AsyncRunner` object to execute a new `ClientHandler` object on a separate thread. This is done asynchronously to allow the server to continue listening for new connections while the client request is being handled.

The `ClientHandler` object reads the incoming request data from the input stream, processes the request, and generates a response. This response is then sent back to the client using the socket's output stream.

Finally, the server returns to the beginning of the loop and waits for another connection. This loop will continue running until the server is stopped or encounters an error.

In conclusion, an HTTP server works by listening for incoming requests on a specified port, accepting connections from clients, and handling those requests by generating appropriate responses. This process is repeated for each incoming connection, allowing the server to handle multiple requests simultaneously.

[TCP server in java (](https://gist.github.com/nkalra0123/cffcf7d3363082b18ac8d5fc038128b9)[github.com](http://github.com)[)](https://gist.github.com/nkalra0123/cffcf7d3363082b18ac8d5fc038128b9)

[NanoHttpd/nanohttpd: Tiny, easily embeddable HTTP server in Java. (](https://github.com/NanoHttpd/nanohttpd)[github.com](http://github.com)[)](https://github.com/NanoHttpd/nanohttpd)

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.