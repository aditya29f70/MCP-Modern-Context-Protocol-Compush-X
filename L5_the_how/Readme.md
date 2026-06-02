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

## import: when ever we add any mcp server we have to re-start the claude-desktop

- for local server connector we can use it very nicely for eg we can give access to our download folder and tell it to organism it in better way

* or you have a project folder you can directly tell it to summaries; it will summaries directly

* now the next server which we are gonna integrate is manim (remote server); we know "3blue1brown" youtube chennai and how great it try to visiualize things; for such visualization they use a python library is called manim; and using manim he code and make such great visualization by himself and coding for such visualization is very hard but after llm came in picture (since llm good enough to code) ;; and more interesting thing is that we have manim-mcp-server and we can connect it with claude desktop ; and after that we just have to text what kind of visualization we want and it will creat code for that in manim directly and we will have direct a visualization video ;; so (input is text) --> output(video)

* so for that we can ask first for the great prompt them give to that mcv-server
* need to go manim-mcp-server github (this serve is build by some offical person) ; and there is readme file where they have menstioned how to connect it to the claude desktop

* and it is a local server ; so first we have to clone that github repo in our computer ; then connect with claude desktop;
* so this is

* since for this we don't have any connector so after cloning we have to add it by **json-file** ;; so copy the manim json things and paste to the claude desktop json-file(go to claude-> setting-> devloper-> edit config-> claude_desktop_config.json)

* it is not done, we have to make some changes with config, eg have to give python absolute path -> for that we use terminal again `which python` or window -> `where python` and for manim `where manim` and for finding absolute path of cloned manim_server.py -> go to that manim_server in the clone one and give command `pwd`

* and restart claude desktop(use task-manager);; and once we done with connection we will give that prompt (for build a animation video for linear transformation); and you will see that just from you prompt claude will analys it and try to do code for manim-server (which is hard things)
* it will show that it need to have **LaTex** install so it can make mathematical symbols but not needed (bz it is around 10-15gb) so it will automatically go with any other ways

## Now we have seen how to connect with local servers by connector as well as through json-file

## Now try to learn how to connect two remote servers

1. Google drive ; good thing is that we already has its connector in claude desktop (note;; this is read-only mcp server)

2. twitter mcp server ; so we don't have its connector so we will try to connect it by json-file;

- first search twitter mcp server on google ; and we will have a git-repo; there is simple instructions how to connect this mcp server to claude desktop

* and you will this is not a remote mcp-server when you are gonna add json-config file to claude desktop there is options `command` and `args` ; behind the seen this json-config install npm for twitter

* so we will see a remote mcp server by connecting json-file (where it file present on any where remotely) ;; if it is not work then try to install **npm**

## now try to see how to connect weather mcp server (remote mcp server, there files are relocated somewhere in the internet)

1. first search **weather mcp server** on google -> github-repo ; and install uv (`pip intall uv`); go AccuWeather and take api key

- why this is remote-mcp server bz in args we gave github url, we didn't give our local file path or something

## How to be know is there any mcp server for any tool or not

- for that we have to search -> **awesome mcp servers** -> we will get a github repo of updated with all kinds of mcp server which are currently present on internet
