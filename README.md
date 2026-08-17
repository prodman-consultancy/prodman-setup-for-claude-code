# ProdMan Setup for Claude Code

An executable runbook that turns a fresh Claude Code install into a fully equipped one.

You hand the file to Claude Code and it audits the machine, installs only what is missing, offers a menu
of connections and working methods, writes a global instruction file, and records what it did as
persistent memories. Everything it installs is global, valid in every project and every future session.

[Leia em português](README.pt-BR.md)

## Use it

1. Download [`prodman-setup-for-claude-code.md`](prodman-setup-for-claude-code.md).
2. Open Claude Code, in the VS Code extension or in a terminal.
3. Paste the file into the conversation, or point Claude at its path, and say "run this setup".

Answer the questions and nothing else. The runbook asks for numbers, not sentences. Nothing is installed
without confirmation, and every item can be added later.

It speaks the language you write in. The runbook itself is written in American English, and the
conversation follows you.

## What it does

**Audits before touching anything.** Every component has three possible states, and each gets different
treatment: absent means install, present and current means leave it alone, present and outdated means
propose an upgrade in place. It never installs over a working version and never reinstalls to "fix"
something.

**Installs the base only if it is missing.** Node.js, Git, Python, uv, Docker Desktop, Google Chrome,
VS Code. Always from the vendor's current official source, never a bundled installer.

**Offers 28 optional items in one menu**, in three groups, answered in a single reply:

| Group | What it covers |
|-------|----------------|
| Connections, 1 to 14 | Browser automation, web search, site diagnostics, Google Workspace, Docker, GitHub, Railway, Supabase, Vercel, Cloudflare, Metabase, Sentry, PostHog, Higgsfield |
| Working methods, 15 to 18 | Minimal-code discipline, engineering practice, security by default, Brazilian Portuguese writing |
| Separate tools, 19 to 28 | Voice transcription, metadata cleaning, skill security scanning, notes, presentations, ERP, CRM, support inbox, scraping, workflow automation |

Each item explains, in plain language, what it does for you, what you need to have before it works, and
whether it costs anything. Nothing is installed for a service you do not have an account on.

**Does the hard configuration for you.** Anything that needs an API key or a permission comes with a
direct link to the exact page that creates it. When that is not enough, Claude drives the browser
through the administrative screens itself, and you only sign in. For Google Workspace that is the
default path, because the manual route means creating a cloud project, enabling one API per tool, and
configuring an OAuth client.

**Writes a global instruction file.** At the end it creates or updates `~/.claude/CLAUDE.md`, inside a
marked block, merging rather than overwriting whatever is already there. The rules it writes depend on
what actually got installed, so a rule never points at a tool that is not present.

**Records what it did.** The last step writes persistent memories describing the setup, the connections
and their accounts, the tools and their paths, and anything still pending. Credentials are never written
into a memory, only which service and which account.

## Requirements

Claude Code, signed in, on Windows or macOS. Everything else the runbook installs for you if it is
missing.

## License

[Apache License 2.0](LICENSE).
