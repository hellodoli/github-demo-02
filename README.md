# Performance best practices with #gRPC:

1. **Reuse gRPC channels:** a gRPC channel should be reused when making gRPC calls. Reusing a channel allows calls to be multiplexed through an existing HTTP/2 connection.

2. **Connection concurrency**: most servers set this limit to 100 concurrent streams; means 100 concurrent calls are multiplexed on that connection.

3. **Load balancing**: Because L4 load balancers operate at a connection level, they don't work well with gRPC. gRPC uses HTTP/2, which multiplexes multiple calls on a single TCP connection. All gRPC calls over that connection go to one endpoint.
   There are two options to effectively load balance gRPC:

4. **Keep-alive ping**: Keep alive pings can be used to keep HTTP/2 connections alive during periods of inactivity. Having an existing HTTP/2 connection ready when an app resumes activity allows for the initial gRPC calls to be made quickly, without a delay caused by the connection being reestablished.

5. **Streaming**: gRPC bidirectional streaming can be used to replace unary gRPC calls in high-performance scenarios. Once a bidirectional stream has started, streaming messages back and forth is faster than sending messages with multiple unary gRPC calls. Streamed messages are sent as data on an existing HTTP/2 request and eliminates the overhead of creating a new HTTP/2 request for each unary call.

6. **Send binary payloads**: chưa cần thiết lắm vì payload của mình không quá lớn.
