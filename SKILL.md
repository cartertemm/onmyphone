---
name: onmyphone
description: Use when the session is being driven from a phone or remote-control mobile app, or when the user mentions being on their phone, on mobile, or away from their computer.
---

# OnMyPhone

## Overview

The user is driving this session from a phone. Typing is slow, the screen is small, and they cannot open a terminal, an editor, or a local file. Compensate: be autonomous, verify things yourself, and make everything reachable by tap.

## Setup (once, when the skill activates)

If the project is a web app (it has a dev-server entry point):

1. Start the dev server as a background task with hot reload.
2. Check `tailscale serve status` for existing mappings. Expose the server with `tailscale serve --bg <port>`, or if HTTPS 443 is already claimed, `tailscale serve --bg --https=8443 <port>`. Never clobber existing serve or funnel config.
3. Confirm the server responds, then post the `https://<machine>.<tailnet>.ts.net[:port]` URL as a plain link so the user can watch changes live.

Before each task report, confirm the server still responds; restart it and repost the URL if it died. If tailscale is not installed or not connected, say so in one line, note the server is running locally only, and keep working. Never post a localhost URL; the phone cannot reach it. Skip this whole section for non-web projects.

## Rules

**Commit and push after each completed task.** Verify the change works, commit with a single-line message, push. Do not ask permission and do not end a task with "say the word if you want it committed"; that costs the user a round trip, and pushing is how they review diffs in the GitHub mobile app. Branching default: substantial or risky work goes on a working branch, small fixes go straight to the default branch. An explicit user preference ("work on main", "always branch") overrides this for the session.

**Verify with evidence.** Before reporting a task done, run the relevant tests or build and exercise the change for real (hit the endpoint, load the page). Report one line of evidence (test count, HTTP status). Never ask the user to check something you can check yourself.

**Never point at the local filesystem.** The user cannot open files by path. In order of preference: inline short content directly; link pushed files via `https://github.com/<owner>/<repo>/blob/<branch>/<path>` (derived from the origin remote); if the file is not on the remote yet and is too long to inline, push first, then link. Use raw.githubusercontent.com links only for large plain-text files the blob view truncates, and only on public repos (check with `gh repo view --json visibility`); raw links 404 on private repos and never render HTML or SVG. To show rendered HTML, the tailnet-served dev server is the viewer. If the remote is not GitHub, inline the relevant excerpt.

**Phone-sized output.** Lead with the outcome in one or two sentences. No wide tables, full-file dumps, or long diffs; if code must be shown, show only the changed hunk. Summarize logs and errors down to the relevant line. Never show code and a prose walkthrough of the same code; pick whichever answers the question.

**Questions are tappable.** When input is needed, prefer a structured multiple-choice question over an open-ended one, and batch related questions into one round trip. In tools without a structured question UI, offer short lettered options answerable with one character.

**Assume dictation.** Prompts may be voice-transcribed. Resolve homophones and stray punctuation from context ("task bored" means taskboard, "question mark" may mean the character). Ask only when the meaning is truly ambiguous, not merely garbled.

**No hanging commands.** Never run anything that waits for interactive input: logins, pagers, confirmation prompts, foreground watch modes. Use non-interactive flags (`--yes`, `--no-pager`, CI mode); long-running processes go to background tasks.

## Red flags

If a reply contains any of these, fix it before sending:

- A `localhost` or `127.0.0.1` URL. Dead on a phone.
- "Let me know if you want this committed." Commit and push, then report.
- A local file path the user is expected to open.
- A question the user must answer with a typed paragraph.
