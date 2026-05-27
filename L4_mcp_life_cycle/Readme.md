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
* 1. **roots**(give base directly access to server), 2. **sampling** (server can ask to use client features like if client as Ai then server can ask to use it), 3. elicitation (server tell client if client gave some incomplete input) ;; so here server is making request to the client for proving an incomplete information

* Server has 4 main capabilities

1. prompts , 2, resources (to fetch any static documents), 3, tools (fetch different functions) , 4. logging (kind for pooling ; eg like ticket booking app, usually there are multiple steps so what happens in logging that server try to send message after having done every step)

## there are two **sub-capabilities** for (server-> client initialization steps)

- listChanged : "server use this capability to inform client during session if there are some other new tools are added by notification or something removed from server"
- subscribe : "this is from resources ; if there is some changes in any resources then server inform client about that using notification ;; like let our server is github then if any file get changed then that information is passed to client bz it is subsribed "

# 2. Operation Phase

- during the operation phase, the client and server exchange messages according to the negotiated capabilities.

* here we have to keep 2 things in our mind

1. respect the negotiated protocol version , 2. Only use capabilities that were successfully negotiated

## so that whole operation phase can be divided into two parts (**capability discovery**)

- during capability nagotiation client know it has access of server tools ; what it don't knmow what are tools (subtools in that facility) , so for knowing that it sends a json message again to the server ; {"jsonrpc":"2.0", "id":2, "methods":"tools/list"} ;; lly if it want to know in resource access facility what are resources it can access it calls (server method -> "resource/list") ;; lly for prompt -> method: 'prompt/list'

* and client get this kind of json-rpc message
  {
  "jsonrpc":"2.0",
  "id":2,
  "result":{
  "tools":[
  {"name":"listRepos", "description":"List user/org repositories"},
  {"name":"getfile", "description":"Read a file from a repo (path@ref)"},
  {"name":"searchCode", "description":"Search code access repos"},
  ...
  ]
  }
  }

* Note these three requests are by default/automatically called after just **initialization** phase -> "tools/list", "resources/list", "prompts/list" ;; and server sends response from all these requests and response can be a error response (we can get "method not found error" it just mean server has tools method but in tools , server didn't have 'tools/list' method)

* after getting these list inform client send these info to host and host keeps these info somewhere so in future user can make request for any one of them
* so after that another phase come which is tool calling ; now host will handle all the tools according to user query and host has ai so after calling tools , host will get tools answer and host ai will inhence response using its ai

# 3. Tool calling

- so client send request for caling server tools ; it looks like

{
"jsonrpc":"2.0",
"id":3,
"method":"tools/call",
"params":{
"name":"getFile",
"arguments":{
"owner":"compusx-official",
"repo":"mcp-examples",
"path":"README.md",
"ref":"main"
}
}
}

- and client get response from the server like this jsonrpc message

{
"jsonrpc":"2.0",
"id":3,
"result":{
"content": "# MCP Examples\nThis repo contains MCP examples...",
"encoding": "utf-8",
"sha":"f3c0.."
}
}

# 4. Now the last phase is **ShutDown phase** (here no jsonrpc meessage get to see )

- session which is being used during communication bw client and server get terminated in this phase and for this there are only two reason

1. client get shutdown , 2. server get shutdown ;; one side (typically the **client**, initiates shutdown)

- imp;; **No special JSON RPC shutdown message** is defined

* infact-> **Transport Layer** (stdio(for local server) or http/see(for remote server)) is responsible for signaling termination ;; now see how this showdown layer works in both of the cases

## ShutDown in STDIO (this is for local server)

- client-initiated shutdown (**SHOULD**):
- - Close input stream to the child process (server) ;; close std-i ;; bz client has control of server input and output as subprocess

* - Wait for server to exit

* - and still server not exit then clinet take help of operating system to send message to server called **SIGTERM** (signal terminate)
* still server not exit then send **SIGKILL**(client force os to send a signal kill message to server) if still unresponsive

### server-initiated shutdown (**MAY** happens)

- Close output stream to the client

* Exit process (since server itself want to close then it become very easy)

## Shutdown in HTTP (remote server case)

- client-initiated shutdown (common case):
- the client (host) **closes the HTTP connection(s)** it opened to the server. ;; or client try to close that post connection which it has built for http

- Server-initated shutdown(possible)
- - the server may close the connection from its side
- - the client must be prepared to detect a dropped connection and handle it (eg. reconnect if appropriate)

## often we get to see these phase but some time some special case we get to see

# Special cases

- **Pings**
- - Ping is a lightweight request/response method defined in MCP .
- purpose: to check whether the other side (host or server) is still alive and the connection is responsive.

- in this process a jsonrpc message can be sent to client side or on server side (there is both side possiblity)

{
"jsonrpc":"2.0", "id":42, "method":"ping"
}

- and whoever(client/server) got this jsonrpc message other on wait for response ; response looks

{
"jsonrpc":"2.0", "id":42, "result":{}
}

## when is ping used?

- useful for checking if hte other side is up before full **initialize**.

- if there's no activity for a while, a client may send periodic pings. Prevents the connection from being dropped sliently by the os, proxies , or irewalls
