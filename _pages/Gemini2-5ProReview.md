---
layout: default
title: "AI-Augmented Coding: Faster Snowpark Dev with Gemini 2.5 Pro (Despite Rabbit Holes)"
permalink: /gemini2-5-pro-review/
--- 
The game of leap frog continues with Google releasing in preview their newest model, Gemini 2.5 Pro. I have access to it via the $8/mo T3.chat app (that has access to lots of models).

I have an original model on time that I call The Eternal Now. I posit that time is not a dimension but solely a measure of duration. Gemini 2.5 Pro, by a good margin, was the most intelligent in evaluating and challenging (tearing apart) my model. I’ve been very pleased with the Gemini 2.0 Flash model for performance and price and 1 million token context window. But that model, while quite useful, is not among the top tier in intelligence. I’m optimistic that Google will continue to under price the market with it’s new model - but they haven’t yet released pricing.

Next, I used it to code python in a Snowflake Snowpark Notebook. It was not great. I spent about two hours going down a rabbit hole. Snowflake’s features in this area are fairly new, and different enough from standard python to cause problems. For models to work, they have to have been trained on examples.

> **Pro tip:** Give your AI the relevant documentation when it’s not creating code that works. Whether it’s from the manufacturer or a tutorial article on a site like Medium, sometimes the AI just hasn’t had enough training data on the tech you are using.

Eventually I stopped and started over with Gemini 2.0 Flash. We got to a working code in three prompts. How so with a model that’s clearly less capable? I simply rethought through the process from the lessons learned with the rabbit hole. And then we were able to avoid spinning gears. I also provided a key doc from Snowflake covering the syntax we needed.

I then talked to 2.5 Pro, provided the working code and tried to find out how we had gone down such an unproductive rabbit hole. The AI pegged the problem on misunderstanding the meaning of an error message that sent it down the wrong path. And who among us hasn’t had THAT experience?

I had intended to give up the $8/mo T3.chat but I saw how quick they were to add the new models to the service. They also have the new DeepSeek R3-0324. The open source model that is behind the world shattering DeepSeek R1. I’ll be taking a deeper drive on that later, but the previous model was very strong already.

Why code with an LLM when you can get sent down a rabbit hole? Rabbit holes are nothing new. I’ve gone down them on my own. But after I reset, I had working code in less than 15 minutes. Even taking the whole 2.5 hours into account, I saved time over hand coding.

I had data that I needed pulled out of a very complicated Excel spreadsheet, shaped a bit, cleaned a bit, and loaded into Snowflake. That wasn’t my goal. That was the prerequisite to working towards my goal: setting up a natural language interface to querying the data with Snowflake’s Cortex Analyst feature.

Again, the feature is new so the models aren’t trained on millions of projects. So I’m walking through one of Snowflake’s quick start tutorials. AI is a force multiplier for sure. It’s cooperation between me and the tools. Sure, I can do “only me” - that was 99% of my career. It’s because I am fully acquainted with the “only me” experience that I appreciate the “AI augmented me” increase in productivity. The AI doesn’t “do the work for me”. I still have to think and solve problems. I just get there faster now.
