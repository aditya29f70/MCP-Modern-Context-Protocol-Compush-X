# How to connect MCP servers to claude desktop (The how)

## strategy

- so we will have a **ready made client like "claude desktop"** and **already existing server like "google drive"** and this client and server will try to maintain a communication bw them

* so in this lec we will not gonna build our own mcp client or mcp server

* Plan of action:
* so we will install claude desktop and try to connect this client this 4 different mcp servers (2 local servers(file system server, manim) and 2 remote servers ("google drive server" and "twitter server"(for read the twitte and poat the twitte)))

* why we took two-two servers for each why not only one for each bz of **connecter** concept

## Type of connections

- to conecting any server to claude dasktop are two ways 1. **using config file** usually in **ai host** like host has a config file (which is a json file) where we put server detail to connect with it and now we also have another way to connect 2. **Using connectors**

* What are connectors? -> A **Connector** is a built-in feature that links claude to MCP servers automatically without the need for manual setup or configuration ;; by just clicking a button

* Why connector exsist -> Most claude desktop users are **non-technical end-users** who just want claude to "talk" to their apps (Notion, google drive, github, slack, etc)

* They don't want to run servers, edit json, or warry about transports.

* The **connector system** wraps on MCP server behind the scenes and handles authenication via OAuth (sign-in with google, github, etc.)

* This keeps things **easy, safe, and consistent**

## if connector is good things then why we don't force other all mcp servers to be convered into connectors why we give first way as well to connect with using json file or "Why not use Connectors always?" there are two main reasons for that or why only some selected mcp servers are converted into connectors

1. Connectors are **curated and managed**

- see pic-1 ; that is why we will see two servers(for connector and json-file) for each **local servers** as well **remote servers**
