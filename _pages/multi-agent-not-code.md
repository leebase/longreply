---
layout: default
title: "Multi-AI Agent Coordination for Non-Code Work "
permalink: /multi-agent-no-code/
---
Multi-AI Agent Coordination for Non-Code Work

Today was cool. I got a ton of client work done that’s due Tuesday. This is a side engagement where I’m on retainer for a few hours a month. I use my own equipment, my own workflow, and increasingly, a team of AI tools. A couple years ago I was CTO of a startup for this same guy. He came back to me specifically because he wanted AI acceleration applied to real work.

The interesting part is that today’s work wasn’t coding.

What I started with was a handwritten to-do list of database-related tasks: 
 - get emailed log files into Snowflake
 - build a repeatable Linux-to-Snowflake ingestion process
 - lay down Medallion architecture (Bronze/Silver/Gold)
 -  prepare for Terraform infrastructure-as-code
 -  set up governance and validation workflow
  
And I needed to
 -  make it executive-reportable
 -  prepare for AI-assisted SQL
 -  deliver something the CEO could understand.

None of those tasks say “write a program.” This is architecture, environment setup, governance, documentation, and executive translation. So the real question was whether AI could meaningfully reduce non-code workload. The answer turned out to be a resounding yes!

I fed the to-do list into Codex on macOS with a simple instruction: turn this into a phased execution plan and tell me what you need that you don’t have. It came back with a structured plan and a list of prerequisites: 
- Terraform CLI not installed
- SnowSQL missing
- no key-pair authentication configured
- no OpenSSL private/public key generated
- no production-aligned Linux execution context
-  That kind of gap analysis normally costs mental energy and time. Instead, I had it in minutes.

From there, I had Codex write prompts for Warp.dev, which is my terminal and now effectively an AI-assisted execution surface. Warp can do anything I can do from a shell because it is my shell. Instead of asking AI to “write code,”  I had Codex write the admin scripts to do the parts that needed to be setup in Snowflake before the rest could be code driven from the snowsql command line.

Codex wrote the code, Warp did the sysadmin tasks, and I did the Snowflake admin tasks that couldn't be code driven. 

Code driven? I thought this wasn't a coding task. It wasn't - but code was generated to do tasks I would have had to do by hand. The code isn't the deliverable.

The logs originate on Linux and must be pushed into Snowflake, so success on macOS was never the end goal. I started with the Codex app on the Mac to scaffold and plan, then moved to Linux and used Codex CLI where the ingestion actually needed to run. The immediate need was loading a specific log file. The longer-term need was building a repeatable ingestion process.

In roughly four hours, I delivered more than just “files loaded.” The outcome included 
- a reusable Linux-to-Snowflake ingestion template
- Excel-to-CSV conversion logic
- batch-tracked ingestion with unique IDs
- row-count validation and replay-safe reruns
- a Bronze layer within Medallion architecture
- Terraform scaffolding ready to assume ownership
- operational runbook documentation
- a resume protocol so another engineer—or another AI—could pick up the work without rediscovery.

That’s structural maturity, not a one-off script. It’s more capability than I would normally attempt within a small retainer window—not because I lack the skills, but because the economics wouldn’t justify that depth.

After the technical work came documentation. I asked Codex to translate the work into a maturity progression story. It generated a comprehensive narrative that was technically accurate but too detailed for client consumption. I used ChatGPT to simplify and reframe it in business language. Then I handed that to Claude Cowork to generate a clean executive PowerPoint deck with a coherent story arc, strong visuals, and professional design. Not bullet dumps—an actual board-ready presentation. I could not have produced that quality of deck in that timeframe myself.

The tasks today were planning, installing, configuring, migrating environments, creating governance, documenting, and communicating impact. Program code was generated along the way, but code was not the point. Operational capability was.

The real takeaway is that I’m not just “using AI.” I’m routing workload across specialized agents under cost and token constraints. Codex handled planning and code generation. Warp handled bare-metal execution. Codex CLI handled Linux-side development. ChatGPT handled strategic framing. Claude handled artifact rendering. Each tool did what it’s best at. No single token pool got exhausted. No cooldown window killed momentum.

What would normally be two to three weeks of fragmented consulting effort was compressed into a single focused execution cycle in about four hours. Not because AI replaced me, but because I coordinated it deliberately.

That’s not hype. That’s delivery.
