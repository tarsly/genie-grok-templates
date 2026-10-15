# Genie Grok Bot Templates

Grok Bot templates for the Genie plugin set: a getting-started onboarding bot, the **Library** (one bot per toolkit, with every workflow included), and **Agents** (scheduled, proactive versions of the toolkits).

## What's inside
- `00-getting-started/`: onboarding bot (clarify goals, map your business, design your AI team). Start here.
- `library/<toolkit>/`: one bot template per toolkit. Contains `system-prompt.md`, `README.md`, and `workflows/<workflow>/SKILL.md`, including a `getting-started` workflow.
- `agents/<toolkit>-agent/`: agent templates. Contains `system-prompt.md`, `capabilities.md` (skills, preferences, suggested schedules), and a `getting-started` workflow. They reuse the matching `library/<toolkit>/workflows/`.

## Set your bot name
Every template uses the placeholder `{{BOT_NAME}}`. Before you import, find and replace `{{BOT_NAME}}` with the name you want (for example `Ava`). Bots are described by role (for example "Sales Prospecting Assistant"), not by persona names.
Quick replace inside a template folder: `grep -rl '{{BOT_NAME}}' . | xargs sed -i 's/{{BOT_NAME}}/Ava/g'` (on macOS use `sed -i ''`).

## Import into Grok Bot
1. Create a new bot (or template) in Grok Bot.
2. Paste the contents of `system-prompt.md` into the bot's instructions.
3. Add each folder under `workflows/` as a workflow. Each one has a `SKILL.md` with `name` and `description` frontmatter. For an agent template, also add the workflows from the matching `library/` folder.
4. Connect the tools the workflows mention (notes or knowledge base, email, calendar, CRM, social, and so on). Connect only what you need.
5. Start a chat and say "get started". The bot runs `getting-started`, saves your preferences, and sets up any schedules you ask for.

## Notes
- The workflows were first written for a plugin runtime. A tool name like `mcp__cloud-brain__write_note` means "your notes connector". Grok Bot uses whatever connector you've linked. Helper scripts from the original plugins are not included.
- Placeholders like `<your-provider-settings-url>` mark spots where you fill in your own values. Example names, companies, and emails (Jane Doe, Sam Lee, Acme, john@taxpro.com, example.com) are fictional.
- Bots never send, post, or purchase anything without your approval.
