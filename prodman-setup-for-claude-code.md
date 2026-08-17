# ProdMan Setup for Claude Code

Source language: American English. Runtime language: the user's language (see Section 1).

This file is an executable runbook. The agent reading it audits the machine, installs only what is
missing, offers a menu of optional tooling, writes a global `CLAUDE.md`, and records what it did as
memories. Everything it installs is installed globally, for every project and every future session.

---

## Section 0. How to use this file

**For the person holding this file:** open Claude Code (VS Code extension or terminal), paste this
file into the conversation or point the agent at its path, and say "run this setup." Then answer the
questions. Nothing is installed without your confirmation. At one or two points you will be asked to
close and reopen the program, which is how new tools become active, and the setup picks up exactly where
it stopped when you come back.

**For the agent:** read Section 1 in full before acting, because it governs everything else. Then read
each phase's section when you reach that phase, not before, and consult the Appendix only when
something fails. Execute the phases in order. Do not skip Phase 1. Do not install anything the machine
already has. Do not install anything the user did not select.

Execution order, fixed:

| Phase | What happens | Skippable |
|-------|--------------|-----------|
| 1 | Audit the machine and report findings | No |
| 2 | Install the missing required base | No |
| 3, 4, 5 | One menu of 28 items, one answer, then install what was picked, in that order. May include one restart in the middle when browser assistance is needed | Yes, the user may pick none |
| 6 | Restart, finish authorizations, verify, report | Only when nothing was installed |
| 7 | Write the global `CLAUDE.md` | No |
| 8 | Write memories, last of all | No |

The order is strict, and Phase 8 is the last of the last. Memories are written only after every
install is done, every verification has run, and the `CLAUDE.md` is on disk. Never write a memory
early, never write one "as you go", and never write one about work that is still in progress. A
memory written before the work finishes records something that is not true yet.

---

## Section 1. Operating rules for the agent

Read this section first. It governs every phase.

### 1.1 Interaction style and pace

This is the highest priority rule in the file. It applies to every phase, every message, and every
question, and it overrides any instinct to be thorough at the user's expense.

**Be brief.** The user is here to finish a setup, not to read. Say what phase you are in, say what you
are about to do, do it, say what happened. No preamble, no restating what the user just said, no
summary of what you are about to explain. If a sentence can be cut without losing information, cut it.
Never write a paragraph where a line works, and never write a line where a number works.

**Keep the user informed of position.** This is a long run with many steps, so every message starts by
placing the user in it, in one short line: which phase, out of how many, and what is happening now.
Something like "Phase 3 of 8, MCP menu" is enough. Never leave the user wondering whether the process
is still moving.

**Ask for the least possible input, in the easiest possible form.** This is not optional politeness, it
is the design of the whole interaction:

| Instead of asking for | Ask for |
|-----------------------|---------|
| The name of a tool | Its number from the list |
| A sentence of confirmation | `Y` or `N`, and accept `S` or `N` too when the user is speaking Portuguese, plus `y`, `s`, `sim`, `yes`, `1` |
| Several separate yes or no questions | One list of numbers covering all of them |
| A choice restated in prose | A number, always |

Accept whatever form the answer arrives in. If the user types `1,3,7` or `1 3 7` or `1, 3 e 7`, that is
the same answer. Never make them retype something because the format was not exact.

**Only expand when asked.** Each item gets its short description and nothing more. If the user wants
more context on something, they will ask, and then explain properly. Do not preemptively teach.

**Move fast, and use the time the machine is busy.** Long installs are the main reason a setup like
this becomes unpleasant. Compress wherever the pipeline allows:

- Run the whole audit battery in one pass, not one command at a time.
- Group independent installs into one call instead of one call per package.
- Present all three menus, items 1 through 28, in a single pass and take one answer for all of them.
  This is the preferred path, not just an accepted one.
- Ask for every credential the selection requires in one message, not one at a time.
- When a long install is running, keep talking to the user about the next decision instead of waiting
  in silence.

**Respect the pipeline, though.** Speed never justifies breaking the order, because later phases depend
on the result of earlier ones:

| Cannot be moved earlier | Because |
|-------------------------|---------|
| Phase 1, the audit | Everything else decides what to do based on it |
| Phase 2, the base | `npx`, `git`, and `uv` have to exist before anything uses them |
| A reload, before using any MCP just installed | An MCP server is inactive until Claude Code restarts, which is why Phase 3 has one intermediate restart when browser assistance is needed, and Phase 6 has the final one |
| Browser authorizations | They only work after the reload |
| Phase 7, the `CLAUDE.md` | Its rules depend on what actually got installed |
| Phase 8, the memories | They record the finished state of everything above |

So the optional phases genuinely come before the mandatory closing ones. Do not try to write the
`CLAUDE.md` or the memories early to save time. Compress inside a phase, never across the order.

**Spend as few of the user's tokens as possible.** The run costs the user real money, and this file is
long. Completeness and rigor are not negotiable, but they are the floor to hit, not a budget to spend:
do the whole job, correctly, with the least consumption that achieves it.

- Read a section of this file when you reach the phase that uses it. Do not re-read what you already
  applied, and do not quote this file back to the user.
- Never restate an item's description twice. It appears once, in the menu.
- Run one command that answers several questions instead of several that each answer one. The audit
  battery is written that way on purpose.
- Check a result once. Do not verify the same thing at the end of a phase and again in the next one.
- Do not paste command output back to the user. Read it, act on it, and report the conclusion in a
  line. Raw output goes to the user only when they ask, or when a failure needs the exact message.
- No progress narration between steps of the same command, no "let me now", no recap of what was just
  done and is visible above.
- Skip whole sections that do not apply. If the machine has no Docker item selected, the Docker
  paragraphs are not read.

**The goal is a user who is done and using the tool.** Finishing fast, correctly, and cheaply is the
measure, not how much was explained along the way.

### 1.2 Language protocol

This file is written in American English. American English is the project's source language and the
language of every identifier, command, file marker, and comment the agent writes into
configuration.

The **conversation** is different. Detect the language of the user's first message and speak that
language for the entire run. Do not ask the user which language they prefer, and do not switch
mid-run.

When the user writes **Brazilian Portuguese**, everything the agent says on screen is Brazilian
Portuguese, with complete and correct diacritics: `á ã â é ê í ó ô õ ú ç`. Never write "voce", "nao",
"esta", or "atencao". Correct verb agreement, correct use of "crase", correct punctuation.

In every language, including English: **never use an em dash (`—`) or an en dash (`–`), and never use
a double hyphen (`--`) as a stand in for one.** Use a comma, a period, a colon, parentheses, or
rewrite the sentence. A single hyphen is fine inside a real compound word and inside CLI flags.

Do not use emoji unless the user uses emoji first.

Product names stay in their original spelling in every language. "Playwright" is "Playwright" in
Portuguese.

### 1.3 Everything is global

Every installation in this runbook is global, meaning available in every project and every future
session:

| Kind | How to install globally | Where it lands |
|------|-------------------------|----------------|
| MCP server | `claude mcp add --scope user ...` | `~/.claude.json`, user scope |
| Plugin or plugin skill | `claude plugin install <plugin>@<marketplace>` | `~/.claude/plugins/` |
| Standalone skill | clone into `$HOME/.claude/skills/<name>/` | `~/.claude/skills/` |
| Python CLI tool | `uv tool install ...` | `~/.local/bin/` |
| Node CLI tool | `npm install -g ...` | global npm prefix |
| Instruction rules | `$HOME/.claude/CLAUDE.md` | user scope, all projects |

Never use `--scope local` or `--scope project`. If a command fails because of the scope flag, fix the
scope, do not fall back to a narrower one.

### 1.4 Always install the current official version

Install from the vendor's official current source. Do not bundle, cache, or pin an installer version.
Every item in this runbook carries an **Official source** URL. If a command in this file fails
because a package moved, was renamed, or changed its flags, open that URL, read the current
instructions, and follow those instead of guessing. Report the discrepancy to the user in one line.

Do not write a version number into any configuration file unless the vendor requires it. Prefer
`@latest` for `npx` packages and the vendor's own installer for applications.

### 1.5 Three states, never a reinstall

Every candidate has three possible states, not two. Phase 1 produces the inventory. Determine the
state before doing anything, and treat each state differently:

| State | What to do |
|-------|------------|
| **Absent** | Install it, current official version. |
| **Present and current** | Nothing at all. Say "already installed, version X, current." Move on. |
| **Present and outdated** | Propose an **upgrade in place**. Never install over it, never reinstall from scratch. |

This applies to everything, without exception: base components, `npm` global packages, Python tools,
MCP servers, plugins, skills, and applications.

**An upgrade is not a reinstall.** Installing over an existing version, or removing and installing
again, is how a working setup gets destroyed: it can discard credentials the user already authorized,
reset a configuration they tuned, and leave two parallel copies on the PATH fighting each other. Every
package manager has a real upgrade path. Use it. The commands are in Section 3.4.

When something is present but broken, say so, show the evidence, and ask before touching it. Do not
"fix" it by reinstalling.

### 1.6 Ask before acting, then keep going

Confirm before installing anything. Confirm again, separately, before anything that creates a paid
resource, touches a credential, or writes outside `~/.claude/` and the user's chosen install
locations.

When one item fails, do not abort the run. Record the failure, tell the user in one line, move to the
next item, and list every failure in the Phase 6 report.

Never ask the user to paste a password. Never echo a full credential back to the screen. When
confirming, show at most the first six characters. Section 1.8 covers how to get a credential.

### 1.7 Explain in plain language

The audience is business people, not engineers. Every menu item gets: what it does for you in one or
two sentences, what you need to have before it works, and whether it costs anything. No jargon
without an immediate plain explanation. Never explain an item using the words in its own name.

### 1.8 Assisted configuration through the browser

Several items need something only a logged in human can get: an API key, a token, a permission, an
account choice. Two paths exist for every one of them, and the offer is always the same.

**Path one, the default: hand the user a link that lands them exactly where they need to be.** Not the
service's home page, not "go to settings and look for tokens". The deep link to the page that creates the
thing, plus what to click when they get there, in one line. Then take the value in a single message.
Most people prefer doing this themselves, and it is faster.

**Path two, always offered, never forced: the agent does it through the browser.** If the user does not
want to, does not know how, or gets lost, drive the browser with the Playwright MCP: navigate to the
right screen, let the user authenticate, and continue from there through whatever administrative steps
are needed, reading the value off the page at the end.

Offer path two in one short line whenever path one stalls, and whenever a task involves more than about
three clicks of administrative setup. Do not make the user ask for help twice.

**Rules for path two, all of them mandatory:**

- **Say what is about to happen before it happens.** A browser window opening and moving on its own
  looks alarming to somebody who has never seen it. One line is enough: "I will open the browser and do
  this setup, you only sign in when the login screen appears."
- **The user authenticates, never the agent.** Never ask for a password, never type one, never accept one
  pasted into the chat. Stop at the login screen, hand control over, and wait to be told it is done.
- **Ask which account before starting**, not after. People have a personal account and a work account,
  and the wrong one produces a working setup pointed at the wrong place.
- **It requires the Playwright MCP**, item 1, installed and active. See the ordering rule in Section 4.4.
- **Narrate nothing while it runs.** One line before, one line after. Not a play by play of each click.
- **Never write a credential to the screen or into a memory.** Put it straight into the configuration.
  When confirming, show at most the first six characters.

This assistance is available at every step of this runbook, not only in the MCP menu. Item 4 is the one
place where it is not optional, because the setup there is genuinely too technical to hand to the user.

### 1.9 Restarts, and resuming exactly where you stopped

Some things only take effect after Claude Code restarts. When that blocks the next step, ask the user to
restart. As many times as genuinely necessary, and no more.

**Minimize them by grouping.** Before asking for a restart, finish everything that does not need one.
Two restarts because you asked too early is a failure of planning, not a property of the tool. A normal
run has exactly one, at Phase 6. A run that needs browser assistance has two, the extra one inside
Phase 3. A run where the user installed nothing has none at all. Anything beyond that needs a reason you
can state in one line.

**A restart is justified only for:** an MCP server you are about to use in this same run, a plugin or
skill you are about to use in this same run, and a Windows PATH that survived the refresh in Section 3.1
without picking up a new command. Docker asking for a machine reboot is a different case: continue with
everything that does not need Docker and leave those items for afterward.

**Never restart for something that is only needed after the run is over.** Everything installed in
Phases 3 through 5 gets picked up by the final restart in Phase 6, so there is no reason to restart as
each item finishes.

**Write the checkpoint before asking.** The restart ends the session, and the conversation may not come
back with it. Without a checkpoint on disk the run restarts from zero and the user is asked for their
selection a second time, which is the worst possible outcome. So, immediately before asking:

Write `$HOME/.prodman-setup/state.md` containing:

- which phase the run is in, and which step inside it
- the user's full selection, by number, exactly as they gave it
- which items are done, which failed and why, which are still pending
- what the restart is for, and the very next action after it
- anything already gathered that is not a secret, for example which Google account was chosen

Never put a credential, token, key, or password in that file. Those go straight into the configuration.

This file is the one exception to keeping temporary work in the session scratchpad, and the reason is
mechanical: a scratchpad belongs to one session, and the whole point here is surviving into the next one.

**It goes in `$HOME/.prodman-setup/`, not inside `$HOME/.claude/`.** Claude Code treats everything under
its own configuration directory as sensitive, so writing there is refused outright or held for approval.
A checkpoint that cannot be written is worse than none, because you only find out at the moment you
needed it. Create the directory if it does not exist.

**How to ask.** One short block, the exact steps for their setup, and how to come back:

| Where they run Claude Code | What to tell them |
|----------------------------|-------------------|
| VS Code extension | Run "Developer: Reload Window" from the command palette, or close VS Code and open it again |
| Terminal | Exit `claude`, then run `claude` again |

Then: "when you are back, send me any message and I continue from here." Say what the restart is for in
one line, and say what comes next, so the pause does not feel like the process broke.

**On the way back.** Read the checkpoint, state in one line where the run is and what is next, and
continue. Do not re run what is already done. Do not ask for the selection again. Do not summarize the
whole run. Do not re audit the machine.

**At the end of the run**, in Phase 8, delete the checkpoint file. Its content that still matters is
already in the memories by then. Leaving it behind would make a future session think a setup is half
finished.

---

## Section 2. Phase 1, audit the machine

Detect the operating system first, then run the whole audit battery in one pass. Do not install
anything in this phase.

### 2.1 Windows audit

Run in PowerShell:

```powershell
$r = [ordered]@{}
$r['os']        = (Get-CimInstance Win32_OperatingSystem).Caption
$r['winget']    = (winget --version)              2>$null
$r['node']      = (node --version)                2>$null
$r['npm']       = (npm --version)                 2>$null
$r['git']       = (git --version)                 2>$null
$r['python']    = (py --version)                  2>$null
$r['uv']        = (uv --version)                  2>$null
$r['docker']    = (docker --version)              2>$null
$r['vscode']    = (code --version | Select-Object -First 1) 2>$null
$r['claude']    = (claude --version)              2>$null
$r | Format-Table -AutoSize
```

Then these, which need their own handling:

```powershell
# Docker daemon actually running, not just installed
docker info --format '{{.ServerVersion}}'

# Google Chrome, three possible locations
@("$env:ProgramFiles\Google\Chrome\Application\chrome.exe",
  "${env:ProgramFiles(x86)}\Google\Chrome\Application\chrome.exe",
  "$env:LOCALAPPDATA\Google\Chrome\Application\chrome.exe") |
  Where-Object { Test-Path $_ }

# Claude Code already authenticated, and what is already configured
claude mcp list
claude plugin list
Get-ChildItem "$HOME\.claude\skills" -Directory -ErrorAction SilentlyContinue | Select-Object Name
Test-Path "$HOME\.claude\CLAUDE.md"
```

### 2.2 macOS audit

Run in Terminal:

```bash
sw_vers -productVersion
brew --version   2>/dev/null || echo "brew: missing"
node --version   2>/dev/null || echo "node: missing"
npm --version    2>/dev/null || echo "npm: missing"
git --version    2>/dev/null || echo "git: missing"
python3 --version 2>/dev/null || echo "python3: missing"
uv --version     2>/dev/null || echo "uv: missing"
docker --version 2>/dev/null || echo "docker: missing"
docker info --format '{{.ServerVersion}}' 2>/dev/null || echo "docker daemon: not running"
code --version   2>/dev/null | head -1 || echo "vscode cli: missing"
claude --version 2>/dev/null || echo "claude: missing"
test -d "/Applications/Google Chrome.app" && echo "chrome: ok" || echo "chrome: missing"
claude mcp list
claude plugin list
ls -1 "$HOME/.claude/skills" 2>/dev/null
test -f "$HOME/.claude/CLAUDE.md" && echo "CLAUDE.md: exists" || echo "CLAUDE.md: absent"
```

### 2.3 Minimum versions

| Component | Minimum | Why this floor |
|-----------|---------|----------------|
| Node.js | 20 | Below 20, several MCP packages fail to start |
| npm | ships with Node | Used by `npx`, which most MCP servers run through |
| Git | 2.30 | Used to install standalone skills and plugin marketplaces |
| Python | 3.11 | Floor for `uv` managed tools |
| uv | current | Runs the Docker MCP and installs SkillSpector |
| Docker Desktop | current | Must be installed **and** the daemon must answer |
| Google Chrome | current | Required by the Chrome DevTools MCP |
| VS Code | current | Where the user works |
| Claude Code | current | Must be installed and signed in |

The floor and the current release are two different questions, and the audit answers both:

| Situation | Classification | Action |
|-----------|----------------|--------|
| Absent | missing | install, Phase 2 |
| Present, below the floor | too old, blocking | upgrade, Phase 2, required |
| Present, above the floor but behind the current release | outdated, not blocking | propose an upgrade, Phase 2, user decides |
| Present, current release | done | nothing |

A component below the floor is never treated as missing. The fix is an upgrade in place, not a second
parallel install. Say which case each row is.

To find out whether a component is behind, ask the package manager rather than guessing:

```powershell
winget upgrade
```

```bash
brew outdated
npm outdated -g --depth=0
uv tool list --outdated
```

For anything not covered by a package manager, compare against the vendor's official page listed in
Section 3.3. Do not assume a version is current because it is recent.

An outdated but working component is a proposal, not a blocker. Show the user the current version and
the available one, say in one line what the upgrade buys, and let them decide. If they decline, note
it and continue. Never upgrade something above the floor without asking.

### 2.4 Report the audit

Show one table before doing anything else. Use the user's language for the labels and keep the
component names as they are:

```
Component        Status              Action
Node.js          v22.14.0            nothing to do
npm              10.9.2              nothing to do
Git              missing             install in Phase 2
Python           3.9.6, too old      upgrade in Phase 2
uv               missing             install in Phase 2
Docker Desktop   installed, stopped  start it in Phase 2
Google Chrome    missing             install in Phase 2
VS Code          1.99.0              nothing to do
Claude Code      2.1.4, signed in    nothing to do
MCP servers      none configured     Phase 3
Plugins          none installed      Phase 4
Skills           none installed      Phase 4
CLAUDE.md        absent              Phase 7 will create it
```

Then say, in one sentence, how many items Phase 2 will install, and ask for confirmation to proceed.

---

## Section 3. Phase 2, install the missing required base

Install only the rows the audit marked as missing or below the floor, plus any merely outdated row the
user approved upgrading. Announce each one before running it.

Docker Desktop and Claude Code are usually already present, because the person installed them before
the session. Verify anyway. "Installed" is not the same as "running" for Docker.

### 3.1 Windows

Use `winget`. If `winget` itself is missing, send the user to the Microsoft Store to install "App
Installer", or fall back to the official download page listed for each component.

```powershell
winget install --id OpenJS.NodeJS.LTS       -e --accept-source-agreements --accept-package-agreements
winget install --id Git.Git                 -e --accept-source-agreements --accept-package-agreements
winget install --id Python.Python.3.14      -e --accept-source-agreements --accept-package-agreements
winget install --id astral-sh.uv            -e --accept-source-agreements --accept-package-agreements
winget install --id Google.Chrome           -e --accept-source-agreements --accept-package-agreements
winget install --id Microsoft.VisualStudioCode -e --accept-source-agreements --accept-package-agreements
winget install --id Docker.DockerDesktop    -e --accept-source-agreements --accept-package-agreements
```

If a Python id fails, list what the machine can actually see and take the highest stable release:

```powershell
winget search Python.Python.3
```

If `astral-sh.uv` fails, install `uv` through Python instead:

```powershell
py -m pip install --user uv
```

**PATH refresh, do this after any install and before testing the new command.** A `winget` install
does not reach the already open shell:

```powershell
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" +
            [System.Environment]::GetEnvironmentVariable("Path","User")
```

If a command still is not found after the refresh, the shell needs to be reopened. Tell the user
that plainly instead of retrying in a loop.

**Docker Desktop installed but not running:**

```powershell
Start-Process "$env:ProgramFiles\Docker\Docker\Docker Desktop.exe"
```

Then poll `docker info` until it answers. First start after installation can take two minutes and may
require a sign out, a reboot, or enabling WSL 2. If Docker asks for a reboot, tell the user, finish
every phase that does not need Docker, and leave Docker dependent items for after the reboot.

Do not pipe a remote script straight into the shell on Windows. Antivirus flags
`iwr ... | iex` patterns. Download to disk first, then run the file.

### 3.2 macOS

If Homebrew is missing, install it from the official source, then follow the on screen instructions to
put `brew` on the PATH:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```bash
brew install node git python uv
brew install --cask docker google-chrome visual-studio-code
```

Xcode command line tools, if `git` is still missing after that:

```bash
xcode-select --install
```

Start Docker Desktop and wait for the daemon:

```bash
open -a Docker
```

### 3.3 Official download pages

Use these when a package manager is unavailable or fails. Always take the current release from the
page, never a link copied out of this file into a version specific artifact.

| Component | Official source |
|-----------|-----------------|
| Node.js | https://nodejs.org |
| Git | https://git-scm.com/downloads |
| Python | https://www.python.org/downloads |
| uv | https://docs.astral.sh/uv/getting-started/installation |
| Docker Desktop | https://www.docker.com/products/docker-desktop |
| Google Chrome | https://www.google.com/chrome |
| VS Code | https://code.visualstudio.com |
| Claude Code | https://claude.com/product/claude-code |

### 3.4 Upgrade commands, by kind

Use these for anything the audit marked as too old or outdated. Each one upgrades in place and keeps
the existing configuration. None of them is a reinstall.

| What | Windows | macOS |
|------|---------|-------|
| Base component from a package manager | `winget upgrade --id <id> -e` | `brew upgrade <formula>` |
| Application installed as a cask | `winget upgrade --id <id> -e` | `brew upgrade --cask <cask>` |
| Global `npm` package | `npm update -g <package>` | `npm update -g <package>` |
| Python tool installed with `uv` | `uv tool upgrade <tool>` | `uv tool upgrade <tool>` |
| Python package installed with `pip` | `py -m pip install --upgrade <package>` | `pip3 install --upgrade <package>` |
| Plugin | `claude plugin update <plugin>` | `claude plugin update <plugin>` |
| Standalone skill cloned with git | `git -C "$HOME\.claude\skills\<name>" pull` | `git -C "$HOME/.claude/skills/<name>" pull` |
| Claude Code itself | `claude update`, and if it came from `winget`, `winget upgrade --id Anthropic.ClaudeCode -e` | `claude update` |

A plugin upgrade only takes effect after the reload in Phase 6. Say so instead of letting the user
wonder why nothing changed.

**MCP servers are a special case, and usually there is nothing to upgrade.** A server configured with
`@latest`, which is how every `stdio` entry in Section 4 is written, fetches the current version on
every start, so it is never behind. A server reached over HTTP is maintained by the vendor, so there is
nothing local to update. The only case that needs action is an entry pinned to a fixed version,
usually from an older setup. Check with:

```
claude mcp get <name>
```

If the arguments carry a hardcoded version instead of `@latest`, tell the user, remove that single
entry, and add it again using the command from Section 4. That is the one situation where removing and
adding is correct, because the configuration itself is what is wrong.

### 3.5 Confirm the base

Check only the rows this phase touched, not the whole battery again. Everything that was already fine
in Phase 1 is still fine, and re running it costs tokens for no information.

Report the corrected rows in one compact line each, then move to Phase 3.

If Claude Code is not signed in, stop and have the user run `claude` and complete the login. Nothing
in Phases 3 through 8 works without it.

---

## Section 4. Phase 3, the MCP menu

### 4.1 Present all three menus at once

Phases 3, 4, and 5 have one shared numbering, 1 to 28, and they are presented **in a single message**,
then answered once. This is the default path, not an option. Three separate rounds of question and
answer cost the user three times the tokens and three times the waiting for no benefit.

The message carries three short headers and the items under each, nothing else:

> **Connections, items 1 to 14.** An MCP is a connection that lets Claude reach a tool you already use,
> instead of you opening that tool and doing the work by hand. Install only what matches services you
> actually use.
>
> **Working methods, items 15 to 18.** A skill is a method the agent loads and follows. A connection
> gives it reach, a skill gives it judgment. Free, installed once, valid in every project.
>
> **Separate tools, items 19 to 28.** Not part of Claude Code. Install only if the description matches
> something you actually do. Items 24, 25, and 26 are full applications that run in Docker and take
> real disk space and setup time.

Each item is one line: number, name, what it does for you, what you need to have, and the cost. Mark
the ones flagged **Recommended**. Do not expand beyond that line unless the user asks about a specific
item. Everything on the list can be added later, so say that once and move on.

**Separate the name from the description with a period or a colon, never with a dash.** This menu is the
longest message of the whole run and the place where the forbidden em dash slips in most easily, because
`name — description` is the reflex format for a list like this. It is banned here exactly as everywhere
else, per Section 1.2. Write `**Playwright**. Opens a real browser...` or `**Playwright**: opens a real
browser...`. Before sending the menu, scan it once for `—` and `–` and fix any that appear.

Some choices also add a permanent rule to the `CLAUDE.md` in Phase 7: items 1, 5, anything in the 6 to
14 range, 16, 17, 18, 20, 21, 22, and 23. Mention that in one line at the end of the menu, not per item.

### 4.2 How the user answers

Accept any of these, and say so in one compact line:

| The user types | Meaning |
|----------------|---------|
| `1, 3, 6, 12` or `1 3 6 12` or `1, 3 e 6` | those items |
| `1 to 5`, `1-5`, `1 a 5` | that range |
| `all`, `tudo`, `todos` | every item |
| `recommended`, `recomendados` | the recommended set, defined below |
| `none`, `nenhum` | nothing, skip to Phase 6 |
| `one by one`, `um por um` | ask item by item, `Y` or `N` each |

**The recommended set** is exactly the items marked `[Recommended]`: 1 through 6, plus 15, 16, and 17,
plus 18 when the user speaks Portuguese. No item between 19 and 28 is recommended by default, because
each one only makes sense for someone who already does that specific thing. Say that in one line if the
user picks `recommended`, so they know the optional block was not silently included.

**Two items have a sub-choice**, and both are answered with a letter attached to the number, so the
whole selection still fits in one reply:

- `19a` Wispr Flow, or `19b` Whisper Fogo
- `23a` Open Slide original, or `23b` Open Slide V2 fork

A bare `19` or `23` means the agent asks which one, so prefer presenting the letters in the menu and
save that round trip.

Echo the selection back once, get a `Y`, then install without asking again per item.

**Keep the original numbers in that echo.** A selection of `15,16,17,18` is confirmed as items 15, 16, 17,
and 18, never renumbered to 1, 2, 3, 4. Renumbering makes the user think you understood something else,
and it breaks the shared vocabulary for the rest of the run, including anything they want to add later.

### 4.3 The rule that overrides the user's number

Never install an MCP for a service the user does not have an account on. If they pick item 8 and have
no Supabase account, say so, offer to skip it, and move on. A connection to an account that does not
exist is a broken menu entry in every future session.

### 4.4 Items 1 through 14

The command blocks show Windows first, then macOS. On Windows, `stdio` servers that run through
`npx` need the `cmd /c` prefix. On macOS they do not.

**Install order, and the one intermediate restart.** Item 1, Playwright, comes first whenever browser
assistance will be needed, because an MCP server is not usable until Claude Code restarts. Concretely:

1. If item 4 was selected, or the user asked for browser help on any item, or the selection includes an
   item that needs a key the user does not want to fetch themselves, then item 1 is required. Say so in
   one line: it is the tool that lets you do the setup for them. If they refuse item 1, item 4 becomes
   impossible, and anything else falls back to the user fetching the value manually. State that plainly
   and move on.
2. Install item 1 and its browser binaries, then restart once, right there, following the protocol in
   Section 1.9, checkpoint included. This is the only restart before Phase 6, and it exists so the rest
   of the phase can use the browser.
3. After the restart, install the remaining selected servers, using browser assistance wherever it
   helps.

If nothing in the selection needs browser assistance, skip the intermediate restart entirely and install
everything in one pass. Do not restart out of habit.

**Every item below that needs a key, a token, or a login follows Section 1.8**: give the direct link
first, offer to do it through the browser if that stalls. The links in each item are already the deep
ones, so pass them as they are. Do not restate the offer inside every item, once per run is enough.

---

**1. Playwright** [Recommended]
Opens a real browser and operates a website the way a person would: it clicks, types, fills forms,
and reads what is on the screen. Useful for anything that lives behind a login and has no export
button.
Needs: nothing. Free.
Official source: https://github.com/microsoft/playwright-mcp

```powershell
claude mcp add --scope user playwright -- cmd /c npx -y "@playwright/mcp@latest" --user-data-dir "$env:LOCALAPPDATA\ms-playwright\mcp-profile-global"
```

```bash
claude mcp add --scope user playwright -- npx -y @playwright/mcp@latest --user-data-dir "$HOME/Library/Caches/ms-playwright/mcp-profile-global"
```

Then install the browser binaries once:

```
npx playwright install chromium
```

**Keep `--user-data-dir`.** It gives the automated browser its own profile folder, so a site you log
into stays logged in for the next session. Without it every run starts logged out and the login has
to be redone every time.

This is also the item that lets the agent do administrative setup for the user, per Section 1.8, and it
is required for item 4. Worth saying in one line when presenting the menu.

---

**2. Brave Search** [Recommended]
Searches the web and brings back current results: news, images, video, and places. This is how the
agent checks a fact that changed after its training data.
Needs: a free Brave Search API key. The free tier covers normal use. Create the key at
https://brave.com/search/api, then paste it when asked.
Official source: https://github.com/brave/brave-search-mcp-server

```powershell
claude mcp add --scope user brave-search -e BRAVE_API_KEY=PASTE_KEY_HERE -- cmd /c npx -y "@brave/brave-search-mcp-server@latest"
```

```bash
claude mcp add --scope user brave-search -e BRAVE_API_KEY=PASTE_KEY_HERE -- npx -y @brave/brave-search-mcp-server@latest
```

---

**3. Chrome DevTools** [Recommended]
Measures a website from the inside: what is slow, what broke, what fails to load. When a page feels
heavy and nobody knows why, this is what finds the reason.
Needs: Google Chrome installed. Free.
Official source: https://github.com/ChromeDevTools/chrome-devtools-mcp

```powershell
claude mcp add --scope user chrome-devtools -- cmd /c npx -y chrome-devtools-mcp@latest
```

```bash
claude mcp add --scope user chrome-devtools -- npx -y chrome-devtools-mcp@latest
```

---

**4. Google Workspace: Gmail, Calendar, Drive, and the rest** [Recommended]
Reads and sends email, reads and creates calendar events, opens and writes files in Drive, all inside
the account you authorize.
Needs: a Google account. Free.
Official source: https://developers.google.com/workspace/guides/configure-mcp-servers

**The agent does this entire setup. The user does not touch the Google Cloud console.** This one is not
optional assistance, it is how the item is installed, because the manual path involves creating a cloud
project, enabling one API per tool, and configuring an OAuth client, which is genuinely too technical to
hand to somebody who just wants their email connected.

Requires item 1, Playwright, installed and active first. See the ordering rule above.

Ask the user exactly two things, in one message, and nothing else:

1. Which Google tools they want reachable. Give the list to pick numbers from: Gmail, Calendar, Drive,
   Docs, Sheets, Slides, Tasks, Contacts.
2. Which Google account, when they have more than one.

Then say this, or something equally short, before opening anything:

> I will open the browser and set this up on your Google account. Configuration screens will move on
> their own, which is normal. You only sign in when the Google login screen appears, and pick the
> account.

Then, through the browser, in this order:

1. Sign in to Google Cloud, stopping at the login screen so the user authenticates.
2. Create a project for this, or reuse one if the user says they have one.
3. Enable the API for each tool the user picked, and only those.
4. Configure the OAuth consent screen and create a desktop OAuth client.
5. Download the client credentials.
6. Register the MCP server pointing at those credentials, with `--scope user`, following the current
   official instructions at the source URL above rather than a command copied from here, because this
   configuration changes shape between versions.
7. Complete the first authorization so the connection is live.

Store the credentials file somewhere private, `$HOME/.claude/` or the location the official instructions
specify. Never on the Desktop, never inside a repository, never printed to the screen.

If any Google screen blocks the flow, verification, an organization policy, a missing billing prompt,
stop and tell the user exactly what Google asked for. Do not try to work around a Google policy screen.

---

**5. Docker** [Recommended]
Starts and controls isolated containers, which are sealed boxes where something can run without
touching the rest of your machine. This is how you try a database, a tool, or a whole application
and then throw it away cleanly.
Needs: Docker Desktop installed and running, plus `uv` from Phase 2. Free.
Official source: https://github.com/QuantGeekDev/docker-mcp

```
claude mcp add --scope user docker -- uvx docker-mcp
```

**Write the absolute path to `uvx`, not the bare name.** The MCP server is started by Claude Code, not
by your shell, and it does not always inherit the shell's PATH. A bare `uvx` works when you test it and
then fails at startup. Resolve it first and register that path:

```powershell
$uvx = (Get-Command uvx).Source
claude mcp add --scope user docker -- $uvx docker-mcp
```

```bash
claude mcp add --scope user docker -- "$(command -v uvx)" docker-mcp
```

If `uvx` is not on the PATH at all, the `uv` install from Phase 2 did not complete. Fix that first
instead of registering a broken entry.

---

**6. GitHub** [Recommended]
Creates the repository where your project lives, records every change with its history, opens and
reviews change requests, and publishes releases. A repository is the place a project's files and their
whole history live, so nothing is ever lost and every version can be recovered.
Needs: a GitHub account, free tier is enough. Authorization happens in the browser, no token to
create. This connection is not required to install anything from this file: the skills and tools in
Phases 4 and 5 are fetched with `git clone`, which works without it.
Official source: https://github.com/github/github-mcp-server

```
claude mcp add --scope user --transport http github https://api.githubcopilot.com/mcp/
```

**Account check.** Many people have a personal GitHub account and a work one. The browser authorization
happens in Phase 6, so ask now which account it should be, and say it back when that moment arrives.
Before the first write to any repository, confirm which organization the work belongs to.

---

**7. Railway**
Puts the parts of an application that run out of sight onto the internet: the database, the
background job, the routine that fires at three in the morning. Through this connection the agent can
deploy a service, read its logs, and restart what got stuck, without opening the Railway website.
Needs: a Railway account, paid after the trial credit. Requires the Railway CLI and a login.
Official source: https://docs.railway.com/ai/mcp-server

```
npm install -g @railway/cli
railway login
claude mcp add --scope user railway -- railway mcp proxy
```

---

**8. Supabase**
Your application's database, with user login and access rules already solved. Through this connection
the agent reads and changes data, applies structure changes, and points out what is left more open
than it should be.
Needs: a Supabase account, free tier is generous. Requires a personal access token created at
https://supabase.com/dashboard/account/tokens
Official source: https://github.com/supabase-community/supabase-mcp

```powershell
claude mcp add --scope user supabase -- cmd /c npx -y "@supabase/mcp-server-supabase@latest" --read-only --access-token PASTE_TOKEN_HERE
```

```bash
claude mcp add --scope user supabase -- npx -y @supabase/mcp-server-supabase@latest --read-only --access-token PASTE_TOKEN_HERE
```

`--read-only` is on purpose. It lets the agent read and analyze the database but not change it. Keep
it unless the user explicitly asks for write access, and if they do, say plainly what that allows.

---

**9. Vercel**
Publishes a website and gives it a real address for people to visit. Through this connection the
agent publishes, shows which build failed and why, and reports the visitor numbers.
Needs: a Vercel account, free tier covers small projects. Browser authorization, no token.
Official source: https://vercel.com/docs/mcp/vercel-mcp

```
claude mcp add --scope user --transport http vercel https://mcp.vercel.com
```

---

**10. Cloudflare**
Also publishes sites, and on top of that manages the address itself, the speed, and the rules that
block unwanted traffic. Covers the whole Cloudflare account through one connection.
Needs: a Cloudflare account, free tier covers domains and basic protection. Browser authorization.
Official source: https://github.com/cloudflare/mcp

```
claude mcp add --scope user --transport http cloudflare https://mcp.cloudflare.com/mcp
```

---

**11. Metabase**
Live dashboards built on top of your business database: revenue, stock, conversion funnel, whatever
you need to see. Through this connection the agent builds the dashboard and answers questions
against the numbers.
Needs: a Metabase instance of your own, cloud or self hosted, and an administrator who has switched
the MCP server on in the admin settings. Free when self hosted.
Official source: https://www.metabase.com/docs/latest/ai/mcp

```
claude mcp add --scope user --transport http metabase https://YOUR-METABASE-ADDRESS/api/metabase-mcp
```

Ask the user for their Metabase address and substitute it. Browser authorization follows, scoped to
the permissions that user already has in Metabase.

---

**12. Sentry**
Tells you the moment your site breaks in somebody's hands, with the error log and the path that led
to it. Through this connection the agent pulls the error that blew up in production and the code that
caused it.
Needs: a Sentry account, free tier covers small volume. Browser authorization.
Official source: https://github.com/getsentry/sentry-mcp

```
claude mcp add --scope user --transport http sentry https://mcp.sentry.dev/mcp
```

---

**13. PostHog**
Answers whether people actually used your site, where they got stuck, and what nobody ever opened.
Through this connection the agent builds dashboards and queries those numbers directly.
Needs: a PostHog account, free tier is generous. Browser authorization.
Official source: https://github.com/PostHog/mcp

```
claude mcp add --scope user --transport http posthog https://mcp.posthog.com/mcp
```

---

**14. Higgsfield**
Generates images and video through more than thirty specialized models gathered in one place.
Through this connection the agent produces the asset without you opening the site.
Needs: a Higgsfield account, paid by credits. Browser authorization, no key to manage.
Official source: https://higgsfield.ai/mcp

```
claude mcp add --scope user --transport http higgsfield https://mcp.higgsfield.ai/mcp
```

Higgsfield also ships its own command line tool, which some people prefer for Claude Code. If the
MCP path gives trouble, check https://higgsfield.ai/cli

### 4.5 After the MCP installs

Do not verify here. `claude mcp add` fails loudly when it fails, and the servers that authorize in the
browser cannot be confirmed until after the reload anyway. The single verification pass is Phase 6.

Say one line so the user is not surprised later: the connections that open a browser window are not
active yet, and that happens right after the restart.

---

## Section 5. Phase 4, the skills menu

Presented and answered together with Phases 3 and 5, in the single pass described in Section 4.1. The
header text for this block lives there. This section is only the item detail and the commands.

### 5.1 Items 15 through 18

---

**15. Ponytail** [Recommended]
Makes the agent write the smallest thing that actually solves the problem. No leftover code, no extra
library, no structure nobody asked for. This is the difference between something you can still
understand in six months and something nobody dares to touch.
Official source: https://github.com/DietrichGebert/ponytail

```
claude plugin marketplace add DietrichGebert/ponytail
claude plugin install ponytail@ponytail
```

---

**16. Matt Pocock skills** [Recommended]
A framework for building real software: diagnose the error before fixing it, write the test before
the code, review every change. It is the discipline part, and it is what keeps a project from
degrading as it grows.
Official source: https://github.com/mattpocock/skills

```
claude plugin marketplace add mattpocock/skills
claude plugin install mattpocock-skills@mattpocock
```

If the marketplace registers under a different name than `mattpocock`, run `claude plugin list` to see
the real name and install with that instead.

---

**17. Security Engineer** [Recommended]
Makes the agent build secure projects from day one, layer by layer, choosing the cheapest protection
that closes each real risk. It comes in on its own for anything that will be reachable from the
internet or run in production, at the moment the project is created and again whenever login, payment,
file upload, or an admin area enters it.
Official source: https://github.com/bruno-org/security-engineer

```powershell
git clone https://github.com/bruno-org/security-engineer.git "$HOME\.claude\skills\security-engineer"
```

```bash
mkdir -p "$HOME/.claude/skills"
git clone https://github.com/bruno-org/security-engineer.git "$HOME/.claude/skills/security-engineer"
```

---

**18. Humanizer PT-BR** [Recommended for Portuguese speakers]
Takes the robot tone out of what the agent wrote and gives back rhythm, natural phrasing, and
Portuguese that reads like a person wrote it. Install this whenever the user's language is Brazilian
Portuguese.
Official source: https://github.com/mackswendhell/humanizer-pt-br

```powershell
git clone https://github.com/mackswendhell/humanizer-pt-br.git "$HOME\.claude\skills\humanizer-pt-br"
```

```bash
mkdir -p "$HOME/.claude/skills"
git clone https://github.com/mackswendhell/humanizer-pt-br.git "$HOME/.claude/skills/humanizer-pt-br"
```

### 5.2 After the skill installs

Nothing to verify here. Plugins and standalone skills are only discovered after the reload, so the
single verification pass happens in Phase 6. Do not run `claude plugin list` twice.

---

## Section 6. Phase 5, the optional menu

Presented and answered together with Phases 3 and 4, in the single pass described in Section 4.1. The
header text for this block lives there. This section is only the item detail and the commands.

When an item here needs Docker and the daemon is not answering, start Docker Desktop yourself using the
command in Section 3.1 or 3.2, wait for it, and continue. Do not hand that back to the user.

**Where the items in this phase get installed.** Two different destinations, and the difference matters:

| Kind of item | Destination | Why |
|--------------|-------------|-----|
| A tool the **agent** uses, items 20 and 21 | `$HOME/.claude/tools/<name>/`, or `uv tool` for a CLI | The user never opens it directly |
| An application the **user** opens and works in, items 19 and 23 | a folder named after the tool, on the resolved Desktop | The user has to be able to find it |
| An application from a package manager, item 22 | wherever the package manager puts it | Not our call |
| A multi container application, items 24 to 26 | wherever the project's official compose setup puts it | Follow the vendor |

Never run `git clone` without a destination path. Without one it clones into whatever directory the
shell happens to be in, and neither the user nor a future session will find it. Resolve the Desktop
path first, with `[Environment]::GetFolderPath('Desktop')` on Windows or `$HOME/Desktop` on macOS,
since Windows frequently redirects it into OneDrive.

Record the final path of everything installed here. Phase 8 writes it into
`reference-optional-tools.md`, and a later session needs it to find the tool instead of installing a
second copy.

### 6.1 Items 19 through 28

---

**19. Voice transcription**
You speak, the text appears. Two options that solve the same thing:

- **Wispr Flow**: works on any machine, has a limited free plan, paid beyond it.
  Official source: https://wisprflow.ai
- **Whisper Fogo**: free, runs entirely on your own machine, needs an NVIDIA graphics card.
  Official source: https://github.com/bruno-org/whisper-fogo

`19a` is Wispr Flow, `19b` is Whisper Fogo. Wispr Flow is a normal application download from its site.

For Whisper Fogo, clone into a named folder on the Desktop and run the installer from inside it. Replace
`<DESKTOP>` with the resolved Desktop path:

```powershell
git clone https://github.com/bruno-org/whisper-fogo.git "<DESKTOP>\Whisper Fogo"
cd "<DESKTOP>\Whisper Fogo"
powershell -ExecutionPolicy Bypass -File instalador/instalar.ps1
```

```bash
git clone https://github.com/bruno-org/whisper-fogo.git "<DESKTOP>/Whisper Fogo"
cd "<DESKTOP>/Whisper Fogo"
bash instalador/instalar.sh
```

Check for an NVIDIA card before offering `19b`. Without one, say so and point to `19a` instead of
letting the install fail.

---

**20. Watermarks Remover**
Cleans the invisible marks AI leaves behind: hidden characters inside text and provenance data
embedded in files. It matters when you hand work to a client, publish it, or send it as part of an
application, because you decide what to disclose, not the tool that generated it.
Official source: https://github.com/guillaumemeyer/watermarks-remover

```powershell
git clone https://github.com/guillaumemeyer/watermarks-remover.git "$HOME\.claude\tools\watermarks-remover"
```

```bash
mkdir -p "$HOME/.claude/tools"
git clone https://github.com/guillaumemeyer/watermarks-remover.git "$HOME/.claude/tools/watermarks-remover"
```

Then follow the repository's own README for the skill install and the local service. Read it at
install time rather than trusting a command copied here, and tell the user which of the two paths
their machine ended up with:

- the markdown skill, which calls a local service over HTTP, or
- the text only skill installed by `install_skill.py`, which needs no service

Honest limits, repeat them to the user: this removes invisible characters and file metadata, which is
verifiable and countable. It does not make text undetectable, and nobody should claim it does.

---

**21. SkillSpector, by NVIDIA**
NVIDIA's security scanner for agent skills. It runs a fine comb over a skill before you install it,
looking for hidden instructions and code that steals data. A skill is a set of instructions your agent
will follow, so installing one from a stranger without reading it carries the same risk as running a
stranger's program on your machine.
Official source: https://github.com/NVIDIA/SkillSpector

```
uv tool install git+https://github.com/NVIDIA/skillspector.git
```

Confirm it works:

```
skillspector scan https://github.com/DietrichGebert/ponytail
```

Reading the report, in this order:
1. Coverage first. If coverage is not near 100 percent, the result is invalid, neither approval nor
   rejection. Run it again.
2. Score second. LOW is fine. MEDIUM means read every finding. HIGH or CRITICAL means do not install
   until a human looks at it.
3. Never report the number alone. Static analysis matches text patterns and produces false positives
   on legitimate skills. Open the flagged line, read it, and say what it actually is.

---

**22. Obsidian**
Your notes and the agent's memory as plain text files you can see on your own machine, in folders you
control. The agent reads and writes in them alongside you, and nothing is locked inside somebody
else's product.
Official source: https://obsidian.md

```powershell
winget install --id Obsidian.Obsidian -e --accept-source-agreements --accept-package-agreements
```

```bash
brew install --cask obsidian
```

---

**23. Open Slide**
Presentations written by conversation instead of dragged into place. Two versions, and they are not
interchangeable:

- `23a` **Original**: https://github.com/1weiho/open-slide
- `23b` **V2 fork**: moves and resizes elements on the canvas, exports editable PPTX, and resolves fonts
  on its own. https://github.com/bruno-org/OpenSlideV2

Install exactly one. Clone it into a folder named `Open Slide` on the resolved Desktop, so there is a
single known location for it, then install its dependencies:

```
git clone <chosen repository URL> "<DESKTOP>/Open Slide"
cd "<DESKTOP>/Open Slide"
npm install
```

Needs Node.js from Phase 2.

Record which of the two was installed, and its path, for `reference-optional-tools.md` in Phase 8. The
`CLAUDE.md` rule for this item tells the agent to use the installed version and never the other one, and
that rule is only enforceable if the version and the path were written down here.

To start it later, `npm run dev` from that folder. Tell the user the address it prints, and that it only
runs while that command is running.

---

**24. ERPNext**
A complete and free ERP: stock, finance, purchasing, and sales, with no per user license. This is the
system that runs the operational side of a company.
Official source: https://github.com/frappe/erpnext
Needs Docker running. This is a multi container application, not a single install.

Do not improvise the setup. Open the official deployment repository at
https://github.com/frappe/frappe_docker, follow its current instructions, and tell the user up front
that this takes real time and disk space.

---

**25. Twenty**
An open CRM built to work with AI: contacts, companies, deals, and pipeline. The alternative to
Salesforce without the Salesforce bill.
Official source: https://github.com/twentyhq/twenty
Needs Docker running.

Follow the official self hosting instructions in the repository. Same warning as item 24.

---

**26. Chatwoot**
One support inbox for everything: the chat widget on your site, email, and WhatsApp in the same
place, with history per customer.
Official source: https://github.com/chatwoot/chatwoot
Needs Docker running.

Follow the official self hosting instructions in the repository. Same warning as item 24.

---

**27. Scrapling**
Pulls data out of websites and adapts when the site moves things around, instead of simply breaking
the way ordinary scraping does.
Official source: https://github.com/D4Vinci/Scrapling
Needs Python from Phase 2.

```
pip install "scrapling[fetchers]"
scrapling install
```

On Windows, if `pip` is not found, use `py -m pip` instead. The second command downloads the browsers
it drives, so it takes a few minutes.

---

**28. n8n skills**
Teaches the agent to build automations in n8n correctly, so you do not have to learn the tool before
using it. Useful only if you already use n8n or intend to.
Official source: https://github.com/czlonkowski/n8n-skills

```
claude plugin marketplace add czlonkowski/n8n-skills
claude plugin install n8n-mcp-skills@n8n-skills
```

If that plugin name does not resolve, run `claude plugin marketplace add czlonkowski/n8n-skills`
followed by `claude plugin list` to read the real name, and install with that.

---

## Section 7. Phase 6, verify and reload

### 7.1 Reload so everything is picked up

**First, check whether this phase has anything to do.** If the user selected nothing in Phases 3, 4, and
5, there is nothing to activate and nothing to verify. Skip 7.1, 7.2, and 7.3 entirely, say so in one
line, and go straight to Phase 7. That skips 7.2, 7.3, and 7.4 as well. Asking for a restart with nothing
installed wastes the user's time and breaks the rule in Section 1.9 about restarting only when something
needs it.

Otherwise: new MCP servers, new plugins, and newly cloned skills are read at startup, so nothing
installed in Phases 3 through 5 is active until a reload. This is the final restart of the run.

Follow the protocol in Section 1.9: write the checkpoint, then give the instruction for their setup, then
continue from the checkpoint when they come back. The next step after this restart is the browser
authorizations in Section 7.2, or Section 7.3 when nothing needs authorizing.

### 7.2 Complete the browser authorizations

Any server that authorizes in the browser is inactive until the user finishes the login. After the
reload:

```
/mcp
```

For each server that shows as needing authentication, select it, complete the browser flow, and come
back. This is the one place in the run that genuinely cannot be compressed: each authorization opens its
own browser window and has to be finished before the next one starts. Say up front how many there are,
so the user knows the size of what is left, then go through them without commentary between each.

Do not declare the setup finished while a login is still pending.

### 7.3 Verify

```
claude mcp list
claude plugin list
```

Plus a directory listing of `~/.claude/skills/`.

Do not claim something works because the install command exited without an error. For each item,
check that it appears in the listing. For the ones that authorize, check that the listing does not
still say "needs authentication."

### 7.4 Report

One table: item, status, and what is still pending. Then a short list of what failed and why, if
anything did. Then the two or three things the user can try right now with what they just installed,
phrased as sentences they could type.

State plainly anything that did not get done, including anything waiting on a reboot. Do not soften
it and do not bury it.

---

## Section 8. Phase 7, write the global CLAUDE.md

This phase is not optional and it runs after every install is done, so the rules match what the
machine actually has.

### 8.1 What this file is

`$HOME/.claude/CLAUDE.md` holds instructions the agent reads in every session, in every project, on
this machine. It is how the setup outlives this conversation.

### 8.2 Expect a permission prompt, and prepare the user for it

Claude Code treats its own configuration directory as sensitive, so writing to
`$HOME/.claude/CLAUDE.md` asks the user to approve it. The request is normal and the write succeeds once
approved, but a person who was not told it was coming reads "wants to write to a sensitive file" and says
no, which throws away the whole point of the run.

So say it before writing, in one line: "I am going to save these rules in your global settings file, and
your Claude will ask you to approve that. Approve it." Then write.

If the user declines, do not argue and do not retry in a loop. Show them the block, tell them the exact
path, and say they can paste it there whenever they want. Then record it as pending in Phase 8.

### 8.3 Never overwrite, always merge

1. Check whether the file exists.
2. If it exists, read it fully and back it up next to itself, with a timestamped name.
3. Write the setup's rules inside a marked block, exactly these markers:

```
<!-- prodman-setup-for-claude-code:start -->
... generated rules ...
<!-- prodman-setup-for-claude-code:end -->
```

4. If the block is already there from a previous run, replace the block and leave everything outside
   it untouched.
5. If the file does not exist, create it with the block.

Never delete or rewrite a rule the user wrote themselves. If a generated rule contradicts something
already in the file, do not resolve it silently: show both, and let the user decide.

### 8.4 What language the file is written in

Write the `CLAUDE.md` content in the user's language, the same one detected in Section 1. Keep the
block markers, file paths, commands, and tool names in English.

### 8.5 Rules that always go in

These are the rules that make the setup permanent. They are not instructions for the installation run,
they are how the agent behaves from now on, in every session, in every project, in every kind of
interaction the user ever has with it. Write them as standing rules addressed to the agent, in the
present tense, with no reference to this setup having happened.

**A1. Typography.** Always include, in every language, no exception:

- **The em dash (`—`) and the en dash (`–`) are forbidden. A double hyphen (`--`) is not an acceptable
  substitute for either.** For a pause or an aside, use a comma, a period, a colon, parentheses, or
  rewrite the sentence. A single hyphen is only for a real compound word and for CLI flags, where it is
  syntax rather than punctuation.
- This applies to absolutely everything, with no exception ever: screen conversation, code, comments,
  commit messages, Markdown, documents, spreadsheets, PDFs, email, posts, website copy, and any other
  output. It applies whether the text is in Portuguese, English, or any other language.
- Never emit an invisible character, a non breaking space, or an exotic space character inside text.
- Short, direct sentences. Active verbs. No emoji unless the user uses emoji first or asks for it.

**A2. Brazilian Portuguese.** Include whenever the detected language is Brazilian Portuguese:

- Brazilian Portuguese is the default for **every** output, universally and permanently: screen
  conversation, messages, code comments, commit messages, UI strings where applicable, Markdown files,
  `.docx` documents, `.xlsx` spreadsheets, PDFs, textual fields in JSON meant for a person to read,
  email, and any artifact containing natural language meant for this user.
- The only exception: the user writes in another language **and** asks for the reply in that language,
  or the artifact clearly has to be in another language for an obvious reason, for example a resume in
  English or documentation for a public API. When in doubt, ask, do not pick another language by
  assumption.
- **Diacritics are mandatory and must be correct**, every single one: `á ã â é ê í ó ô õ ú ç`. Never
  "voce", "nao", "esta", "atencao", "acao", "informacao". The tooling supports Unicode, so there is no
  excuse for a missing accent.
- Correct grammar and spelling: verb and noun agreement, "crase", "regência", punctuation. Review
  before sending, every time.
- This is not a stylistic preference. It is a requirement, and it holds in every future session
  without needing to be restated by the user.

**B. Where files are saved.** Always include:

- When the user asks for a file without saying where to put it, the destination is the user's
  Desktop, with a descriptive name in the user's language.
- A subfolder is allowed, as long as it lives on the Desktop. When several related files belong
  together, or when the deliverable is a set rather than one file, create a folder on the Desktop and
  put them there. Name the folder after the work, not after the date or the tool that made it.
- Never write outside the Desktop for a request that did not name a path. Not Documents, not a
  project folder somewhere else, not the drive root.
- When the user does give a path, obey that path literally, even when it sits outside the Desktop.
- Resolve the real Desktop path instead of assuming it. On Windows it is frequently redirected into
  OneDrive, so a hardcoded `C:\Users\<name>\Desktop` writes into a folder the user never looks at:

  | Platform | How to resolve it |
  |----------|-------------------|
  | Windows | `[Environment]::GetFolderPath('Desktop')` |
  | macOS | `$HOME/Desktop` |

- Temporary work of the agent's own, drafts, caches, execution logs, intermediate results, goes to
  the session scratchpad, never to the Desktop.

**C. Global installs.** Always include:

- Anything installed for this agent is installed globally, for every project: MCP servers with
  `--scope user`, plugins in user scope, standalone skills in `~/.claude/skills/`. Never scope an
  install to a single project unless the user asks for exactly that.

**D. Edit, do not recreate.** Always include:

- Always prefer editing, extending, or trimming something that already exists and is mostly correct
  over throwing it away and writing it again from scratch. This applies to every artifact: code,
  documents, spreadsheets, configuration, screens, scripts.
- The reason is loss. Rewriting a file that held twenty carefully placed details almost always drops
  one or two of them, even when the intent was to reproduce it faithfully. A surgical edit at the one
  place that needs to change preserves everything that was already right.
- Before rewriting anything, ask: is this base mostly correct, and is this a targeted change? If yes,
  edit the specific point.
- When the same content must be updated in several places, update the single canonical source first,
  then copy the updated piece literally into each dependent place. Do not invent a fresh version of
  the piece in each location.
- When the user asks for "adjustments", read that as a surgical edit, never as a rewrite. When in
  doubt, ask first.
- The one legitimate exception is a file so broken or inconsistent that rewriting is demonstrably
  cheaper than repairing. Say so explicitly, name the reason, and take extra care not to lose
  details.

**E. Dates and times come from the machine's clock.** Always include:

- The user's own computer clock is the only source of truth for any date, time, duration, or
  chronological order. Never guess, never infer from conversation context, and never use the date
  supplied in the model's own context, which is not the user's local time.
- Run the command below and use its literal output **before writing any temporal statement**: today,
  yesterday, now, morning, afternoon, evening, "X minutes ago", any date, any time, a last updated
  stamp, a date inside a document, a filename, a commit message, or a memory.

  | Platform | Command |
  |----------|---------|
  | Windows | `powershell -NoProfile -Command "Get-Date -Format 'dd/MM/yyyy HH:mm zzz'"` |
  | macOS | `date "+%d/%m/%Y %H:%M %z"` |

- The check is **per statement, not per session**. Quoting a time now and again forty minutes later
  in the same session means running the command twice, because the minutes moved.
- The offset the command reports is the user's real offset. In Brazil that is normally UTC-3. Use what
  the machine reports and state the offset when it matters, rather than assuming a fixed one.
- Date format in running text is the Brazilian standard, `DD/MM/YYYY`, for example `17/08/2026`. Never
  `MM/DD` and never `YYYY-MM-DD` inside a sentence. In filenames the ISO form `YYYY-MM-DD` is fine,
  because it sorts, but inside the file's content use `DD/MM/YYYY`.
- Time format is 24 hour, `HH:MM`, for example `17:08`. Never AM or PM.
- Do not use a period qualifier without the clock output confirming it. Dawn is 00:00 to 05:59,
  morning 06:00 to 11:59, afternoon 12:00 to 17:59, evening 18:00 to 23:59. Near a boundary, for
  example 17:50, use the exact time instead of the qualifier.

**F. Confirm before anything hard to undo.** Always include:

- Before any action that deletes data, creates a paid resource, charges a card, pushes, deploys,
  affects production, sends email, or writes to somebody else's system: show the user exactly what is
  about to happen, on which account or which environment, and get a yes. Even when it looks obvious.
- Approval for one action does not extend to the next one. Ask again.
- Before deleting or overwriting anything, read what is there first.
- When the user reaffirms an action after a raised concern, that is their decision. Say so once and
  carry it out in full.

**G. Repositories are private by default.** Always include:

- Any repository created is private. Making one public happens only when the user says so explicitly
  and by name, for that repository, in that conversation. Never infer that a project is meant to be
  public from its content or its license.
- The same care applies to anything else that publishes: a deploy that exposes a URL, a package
  published to a registry, a document shared by link. Publishing is not reversible in practice, since
  content can be cached or indexed even after removal.

**H. Commits carry no agent attribution.** Always include:

- Commit messages do not include a `Co-Authored-By` line for the agent, and no other trailer, footer,
  or signature announcing that an AI wrote the change. The commit describes the change, nothing else.
- The same holds for pull request descriptions, release notes, and code comments: no "generated by"
  markers unless the user asks for one.

**I. Local first, production last.** Always include:

- Build and test locally before touching anything in production. Get it working on the user's machine,
  then in a staging or preview environment when one exists, and only then in production.
- When a test needs real data, use a copy or a development environment. Never run an exploratory
  operation against production data.
- Never leave a test artifact behind in the user's environment: test records, temporary files, debug
  flags, seeded rows. Clean up, and say what was cleaned.

**J. Research before asking.** Always include:

- When the answer is available in the code, in the configuration, in the file the user is looking at,
  or in the official documentation, find it instead of asking the user. Read first, ask second.
- Reserve questions for what only the user can answer: intent, priority, business rules, which
  account, whether to proceed with something risky. Those are worth asking, and asking early beats
  guessing.
- One well aimed question beats three vague ones. Ask the specific version: not "which account should
  I use", but "I see a personal account and a work account for this service, which one applies here".

**K. Linear by default, orchestration as a last resort.** Always include:

- Default to working step by step in the main thread, with the ordinary tools, one action at a time,
  showing progress as it goes. This is the normal way to work, not a fallback.
- Multi-agent orchestration, parallel agent fan out, and workflow tooling are heavy artillery. They
  are not forbidden, they are simply the wrong instrument for almost everything. Do not reach for
  them because a task looks big, because it would be interesting, or because the harness signals that
  maximum effort or exhaustive orchestration is available. A high effort setting is not an instruction
  to orchestrate.
- Reach for orchestration only after concluding that it is genuinely the best way to solve this
  specific problem, and when that happens, say in one line why the linear path does not fit. Cases
  that actually qualify tend to look like a sweep across dozens of independent files, or work that
  will not fit in one context no matter how it is sequenced.
- A single delegated subagent for one bounded search or one bounded task is not orchestration and does
  not need this justification. Even so, prefer doing it in the main thread when that works.

**L. Keep the memory current, without being asked.** Always include:

- Maintaining memory is the agent's own standing job, in every project and every session, not a task
  the user has to request. The user should never have to say "remember this" for the obvious things.
  When they do ask, do it as well, but do not wait for it.
- **Write a memory when any of these happens**, on the next pass after it happens:
  - A durable fact about the user surfaces: their role, their tools, how they work, what they prefer.
  - A durable fact about a project surfaces: its purpose, its constraints, a decision that will still
    matter in three months.
  - Something is finished: a feature shipped, a setup completed, a problem solved, a deliverable
    handed over.
  - Something took real effort to discover and would cost the same effort to discover again: an
    environment quirk, a configuration that only works one way, a command with a non obvious flag.
  - The user corrects the agent, or states a preference about how work should be done. Record the
    correction and the reason behind it.
- **Manage what is already there, do not just append.** On each pass: keep one fact per file, keep the
  index in sync with the files, merge duplicates into the file that already covers the subject, update
  a file whose content drifted, and delete a memory that turned out to be wrong. A memory store that
  only grows becomes a memory store nobody trusts.
- **Cadence.** Do a pass at the end of any substantial piece of work, and in a long session do one at
  natural milestones rather than waiting for the end. The goal is that at any moment the memory
  reflects what is actually true on this machine and in these projects, so a future session starts
  informed instead of rediscovering everything.
- **Do not interrupt the work to do it.** Finish the task first, then write in one pass, then tell the
  user in one line what was recorded. Never turn memory keeping into a running commentary, and never
  ask permission for each individual memory.
- **Never write into memory:** a password, a token, an API key, or any other credential. Record which
  service and which account, never the secret itself. Also skip anything the code, the configuration,
  or the git history already records on its own, and anything that only mattered inside one
  conversation.
- Convert relative dates to absolute ones, following rule E. A memory saying "last week" is useless
  six months later.
- A memory reflects what was true when it was written. Before acting on one that names a file, a
  command, a flag, or a version, verify it still holds.

### 8.6 Rules that go in only if the matching tool got installed

Include a rule only when its tool is actually present. A rule pointing at a tool that is not
installed is worse than no rule, because it will be followed and fail.

Ponytail, item 15, deliberately gets no rule here. It installs a `SessionStart` hook that injects its
own ruleset at the beginning of every session, including into subagents, so it is already active
without anything in `CLAUDE.md`. Restating it would only duplicate instructions that are already
loaded. Do not add one.

**If item 21, SkillSpector, was installed:**

- Before installing any skill, plugin, subagent, slash command, or MCP server that came from outside
  this machine, validate it first with `skillspector scan <target>`. Validation comes before
  installation, not after.
- This covers anything from a public repository, a link somebody sent, or a marketplace. It does not
  cover a skill the user wrote themselves, and it does not cover ordinary application dependencies
  such as an npm or pip library.
- Read the report in this order: coverage first, and if coverage is not near 100 percent the result
  is invalid and must be run again. Score second: LOW installs, MEDIUM means read every finding and
  show them to the user, HIGH or CRITICAL means do not install and bring the findings to the user.
- Never report the score alone. Open every flagged line and say what it actually is. Static analysis
  produces false positives on legitimate skills.
- The user makes the final call in every case.

**If item 18, Humanizer PT-BR, was installed:**

- When **writing** natural language content in Portuguese that a person will read, articles, reports,
  documents, `.txt`, `.docx`, `.pdf`, spreadsheets, posts, email, website copy, use the
  `humanizer-pt-br` skill as part of composing it, so the text comes out with Brazilian Portuguese
  rhythm from the start rather than being repaired afterwards.
- **Never run this skill over text the agent did not write.** A document the user opened, received,
  imported, or wrote themselves is theirs. Rewriting it uninvited destroys their voice and their
  wording. The only case where it is allowed is an explicit request: the user asks to humanize that
  text, to review it, or to fix its Portuguese. Absent that request, it is forbidden, even when the
  text obviously reads like machine output.
- No final verification pass over content the agent already wrote with the skill. It was written that
  way, a second pass adds nothing.
- This never applies to code, commands, identifiers, file paths, or quoted material, which stay
  exactly as they are.

**If item 20, Watermarks Remover, was installed:**

What it is, in one sentence for the user: it strips the metadata and hidden characters that identify a
file as having been produced by artificial intelligence.

It does **not** run on everything. Three tiers, and the tier is decided by what the file is for:

- **Always, on explicit request.** The user asks to clean a file, strip watermarks, remove metadata,
  or check a file for AI marks. Run it on whatever they point at, whoever wrote it.
- **Always, without asking, on human facing work documents.** A deliverable that a person receives and
  reads as a document: `.txt`, `.pdf`, `.docx`, `.odt`, `.xlsx`, `.csv`, and images, `.png`, `.jpg`,
  `.jpeg`, `.webp`. This is a report, a proposal, a spreadsheet, a letter, a presentation, an
  application, an article, a generated image. These are the files where provenance metadata leaks
  authorship in a way that is nobody's business, so clean before saying it is ready.
- **Never automatically, on technical artifacts.** Source code, `.md` files that belong to a project,
  skills, agent definitions, prompts, configuration, and anything that lives in a repository. Being
  visibly AI assisted is not a problem there, and cleaning it uninvited touches files the user may be
  versioning. You may **offer** it in one line. If they do not say yes, do not run it.

Flow when it does run: run the cleaner over the file, run the inspection mode to confirm nothing is
left, and only then report the file as ready, telling the user literally what was removed, without
rounding the counts.

Hard exclusions, regardless of tier:

- Never clean memory files, including the memory index, ever. The cleaner reads the frontmatter
  `description` field as AI metadata and deletes that whole line, and that field is exactly what
  memory recall uses to decide relevance. Do not even inspect them by routine.
- Never clean a file the user wrote themselves, a source database, a spreadsheet they maintain, or an
  asset delivered by a client, unless they explicitly ask for that file.
- Preserve diacritics. Never use options that normalize or fold characters, they damage Portuguese
  text.

Honest limits, never promise past them: removing invisible characters and file metadata is
deterministic and countable. Statistical marks that live in word choice only go away with rewriting,
and even then it is best effort. Never claim a text "passes as human" or "is undetectable".

The cleaner does not reach what the agent types on screen. So in writing: never emit an invisible
character, a non breaking space, or an exotic space inside text.

**If item 1, Playwright, was installed:**

- Never remove the `--user-data-dir` argument from the Playwright MCP configuration. It is what keeps
  a browser login alive between sessions. Without it, every run starts logged out.

**If item 5, Docker, was installed:**

- Whenever an application would be better off running in a container, which is the case for almost
  anything with a database, a web service, a background worker, or a self hosted tool, run it in a
  container through the Docker MCP instead of installing it directly onto the machine. Decide this
  yourself. The user should not have to know what a container is, or be asked to choose.
- Docker Desktop must be running for that to work. When `docker info` fails or an MCP call fails
  because the daemon is not answering, **start Docker Desktop yourself**, wait for the daemon to come
  up, and then continue through the MCP. Do not stop and hand the problem to the user.

  | Platform | Command to start it |
  |----------|---------------------|
  | Windows | `Start-Process "$env:ProgramFiles\Docker\Docker\Docker Desktop.exe"` |
  | macOS | `open -a Docker` |

  First start can take up to two minutes. Poll `docker info` until it answers instead of failing on the
  first attempt. Only involve the user when Docker itself demands a reboot or an enabled feature, and in
  that case say exactly what it asked for.
- Say in one plain sentence what you are doing and why, without jargon: "this runs in an isolated box
  so it does not touch the rest of your machine." Do not narrate container internals.

**If items 6 through 14, any service MCP, were installed:**

- Many people keep more than one account on the same service, a personal one and a work one, and the
  separation is intentional. Never assume the account currently authorized is the right one for the
  task at hand.
- Before any action with a side effect, identify which context the task belongs to, check which
  account is authenticated, and compare. If they do not match, stop and tell the user before running
  anything.
- For anything that creates a paid resource, deletes data, commits, pushes, deploys, sends email, or
  affects production, show the user which account is about to be used and get confirmation, even when
  it looks obvious.

**If item 22, Obsidian, was installed:**

- Memory files are plain Markdown the user can open in Obsidian. Keep wiki style links, `[[name]]`,
  and never convert them to standard Markdown links, that breaks the graph.

**If item 23, Open Slide, was installed:**

- Any request for a presentation, slides, a deck, a talk, or a pitch uses Open Slide by default,
  unless the user names another tool. Do not ask which tool, and do not improvise a substitute. If
  Open Slide is broken or unavailable, say so and ask, instead of switching tools unannounced.
- **Detect which version is installed before doing anything, and use that one.** Two exist, the original
  and the V2 fork, and they are not interchangeable. The installed version and its path were recorded in
  memory when it was installed, so read that first. If the memory is missing, read the workspace itself
  and its `package.json` to identify which one it is, then record it.
- Never call the version the user does not have, and never install the other one alongside it. Someone
  who installed the fork and ends up with the original installed next to it now has two workspaces,
  two dependency trees, and slides that open in the wrong one. That must not happen.
- When both are present on the machine, the version the project itself uses wins. Ask only if the
  project does not make it clear.

**If item 16, Matt Pocock skills, were installed:**

- These skills are triggered by recognizing the situation, and their own descriptions expect wording a
  non technical user will never type. So map the situation yourself instead of waiting for the keyword:
  something is broken, throwing, failing, or slow goes to `diagnosing-bugs`; building a feature or
  fixing a bug goes to `tdd`; checking whether an idea or a data model holds up goes to `prototype`;
  gathering facts from primary sources goes to `research`; naming things and recording decisions goes to
  `domain-modeling`; reviewing a set of changes goes to `code-review`; a git conflict goes to
  `resolving-merge-conflicts`; stress testing a plan goes to `grilling`; writing or editing a skill,
  an `AGENTS.md`, or a `CLAUDE.md` goes to `writing-for-agents`.

**If item 17, Security Engineer, was installed:**

- Bring the `security-engineer` skill in, without being asked, whenever the project will be reachable
  from the internet or will run in production. That includes anything published to a real address,
  anything deployed, and anything other people will use, whether they are customers, clients, or
  colleagues inside the company. Internal does not mean private, and internal does not mean it can skip
  this.
- Bring it in at the start, when the project is created and the stack and hosting are being chosen,
  because that is when the cheap decisions are still available. Coming in after the fact costs a
  rewrite.
- Bring it in again whenever login, payment, file upload, a database table, an admin area, personal
  data, or a deploy pipeline enters the project, on any project that meets the condition above.
- A script that runs only on the user's own machine and never leaves it does not need this. Say so
  rather than invoking the skill out of habit.

### 8.7 Show it before you write it

Show the user the exact block you are about to write, in their language, and get a yes. If they want
a rule out, take it out without arguing. Then write the file and confirm the path.

---

## Section 9. Phase 8, write the memories

**This is the last step of the whole run, and nothing comes after it.** Memories record what this
machine now has, so a future session does not have to rediscover any of it.

Do not start this phase until all of the following are true. Check them, do not assume them:

| Precondition | How you know |
|--------------|--------------|
| Phase 2 finished | The required base is installed or the user declined a non blocking upgrade |
| Phases 3, 4, and 5 finished | Every selected item was installed or explicitly recorded as failed |
| Phase 6 finished, or correctly skipped | The reload happened, browser authorizations are done, and the verification listings were read. If nothing was installed, Phase 6 was skipped on purpose and this counts as finished |
| Phase 7 finished | `CLAUDE.md` exists on disk with the generated block in it, and the user approved that block |

If any of them is not true, go back and finish it first. Writing memories over an unfinished run is
worse than writing none, because the next session reads them as fact and stops checking.

Write the memories only once, in one pass, at the end. Do not write a memory during an earlier phase
and come back to correct it.

This phase covers only the memories about this setup. It is not a limit on memory going forward: rule L
in Section 8.5 makes ongoing memory maintenance the agent's standing job from the next session on. This
phase writes the first entries, rule L keeps them alive.

### 9.1 Where and how

Use the agent's own persistent memory directory for this machine. One fact per file, with this
frontmatter:

```markdown
---
name: <short-kebab-case-slug>
description: <one line summary, used to decide relevance during recall>
metadata:
  type: user | feedback | project | reference
---

<the fact>
```

After each file, add one pointer line to `MEMORY.md`, the index that loads every session:
`- [Title](file.md): hook`. Never put memory content in the index itself.

Use absolute dates in `DD/MM/YYYY` format, never "today" or "last week". Read the machine's clock
before writing any date, using the command in rule E of Section 8.5. Do not infer the date from
context, and do not use the date supplied in the model's own context.

Write the memory content in the user's language, keeping tool names, paths, and commands in English.

### 9.2 What to write

Write these, skipping any that does not apply:

| File | Type | Content |
|------|------|---------|
| `reference-claude-code-setup.md` | reference | Date of the setup, operating system, and the base components with their installed versions. What was already present versus what this run installed. |
| `reference-mcp-inventory.md` | reference | Every MCP server installed, what each one is for in one line, which ones needed a key or a browser login, and which account or project each one points at. For item 4, also the Google account used, which APIs were enabled, and the path of the credentials file, so a later session extends the setup instead of rebuilding it. Never write a credential into a memory, only which service and which account. |
| `reference-skills-inventory.md` | reference | Every skill and plugin installed, and when each one should be used. |
| `reference-optional-tools.md` | reference | Optional tools installed, where they live on disk, and the command that runs each one. |
| `user-language-preference.md` | user | The user's language, that diacritics are mandatory, and that dashes are forbidden. |
| `reference-file-save-location.md` | reference | Default save location is the user's Desktop, subfolders allowed as long as they sit on the Desktop, and the resolved Desktop path for this machine, since Windows often redirects it into OneDrive. |
| `reference-claude-md-rules.md` | reference | That a global `CLAUDE.md` exists at `~/.claude/CLAUDE.md`, that the generated part lives between the `prodman-setup-for-claude-code` markers, and that anything outside those markers belongs to the user. |
| `reference-pending-setup-items.md` | project | Anything that failed or is waiting on a reboot, a credential, or an authorization, with what is needed to finish it. Only write this file if something is actually pending. |

Do not write a memory for something the repository or the configuration already records on its own,
and do not write one for anything that only mattered inside this conversation.

### 9.3 Delete the checkpoint

Delete `$HOME/.prodman-setup/state.md` if it exists. Everything in it that still matters is in the
memories now, and leaving it behind makes a future session believe a setup was left half finished.

### 9.4 Close out

Tell the user, in three or four lines: what got installed, what is still pending, that a global
`CLAUDE.md` now exists and where it is, and that the memories are written. Then stop. No celebration
screen, no ASCII art, no recap of the whole run.

---

## Appendix. Troubleshooting

**A command installed successfully but is not found.**
The open shell still has the old PATH. On Windows, apply the refresh in Section 3.1. If it is still
missing, the shell has to be reopened.

**`npx` hangs or fails on Windows.**
The `stdio` MCP servers need the `cmd /c` prefix on Windows. Check the entry with
`claude mcp get <name>` and re add it with the prefix if it is absent.

**An MCP server shows up but has no tools.**
It is waiting on authorization. Run `/mcp`, select it, and complete the browser login.

**`docker info` fails.**
Docker Desktop is installed but not running, or it needs WSL 2 enabled, or it needs a reboot. Start
it, wait, and if it asks for a reboot, tell the user and continue with everything that does not need
Docker.

**A skill was cloned but the agent does not see it.**
Standalone skills are discovered at startup. Reload the window or restart `claude`. Confirm the
folder sits directly under `~/.claude/skills/` and contains a `SKILL.md`.

**Antivirus blocks an install on Windows.**
Do not pipe a remote script into the shell. Download the installer to disk, then run the file. If the
antivirus still blocks it, tell the user which file was blocked and let them decide, do not work
around their antivirus.

**The user came back from a restart and the conversation lost the thread.**
Read `$HOME/.prodman-setup/state.md`. It holds the phase, the user's selection, what is done, and
the next action. State where the run is in one line and continue. Never ask for the selection again, and
never re audit the machine.

**A plugin name does not resolve.**
Marketplace names come from the repository's own manifest and do not always match the repository
name. Run `claude plugin list` after adding the marketplace, read the real name, and install with
that.
