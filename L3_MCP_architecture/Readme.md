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
