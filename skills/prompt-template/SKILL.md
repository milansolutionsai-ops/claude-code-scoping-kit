---
name: prompt-template
description: A canonical prompt anatomy (ROLE / CONTEXT / TASK / CONSTRAINTS / OUTPUT), a tier table for when to use how much structure, and hard rules for prompts inside automation flows (Make.com, n8n, Zapier AI steps) and Claude Code sessions. Use when writing or reviewing a prompt, a system prompt, an AI module inside an automation, or an agent instruction set. In this kit it is also the shape for the scoping and estimating briefs you hand to Claude.
---

# Prompt structure (canonical template)

Every non-trivial prompt (to Claude, in an automation flow, or in an agent system prompt) follows this anatomy:

```
[ROLE]        Who the model is / what persona it's operating as
[CONTEXT]     What situation, project, or data it's working with
[TASK]        What exactly it must produce (verb-first, specific)
[CONSTRAINTS] What it must NOT do (format, scope, tone, data boundaries)
[OUTPUT]      Exact shape of the response (length, format, fields, language)
```

### Full example (AI step in a CRM automation)

```
[ROLE]
You are a booking assistant for a small physiotherapy clinic.

[CONTEXT]
A new lead has just submitted the website contact form. Their data is in
{{contact.first_name}}, {{contact.email}}, {{contact.phone}}. They asked about {{form.service}}.

[TASK]
Write a personalised first-touch SMS. Use the lead's first name and the service they asked
about. The goal is to book a free 15-minute call.

[CONSTRAINTS]
- Do not mention pricing.
- Do not make guarantees about results.
- Max 3 sentences.

[OUTPUT]
Plain text. No labels, no quotes, no markdown. Ready to send as-is.
```

### Example (scoping brief for Claude Code, using this kit)

```
[ROLE]
You are scoping a feature list for a client, using the scoping and time-estimation skills.

[CONTEXT]
The client's request list is in requests.md. The app repo is this folder. Their CRM is
connected through the CRM's MCP server.

[TASK]
Verify every item against the repo and the CRM, classify each by cost profile, estimate
the net-new and partly-built items, and package them into monthly batches of 20 hours.

[CONSTRAINTS]
- No estimate for any item you have not verified. Label unverified items.
- No hours in the client-facing doc.

[OUTPUT]
Two markdown files: scope-internal.md and scope-client.md, following templates.md.
```

---

### Prompt tiers: when to use what

| Tier | Use when | Structure needed |
|------|----------|-----------------|
| **Quick ask** | Single fact, lookup, short task | None, just ask |
| **Task prompt** | Generating content, writing a flow, analysing data | TASK + OUTPUT minimum |
| **Agent system prompt** | Claude Code session, automation AI module, voice agent | Full ROLE + CONTEXT + TASK + CONSTRAINTS + OUTPUT |
| **Multi-step chain** | Sequential agents or modules passing data | Each step gets its own mini-prompt; define the handoff schema (JSON field names) explicitly |

---

### Rules for prompts in automation flows (Make.com / n8n / Zapier)

1. **One task per module.** Never chain "summarise AND translate AND format" in one prompt. Split into sequential AI modules.
2. **Name your output fields explicitly.** Tell the model: "Return JSON with fields: `message`, `language`, `confidence`." Never free text when downstream modules parse the response.
3. **Inject dynamic data via variables, not in the system prompt.** System prompt = stable instructions. User message = dynamic context (contact data, form values, prior step output).
4. **Set temperature explicitly.** Creative copy: 0.7-0.9. Classification or extraction: 0.0-0.2. Structured output: 0.0.
5. **Add a fallback instruction.** "If you cannot determine X, return `null` for that field. Never guess."

---

### Rules for Claude Code / Claude API prompts

1. **Be the spec, not the vibe.** "Build a webhook handler for the CRM that..." beats "Can you help me with a webhook?" Specific = faster = fewer back-and-forths.
2. **State what already exists.** "There's already a `webhookRouter.js` in `/api/routes/`. Add a new endpoint there, don't create a new file."
3. **Scope the blast radius.** "Only touch `ContactService.js` and its test file. Don't refactor anything else."
4. **Specify the output contract.** "Return the finished code only. No explanation. No markdown wrapper."
5. **For multi-file work:** list the files involved upfront so Claude reads them before acting.
