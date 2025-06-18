---
layout: default
title: "LLM API Pricing from OpenRouter.ai"
permalink: /llmpricing/
--- 
# OpenRouter LLM Model Cost Comparison (Simplified & Categorized)

This document provides a highly streamlined overview of Large Language Models (LLMs) available via OpenRouter, focusing on comparative developer costs within functional tiers.

## Data Schema Reference:

| Column Name                           | Description                                                               | Unit/Format      |
| :------------------------------------ | :------------------------------------------------------------------------ | :--------------- |
| `Model Name`                          | Specific identifier for the LLM.                                          | String           |
| `Developer`                           | The originating company/developer of the LLM.                             | String           |
| `Estimated Cost per 1M Tokens (80% In / 20% Out)` | Calculated as (Min Input Cost * 0.8) + (Min Output Cost * 0.2).   | Decimal ($)      |
| `Price Leader`                        | Indicates if this model is the lowest estimated cost in its category.     | Boolean (✓)      |
| `% More (vs. Leader)`                 | Percentage more expensive than the estimated cost leader in its category. | Percentage (%)   |

---

## 1. Ultra-Cost-Optimized / Lightweights

These models prioritize extreme cost-efficiency for rapid, high-volume processing where cost per token is the paramount concern.

**(Price Leader: Gemini 2.5 Flash Lite Preview 06-17 & GPT-4.1 Nano @ ~$0.16)**

| Model Name                      | Developer | Estimated Cost per 1M Tokens (80% In / 20% Out) | Price Leader | % More (vs. Leader) |
| :------------------------------ | :-------- | :---------------------------------------------- | :----------- | :------------------ |
| Gemini 2.5 Flash Lite Preview 06-17 | Google    | $0.16                                           | ✓            | 0%                  |
| GPT-4.1 Nano                    | OpenAI    | $0.16                                           | ✓            | 0%                  |

---

## 2. Balanced / Workhorse Models

These models offer a robust blend of capabilities and cost, representing a solid default choice for many common LLM workloads.

**(Price Leader: Gemini 2.5 Flash & Grok 3 Mini Beta @ ~$0.34)**

| Model Name        | Developer | Estimated Cost per 1M Tokens (80% In / 20% Out) | Price Leader | % More (vs. Leader) |
| :---------------- | :-------- | :---------------------------------------------- | :----------- | :------------------ |
| Gemini 2.5 Flash  | Google    | $0.34                                           | ✓            | 0%                  |
| GPT-4.1 Mini      | OpenAI    | $0.64                                           |              | 88.24%              |
| Grok 3 Mini Beta  | xAI       | $0.34                                           | ✓            | 0%                  |
| o3 Mini           | OpenAI    | $1.76                                           |              | 417.65%             |

---

## 3. High-Capability / Advanced Reasoning Models

These models excel in complex tasks, advanced reasoning, coding, and situations requiring deeper understanding or more precise instruction following. They come with a moderate increase in cost compared to workhorse models.

**(Price Leader: Gemini 2.5 Pro & GPT-4.1 & o3 @ ~$3.00)**

| Model Name        | Developer | Estimated Cost per 1M Tokens (80% In / 20% Out) | Price Leader | % More (vs. Leader) |
| :---------------- | :-------- | :---------------------------------------------- | :----------- | :------------------ |
| Gemini 2.5 Pro    | Google    | $3.00                                           | ✓            | 0%                  |
| GPT-4.1           | OpenAI    | $3.20                                           |              | 6.67%               |
| o3                | OpenAI    | $3.20                                           |              | 6.67%               |
| Grok 3 Beta       | xAI       | $5.40                                           |              | 80.00%              |
| Claude Sonnet 4   | Anthropic | $5.40                                           |              | 80.00%              |

---

## 4. Top-Tier / "Pro" Models

These are the flagship models, representing the highest level of intelligence and specialized capabilities, tailored for the most critical and complex tasks, often at a premium.

**(Price Leader: Claude Opus 4 @ ~$27.00)**

| Model Name      | Developer | Estimated Cost per 1M Tokens (80% In / 20% Out) | Price Leader | % More (vs. Leader) |
| :-------------- | :-------- | :---------------------------------------------- | :----------- | :------------------ |
| o3 Pro          | OpenAI    | $32.00                                          |              | 18.52%              |
| Claude Opus 4   | Anthropic | $27.00                                          | ✓            | 0%                  |