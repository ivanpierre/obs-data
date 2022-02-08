# Matrix

[Matrix.org](https://matrix.org/)

Matrix is really a **decentralised conversation store** rather than a messaging protocol. When you send a message in Matrix, it is replicated over all the servers whose users are participating in a given conversation - similarly to how commits are replicated between Git repositories. There is no single point of control or failure in a Matrix conversation which spans multiple servers: the act of communication with someone elsewhere in Matrix shares ownership of the conversation equally with them. Even if your server goes offline, the conversation can continue uninterrupted elsewhere until it returns.

This means that every server has total self-sovereignty over its users data - and anyone can choose or run their own server and participate in the wider Matrix network. This is how Matrix democratises control over communication.

By default, Matrix uses simple [HTTPS+JSON APIs](https://matrix.org/docs/spec/client_server/latest.html#api-standards) as its baseline transport, but also embraces more sophisticated transports such as [WebSockets](https://github.com/matrix-org/matrix-doc/blob/master/attic/drafts/websockets.rst) or [ultra-low-bandwidth Matrix](https://matrix.org/blog/2019/03/12/breaking-the-100-bps-barrier-with-matrix-meshsim-coap-proxy) via CoAP+Noise.

Alice's homeserver adds the JSON to its graph of history, linking it to the most recent unlinked object(s) in the graph.  
  
The server then signs the JSON **including the signatures of the parent objects** to calculate a tamper-resistent signature for the history.

The server then sends the signed JSON over HTTPS to any other servers which are participating in the room.

The destination servers perform a series of checks on the message:

-   Validate the message signature to protect against tampering with history
-   Validate the HTTP request's auth signature to protect against identity spoofing
-   Validate whether Alice's historical permissions allow her to send this particular message

If these checks pass, the JSON is added to the destination servers' graphs.

Destination clients receive Alice's message with a long-lived GET request. (Clients are free to implement more efficient transports than polling as desired).
