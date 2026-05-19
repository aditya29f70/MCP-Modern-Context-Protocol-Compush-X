## Before starting

- 1.  The why;; what exactly it solves

- 2. The What (what is mcp server, what is client, what is host, how they interact)

- 3. The how

- we see a problem statement , so we can analys what is it

- -> Learning in the age of ai;; how to update themself --> use newsletter (of diff companies) -> like **The neuron**

- so sir thourght why we don't start our own newsletter (problem we will have to do more reasearch on a daily basis)

- now he thought to build it by using MCP , mean he want to do it's research by Ai, Creation by Ai, Designing also why ai, or we want to automate it's whole process by ai

- how sir achieve this

- step 1 - the structure (it is imp to having the structure of our newsletter (what kind of content you want to push))

-> in total there are 9 sections (1. introduction section, 2. Big story of the week, 3. 3-5 Quick updates, 4. Top Research Papers, 5. Top Github Repos(big companies publish their repo regular on these topics), 6. 1 Quick Tutorial, 7. Top Ai products, 8. Top X posts, 9. Closing notes (summaries and give a trigger of next post))

- **step2 defining the process, their should be three steps (Research(we will tell at which plateform what you have to read/search --> you will get notes from diff sites) -> Editing -> designing)**

- step3 Deciding the tools (which are used to build it before going to know what were research/editing/designing)

- - The main Ai -> Claude ;; bz mcp comes from anthropic;; and more comfortable (no lagging)

* - The tools -> Github , Web search, Google drive, Arxiv (arkaiv), Gmail, Product hunt.

* so we use MCP to connect Claude to these tools

* think we have main ai (claude) and we have given multiple tools capability and this connection which is happened between claude and these tools is done by MCP

## Step 4 - The research phase (in this phase we will do two imp works)

- 1. try to know on which topics we have to research
- 2. we will conduct a research on that topics

flow would be

- we will give a prompt to claude (detail prompt, there we will give two instructions )
- - claude has to go at my google drive and fetch two files 1. content ideals (where we have already listed topics on which our newletter can be published) and second file is performance data (where we have putted some dummy data which will tell claude about past user response about newletter; eg. date, subject, open rate, click rate, avg read time, top section etc **and we catch these data using mailchimp like tools**) ;; so we try to make our claude to habituate to see first **content ideas** , **performance data** and **Email feedback**(we also have given our email excess; live we have told it to go email and study all the feedback emails ) and decide accordingly what you should add in next newsletter;;;; so **this our part one of the research phase where we are decide that what would be our next topic to research on**

* - and when we get ideal(**insights**) from research phase what would be topics then we want to send this claude to 5 different places to conduct the research

1. we will send to **web search tool** so it will fetch very relavent news about that researched topics
2. **git-hub access** find out trending repo
3. **Product hunt** so it will find trending products in last week
4. **Arxiv** so fetch trending research paper
5. **twitter, x** so fetch conversation for leading personalities

- so from these 5 plateforms we will get 5 markdown files and each will have things related to those researched topics
- now we will take and move to editing phase; this will be conclusion part of our research phase

## Step 5 - Editing phase

- now we have 5 reasearched documents; now using these files we have to design final draft for our newsletter
- so for that we will again give a prompt to our claude and once it done with files reading, it has to go google drive again where we have a sample newsletter prepared

- and also telling to fetch all research documents from my desktop and take ideal from drive about newsletter (how final newletter would look) and on the basis of that format , convert those research document to **final newsletter draft** and after that we will save final draft to the same desktop folder

## step 6 - Designing phase

- same, a prompt will be given to claude ; and a simple things we will write , it has to go to destop and fetch final draft document and in that prompt itself contain some ideal about desinging( ideal about how exactly how newsletter ui looks) and on the basis of that given design draft our final drafted document -> that design would be in **html page format**

- so summary ; we are using claude to make a html page where we give **ideal about designing at prompt itself** and content of that html page will come from final draft (from desktop) ; and at the end we will have html page and we will send back to desktop

* and once we get that html file we just need a **mailchimp** like tool and mail to our contacts

## Now see the paractical how we can do these all works just in three prompts;

- so we need claude desktop (opus model), and you can see what kind of MCP tools we can access ; we hvae three prompts 1. for research phase, 2. editing phase, 3. designing phase ;; try to see how these MCP works

## if we want to add MCP server = we have to go claude -> setting-> developer -> go claude_desktop_config.json(here we have to just add a json file ; for add each of our MCP tool)

- so we just have to add config related code to add a MCP server ; not any function or api call needed

- so we just need that config for any tool then claude desktop easly can handle it ;; and claude can to any kind of work while using it ;; without writting much code

- so this is about adding MCP to claude desktop;; but how exactly it works or what its architecutre we will see further lecs

* and also we will try to build our own MCP server, MCP client ;; and will solve lly like problem while using MCP server
