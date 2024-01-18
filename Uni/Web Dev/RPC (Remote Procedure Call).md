RPC - defining a service, specifying the methods that can be called remotely with their parameters and return types

gRPC is an open source framework that implements RPC
protobuf (Protocol Buffers) - the Interface Definition Language (IDL) for describing both the service interface and the structure of the payload messages
stub - a piece of code that converts parameters passed between the client and server during a remote procedure call, is essentially just a usability feature to provide the appearance the remote method is present locally

gRPC has four types of service methods
1. unary RPC - client sends a single request to the server and gets a single response back
```proto
rpc SayHello(HelloRequest) returns (HelloResponse);
```
2. Server streaming RPCs where the client sends a request to the server and gets a stream to read a sequence of messages back
```proto
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse);
```
3. Client streaming RPCs where the client writes a sequence of messages and sends them to the server, again using a provided stream. Once the client has finished writing the messages, it waits for the server to read them and return its response.
```proto
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse);
```
4. Bidirectional streaming RPCs where both sides send a sequence of messages using a read-write stream. The two streams operate independently, so clients and servers can read and write in whatever order they like
```proto
rpc BidiHello(stream HelloRequest) returns (stream HelloResponse);
```


### Protocol Buffers
Used to serialize structured data
The structure of data is defoned in a proto file (.proto)
It contains data structured as messages
```proto
// The greeter service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```
The protocol buffer compiler protoc can be used to generate data access classes from the proto file in a supported programming language

gRPC Python - Todo App
Server
```python

```

Client
```python
```

Proto File