# GhostViral MCP

A remote [Model Context Protocol](https://modelcontextprotocol.io) server that lets an AI agent make a finished faceless video on a [GhostViral AI](https://ghostviralai.com) account: script, licensed stock footage, voiceover, captions, sound and an MP4.

- **Endpoint:** `https://ghostviralai.com/mcp` (Streamable HTTP)
- **Auth:** OAuth 2.1 with PKCE and dynamic client registration. You sign in to GhostViral in the browser; there are no API keys to copy.
- **Registry name:** `com.ghostviralai/ghostviral` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=ghostviral)
- **Docs:** https://ghostviralai.com/docs/mcp
- **Works with:** Claude (web, desktop, Claude Code), ChatGPT, Cursor, VS Code and other MCP clients that support remote servers with OAuth.

This repository holds the public description of the server (this README and `server.json`). The server itself runs on ghostviralai.com; there is nothing to install or self-host.

## What it makes

- Vertical 9:16 Shorts, Reels and TikToks of 15, 30, 45 or 60 seconds (1080×1920)
- Horizontal 16:9 YouTube videos of 5, 8 or 12 minutes (1920×1080)
- Visuals are licensed stock video and photos (Pexels, Pixabay and open-licence archives), never AI-generated faces or clips
- Script, voice and captions in 32 languages

## Connect

**Claude:** Settings → Connectors → Add custom connector → URL `https://ghostviralai.com/mcp`, then sign in.

**Claude Code:**

```bash
claude mcp add --transport http ghostviral https://ghostviralai.com/mcp
```

**Cursor** (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "ghostviral": { "url": "https://ghostviralai.com/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`):

```json
{
  "servers": {
    "ghostviral": { "type": "http", "url": "https://ghostviralai.com/mcp" }
  }
}
```

The first tool call opens the GhostViral sign-in page. New accounts get 60 free credits, no card.

## How an agent uses it

The agent makes every creative choice; the tools carry them out.

1. `set_script`: the agent's script (or one written on request), with sources for factual topics
2. `search_stock`: licensed clips and photos for each scene, with thumbnail previews to inspect
3. `set_timeline`: the picks, one scene per sentence, with a pacing check
4. `estimate_credits`, then `synthesize_voice` (credits are charged here, once per video)
5. `set_captions`, optional `set_motion`, `search_sounds`, `set_audio`, `review_audio`
6. `start_render`, then `poll_render` and `get_download_url`

`start_render` returns a command that encodes the MP4 on the agent's machine with FFmpeg. When the agent cannot run commands, `renderMode: "server"` renders Shorts up to 60 seconds on GhostViral's server instead. After an agent render, `poll_render` reports a check of the file: length, size, loudness, and black or frozen stretches.

## Tools

| Tool | What it does |
|---|---|
| `create_project`, `get_project`, `list_projects` | Start and read video projects |
| `get_options`, `list_voices` | Formats, caption styles, voices |
| `set_script`, `plan_visuals` | Script; optional stock search ideas |
| `search_stock`, `set_timeline` | Find licensed footage, set the scene cuts |
| `estimate_credits`, `synthesize_voice` | Cost before spending; the voiceover |
| `set_captions`, `set_composition`, `set_motion`, `get_motion_guide` | Captions, layout, motion and graphics |
| `search_sounds`, `design_sound`, `set_audio`, `set_music`, `review_audio` | Music bed, effects, mix check |
| `start_render`, `submit_render`, `poll_render`, `get_download_url` | Render and download |
| `create_faceless_video` | One call for a quick Short |
| `create_schedule_plan`, `list_schedule_plans`, `update_schedule_plan`, `next_scheduled_render`, `list_social_accounts`, `schedule_post` | Scheduling on paid plans |

Prompts (`prompts/list`): `produce_faceless_short`, `pick_stock_timeline`, `choose_soundtrack`.

Every tool carries a title and read-only or destructive hints, so clients can ask before anything that spends credits.

## Example requests

- "Make a 30-second vertical Short about why octopuses have three hearts."
- "Research the history of the Titanic and make an 8-minute 16:9 YouTube video."
- "Turn this script into a 60-second TikTok with Pro Bold captions and a calm male voice."

## Pricing

Free trial: 60 credits, no card; free exports carry a watermark. Paid plans from $4.99 a week. A 30-second Short with an AI voice costs 15 credits. Details: https://ghostviralai.com/pricing

## Privacy and support

- Privacy policy: https://ghostviralai.com/privacy
- Terms: https://ghostviralai.com/terms
- Issues with the server: open an issue in this repository.
