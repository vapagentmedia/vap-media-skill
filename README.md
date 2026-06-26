<p align="center">
  <img src="assets/logo.png" alt="VAP AI Saraswati emblem" width="120">
</p>

# VAP Media API Skill for AI Assistants

Use VAP Media API from Claude Code, Codex CLI, Gemini CLI, and other skill-aware AI assistants to generate images, video, and music.

This skill stays focused on the Media API surface. It is a distribution and sales entry point for VAP media generation, not a replacement for the broader VAP AI platform.

## What It Does

- Image generation through VAP Media API
- Video generation through VAP Media API
- Music generation through VAP Media API
- Free image trial mode without an API key
- Authenticated Media API mode with a VAP product key

## Installation

### Claude Code

```bash
cp SKILL.md ~/.claude/skills/vap-media.md
```

### Codex CLI

```bash
cp SKILL.md ~/.codex/skills/vap-media.md
```

### Gemini CLI

```bash
cp SKILL.md ~/.gemini/skills/vap-media.md
```

## Quick Start

1. Install the skill in your assistant.
2. Ask for an image, video, or music generation.
3. The assistant calls VAP and returns the generated media URL.

## Free Trial vs Media API Key

| Feature | Free Trial | Media API Key |
| --- | --- | --- |
| Images | 3/day | Plan-backed access |
| Videos | Not included | Plan-backed access |
| Music | Not included | Plan-backed access |
| API key | Not required | Required |

Get a Media API key from Developer Hub:
https://vapagent.com/developer/?key=media#keys

View Room and Media plans:
https://vapagent.com/new-dashboard/?billing=monthly#plans

## Links

- VAP AI: https://vapagent.com/
- Developer Hub: https://vapagent.com/developer/
- Media API key: https://vapagent.com/developer/?key=media#keys
- Dashboard: https://vapagent.com/new-dashboard/
- GitHub: https://github.com/vapagentmedia/vap-media-skill

## License

MIT License. See LICENSE.
