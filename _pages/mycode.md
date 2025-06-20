---
layout: default
title: "It's My Code, Not Vibe Code"
permalink: /mycode/ 
---
# It’s My Code, Not Vibe Code

I really don’t like **“Vibe Coding”** as applied to any use of AI tools to code.

Vibe Coding is when you “get stuff developed without looking at the code.”  
It’s fine for weekend projects and has its place.  

But **real code delivered to clients isn’t vibe coding** — it’s code I need to understand, verify, vet, and deliver with my and my company’s reputation on the line.  
It’s **my** code — developed much faster and to high quality with the assistance of AI.

---

My current client has no restrictions on using AI, and I’m having a ball with all my tools and the productivity they bring.

One of them is the free **[Roo Code](https://roocode.com)** plugin for VS Code.  
I can give prompts that spur action across an **entire codebase**, not just a single file like with a chatbot.  
It’s **AMAZING**.

---

The difference between vibe coding and what I’m doing is that I’m paying attention — to the code, the documentation, the testing.  
I’m not writing much of it — I’m the **product owner, project manager, and senior developer** all rolled into one.

I *could* be writing the code. But for this project, I’ve gotten great results from the AI (various ones via API).

---

The task was to create an automated **testing framework for Snowflake RBAC** (role-based access controls).  
For each role, I wanted comprehensive **positive and negative** tests:

- ✅ Can you do everything you’re supposed to do?  
- ⛔ Are you blocked from doing what you shouldn’t?

---

The AI and I wrote **two weeks’ worth of code in about a day**.  
Then I tested, walked through, evaluated — **took ownership**.

But I had this *itch* that something wasn’t right. The code worked. It was sophisticated, clever, and comprehensive —  
but it wasn’t actually testing the **right thing**.

What it was doing was verifying that each granted privilege worked.  
For example, if I gave a role read rights, the test verified that it could `SELECT` from a table.  
But that’s just confirming **Snowflake’s built-in behavior** — not whether *my roles* were designed correctly.

---

What I needed was a proper **canon** of what database/schema level roles (`ro`, `rw`, `fa`) **should** be like —  
and then test *my* roles **against the canon**.

That was **my brain**, **my intelligence**, **my ownership** of the task.

---

Now imagine I had hand-coded that blind alley.  
Instead of one day of misguided effort, I’d have burned **two weeks**.

Instead, the AI and I talked it out, wrote a **refactoring prompt**, and within **5 minutes** the **entire app** — including documentation —  
was refactored to compare roles against canon.

---

After debugging the code with the AI, I used the most powerful model I had — **OpenAI’s o3 Pro** —  
to review my requirements file and do a **comprehensive code review**, including the documentation.

It found real issues — and then I brought in a cheaper but powerful model (**Gemini 2.5 Pro**) to implement the suggested improvements.

---

**All in:**  
- 🕒 About **a day and a half of coding**  
- 💻 A solid **two weeks’ worth of results**  
- 📚 **Better documentation** than I usually produce  
- 💸 For about **$8 in API costs**

We coded together. We thought together. We architected together.  
**Faster than me alone, for sure.**

---

Now I’ll present the code to the client, walk them through it, explain every decision, and answer every question.  

Because:

> **It is MY code — not VIBE code.**  
> **Delivered faster, tested deeper, and documented better — with generative AI as my partner, not my replacement.**