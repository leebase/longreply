---
layout: default
title: "AI Allows Easy Utility Creation"
permalink: /ai-utility-creation/
---
# AI Allows Easy Utility Creation

I am loving the AI enhanced development life.  We are way past merely going to a chatbot and asking "ChatGPT/Claude to write code for xyz".  That was already quite nice, but now I'm senior solutions architect, project manager and scrum master - and I have a team of AI agents who work for me.  I kid you not, it feels a LOT like this.  Here's an example.

I am working on a database project and need to bring 4 csv's into Snowflake.  The tables don't yet exist, and I want to quickly create the DDL sql to create them based on the csv and it's filed names.  I used Cusror, Kilo Code pluggin and Grok 4 Code Fast as the LLM and had my 4 ddl's created in 20 minutes.  I now have python code that I can run on any new csv's in the future.  

But wait - there's more. I know that such a tool is useful for pretty much anybody.  So I'm using OpenAI Codex to turn it into an OpenSource project that I'll have on github soon.  I had Codex perform a code review and create a ToDo file of improvements including adding the MIT license to the readme.  

I had Codex then create a Sprint plan to have "working code that is testable and provides value but doesn't try to boil the ocean".  I can then have Codex (or any of my agentic coding tools) execute a single sprint.  Then I provide human evaluation and oversight.  Breaking the work into small, reasonable chunks makes this much easier.

Why switch between the AI tools?  No reason other than I'm exploring the tools to decide which I like best.  And that's going to be a moving target as improvement and competition is rapidly developing.  Right now I'm loving Codex/Gpt-5-codex for planning, organization and code review. Kilo Code/Grok-4-code-fast for development.  Grok isn't "as smart" as gpt-5-code-high, but it's really good, really fast, and nicely priced.  Great in the role of iterative development.

I am very comfortable in the solution architect, project lead role as I've been doing so in my career for a couple decades now.  For some, you may want to search for a "how to lead an AI agent team" -- or simply talk to an AI to develop such a document for you.  

Keep in mind that I was able to get working code in about the same amount of time that I would have manually created the 4 ddl's I needed for the current task I was working on.  But now I have a reusable tool for the future as this is a common task.  Maturing the project with follow on ideas and improvements and turning it into open source -- just "doing that nerd thing".  It's fun.

As always, one should keep the "agentic safety" quadrant in mind.  The more mission critical a task is, and the less you know about how to do the task mannually - the more danger there is in using AI.  In this case, the end result are sql scripts that I can read and hand edit to fix if needed.  The downside to an error is quite minimal.  And sql/ddl is my strong suit, even if python coding isn't.  This will be run locally in a cli, not hosted in the cloud with people entering their personal data.  

The AI isn't reducing my software development skills, it's increasing my software development output. Making it viable to create software where normally time constraints would have prevented it.