# going to build Expense tracker mcp server

- like you will use text and to say about you expenses and it will note down in the database and we can ask about our expenses

## Plan of action

- first build a demo server -> calculater server (for understanding all basics flow)

- then move to the **Expense tracker mcp server** and in next lec we will make it remote server by some inhencement

* before moving forward try to know about FastMCP or MCP SDK

## FastMCP or MCP SDK

- mcp -> here protocal means -> set of rules (and following these rules we can build our own mcp servers)

- so we search how to build mcp server we often get two libraries using that we can build mcp servers -> FastMCP and MCP SDK

- why these libraries bz building these mcp servers from sratch is very 1. Complex 2. reduntent

- so for that we can know about mcp history from 2023 about its library

1. Birth of MCP (2023-2024)

- official python sdk (mcp); mcp sdk released
- - mcp.server -> build server
- - mcp.client -> build clients
- - mcp.cli -> CLI for debugging /testing

- this becomes the **reference implementation**

* we can install it just by command `pip insall mcp[cli]`

* problem -> Raw SDK = verbose and boilerplate heavy
* - manual `list_tools`, `call_tool`, `list_prompts`, etc.
* - Explicit schema definitions required

* solution -> FastMCP helper inside SDK (mcp.server.fastmcp) (2024)
* decorator-driven, type-hint aware:

* popular for **teaching and prototyping**

* further MCP-SDK adopt FastMCP
* so we have `mcp.server.fastmcp import FastMCP`

## 3. FastMCP break out (2025) ; builder of mcp server thought we can inhence fastmcp server in different senorios; but mcp-sdk was only thinking till mcp-server ;; so due to that both mcp-sdk and Fast-mcp got breaked down; and esteblished has a independent libraries

- FastMCP adoption grown rapidly
- sdk maintainers goals:
- - Keep **SDK minimal and spec-focused**
- - let FastMCP evolve faster (QoL APIs, transports, integrations)

- FastMCP 2.x released as standalone package:
- so two options we have mcp-sdk's fastmcp -> that is fastmcp version 1 and second is fastmcp itself this is its version 2 ; currently most of the codes are lly in version1 and version2

* lly inovation is not new ;; it is kind of lly like wsgi(web server getway interface) which help python to interact with web-serversn (like flask which is written in wsgi but easly to connect with servers so we can assume it like wsgi->lly->mcp-SDK and flask->lly->Fast-MCP)

* so we will learn **Fast-MCP**

## Now let build our first mcp server

- there is two tools 1. dice rolling where it will give 1-6 any one random number 2. two number sum

- Basic setup
- - install uv
- - create a project folder named fastmcp-demo-server
- - open the folder in vs code
- - open terminal
- - execute the command `uv init .`
- - `uv add fastmcp` it is like `pip install fastmcp`
- - `fastmcp version` or `uv run fastmcp version`
- - create a basic server
- - test the server - uv run fastmcp dev main.py or `uv run fastmcp dev inspector main.py`
- - run the server - `uv run fastmcp run main.py`
- - add the server to claude desktopo - `uv run fastmcp install claude-desktop main.py` or `uv run fastmcp install claude-desktop main.py  --config-path "C:\Users\Aditya Kumar\AppData\Local\Packages\Claude_pzs8sxrjxfjjc\LocalCache\Roaming\Claude"`

To run your server:

- - `uv run python server.py`

- uv is a package manager which is faster than pip

## Now let move to expense tracker mcp server (check test.py)

- there would be mainly three features

1. Too add expense
2. list expense
3. summarize
4. fourth feature i should implement ; **Edit expense** , 5. delete expense
5. creadit add feature

- will use sqlite database

* currently claude response to add things in database is very random; it is not fixed -> so for that we will add **resource** in the mcp server and that will be a json file (where we write all posible values of a col in a table so claude must to response any one of them )

## FastMCP vs FastAPI

- design philospies are same
- and fastmcp was built compatible with fastapi

- means we can easly build a fastapi app to fastmcp server vice versa
- that reduce time to add same features in fastmcp language from fastapi

## now in next video we will convert this mcp server to remote server

- like remote mcp server is located on different device . Benfit
- - can have mulitple clients
- - since they run on a powerful server on internet that why they also powerful in compare local server
- - slow in-compare to local serve bz in local server host and server run on same system
