# Velora — Conversation Studio

Local working preview for Steve: a large head-and-torso conversation portrait, selectable expressions, a synthetic voice preview, and a repaired device-action sandbox. Based on the supplied architecture studio ZIP, now renamed Velora.

**This is a React/Express web prototype. There is no installable Android APK or Windows companion executable yet.**

## Start on Windows

1. Extract the entire ZIP to a folder.
2. Double-click `Start-Velora.cmd`. It installs locked dependencies on first run, builds the interface, and starts the local server.
3. Open http://127.0.0.1:3000. Leave the terminal open; Ctrl+C stops it.

Requires Node.js 22.12 or newer and npm. The launcher can also use the bundled Codex Node runtime when it exists. Internet is needed for the first dependency installation; a configured API key is not required for the local demo. The preview already opened in this chat uses port 4317.

Manual setup:

```powershell
npm ci --ignore-scripts
npm run build
$env:NODE_ENV = 'production'
npm start
```

Development: `npm run dev`. Checks: `npm run lint`, `npm test`, `npm run build`.

## What works

- Open head-and-torso portrait, expression picker, larger view, and a compact portrait shortcut on other screens.
- Local WAV preview with mouth frames driven by the audio actually being played. Stop and end reset the face. User-selected audio stays in the browser.
- Selectable device speech voices, speech speed, optional read-aloud for replies, and motion toggle. Device speech uses estimated mouth timing because this browser speech API does not supply the output samples to the analyser.
- A custom still portrait can be selected for the current session. Expression, voice choice and animation settings persist in this browser.
- Text commands with validated parameters, server-held simulated device state, single-use approval, cancellation and routine execution that stops after failure or cancellation.
- Optional OpenAI text conversation through the server. Only recent conversation context is sent; no avatar image or local audio is uploaded. Model text never executes actions.
- Synthetic vision examples and explicitly incomplete native blueprints.

## Try it

Choose **Try her voice**, select an emote, then try:

- `Velora, set a 45 second timer`
- `Enable Do Not Disturb` followed by `Disable Do Not Disturb`
- `Turn on flashlight` followed by `Turn off flashlight`
- `Set my PC volume to 25 percent`
- `Lock my PC` → review and confirm the simulated action
- `Do not lock my PC` → no execution

Timers and device controls only update this sandbox; they do not control the phone or PC. Microphone capture, live camera analysis, a system overlay, real phone pairing and native hardware wake words are not implemented.

## Optional OpenAI chat

Copy `.env.example` to `.env` and set `OPENAI_API_KEY` locally. `OPENAI_MODEL` defaults to the inherited `gpt-4o-mini` setting and can be changed to a compatible Chat Completions model available to your account. Restart the server afterward. The key stays on the server and must never be added to client code or shared ZIPs. API availability and billing depend on your account. No paid API request was made during this build; provider behavior was tested with mocked responses.

## Avatar assets and limits

Original JPG artwork is preserved. CSS provides the head-and-torso framing and lower fade; the white background has not been removed. The floating shortcut is inside the web page, not a desktop or Android overlay.

The supplied rest / open / wider-open files give **three distinct mouth poses**. Four amplitude slots remain in the component, with slots 1 and 2 sharing the same open image. Speech is an amplitude animation, not phoneme-accurate lip sync. Emotes display while idle and return after speech; their own mouth sets, blinking and head tracking are future work.

Image names are in `public/avatar/`. The included WAV was synthesized locally using a Windows device voice and now introduces Velora. The artwork was supplied by the user; this package does not establish third-party redistribution rights. Velora is the chosen working product name; availability has not been checked.

## Security boundary

The server binds only to loopback. API routes require a same-origin local browser session, an HttpOnly SameSite cookie, and a CSRF header. Action requests require a timestamp and nonce. Pending approvals are bound to the session, action and original arguments, expire after 60 seconds, and can be consumed once. Policy changes and simulated pairing changes invalidate pending approvals.

This protects a **single-user local console**, not a remote companion. Any process running as the local user can create a local session. Do not expose this server through a public tunnel or LAN port. Production pairing, encrypted remote transport, persistent device identity and native OS authorization remain to be designed.

Preferences and routines use browser storage. Conversations are held in memory and reset on reload. Simulated device state, approvals and audit history reset when the server restarts.

See `HANDOFF.md` for implementation changes, verification and the next development steps.
