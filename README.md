# Media URL Transcriber MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-media-url-transcriber)](https://www.npmjs.com/package/@mambalabsdev/mcp-media-url-transcriber) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-media-url-transcriber)](https://github.com/mambalabsdev/mcp-media-url-transcriber/blob/main/LICENSE)

An MCP server for the Mamba Labs Media URL Transcriber actor on Apify. Turn media file links into transcripts on your own provider key.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give direct audio or video file links and your own Deepgram, AssemblyAI, Groq, or OpenAI key. The actor sends each file to that provider and returns one row per file with the transcript, timestamped segments, language, and duration. You pay your provider directly and pay this actor per media minute. It does not download from platforms such as YouTube or TikTok.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-media-url-transcriber": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-media-url-transcriber"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `transcribe_media_urls` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Transcribe this podcast episode file on my Deepgram key."
- "Turn these 10 recorded call files into text with speaker labels."
- "Check whether these media links can be read before I transcribe them."

## Tool

`transcribe_media_urls`. One tool. Example arguments:

```json
{
  "media_urls": [
    "https://dpgr.am/spacewalk.wav"
  ],
  "provider": "deepgram"
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `media_urls` | array | no | Direct links to audio or video files (mp3, wav, m4a, mp4, and others) that answer without a login. Platform pages such as YouTube, TikTok, Instagram, Vimeo, or Spotify are refused with a free row. In Clay, put the column inside a one item list. |
| `provider` | string | no | Whose API transcribes the file, on your key: deepgram (default), assemblyai, groq, or openai. openai takes files up to 25 MB; the others read the URL. |
| `api_key` | string | no | Your own key for the provider above. It is stored encrypted by Apify, sent only to that provider, and never written to a row or a log. With no key the run checks up to 3 files and transcribes nothing, for free. |
| `model` | string | no | Optional. The provider's model name. Defaults: deepgram nova-3, assemblyai its own default, groq whisper-large-v3-turbo, openai whisper-1 (the only one accepted there). |
| `language` | string | no | Optional. A language code such as en or de. Empty lets deepgram and assemblyai detect it; whisper models detect it on their own. |
| `diarize` | boolean | no | yes or no (default no). Labels who spoke each segment. deepgram and assemblyai only; your provider may bill it as an add on. |
| `include_segments` | boolean | no | yes (default) or no. Adds a list of segments with start and end seconds to each row. |
| `max_charge_usd` | string | no | Optional. No new file starts once this is used up. A transcript your provider already billed is still delivered, charged up to this amount, and the run then stops. It limits this actor's charge, not your provider's bill. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `media-minute`, one minute of media transcribed, rounded up per file | $0.005 | $0.00475 | $0.0045 | $0.00425 |
| `apify-actor-start`, per run start | $0.003 | $0.003 | $0.003 | $0.003 |

Refused, failed, and silent files, media checks with no key, and refused input are free. Mamba Labs pays the platform costs of the run. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/media-url-transcriber](https://apify.com/mambalabs/media-url-transcriber)

## License

MIT. See [LICENSE](LICENSE).
