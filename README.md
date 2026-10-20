# Deelraj: a voice assistant for Gmail and Google Calendar

Talk to Claude from a web page. It can check your calendar, search and read your
email, and (after you confirm) send emails and create events.

## 1. Google setup (one time, about 10 minutes)

1. Go to https://console.cloud.google.com and create a project.
2. Enable the **Gmail API** and the **Google Calendar API** (APIs & Services > Library).
3. Configure the **OAuth consent screen**: choose External, then add your own
   Google account under **Test users**.
4. Create credentials: **OAuth client ID** > application type **Web application**.
   Add this Authorized redirect URI: `http://localhost:5000/oauth2callback`
5. Download the JSON and save it in this folder as `credentials.json`.

## 2. Run it

```bash
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env        # then edit .env: add your Anthropic API key and timezone
python app.py
```

Open http://localhost:5000 in Chrome, Edge or Safari, click **Connect Google account**,
approve access, then tap the microphone.

## Talking to Deelraj

- Tap the mic and speak, or press **Turn on hands-free** and say **"Hello Deelraj"**.
  He answers with a greeting, then listens. You can also say it in one go:
  "Hello Deelraj, what's on my calendar tomorrow?"
- After he replies he keeps listening for a follow-up, so it feels like a conversation.
- With hands-free on, the browser listens continuously for the wake phrase (audio goes to
  the browser's speech service while it is on). The tab must stay open.
- For the most natural voice, use Chrome or Edge and pick up their "Natural" or "Online"
  voices (the page chooses the best one automatically).

## How it works

- `static/index.html`: mic button. Uses the browser's SpeechRecognition (speech to text)
  and speechSynthesis (text to speech).
- `app.py`: Flask server. Sends your words to Claude with tools for Gmail and Calendar,
  runs the tools it asks for, and returns a short reply to speak.

## Safety design

- **Writes need confirmation.** Claude can only *stage* an email or event. Nothing is
  sent or created until you press the button or say "yes". The model cannot confirm on its own.
- **Minimal scopes.** Read mail, send mail, edit calendar events. Nothing else.
- **Email text is treated as untrusted.** The system prompt tells Claude never to follow
  instructions found inside emails, and because writes need your confirmation, a malicious
  email cannot send anything on its own.
- Local only: binds to 127.0.0.1, and `token.json` / `.env` stay on your machine.

## Notes

- Chrome's speech recognition sends audio to Google's servers. For fully private or
  higher-accuracy transcription, swap in Whisper or Deepgram on the server.
- Google may show an "unverified app" warning because it is your own test project.
  That is expected.
- Ideas for next steps: reply to a thread, delete or move events, a wake word,
  streaming replies into text-to-speech for lower latency.

## Publishing on GitHub

Push the code to a GitHub repository (the `.gitignore` already keeps `.env`,
`credentials.json` and `token.json` out). Never commit those files.

GitHub Pages only hosts static files, so it cannot run `app.py`. To use Deelraj,
run the server yourself (locally, or on a host such as Render, Fly.io or Railway).
If you deploy it publicly, add a login in front of it: anyone who can open the page
could otherwise read your email through your Google token.
