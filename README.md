# Deelraj

A Siri-style voice assistant for Gmail and Google Calendar, powered by Claude.
Say "Hello Deelraj" and ask about your day, search your email, or have him draft an
email or schedule an event. He always asks before sending or creating anything.

## Features

- **Wake phrase:** turn on Hands-free and say "Hello Deelraj", or just tap the glow
- **Siri-style interface:** a black screen with an animated, colorful wave that reacts
  to listening, thinking and speaking
- **Calendar and email by voice:** check your schedule, search and read email
- **Confirmation first:** emails and events are only sent or created after you say yes
- **Conversational replies:** short, natural answers, with follow-up questions understood

## Setup

### 1. Google (one time)

1. Create a project at https://console.cloud.google.com
2. Enable the **Gmail API** and the **Google Calendar API**
3. Set up the OAuth consent screen (External) and add yourself as a test user
4. Create an **OAuth client ID** of type Web application, with this redirect URI:
   `http://localhost:5000/oauth2callback`
5. Download the JSON file and save it in this folder as `credentials.json`

### 2. Run it

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # then add your Anthropic API key and timezone
python app.py
```

Open http://localhost:5000 in Chrome, Edge or Safari, click **Connect Google account**,
then tap the glow or switch on **Hands-free** and say "Hello Deelraj".

## Using Deelraj

- Say "Hello Deelraj", wait for his greeting, then ask your question
- Or say it all at once: "Hello Deelraj, what's on my calendar tomorrow?"
- After he answers he keeps listening briefly, so you can ask a follow-up
- When he drafts an email or event, say "yes" or "cancel", or use the buttons
- You can also type a request in the box at the bottom

## How it works

- `static/index.html`: the interface. It uses the browser's speech recognition and
  speech synthesis, and draws the animated wave on a canvas.
- `app.py`: a Flask server. It sends your words to Claude with tools for Gmail and
  Calendar, runs the tools Claude asks for, and returns a short reply to speak.

## Safety

- Sending an email or creating an event needs your confirmation. The model cannot
  confirm for itself.
- Minimal Google permissions: read mail, send mail, edit calendar events
- Email text is treated as untrusted and never followed as instructions
- Runs only on your own machine (127.0.0.1)

## Notes

- With Hands-free on, your browser listens continuously for the wake phrase, and its
  speech service processes that audio. Turn it off when you don't need it.
- Voice input and hands-free work in Chrome, Edge and Safari.
- Google may show an "unverified app" warning because this is your own test project.
  That is expected.
- GitHub Pages cannot host this project, because it needs the Python server.

## Security

Never commit `.env`, `credentials.json` or `token.json`. They contain your API key and
Google access. The included `.gitignore` already excludes them.
