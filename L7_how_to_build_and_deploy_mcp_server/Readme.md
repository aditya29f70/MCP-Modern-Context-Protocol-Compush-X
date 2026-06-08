- like remote mcp server is located on different device . Benfit
- - can have mulitple clients
- - since they run on a powerful server on internet that why they also powerful in compare local server
- - slow in-compare to local serve bz in local server host and server run on same system

## Plan of action

- note; remote and local mcp server seem same in code point of view

1. First build Simple remote server
2. Make our expense tracking mcp server to remote mcp server and deploy it -> we can deploy on cloud or render like plateform but we will deploy on **fastmcp cloude** free service
3. find some problems and rectify them

## Steps for simple remote server

1. install uv
2. create new folder
3. open folder in vs code
4. `uv init .`
5. `uv add fastmcp`
6. create simple server
7. Run the server `uv run main.py` or `fastmcp run main.py --transport http --host 0.0.0.0 --port 8000` or simply `uv run main.py`
8. Test the server using mcp inspector
9. Create a GitHub repo
10. Git init
11. git add .
12. git commit -m "initial commit : simple MCP server"
13. git remote add origin ".."
14. git push origin main
15. create an account on fastMCP cloud
16. Deploy on FastMCP cloud

- once we deploy our custom remote mcp now we can add in our cluade desktop there is feature in claude desktop (setting->connector-> add custom connector) # Note ; this option only come for pro subscription

## now Expense tracker Remote mcp server

- you have to just make changes in you previous simple remote mcp server and push on githug fastmcp cloud is smart enough to identifiy the changes

## now this custom remote mcp server have some flows and we will try to resolve those so it will become a perfect mcp server

1. first flow -> our whole mcp server run syncronusly (so our tools and resources are syncronus or they are blocking )

- eg let if one user class add expense feature and it basically using database so now other users have to wait untill this call is not completed

- so we can convert this syncronus server to asyncronus (by using async wait feature of python)
- so all tools ,resources and prompt we will make those to async and also make our database async so for that we will use a diff database call aio(asyn input output)-sqlite

* first you have to add aiosqlite -> `uv add aiosqlite` and now just push you asynconised code on github
* now it can handle multiple users concantly

2. second flow -> centeral database like if there are more than one users then all of them are adding there expenses in same database

- solution we should have user id column ; what problem is that we are not putting any authentication layer in bw so how do we know that user is user1 or user2 (confirm)

- so there is also a need of authentication in this whole process (this is advance topic that how to add authentication layer)

## next lec we will see how to build our own mcp client so we will not rely on claude desktop only

- will learn sampling , authentication , lstitation etc
