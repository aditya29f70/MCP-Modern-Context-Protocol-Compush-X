# so in this lec we will discuss the 'what' aspect

- (MCP arch, mcp life cycle(how mcp client and mcp server talk to each others), mcp advance topics) in this lect we will see only **MCP arch** part

## simplest version

- it has two imp things mainly
- - Host ; our ai-chat-bot
- - Server; tool which as capbility to execute a particular task

* what usually happens, host get user query-> it gives query to llm/chat-bot and llm check weather it has to call any tool for getting it's ans or llm response is enough (let there is needed to call tool)it tells host to call those tool/server now server run and return back it's response to host and host gives to llm and llm return back refine information to host and host gives to user

## One imp thing is that ,in reality host don't directly communicate with server

- whenever host has to communicate with server, for that it goes to it's helper friend **MCP client**

- and client can speak same mcp language that the mcp server speaks ; that is why client has that supper easy to communicate with server

- now how it works; host will get query from user (let what is my recent commit on github) host will give to llm/chat-bot and get back a **high level request** and that high level request is sent to client, and client has to convert that request to **mcp compatible request** and once that conversion is happened ,client will send that request to the server; since that request is in mcv format so server can read that request very easly and server works for that task and make **a structured mcp response** and send back to the client and client has to trasnlate this structured mcp request to that language which host can understand and work on it; and this is the process after adding a client

* now let talk litter bit more about connection bw client and server is a **one-and-one** relationship ; means a client can connect only one server at time and communicate ;; if we add one more server to my host so now that client can connect whith that server which is connected with first server and communicating ; so if host has to communicate with that server as well so for that it has to has one more client and that client again connect with **one and one** way with that another server ;; lly if there is any other server we want ,we need to add another client as weel

* MCP arch after having host-client-server knowledge --> see pic2 ;;
* so for that we can take eg/analogy of our phone-sim-network;; to calling someone we need to be connected with a network and for having connected with that network we must have that network sim ; here **phone-host**, **sim-client**, **network-server** ; and sim has **one and one** relationship with network

## Benifit to follow that architecture

* due to **decoupling**
* * safety (one client-server pair is independent to other client-server pair) so we can run things parallel as well 
* * parallelism

* Scalability 
* * now we can connect any huge number of servers to a host


## **Primitives** in MCP arch (imp component in the arch) ;; this is the things the server can offer to host. 

* Eg , Tools, resources , prompts etc
* **Tools** => Actions the AI ask the server to perform ;
eg let we have git-hub server, so it will be giving primitives/tools like ;tool by using it we can ask how many repo we have ; we can also have tool which find recent commit etc 

* **Resources** -> Structured data sources that the ai can read ;; static tiings


* Prompts - Predefined prompt templates or instructions that the server offers to help shape the ai's behavior


## In MCP we have to discuss two final things 
* MCP arch's **data layer** and **transport layer**


## MCP Data Layer
* the data layer is the language and grammar of the mcp ecosystem that everyone agrees upon to communicate

* MCP is the communication bw mcp server and mcp client but we have seen how to use and how it looks; and we study this MCP language and it's grammar in the data layer

* imp fact about MCP data layer => in MCP, **JSON RPC 2.0** serves as the foundation of the data layer => **Json rpc 2.0** is grammer of our mcp language ; or its give us the rule to write the mcl language 

## What is JSON RPC 2.0

* Json rpc stands for **javaScripts Object notation- remote procedure call.**

* A **Remote procedure call** (RPC) allows a program to execute a function on another computer as if it were local , hiding the details of network communication and data transfer. this abstraction makes it easier to build distributed applications.

* means we are building **distributed application** (means its components are at different computers machine) and in this senario **RPC** helps a lot; rpc call these functions which are at differernt machine like they are in same machine; 

* eg. instead of writing add(2, 3) locally , you send a request to the server saying **please run **add** with parameters 2 and 3**  ;; it is basically calling a remote procedure or function

* so what is **JSON RPC** it is , a marries bw RPC and json

* * **json-rpc** combines the concept of **remote procedure calls** with the simplicity  of **Json** allowing developers to structure RPC requests and responses in a standardized json format.;; basically we are using json language to write RPC (or calling rpc) or our communication lang is json for rpc and currently we are using 2nd version of json rpc


* eg let we have to call a function which is at different computer to run our code so for calling that function we use **RPC** and using json lang eg

* request structure
{
  "jsonrpc":"2.0",
  "method":"add",
  "params": [2, 3],
  "id":1 ; help to identify its response from the server
}

* server response will also be in json-rpc format

{
  "jsonrpc":"2.0",
  "result":5,
  "id":1
}

* if there is not any such function then server will send error json rpc response
{
  "jsonrpc": "2.0",
  "error": {"code": -32601, "message":"Method not found"},
  "id":1
}

* it kinds of rest-ful api but techincal they work differently

## so the grammar/rules of mcp communication is **Json-rpc** these format/grammar which we saw are followed 

## now try to see how **mcl data layer** use json-rpc to make request and response

## 'tool/list' is a primitive/tool of a mcp server  which give list of tools present at server

* 1) discover tools (tools/list)

request

{
  "jsonrpc":"2.0",
  "id":1,
  "method": "tools/list",
  "params":{}
}


Response

{
  "jsonrpc":"2.0",
  "id":1, 
  "result":{
    "tools":[
      {"name":"github.listIssues", "description":"List repo issues"},
      {"name":"github.listPulls", "description":"List repo PRs"},
    ]
  }
}


* 2) call a tool (tools/call -> list issues)

* request

{
  "jsonrpc":"2.0",
  "id":1,
  "method": "tools/call", ; this is a primitive of that server
  "params":{
    "name": "github.listIssues",
    "arguments": {"owner":"ownerNmae", "repo": "repoName"}
  }
}

* response

{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "content":[
      {"id":271, "title":"Crash on startup", "state":"open"},
      {"id":272, "title":"Null pointer bug", "state":"open"},
    ]
  }
}


* 3) we also have **resource primitives** (resource/list) ;; lly things are there

* request

{
  "jsonrpc":"2.0",
  "id":1,
  "method": "resources/list",
  "params":{}
}

Response

{
  "jsonrpc":"2.0",
  "id":1, 
  "result":{
    "resources":[
      {"uri":"github://repo/owner/rep", "name":"Main repo"},
      {"uri":"github://repo/owner/rep/issues", "name":"Open issues"},
    ]
  }
}


* 4) Read a resource (resources/read)

* request

{
  "jsonrpc":"2.0",
  "id":4,
  "method": "resources/read",
  "params":{"url":"github://repo/owner/rep/issues"}
}


* response

{
  "jsonrpc":"2.0",
  "id":4, 
  "result":{
    "content":[
      {"uri":"github://repo/owner/rep", "text":[{...issues..}]},
    ]
  }
}


* 5) in json-rpc we can make multiple requests to server it is possible and called -> **Batching of request (many in one request)

* request (we are making multiple requests)

[
  {
    "jsonrpc":"2.0",
    "id":5,
    "method": "resources/read",
    "params":{"url":"github://repo/owner/rep"}
  },
  {
    "jsonrpc":"2.0",
    "id":6,
    "method": "resources/read",
    "params":{"url":"github://repo/owner/rep/issues"}
  }
]


* responses

[
  {
    "jsonrpc":"2.0",
    "id":5, 
    "result":{
      "content":[
        {"uri":"github://repo/owner/rep", "text":[{...issues..}]},
      ]
    }
  },
  {
    "jsonrpc":"2.0",
    "id":6, 
    "result":{
      "content":[
        {"uri":"github://repo/owner/rep/issues", "text":[{...issues..}]},
      ]
    }
  }
]



* 6) new thing ; here we can send notification (fire-and-forget); mean if client want to send any message to server it can , and lly if server want to send any message to client it can

* * notification means , we send thing irrespecting of aspecting any back response

* eg. we have subscribe and unsubscribe methods at resource; what happened after subscribe, server notify client after having any chances at document

eg; (server -> sending to client)

{
  "jsonrpc":"2.8",
  "method": "files/updated",
  "params":{
    "fileId":"abc123",
    "name": "ProjectPlan.docx",
    "updatedBy":"alice@example.com",
    "timestamp":"2025-09-12T08:15:30Z"
  }
}

* you can see 'id' didn't use that means we don't need and back response;; due to json-rpc we have also notification feature in MCP

* 7) Error eg we saw; so imp thing -> json-rpc has it's own error codes and diff error code has their own meaning which we will see

## Why Json RPC for data layer?; why not rest-api bz
* It is light-weight protocol (writing it is easier, debug is easier and over the trasnport medium is also easier to send );; rest-api use http(hyper text transfer protocol) and here lot of meta data , header attached when we make a requet( so it is litter bit time taking and costly )



* Supports bi-directional communication (rest-api also has client-server arch but that is one way communication ; always client request to server first); here server can make a request and send to client and start communication

* It is transport-agnostic; through transport we are seating at a machine and calling another machine functions ;; (like http in restful api) but in json-rpc doesn't have any specifit transport enfact json-rpc is flexible it can work with http, stdio-io, webshoket and if we want we can also build our own cuttom transport;; we will see how having transport agnostic of json-rpc is benificial after covering **transport** topic


* it supports **batching** (in restful api we can make only one request at a time)

* support notification (in restful api , it works one request and one response rule)

etc


## MCP transport Layer

* The **transport layer** is the mechanism that moves JSON-RPC messages between the client and server. (like http)

* transport layer always has **mode of transport** and that mode of transport depend on thing that what kind of **server we want to communicate with**

* there are two kind of servers in mcp
1. Remote ,2. Local

* A **local server** is a program running on our own computer. (or host computer where it is running )

* A **remote server** is a program running on another computer (somewhere else on the computer or internet) that you connect to over a network.

## A local server is a program running on your own   computer -> **STDIO** (standard input - output)
## A **Remote server** is a program running on another computer (somewhere else on the network or internet) that you connect to over a network. 

* or if we are deling with local server then the mode of transport is used in our transport layer is -> **STDIO** (call STD-IO)
* if we are using remote server then the mode of transport is used in our transport layer is -> **HTTP** /**SSE**

## we will discuss both mode of transport

* 1) Local server - STDIO; **STDIO** refers to the built-in streams every program has.; and there is two possible streams
1. stdin (standard input)-> (input the program reads) ; external source from where we take input 
2. stdout (standard output) -> (output the program writes) ; external source to where we show the output 


* In mcp , these streams are used as transport layer between the client and server (how it works, it works in three steps )

## How does STDIO work?

* step1; The host launches the server as a subprocess on the same machine; our host (ai-chatbot) it lanches server as subprocess on the same machine a parent child relationship establisted bw the host and server and that means our host has control on server input and output

* step2: the host(client) write json-rpc messages into the server's stdin
* step3: the server reads those messages, processes them and writes back responses to it's stdout


* and through this control host client send json-rpc messages to server stdin and server recieve these json-rpc messages it read and process it and send it back as stdout to the host this how the entire communication works



## Benefits of the STDIO as transport

* **Fast** -> data is passed directly bw processes 

* **secure** -> no open network port that could be attacked; communication is only local

* **simple** -> every language/runtime(c, c++, python)  supports reading/writing from stdin/stdout. no extra libraries required. 

## remote server (transport-> HTTP+ SSE)

* using http allow the host to reach servers running anywhere

* Host sends JSON RPC requests using POST requests with a json payload (this JSON RPC is putted in json payload)

* the transport supports standard HTTP auth methods (like API keys)


* SSE  stands for server sends events, and it's an extension of http (this is for streaming perpus)

* using SSE the server sends multiple messages to client over a single open connection

* so instead of sending on large JSON blob, the server can steam chunks of data as they are ready
* ideal for long running tasks or incremental updates (streaming)


## there were a thing mentioned 'it is transport-agnostic' for question -> why json rpc for data layer?
-> means we can implement json-rpc with different transport methods, json-rpc don't have any problem to do this
* we saw that when we were working with local server we used stdio transport and for remote server http/see but in both of the places messages were written through json-rpc


## that is what anthropic does they completly seperate data layer and transport ; if we can change transport method , data layer is not effected


