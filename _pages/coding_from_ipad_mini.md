---
layout: default
title: "Coding from iPad Mini"
permalink: /ipad-mini-coding/
---
# Coding from my iPad Mini

Programming on my iPad Mini? Yes — I actually programmed from my Lay‑Z‑Boy while watching sports. I almost titled this “ChatGPT Codex wrote my code while I slept,” but the truth is I wasn’t sleeping — I could have been. Strap in: agentic software development continues its crazy march.

Earlier I wrote about whipping together a utility to generate `CREATE TABLE` SQL from a CSV or Excel spreadsheet. I had four CSVs I wanted to bring into Snowflake. With Kilo Code (a VS Code plugin) and Grok 4 Code Fast I had a working Python script in likely less time than manually creating the SQL. Now I have a reusable utility.

I then used the OpenAI Codex CLI to mature the code into an open source project architected to support multiple SQL dialects, starting with Snowflake and MySQL.

## Codex Web client on the iPad Mini

The next step was using the Codex Web client at https://www.chatgpt.com/codex. It’s the same coding LLM whether invoked from the CLI, a VS Code plugin, or the web interface. The web experience is particularly interesting because you can connect your GitHub account and it runs code in its own environment.

From my iPad Mini I asked it to add support for PostgreSQL, MySQL, and Databricks. With no further action from me, it:

- designed the solution,
- coded and tested it,
- created a new branch,
- checked it into git,
- created a Pull Request,
- spun up an environment,
- installed the Python libraries,
- ran the code, and
- fixed any of its own errors.

How cool! I reviewed the PR in the web UI just as if a person had contributed to the project. I merged that PR, then requested support for Oracle and SQL Server. It produced another PR, which I also reviewed and merged — all while watching TV and entering requests from my iPad Mini.

## Morning after

This morning I pulled the changed code to my local machine. I had Gemini 2.5 Pro review the two PRs, then I verified the code worked — and it did.

The safety here is the branch/PR workflow: nothing changes your mainline until you review and accept the PR. This project is relatively small, but I look forward to applying this workflow to something far more significant.

Link to the Gemini Code Review:  
https://leebase.github.io/longreply/gemini-codereview-codex/