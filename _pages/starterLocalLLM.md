---
layout: default
title: "M4 Mac Mini - Local AI - Findings After A Couple Months"
permalink: /starter-local-llm/
---
# M4 Mac Mini - Local AI - Findings After A Couple Months

I splurged $600 on a base model M4 Mac Mini to have a platform to begin my exploration of using local LLM’s.  Of course a 16gig ram, base model anything - is not a dream machine for local AI.  It’s a step before plunging $10k on a M3 Ultra Mac Studio with 512 gig ram, my current AI dream machine.  So, what have I learned?

## TLDR
I'll start with the conclusion.  Local LLM's are fine for playing with GenAI in an environment you control.  The $600 M4 Mac Mini base is well suited for this task.  The models are shockingly performant and smart -- FOR -- the limited environment.  I find that I would rather continue using "real" ChatGPT, Claude, Grok and Gemini.  They are simply far more intelligent.  However, it's nice to be able to run a local interface like OpenWebUI and call out to those hosted models via API.  If you are trying to get away from big tech, there are excellent hosted open soure models at groq.com and fireworks.ai.  They are nearly on the same tier intelligence wise, perform well, and have a VERY reasonable API cost.  Those hosting platforms don't use your data to train models - they don't any models at all.

## How is the M4 Mac Mini

It's a power house.  Don't let the name fool you, it's only mini in size.  Where Intel and AMD small form factor PC's use "mobile chips" - Apple's mobile chip is the same as the desktop chip.  Best in class desktop chip power running on the electricity of a mobile chip.  This machine is a steal at $600.  Seriously, you can't touch it with anyting at that price from Intel or AMD.

16 gig is fine to learn with and use local llm's.  Apple Silicon chips have unified memory, so you can run at gpu speed instead of CPU speed.  You'd pay a LOT more to get 16gig on a video card.

I was disappointed to find out that none of the models use the Apple Silicon NPU's yet.

I added a 2tb nvme via a Thunderbold 4 enclosure for $150 more.  Runs at the same speed as the internal 250gb drive.  Happy with this cost saving choice.

Do you need an Apple Silicon Mac?  No.  Frankly, none of the local llm's that can run on reasonable systems are smart enough for my uses.  Running OpenWebUI can using cloud hosted models via API can run on any system.  And that's how I ended up after experimenting with the locally running models.

## Are Local Models Good Enough
Yes. And No.  

### No - Not Smart Enough
Let's start with the no part.  For my work, and even for my personal use - I want the smartest models.  These local models are neat for what they are, but they aren't close to ChatGPT, Claude or any other commercial model.  The free tier of Gemini 2.0 Flash and Grok 3 - are light years ahead.  I believe at the free tier, they do train on your prompts.  But the $20/month ChatGPT plan has an opt out of training which I selected.  More on hosted open soure models later.

### Yes - Plenty Smart Enough to Learn On
Learning how to develop your own agents, do RAG training with your own documents, or LORA fine tuning?  Yes, the small local models are more than smart enough, fast enough and completely free and data secure.

## Hosted Open Soure Models - Cheap and Powerful
So many open source models are nearly as good as the best commercial models. Deep Seek R1 shocked the world bringing a ChatGPT quality thinking model for pennies.  However, do not use the China hosted version, it's a security nightmare.  But it's open source, and lots of hosts have spring up.  I have experience with two.  I use them via API's from my locally hosted OpenWebUI.

### Groq.com - Super Fast, Open Source Models
The Groq folks (not to be confused with xAI's Grok) have built a special chip to vastly speed up inference (using the llm model for chats).  To show off their wares, they host several popular models including Lama 3.3, Deep Seek R1 and R3 and Qwen QwQ.  So far I'm doing fine on thier free tier.  Biggest draw back is they don't host the 670B version of Deep Seek, just the 70B.  What they do host, they have by far the highest performance.  And for learning how to train your own agents and such, are a good option as a step up from local llms.  They do not train models at all, let alone on your data.

### Firewworks.ai - Well priced, performant and full sized Open Source Models
Fireworks models also perform well.  There are many models to choose from and they respond quickly when a new OpenSource model hits the scene.  I put $10 in my account a month ago and still have $6 left.  It's not free, but it's close to it.  I like having the full DeepSeek R1 and V3 to use.

## What local software for LLM's

1. **Local Chat (Simple)** For just running a local chat, **LM Studio** is best on a Mac as it has MLX models which are optized for running on Apple Silicon.
2. **Local Chat (Advanced)** For more of a ChatGPT complete experience there is **OpenWebUI**  You can chat with local llm's, hosted llm's via api's, RAG document stores, and more.
3. **Run LLMs** Ollama appears to be the consensus winner in this area.  A huge library of local llms.  Can run on CPU or GPU.
4. I can't at this point recommend software for agents, training, RAG etc.  I'm just beginning my exploration.

## Conclusion
I'm glad I bought the $600 M4 Mac Mini instead of the M4 Pro.  Frankly, I didn't need a new machine at all.  For me, I'd rather have the performance and extra smarts of the cloud hosted LLMs at this point in time.  Even developing local agents and such - the cost of the Fireworks.ai hosted API's are so inexpensive, I can't see how I'd use them so much to make running local a need.