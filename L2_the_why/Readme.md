## What are things which mcp really resolve (the why factor) ; and technically try to know why MCP is really needed

## The Arrival of LLMs

- ChatGpt was launched on 30 nav 2022;
- It crossed 1M users in 5 days
- then it crossed 100 million users in 5 days
- then it crossed 100 million users in 2 months
- it was a completly different class of software

## Waves of adoption (we can say there adoption was happened in three stages)

1. Wave 1 - Pure wonder

- where we tried to made it wrong by asking weird questions
  like, Explain Quantum physics from a cat's perspective
  -> what would happen if gravity worked backwards

- Impact -Social media exploded

2. wave 2 - professional adoption

- people started thinking it this as capbility that we can use it in our professional works
- eg; lawyers: "summarize this 50-page contract."
- Developers: "Debug this python function."
- Teachers: "create a lesson plan about photosynthesis."

- here we realeased this chat-bot has serios potential to become our work pattern; if we want we can double our productivity double ;
- Impact - Individual productivity boom

## Wave 3 - the api revolution

- apis were released to use for general public
- and copilot across word, excel, powerpoint, and outlook

impact - ai become more accessible

## now all the software (mostly) were enable to having ai ; and tried that this ai will more accessible

## The problem of fragmentation

- Ai in notion couldn't talk to AI in slack
- Vs code coding assistant knew nothing about discussions in MS teams
- people found themselves living in multiple ai words
- users were juggliing bw multiple ai assistants

## The vision vs the reality

- They wanted one unified ai partner that can understand their work
- A unified tool that can solve any problem related to their work
- Users never wanted 5 different ai tools
- but there was a big problem in building a unified ai agent -> **main problem is problem of context\*** that why it name also has **context** word

## What is context

- context is everything an ai can "see" when it generates a response. or More formally, context refers to the information (conversation history, external docs etc) that the LLM uses to generate a response.

* for eg. while chatting with chatgpt, the past messages forms the context (this is a simple eg of to find context)

* eg. let take a software related eg , we have to add two factor authentication layer so for that so how it is happened let go in detail
* - so a ticked is raced on jira like plateform and that ticket will will be assigned to that software engineer
* - and to develop that feature that engineer has to go github and full latest update from their
* - since we have to implement two factor authentication, need to see the database schema (by using MySql)
* - and since we have to also follow certain security guidliness in order to implement two factor authentication so we will go drive and fetch security document
* - and if we stuck somewhere we use **slag** like site to call help from sinior

* so you can see our contexts are exsisting at different plateforms (this is not like our context is exsisting at one place)

## think if we want to do this software engg. workflow with ai with help of chat-cpt , what would be our approch to do this

- we copy the (the guideness, what ever written on jira) and give to the chat-gpt
- go to githug and take out some security related files from there and give it to chat-gpt
- go the MySQL and fetch the database schema and give to the chat-gpt

* fetch security related documents from the drive and give to the chat-gpt
* and if there is any discusss has been happened on the slag related to this ticket we will copy and give to the chat-gpt

## Now after giving these contexts we will ask to chat-gpt how to implement two factor authentication in such a system

## **Context is scattered** and how much hard to make context

- The copy-Paste Hell => after asking any question you can see, lots of contexts to be created
- Need to paste thousands of line to ask one simple question
- developers have become human apis
- context assembly time > development time
- managing what the ai remembers
- scaling problems

## **real problem is context assembling** in real world senario our context usally exsist cross system exsist

## if we want to from our chat-bot that it can understand each things related to our works and help to solve problems so it is importand that **it is able to see our works** it would be its context part (problem is our works are scattered, and we are manually copy pasting things)

## it will be good if llm can able to call these tools

## The Solution - Function Calling

- OpenAi introduced function calling in mid 2023

* function calling is a way using which LLMs can call ext funcitons
* what it does, it take context and try to **match function description for all the functions** from it's functions list and ask to call one more related function -> tooll calling concept came to the picture

## now peoples start building tools and tool conntectors for google drive, git-hub

- Salesforce integrations for sales teams
- slack bots that could read messages history and channel context
- google drive connectors for document access and collaboration
- database query tools that could analyze company data
- github integrations for code review and pull rquest management
- some internal tools were also created

## Now tools are not scattered, those are connected with chat-gpt through tools and chat-gpt can see our entire works

## after getting this tools solution people was thinking they have resolve the **context assembling problem**

- whether it has capability to fetch different context and main the tools calling history ; but it also some fault

## Problem with the tool

- You we have to provide context and it has scattered about different tools
- and for each tool we have to write a function; how it is problamatic

* Let a company is using three ai related system 1. chat-bot (for Ques-Answering), 2. coding agent (for their employ) , 3, Analytics agent ; and each system as access to different tools like jira, slack, github, mysql, google-dirve ; since these all are being used by different system , so these tools functions would be written a lot number of times if there where lot of ai systems (let N system and M tools , so number of funtions are written -> N\*M) and this is --> **Development Nightmare**

* Why development nightmare:
* - bz each function will have different methods
* - diff data formats and api patterns
* - diff error handling

* so just stablising these function make a huge works for development

## Problem with tools

- Maintenance problem (let we have 3 ai-system and those each are using 10 tools then there would be 30 functions and for each function we have to alway maintain their changes which can be happened for any function)

- security frangmentation (good chance that a hack is performed); each function has their own security guidenes
- cost and time both are wastage (why we built these ai-system so we can make developers works easier now you can see we need more developer to maintain these tools for that system which was build to make developers works easier)

## Note ; code is chagned when we integrate a diff llm to a same tool

# Overview of the problem

- let we have three llms (perplexity, cursor, chat=gpt) and want to integrate a tool (git-hub) with all these llm ; so for that we need three diffent integration code

- **Every Ai tool was building its own way to call every API**.

- it would be great => rather than creating three integration , we build a single integration for connecting git-hub ; which can work for perplexity, curson and chat-gpt

- **Github builds an integration that can be used by any ai tools**

## and to resolve that problem MCP came to picture (Enter MCP) ;

- why it is called modern context protocol bz it is modern way of communication bw client(eg ai-system(eg.chat-gbt) and server(eg. gitHub)) and for that client send protocol to server for getting context from there. or it is modern way to make protocol to get context from the server

- the communication it build bw **client** and **server**; and usually **a client is added with muliple servers**

- now the question is how we can make **client** to **MCP client** and **server(tool)** to **MCP server**

* if we read MCP doc so we can do code for this communication itself which is happened by MCP manually ; but anthropic give us a advantage that they provide **build in SDK**

* so if we want to make our ai-chat-bot to compatible mcp client , we just need to install **MCP client sdk** at our machine ; and lly we want to make our server to mcp complient server , we just need to install **MCP server sdk** library ; this is the beggest diff

## Let go see it at technical level; **MCP vs Function/Tool calling**

- usually what we do usually in tool calling -> make a function where we basically call api ; and some where at that server we had defined that api eg

@app.route("/weather")
def weather_endpoint():
city= request.args.get('city') # query internal weather database
result= loopup_weather(city)
return jsonify(result)

- so this kind of our server
- and for access this feature of getting city weather, we use this api ("/weather") while building weather tool function
  eg

def get_weather_endpoint():
url= f"https://myweatherapi.com/weather?city={city}&key=API_KEY"
response= http_get(url)
data= json_parse(response)
return f"{city}: {data['temp_c']}C, {data['condition']}"

- this is our client side

- this normal tool calling

## now try to see what things are changed in MCP (MCP vs API)

- so mcp also has a server but the things is, here server is built using MCP; internally it is doing same things after calling mcp server api

from mcp.server import Server

server= Server("WeatherServer")

@server.tool("get_weather")
def get_weather(city:str):
data= lookup_weather(city) # e.g.
return {
"temperature":data['temp_c'],
"condition":data['condition']
}

server.start()

- the main difference come from the client side
- at client side we don't have to write much code

- Ai client jsut call tools via MCP
  result= call_tool("get_weather", {"city":"Landon"}); since we have already integrated/configured cliend and server so they are speaking in same language that is MCP so we don't need to write diffent tool code all the things are handled by server

- summary ;
- in before client had some code to call server api and server had some code to run that api logic whant ever it is asked and we need both side codes (client as well as server) so maintain this communication

- but in MCP we only need to write login at server side and just add a config to client side now the communication become standard bw them (basically here we tried to standardise server side so any client communicated directly and this uniform connection way for every client)

## so this was the major differance bw MCP and tool calling (Server does the heavy lifting)

- let we are building gitHub mcv server then github server has responsibility to do all the things eg
- - Authentication with GitHub
- - API rate limiting
- - Data format translation
- - Error handling
- - GitHub-specific business logic

* The client has to just connect to the server so communication become in same language

## Benefits

- we just need to config client side about server, server will handle each and every things ; so we don't need to learn more about diff way to connect ai-system to diffent tools ; now if we have 10 tools and 3 ai-system now we only need to learn 10 server logic which do every things

- so if there are N clients and M servers = M + N integrations ;

- since each things are being happened on server side we **don't maintenance overhead** ;; any changes on server side can't effect client side

- reduced cost and time (bz almost every great server have build there mcp server so client just need to add it's config )

- better security (bz at client side they just has to maintain a config json file(which is much simpler than before what we were doing), and every great security things are written on server side)

## Now we have known how mcp do technically ; also try to understand why it is popular very fast

## MCP Ecosystem

- More ai chatbots supporting MCP -> More valuable for services to build MCP servers
- (and diff chatbot started supporting McP) -> so different servers was forecd to make their server to mcp server

- so More MCP servers available => more valuable for ai tools to support MCP (building a strong circle)

* So more adoption -> More standardization -> more ecosystem value

* Not supporting MCP meant being cut off from this massive rapidly growing ecosystem ( and have to write lot of custom code from both of the side to make sure it is intercting with each of the server)
