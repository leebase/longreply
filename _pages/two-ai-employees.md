---
layout: default
title: "AI Employees Building Applications Autonomously"
permalink: /two-ai-employees/
---
# AI Employees Building Applications Autonomously

## The Vision

Over the weekend I was working on my autonomous agents. The desire was to move from AI automation -- where I supply all the planning, architecture, etc. and the AI writes and debugs the code -- to something far more ambitious.

While that's amazing as it is, I wanted to provide a mission and have a round-the-clock autonomous AI employee coming up with the ideas on how to achieve the mission and then executing it. It's not that I plan on having zero interactions -- it's that I want the AI employee taking initiative even when I'm not specifically directing it.

## The Architecture

I have 3 projects involved:

### 1. The Orchestrator
You provide all the steps the AI is to take, and it enforces the steps are actually taken. This includes testing and looping back to fix until all tests pass. If you've ever done complex work with AI coding, you know they don't always follow the plan -- and can even lie about having done something they didn't do. My orchestrator takes care of that.

### 2. The Autonomous Agent Platform
Takes a mission, and on a schedule:
- Wakes up and reads the mission
- Ideates on how to accomplish it
- Builds a backlog
- Scores the backlog
- Takes on the step that provides the most value
- Makes a plan and calls the orchestrator
- Considers what was accomplished
- Logs lessons learned for next time -- continuously improving itself

### 3. The Self-Healing Loop
If something broke, determine which layer was the root cause, fix the problem there, and try the mission again. Repeat until the mission works.

## Test Run: Campaign Manager

Yesterday I gave it the mission to elect my friend to parliament in Canada. It's absurd -- but just crazy enough to see how creative the autonomous AI campaign manager could be.

I set up the self-healing loop:

- **2.5 hours**: AI kept working and fixing until the campaign manager AI employee, the orchestrator, and the autonomous platform were all debugged.
- **Overnight (every hour)**: The AI woke up and pursued the goal -- writing questions for me and my friend, plus laying foundations for the campaign.

That was a silly use case whose main value was proving out my stack.

## The Real Missions

Tonight I created 2 new missions:

### Mission 1: Linux Utilities Developer

Every hour my AI Linux Developer will write Linux utilities in C with "all the goodness" I've heard folks here laud about how software should be. Seeded idea: `sysdiff` -- a utility to track your computer's settings to catch any drift.

I will submit the code to your eyes and you can laugh at how badly written it is -- if, in fact, it's going to be bad code. I have specific instructions for excellent code to be written.

### Mission 2: ERD for Snowflake Developer

A basic ERD tool for Snowflake -- something I personally want. I gave it some open source projects to fork and use as the base so I'm not having it reinvent the wheel.

## What's Next?

Should be fun to see what actually happens. I'll be sharing the results -- good, bad, or ugly.