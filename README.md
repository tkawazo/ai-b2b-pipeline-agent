# AI agent for a B2B enterprise sales pipeline

A case study of how I automated the weekly pipeline review for a payments consultancy that sells to banks, acquirers and fintechs across Latin America. It runs on Claude Code as a scheduled cloud routine connected to email and file storage.

> Client names, contacts and results are confidential. The data in `sample_pipeline.json` is **fictional** and only shows the schema.

## The problem

Enterprise B2B sales to financial institutions means long cycles, many stakeholders per account, and a lot of outreach that bounces or goes unanswered. Every Monday I spent **about 4 hours** reconciling the inbox against a pipeline spreadsheet: who answered, which emails bounced, who needs a follow-up, and which accounts have no contact at all.

## What the agent does every Monday

```
Inbox (last 7 days) ──┐
Sales Navigator CSV ──┼──> Claude Code routine ──> pipeline JSON (versioned)
Pipeline JSON (v_n) ──┘                        ──> weekly briefing (HTML)
                                               ──> draft follow-ups (for my approval)
```

1. **Audits the inbox:** matches sent emails, replies and bounces against every account in the pipeline.
2. **Diagnoses bounces:** groups them by domain and security filter, and recommends whether to retry, find a new address or switch to LinkedIn.
3. **Prioritizes:** tier of the account (T1–T4) × momentum (hot / live / sent / pending); accounts with no contact get top urgency.
4. **Writes the briefing:** a 5-bullet executive summary plus an action queue for the week.
5. **Drafts follow-ups** from templates. **It never sends anything:** I review and send every message myself.

## Results

| | Before | After |
|---|---|---|
| Weekly pipeline review | ~4 h | ~30 min |
| Inbox audit | 2–3 h manual | ~10 min, automatic |
| Pipeline updates | Manual spreadsheet | Versioned JSON, updated every week |

## Design decisions

- **Human in the loop for anything outbound.** The agent prepares; I approve and send.
- **JSON instead of a spreadsheet** so every weekly version can be diffed and queried.
- **One source of truth per entity**, with an explicit `next_action` and `urgency`, so the briefing is an action list rather than a report.

## Files

- `sample_pipeline.json`: schema with fictional entities.
