# Video Summarizer for YouTube

[العربية](README.md) | [English](README.en.md)

A Chrome/Edge extension that summarizes YouTube videos using AI. It fetches subtitles through a local backend, generates summaries in the language you choose, and works with OpenRouter, OpenAI, or local OpenAI-compatible models.

Important:
- The video must have subtitles available on YouTube, including auto-generated subtitles.
- Videos longer than 3 hours are not supported.

## Features

- Reliable subtitle extraction through a local `yt-dlp` backend
- AI summaries in multiple languages
- Default summary language: `العربية (Arabic)`
- Resizable in-page summary popup
- Font size controls inside the popup
- Localized popup UI for supported summary languages
- Settings page in English and Arabic
- Works with OpenRouter, OpenAI, Ollama, LM Studio, LocalAI, and other OpenAI-compatible endpoints
- Direct-to-provider API calls with no relay server

## Install the Extension

### Chrome Web Store
1. Open the Chrome Web Store page for **Video Summarizer for YouTube**
2. Click **Add to Chrome**
3. Confirm the installation
4. Pin the extension if you want faster access

## Start the Backend

The extension requires the backend to be running on your machine so it can fetch subtitles.

### Option A: Download the backend binary

Download the correct backend package for your operating system from this repository’s **Releases** page.

Available packages:
- Linux: `yt-summ-backend-vX.Y.Z-linux-x64.tar.gz`
- Windows: `yt-summ-backend-vX.Y.Z-windows-x64.zip`
- macOS (Apple Silicon): `yt-summ-backend-vX.Y.Z-macos-arm64.tar.gz`

How to run it:
1. Download the package for your OS
2. Extract the archive
3. Run the included executable
4. Keep it running while using the extension

### macOS note

The current macOS backend build may be unsigned.

If macOS blocks it:
1. Open **System Settings** → **Privacy & Security**
2. Find the warning for the downloaded app
3. Click **Open Anyway**
4. Confirm the second prompt

### Option B: Docker

If you prefer Docker, run:

```bash
docker pull ghcr.io/abdulazizalmalki-gh/video-summarizer-for-yt-backend:latest
docker run --name yt-summ-backend --rm -p 8765:8765 ghcr.io/abdulazizalmalki-gh/video-summarizer-for-yt-backend:latest
```

Optional detached mode:

```bash
docker run -d --name yt-summ-backend -p 8765:8765 ghcr.io/abdulazizalmalki-gh/video-summarizer-for-yt-backend:latest
```

Optional cookies mount for fewer YouTube `429` subtitle errors:

```bash
docker run --name yt-summ-backend --rm \
  -p 8765:8765 \
  -v /absolute/path/to/cookies.txt:/app/cookies.txt:ro \
  ghcr.io/abdulazizalmalki-gh/video-summarizer-for-yt-backend:latest
```

Health check:

```bash
curl http://localhost:8765/health
```

## Configure the Extension

1. Click the extension icon
2. Open **Settings**
3. Leave the backend URL as:
   - URL: `http://localhost`
   - Port: `8765`
4. Click **Test Connection**
5. Choose your AI provider
6. Enter the required API key or local endpoint
7. Save settings

## AI Providers

### OpenRouter

Recommended default hosted provider.

Steps:
1. Go to [https://openrouter.ai/keys](https://openrouter.ai/keys)
2. Sign in or create an account
3. Create an API key
4. Copy the key
5. In extension Settings:
   - choose **OpenRouter**
   - paste the key
   - optionally click **Refresh** and choose a model
   - click **Save Settings**

### OpenAI

Steps:
1. Go to [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Sign in or create an account
3. Make sure your API account has billing or credits
4. Create a new secret API key
5. Copy the key
6. In extension Settings:
   - choose **OpenAI**
   - paste the key
   - click **Save Settings**

### Local LLM

Supported examples:
- Ollama
- LM Studio
- LocalAI
- other OpenAI-compatible endpoints

Common endpoints:
- Ollama: `http://localhost:11434`
- LM Studio: `http://localhost:1234/v1`
- LocalAI: usually an OpenAI-compatible `/v1` endpoint

Local setup steps:
1. Start your local model server
2. In extension Settings, choose **Local LLM**
3. Enter the endpoint
4. Click **Refresh**
5. Select a model
6. Click **Save Settings**

## How to Use

1. Start the backend
2. Open a YouTube video
3. Make sure the video has subtitles and is 3 hours or shorter
4. Click the floating **Video Summarizer for YouTube** button on the page
5. The language selector defaults to **العربية (Arabic)**
6. Change the summary language if needed
7. Click **Summarize**
8. Copy the result, resize the popup, or adjust the summary font size

## Troubleshooting

### Backend not reachable

- Check `http://localhost:8765/health`
- Confirm the extension backend settings still point to `http://localhost:8765`
- If using Docker, check:

```bash
docker ps --filter "name=yt-summ-backend"
```

### No subtitles found

- The video may not have subtitles enabled
- Auto-generated captions may be unavailable on some videos
- The extension cannot summarize videos with no subtitles at all
- Videos longer than 3 hours are not supported

### 429 subtitle errors

If YouTube rate-limits subtitle downloads:
1. Wait a while before trying again
2. Avoid repeatedly summarizing many videos in a short period
3. Make sure the backend is still running, then retry later

### Provider errors

- Make sure a provider is selected in Settings
- OpenAI and OpenRouter require valid API keys
- Local LLM requires both an endpoint and a selected model
- Save settings after making changes

## Privacy

- Subtitle fetching happens through your local backend
- AI requests go directly to the provider you configure
- Provider secrets are stored locally in the browser
