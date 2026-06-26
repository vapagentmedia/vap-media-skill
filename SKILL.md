---
name: vap-media
description: VAP Media API skill for image, video, and music generation through VAP. Supports free image trial mode and authenticated Media API key mode.
homepage: https://vapagent.com/developer/
---

# VAP Media API Skill

Generate images, video, and music through VAP Media API. Use this skill when a user asks an AI assistant to create media assets from natural-language prompts.

## Product Boundary

This skill is for the VAP Media API surface. Do not route coding-model requests through this skill. Coding agents and IDE model workflows use the separate VAP Coding Plan API with model ID `vap-code`.

## Mode Selection

Check if `VAP_API_KEY` is set:

- `VAP_API_KEY` not set -> use Free Trial mode for image generation only.
- `VAP_API_KEY` set -> use authenticated Media API mode for image, video, and music.

Get a Media API key from Developer Hub:
https://vapagent.com/developer/?key=media#keys

View Room and Media plans:
https://vapagent.com/new-dashboard/?billing=monthly#plans

---

## Free Trial Mode (No API Key)

Free Trial mode supports image generation without signup. Limit: 3 image generations per day per IP.

### Generate Image

```bash
curl -s -X POST https://api.vapagent.com/v3/trial/generate \
  -H "Content-Type: application/json" \
  -d '{"description":"a serene mountain lake at sunset"}'
```

Returns `{"task_id":"UUID","status":"pending","remaining":2}`.

### Poll Result

```bash
curl -s https://api.vapagent.com/v3/trial/status/TASK_ID
```

Returns `{"status":"completed","image_url":"https://..."}` when done.

### Free Trial Errors

- 429 -> Daily limit reached. Tell the user to get a Media API key from https://vapagent.com/developer/?key=media#keys
- 503 -> Trial service temporarily unavailable.

---

## Authenticated Media API Mode

Authenticated mode uses `VAP_API_KEY` as a Bearer token and supports image, video, and music generation. The key is product-scoped to VAP Media API access.

### Create Task

```bash
curl -s -X POST https://api.vapagent.com/v3/tasks \
  -H "Authorization: Bearer $VAP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "image",
    "description": "a futuristic cityscape at night",
    "aspect_ratio": "16:9"
  }'
```

Returns `{"task_id":"UUID","status":"pending","estimated_cost":"0.1800"}`.

### Poll Result

```bash
curl -s https://api.vapagent.com/v3/tasks/TASK_ID \
  -H "Authorization: Bearer $VAP_API_KEY"
```

Returns `{"status":"completed","result":{"output_url":"https://..."}}` when done.

### Task Types

| Type | Public surface | Params |
| --- | --- | --- |
| image | Aura Image Turbo | description, aspect_ratio |
| video | Pimo AI-Video | description, duration, aspect_ratio, generate_audio |
| music | Pira V5.5 | description, duration, instrumental |

### Authenticated Errors

- 401 -> Invalid API key. Ask the user to check their Media API key in Developer Hub.
- 402 -> Plan or balance access is insufficient. Send the user to https://vapagent.com/new-dashboard/?billing=monthly#plans

---

## Assistant Instructions

When a user asks to create, generate, or make an image, video, or music:

1. Improve the prompt with useful style, lighting, composition, mood, or production details.
2. Check mode by detecting whether `VAP_API_KEY` is set.
3. Use Free Trial mode only for image requests when no key is available.
4. Use authenticated Media API mode for image, video, and music when `VAP_API_KEY` is available.
5. Poll until the task is completed or failed.
6. Return the final media URL and concise status to the user.

If the free trial limit is hit, say: `You have used the free image trial for today. Get a VAP Media API key at https://vapagent.com/developer/?key=media#keys`

Do not expose internal provider details as the product identity. The visible product is VAP Media API.

---

## Examples

### Free Trial Image

```bash
curl -s -X POST https://api.vapagent.com/v3/trial/generate \
  -H "Content-Type: application/json" \
  -d '{"description":"a golden retriever puppy playing in a field"}'

curl -s https://api.vapagent.com/v3/trial/status/TASK_ID
```

### Authenticated Image

```bash
curl -s -X POST https://api.vapagent.com/v3/tasks \
  -H "Authorization: Bearer $VAP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"type":"image","description":"cyberpunk street at night, neon lights","aspect_ratio":"16:9"}'
```

### Authenticated Video

```bash
curl -s -X POST https://api.vapagent.com/v3/tasks \
  -H "Authorization: Bearer $VAP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"type":"video","description":"ocean waves crashing on beach","duration":6,"generate_audio":true}'
```

### Authenticated Music

```bash
curl -s -X POST https://api.vapagent.com/v3/tasks \
  -H "Authorization: Bearer $VAP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"type":"music","description":"upbeat electronic dance music","duration":120,"instrumental":false}'
```

---

## Prompt Tips

- Style: photograph, cinematic, 3D render, watercolor, flat illustration
- Lighting: golden hour, neon lights, soft diffused light, dramatic shadows
- Composition: close-up, aerial view, wide angle, centered subject
- Mood: serene, energetic, mysterious, premium, playful

---

## Links

- Developer Hub: https://vapagent.com/developer/
- Media API key: https://vapagent.com/developer/?key=media#keys
- Room and Media plans: https://vapagent.com/new-dashboard/?billing=monthly#plans
- VAP AI: https://vapagent.com/
- GitHub: https://github.com/vapagentmedia/vap-media-skill
