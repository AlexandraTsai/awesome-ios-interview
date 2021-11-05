* [General](#general)
  * [What is the difference in functional programming and OOPS?](#what-is-the-difference-in-functional-programming-and-oops)
  * [HTTP request types](#http-request-types)
  * [Communication protocols](#communication-protocols)
   	* [Network socket](#network-socket)
   	* [WebSocket](#websocket)
   	* [TCP Stream Socket](#tcp-stream-socket)
   	* [UDP Datagram Socket](#udp-datagram-socket)
   	* [CFStream](#cfstream)
    * [NSStream](#nsstream)
  * [REST](#rest)
  * [SOAP](#soap)
  * [Deep and shallow copy](#deep-and-shallow-copy)
  * [Difference between functions and methods in Swift?](#what-is-the-difference-between-functions-and-methods-in-swift)
  * [Protocol Oriented Programming](#protocol-oriented-programming)  

# General

## What is the difference in functional programming and OOPS?
<center><img src = "https://cdn-images-1.medium.com/max/1600/1*wo1pE2iCDBIgVaCIPxw6cw.png" width=500></center>

## HTTP request types
An HTTP request is a class consisting of HTTP style requests, request lines, request methods, request URL, header fields, and body content. The most common methods that are used by a client in an HTTP request are as follows:

1) GET:- Used when the client is requesting a resource on the Web server.

2) HEAD:- Used when the client is requesting some information about a resource but not requesting the resource itself.

3) POST:- Used when the client is sending information or data to the server—for example, filling out an online form (i.e. Sends a large amount of complex data to the Web Server).

4) PUT:- Used when the client is sending a replacement document or uploading a new document to the Web server under the request URL.  

# Communication protocols

## Network socket

__Network socket__ is an internal endpoint for sending or receiving data at a single node in a computer network. Concretely, it is a representation of this endpoint in networking software (protocol stack), such as an entry in a table (listing communication protocol, destination, status, etc.), and is a form of system resource.
The term "socket" is analogous to physical female connectors, communication between two nodes through a channel being visualized as a cable with two male connectors plugging into sockets at each node. Similarly, the term "port" (another term for a female connector) is used for external endpoints at a node, and the term "socket" is also used for an internal endpoint of local inter-process communication (IPC) (not over a network). However, the analogy is strained, as network communication need not be one-to-one or have a channel.

## WebSocket

__WebSocket__ is a computer communications protocol, providing full-duplex communication channels over a single TCP connection. The WebSocket protocol was standardized by the IETF as RFC 6455 in 2011, and the WebSocket API in Web IDL is being standardized by the W3C.
WebSocket is a different TCP protocol from HTTP. Both protocols are located at layer 7 in the OSI model and, as such, depend on TCP at layer 4. Although they are different, RFC 6455 states that WebSocket "is designed to work over HTTP ports 80 and 443 as well as to support HTTP proxies and intermediaries" thus making it compatible with the HTTP protocol. To achieve compability, the WebSocket handshake uses the HTTP Upgrade header[1] to change from the HTTP protocol to the WebSocket protocol.
The WebSocket protocol enables interaction between a browser and a web server with lower overheads, facilitating real-time data transfer from and to the server. This is made possible by providing a standardized way for the server to send content to the browser without being solicited by the client, and allowing for messages to be passed back and forth while keeping the connection open. In this way, a two-way (bi-directional) ongoing conversation can take place between a browser and the server. The communications are done over TCP port number 80 (or 443 in the case of TLS-encrypted connections), which is of benefit for those environments which block non-web Internet connections using a firewall. Similar two-way browser-server communications have been achieved in non-standardized ways using stopgap technologies such as Comet.

## TCP Stream Socket
Is much more commonly used, as it provides the framework for a complete, structured "conversation" to occur between the two endpoints. TCP connections provide a means to ensure the message was received, and guarantees that packets are received in order. TCP is used by protocols including HTTP, FTP, and others where data must be reliably sent and received in order. To keep track of the ordering of packets, TCP employs a sequence number, which identifies the sequence of each packet. This not only keeps your conversation in order, but also adds a basic level of protection against some forms of spoofing (data forgery by a malicious party).

* Dedicated & point-to-point channel between server and client.
* Reliable and Lossless.
* Data sent/received in the similar order.
* Long time for recovering lost/mistaken data
* Web, SSH, FTP, TELNET, SMTP, IMAP/POP

## UDP Datagram Socket

Is used for sending short messages called datagrams to the recipient. Datagrams are single packets of data that are sent and received without any "return postage." There is no guarantee that the recipient will receive a particular packet, and multiple packets may be received out of order. Datagrams are generally thought of as unreliable, in the same way that a carrier pigeon can be unreliable. This form of communication is used for sending short query/response-type messages that do not require authentication, such as DNS (name resolution) lookups, as well as by some protocols where lost packets are irrelevant; such as live video streams and online multiplayer games, where an interruption can be ignored.

* No dedicated & point-to-point channel between server and client.
* Not 100% reliable and may lose data.
* Data sent/received order might not be the same
* Don't care or rapid recovering lost/mistaken data
* Tunneling/VPN (lost packets are ok - the tunneled protocol takes care of it)
* Media streaming
* Games that don't care if you get every update
* Local broadcast mechanisms (same application running on different machines "discovering" each other)
***
```c
CFSocketRef CFSocketCreate (
    CFAllocatorRef allocator,
    SInt32 protocolFamily,
    SInt32 socketType,
    SInt32 protocol,
    CFOptionFlags callBackTypes,
    CFSocketCallBack callout,
    const CFSocketContext *context
);
```
## CFStream

Socket streams provide an easy interface for reading and writing data to or from a socket. Each socket can be bound to a read and write stream, allowing for synchronous or asynchronous communication. Streams encapsulate most of the work needed for reading and writing byte streams, and replace the traditional `send()` and `recv()` functions used in C. Two different stream objects are used with sockets: `CFReadStream` and `CFWriteStream`

## NSStream

__`NSStream`__ is an abstract class that defines the fundamental interface and properties for all stream objects. `NSInputStream` and `NSOutputStream` are subclasses of `NSStream` and implement default input-stream and output-stream behavior. `NSStream` is built on the `CFStream` layer of Core Foundation. This close relationship means that the concrete subclasses of `NSStream`, `NSOutputStream` and `NSInputStream`, are toll-free bridged with their Core Foundation counterparts `CFWriteStream` and `CFReadStream`. Although there are strong similarities between the Cocoa and Core Foundation stream APIs, their implementations are not exactly coincident. The Cocoa stream classes use the delegation model for asynchronous behavior (assuming run-loop scheduling) while Core Foundation uses client callbacks. Despite their strong similarities, __`NSStream` does give you a major advantage over `CFStream`. Because of its Objective-C underpinnings, it is extensible.__ You can subclass `NSStream`, `NSInputStream`, or `NSOutputStream` to customize stream attributes and behavior. For example, you could create an input stream that maintains statistics on the bytes it reads; or you could make a `NSStream` subclass whose instances can seek through their stream, putting back bytes that have been read. `NSStream` has its own set of required overrides, as do `NSInputStream` and `NSOutputStream`.

## REST
An architectural style called REST (Representational State Transfer) advocates that web applications should use HTTP as it was originally envisioned. Lookups should use GET requests. PUT, POST, and DELETE requests should be used for *mutation, creation, and deletion respectively *.

REST proponents tend to favor URLs, such as

server.com/catalog/thing/1723
but the REST architecture does not require these “pretty URLs”. A GET request with a parameter

server.com/catalog?thing=1723
is every bit as RESTful.

GET requests should never be used for updating information. For example, a GET request for adding an item to a cart

server.com/addToCart?cart=314159&thing=1723

would not be appropriate. GET requests should be idempotent. That is, issuing a request twice should be no different from issuing it once. That’s what makes the requests cacheable. An “add to cart” request is not idempotent—issuing it twice adds two copies of the item to the cart. A POST request is clearly appropriate in this context. Thus, even a RESTful web application needs its share of POST requests.

## SOAP

SOAP (originally Simple Object Access Protocol) is a protocol specification for exchanging structured information in the implementation of web services in computer networks. Its purpose is to induce extensibility, neutrality and independence. It uses XML Information Set for its message format, and relies on application layer protocols, most often Hypertext Transfer Protocol (HTTP) or Simple Mail Transfer Protocol (SMTP), for message negotiation and transmission.

SOAP allows processes running on disparate operating systems (such as Windows and Linux) to communicate using Extensible Markup Language (XML). Since Web protocols like HTTP are installed and running on all operating systems, SOAP allows clients to invoke web services and receive responses independent of language and platforms.

## Deep and shallow copy
There are two kinds of object copying: shallow copies and deep copies. The normal copy is a shallow copy that produces a new collection that shares ownership of the objects with the original. Deep copies create new objects from the originals and add those to the new collection.

## What is the difference between functions and methods in Swift?

Both are functions in the same terms any programmer usually knows of it. But functions are globally scoped while methods belong to a certain type.

## Protocol Oriented Programming

Protocol-Oriented Programming is a new programming paradigm ushered in by Swift 2.0. In the Protocol-Oriented approach, we start designing our system by defining protocols. We rely on new concepts: protocol extensions, protocol inheritance, and protocol compositions. The paradigm also changes how we view semantics. In Swift, value types are preferred over classes. However, object-oriented concepts don’t work well with structs and enums: a struct cannot inherit from another struct, neither can an enum inherit from another enum. So inheritancefa - one of the fundamental object-oriented concepts - cannot be applied to value types. On the other hand, value types can inherit from protocols, even multiple protocols. Thus, with POP, value types have become first class citizens in Swift.

__Protocol Extensions__

You can extend a protocol and provide default implementation for methods, computed properties, subscripts and convenience initializers.

__Protocol Inheritance__

A protocol can inherit from other protocols and then add further requirements on top of the requirements it inherits.

__Protocol Composition__

Swift types can adopt multiple protocols.
