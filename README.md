<p align="center">
  <img src="banner.svg" alt="Deelraj, a Siri-style voice assistant for Gmail and Google Calendar" width="100%">
</p>

<p align="center">
  Say <b>“Hello Deelraj”</b> and ask about your day, search your email, or have him draft
  an email or schedule an event. He always asks before sending or creating anything.
</p>

<img src="divider.svg" alt="" width="100%">

## What he does

| Feature | What it does |
|---|---|
| **Wake phrase** | Turn on Hands-free and say “Hello Deelraj”, or just tap the glow |
| **Siri-style screen** | A black screen with a glowing wave that reacts to listening, thinking and speaking |
| **Calendar and email** | Check your schedule, search and read email, all by voice |
| **Confirmation first** | Emails and events are only sent or created after you say yes |
| **Natural replies** | Short, friendly answers, and follow-up questions work |

<img src="divider.svg" alt="" width="100%">

## Set it up

**1. Google (one time)**

1. Create a project at https://console.cloud.google.com
2. Enable the **Gmail API** and the **Google Calendar API**
3. Set up the OAuth consent screen (External) and add yourself as a test user
4. Create an **OAuth client ID** of type Web application with this redirect URI:
   `http://localhost:5000/oauth2callback`
5. Download the JSON and save it in this folder as `credentials.json`

**2. Run it**

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # then add your Anthropic API key and timezone
python app.py
```

Open http://localhost:5000 in Chrome, Edge or Safari, click **Connect Google account**,
then tap the glow or switch on **Hands-free** and say “Hello Deelraj”.

<img src="divider.svg" alt="" width="100%">

## Talking to Deelraj

- Say “Hello Deelraj”, wait for his greeting, then ask
- Or all at once: “Hello Deelraj, what’s on my calendar tomorrow?”
- After he answers he keeps listening briefly, so you can ask a follow-up
- When he drafts an email or event, say “yes” or “cancel”, or use the buttons
- You can also type a request in the box at the bottom

## How it works

- `static/index.html` is the interface. It uses the browser’s speech recognition and
  speech synthesis, and draws the animated wave on a canvas.
- `app.py` is a Flask server. It sends your words to Claude with tools for Gmail and
  Calendar, runs the tools Claude asks for, and returns a short reply to speak.

<img src="divider.svg" alt="" width="100%">

## Safety

- Sending an email or creating an event needs your confirmation. The model cannot
  confirm for itself.
- Minimal Google permissions: read mail, send mail, edit calendar events
- Email text is treated as untrusted and never followed as instructions
- Runs only on your own machine (127.0.0.1)

**Never commit `.env`, `credentials.json` or `token.json`.** They hold your API key and
Google access. The included `.gitignore` already excludes them.

## Notes

- With Hands-free on, your browser listens continuously for the wake phrase and its speech
  service processes that audio. Turn it off when you don’t need it.
- Voice input works in Chrome, Edge and Safari.
- Google may show an “unverified app” warning because this is your own test project.
- GitHub Pages cannot host this project, because it needs the Python server.
