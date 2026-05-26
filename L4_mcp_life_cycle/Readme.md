## The mcp lifecycle

- we saw about its arch (host, client, server) now in this lec we will see how their life cycle works
- What is MCP life-cycle ?
- - the **mcp life cycle** describes the **complete sequence of steps** that govern how a host(client) and server establish, use, and end a connection during a **sesssion** ; session ia a one contineous connection bw client and server

* Stages of MCP Lifecycle
* - MCP lifecycle has three stages
* 1. **Initialization**(host/client try to connect with server at the start of the session), 2.**operation** (we are sending user's question to server and server responding on it), 3. **shut down** (we break session either by closing the host/client(mostly) or server)

# 1. Initialization Phase

- The initialization phase **Must** be the first interaction between client and server.

* Establish protocol version compatibility (bw client ans server); check whether our client and server are on the same protocal version or not ;; if not then there is chances that our code will crash in future

* Exchange and negotiate capabilities (in similar lang; client tell server what it can do and server tell client what it can do )

## we can also break down intialization phase to three steps

- Step 1

* - the client sends a **initialize** request containing; eg (where client server that what it can do for server)
    {
    "jsonrpc":"2.0",
    "id":1,
    "method":"initialize", ## **you are calling a server method, its name is "initialize"**
    "params": {
    "protocolVersion":"2025-03-26", ## this **MCP protocol version**
    "capabilities":{
    "roots":{"listChanged":true}, ## this and below one ; **client capabilities** ; first is roots using it host gives access of directly to server
    "sampling":{} ## this is for if server needs to call ai it can use client side ai
    },
    "clientInfo":{"name":"IDEPlugin", "version":"1.0.0"} # clent sends its implementation info (client name and version)
    }
    }

* step 2 (once server get this client request it response back to client)

* - The server sends its own capabilites and info

  {
  "jsonrpc":"2.0",
  "id":1,
  "result": {
  "protocolVersion":"2025-03-26", ## this **MCP protocol version**
  "capabilities":{
  "tools":{"listChanged":true}, ## this and below ; **server capabilities** ;
  "resouces":{ ## this is for if server needs to call ai it can use client side ai
  "listChanged":true,
  "subscribe":true
  }
  },
  "serverInfo":{"name":"IDEPlugin", "version":"1.0.0"}, # (server name and version)
  "instructions":"Server is ready to accept commands"
  }
  }

* step 3 (once intialization things are successfully completed, now client has a responsibility to send a notification to server to convey that connection is successful) -> called **intialized notification**
* - After successful initialization, the client **MUST** send an **initialized** notification in indicate it is ready to begin normal operations: eg

{
"jsonrpc":"2.0",
"method":"notifications/initialized"
} # since it is notification so there is not any 'id'. means server don't has to response back

## Now the client and server are connected

- during this intialization we have to keep two things in our mind (important rules)
- The client **should not** send requests other than **pings** before the server has responded to the **intialize** request. (imp; and will see about **ping** request) ;; here client has to wailt unitill (step 2 is not done)

* The server **should not** send request other than **pings** and **logging** before receiving the **initilized** notification. (here server has to wait untill step 3 is not done)

## Before moving next phase ;;; we should discuss two important things

- 1. Version Negotiation (we saw that client and server was sharing their protocol versions now the thing which we are gonna talk , what if there is a protocol versions mismatch bw them (called varion negotiation))

* so after that client go to its config file and check what all mcp versions it supports and check this server protocol version comes under that version list or not ;; if that version comes in that list then only client send the notification and if not there then client directly disconnect from the server ;; ;that is called version nagotiation

* in config there would be **SUPPORTED_PROTOCOL_VERSIONS**= ["2025-03-26","2024-11-05"]

* 2. Capability Negotiation

* - Client and server capabilities establish which protocol features will be available during the session.

* we know in this **initiliazation** client and server share there capability so they will know which things they can ask to call

* what are capabilities exsist bw client and server

* client has three majar capabilities
* 1. roots(give base directly access to server), 2. sampling (server can ask to use client features like if client as Ai then server can ask to use it), 3. elicitation (server tell client if client gave some incomplete input) ;; so here server is making request to the client for proving an incomplete information

* Server has 4 main capabilities

1. prompts , 2, resources, 3, tools , logging
