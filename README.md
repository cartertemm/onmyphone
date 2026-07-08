# OnMyPhone

OnMyPhone is a dead simple, highly opinionated skill that makes it easier to code on the go with remote control sessions in Claude Code and Codex.

## Assumptions

1. If a project is a web app, I want the preview reachable from my phone, so the agent should start the dev server and expose it through Tailscale when available. This way all you have to do is click a link and switch back to your browser, refreshing when needed.
2. If work is finished, I want evidence, and I don't want to spend a whole lot of time debugging from a small screen while I'm doing life stuff. So the agent should verify the change before reporting back.
3. If a task is complete, I may want to review it from GitHub mobile, so the agent should commit and push it.
4. If I'm doing something minor, i.e. fixing typos, pushing to main is fine.
5. If I am reading on a phone, long logs and wide diffs are a pain, so replies should stay short and tap friendly.
6. If I am using dictation, weird wording is normal, so the agent should resolve obvious transcription mistakes.

## Install

Clone this repo into your agent's skills directory.

Codex on macOS or Linux:

```sh
git clone https://github.com/cartertemm/onmyphone.git ~/.codex/skills/onmyphone
```

Codex on Windows PowerShell:

```powershell
git clone https://github.com/cartertemm/onmyphone.git $env:USERPROFILE\.codex\skills\onmyphone
```

Claude Code on macOS or Linux:

```sh
git clone https://github.com/cartertemm/onmyphone.git ~/.claude/skills/onmyphone
```

You can then activate the skill in any CLI session by typing /onmyphone, or by simply telling the agent that you're working from your phone.

## Security and judgment

[Remote control comes with security limitations](https://thehackernews.com/2026/02/claude-code-flaws-allow-remote-code.html), especially in enterprise environments.
If you don't configure this correctly, or if you give the agent more access than it needs, you might be trading convenience for command execution or credential exposure. This has nothing to do with this particular skill and everything to do with the nature of remote control sessions.

Be careful. It is up to you to understand the limitations and work around them based on what you are comfortable with.

At a minimum, never run a remote control session without first enabling multi-factor authentication on your Anthropic and/or OpenAI account.

Also, though it shouldn't need saying, coding from mobile is not smart or realistic when the code has production implications.

I keep all of my "vibed" projects, those built mostly through agentic sessions where I'm not reviewing generated code line by line, in a dedicated repo so people know what they are getting into if they want to use them or test them out. I believe there is importance in clear separation between trusting me, and trusting my AI. This has worked well so far.

## License

Public domain. See [LICENSE](LICENSE).
