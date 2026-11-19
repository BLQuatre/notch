# Claude Code monitor

Notch shows each Claude Code session as working, waiting for you, done or idle, and lets you answer permission requests, questions and plans right from the notch. It receives Claude Code [HTTP hooks](https://docs.claude.com/en/docs/claude-code/hooks) on `http://127.0.0.1:27210/hook`.

## Token

Every hook must send `Authorization: Bearer <token>`, otherwise Notch ignores it. Notch generates the token on first launch: copy it from **Settings › Claude › Hook token**, then either export it as `NOTCH_TOKEN` in the shell that runs Claude Code, or add it to the `env` block of `~/.claude/settings.json`:

```json
{
	"env": { "NOTCH_TOKEN": "<token>" }
}
```

## Hooks

Merge this into `~/.claude/settings.json` (inside WSL if Claude Code runs there). If you already have hooks for the same events, add this entry to their array instead of replacing them.

```json
{
	"hooks": {
		"SessionStart": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		],
		"UserPromptSubmit": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		],
		"PreToolUse": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		],
		"PostToolUse": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		],
		"PostToolUseFailure": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		],
		"PermissionRequest": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 310
					}
				]
			}
		],
		"Notification": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		],
		"Stop": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		],
		"SessionEnd": [
			{
				"hooks": [
					{
						"type": "http",
						"url": "http://127.0.0.1:27210/hook",
						"headers": { "Authorization": "Bearer $NOTCH_TOKEN" },
						"allowedEnvVars": ["NOTCH_TOKEN"],
						"timeout": 2
					}
				]
			}
		]
	}
}
```

If you change **Settings › Claude › Hook port**, update the URLs above too. Notch switches to the new port without restarting.

## Notes

- WSL must use `networkingMode=mirrored` in `%UserProfile%\.wslconfig` so that `127.0.0.1` reaches Windows.
- When Notch isn't running, the connection is refused immediately and Claude Code carries on.
- A permission request is held for up to 5 minutes while the notch shows an **Allow / Always / Deny** card (or the questions Claude asks, or the plan to approve), so the `PermissionRequest` timeout must stay above that. The terminal prompt stays usable meanwhile: whichever answers first wins. **Always** applies the rule Claude Code suggests for that tool.
- **Sound on each permission request** plays the Windows notification sound when a session needs you.
- Turning off the Claude Code module (**Settings › Modules**) hides the monitor and leaves permission requests to the terminal.
- Done sessions turn idle once you have opened and closed the notch.
- Interrupting Claude (Esc) sends no hook: a session stuck on "working" turns idle after 10 minutes without events, and any session disappears after 2 hours without events.
