# Velora for Android — 0.4 native preview

This is an actual Kotlin Android application, with a compiled, debug-signed APK. It does not use a WebView or require the earlier browser studio to run. Intended first device: Steve's Galaxy S25 Ultra.

## Try it on the phone

Install `Velora-Android-0.4-debug.apk` over the previous build and open **Velora**. Version 0.4 is installed on Steve's S25 Ultra. Existing ChatGPT credentials are retained when updating this same debug-signed app. Use voice settings to preview the selected installed voice; **Sample voice** plays the included synthetic sample. The portrait's mouth follows audio playback and **Stop** returns it to rest. Choose an expression, or open **Settings** to select an installed speech engine, voice, speed and pitch. Pitch support depends on the selected engine.

Replies no longer scroll the screen down to the text. **Show conversation** opens the transcript when you want it; **Hide conversation** closes it. Both incoming streamed text and finished replies leave your scroll position alone.

## Hey V and floating Velora — new in 0.4

1. Tap **Hey V · floating companion → Start Hey V** while Velora is visible.
2. Allow Android's **Appear on top**, return to Velora, and allow microphone and notification permissions when requested. Notifications provide a visible Stop control.
3. Go to the Home screen or another app, keep the phone unlocked with its screen on, and say **Hey V**. Pause until the transparent head-and-torso portrait appears with **Listening…**, then say your request, for example **open Settings**.
4. Drag her portrait to move it. **Talk** listens to another request, **App** opens Velora, and **×** hides the portrait while leaving Hey V enabled. **Stop listening** in the notification turns the listener off completely. An idle portrait hides after 45 seconds.

The Vosk recognizer and English model are bundled and run on the phone. Wake monitoring and command transcription do not upload audio or retain an ambient recording. Recognized conversational text and recent overlay conversation context go to the connected ChatGPT account; recognized local phone commands use the existing review/handoff flow. Overlay conversation requires the ChatGPT-plan route; it does not silently use the separate paid API-key mode. Text sent after a mistaken wake can still be treated as conversation, so turn listening off when it is unwanted.

This preview listens only while the phone is unlocked, screen on, outside the main Velora app, and Android reports normal audio mode. Locking/screen-off/call audio pauses capture and hides the portrait; returning to the main app pauses background capture so its Talk button can operate. It resumes after those conditions end. It does not start itself after reboot or process termination; reopen Velora to enable it again. It uses a normal microphone foreground service, not low-power hardware wake detection. Battery cost, accent/noise performance, false triggers, real-call behavior and Samsung background-management behavior need device trials. Say the wake phrase separately from the request. It is not speaker authentication.

Four Desktop mouth files are included as supplied in the main app. The floating portrait uses a transparent derivative made from all four references, with no enclosing portrait circle or white panel. See `PORTRAIT-ASSETS.md` for exact source mapping. Talking animation follows audio energy; expressions do not yet have their own talking variants.

## NekoSpeak voices

NekoSpeak 1.4.2 was installed separately on Steve's S25 from the supplied ARM64 APK after its SHA-256 matched the official release digest. Open NekoSpeak to finish its setup and select/download a voice, then choose **NekoSpeak** in Velora's installed speech-engine settings and preview it. This has not yet been verified for sound quality, synthesis latency or mouth timing on the S25. It does not replace Hey V recognition.

[NekoSpeak's repository](https://github.com/siva-sub/NekoSpeak) documents support for Piper, Kokoro, Kitten and Pocket-TTS, including custom voice enrollment. This offers more voice choices than using Piper alone; it is not evidence that every model will sound better or run faster. Model downloads and licenses vary. The APK and models are not bundled into Velora.

## Phone actions — new in 0.3

Tap **Talk** or type a command. These commands run locally without ChatGPT or API billing:

| Say | What happens |
| --- | --- |
| `Open Spotify` or `Open Settings` | Matches a launchable app in your current phone profile and asks Android to open it. Similar names produce a choice; missing apps produce an explanation. |
| `Call Steve` | Looks up a matching contact, shows the number for review, then opens the dialer. You press Call there. A regular phone number also works. |
| `Text Steve saying I'm on my way!` | Preserves the message content, shows recipient and body for review, then opens a text draft. You review and send in the messaging app. |
| `Navigate to Central Park` | Confirms the destination, then opens driving directions in Google Maps or a browser. Check the location and start navigation there. |
| `Navigate home` | Uses a destination you save in Velora; asks you to set it the first time. |
| `Set a 5 minute timer` | Confirms the duration and opens the Clock timer screen. |
| `Phone actions` | Shows examples, an installed-app picker, and home-destination settings. |

The first named call or text offers an explanation before requesting Android Contacts permission. Contact matching is local; contact records and app lists are not sent to ChatGPT. Phone-number commands do not require Contacts access. Multiple matching contacts or numbers require a choice. Ordinary conversation still goes to the selected chat service, so information you include in conversational messages is not covered by the local-command behavior.

There is no direct-call or SMS-send permission. Cancelled requests do not launch actions. Home can be removed using **Phone actions → Home destination → Forget home**. The latest action result remains visible beneath the phone-actions button when you return, independently of voice and sign-in status, and survives activity recreation. It is not a persistent audit history.

This is the first phone-action layer: opening apps and handing off specific tasks. It does not yet read arbitrary app screens, tap through third-party workflows, read notifications/calendar, or run a multi-step task autonomously. The model cannot execute phone commands from its replies. The current local parser recognizes explicit command forms and common polite prefixes, not arbitrary conversational planning. Directions are a request to Maps, not a claim that navigation has started; calls and texts are prepared, not placed/sent.

## Connect your ChatGPT plan

1. Open **ChatGPT connection → Continue with ChatGPT**.
2. Sign in on the official OpenAI page in your phone's browser and review permission to use your ChatGPT plan.
3. On the return page, tap **Open Velora**. If your browser does not open the app, switch back manually. The connection screen now shows progress and the final result independently of voice status. Keep the app running during sign-in; approval received does not yet mean the connection is verified.
4. Once verification succeeds, the app selects the first available model returned by your account. **Choose available model** lets you change it. A connected account shows **Connected to ChatGPT**, not another sign-in prompt. If setup fails, the error remains on screen and survives reopening the app; use **Copy connection details** to report the failing step. These details exclude passwords, authorization codes, tokens and identity claims.
5. Tap **Talk** or type a message. **Using ChatGPT plan** identifies the selected route; **Manage usage** opens ChatGPT's usage settings.

This is an implementation of OpenAI's documented local-app sign-in flow. **Successful sign-in, identity verification and model discovery were verified on Steve's Galaxy S25 Ultra on October 24, 2026.** A completed real reply has not yet been verified. Eligibility, workspace policies, rollout and shared plan limits can still prevent access. No API credits are required to attempt this route, and failures never silently switch to API billing. Any optional ChatGPT credit spending is controlled in your ChatGPT settings.

Your existing ChatGPT chats and memories are not imported. Voice input and playback continue through Android; this connection sends text and recent local conversation context. Voice playback starts after the text reply completes.

**Talk** requests microphone permission only when pressed. Velora prefers Android's on-device recognizer when available. If only a service that may process audio online is available, the app explains that before starting. Typing remains available if speech recognition is unavailable.

For installation from the development PC, connect the phone with USB debugging enabled and approve the phone's debugging prompt. Use the exact device serial returned by `adb devices`:

```powershell
adb -s PHONE_SERIAL install -r Velora-Android-0.4-debug.apk
```

The debug APK is for testing. It has not been submitted to the Play Store and should not be described as a finished assistant.

## Implemented

- Native head-and-torso portrait with the user's four Desktop mouth assets and four emotes; separate transparent floating portrait derived from those references.
- Optional on-device Hey V listening, offline command transcription, foreground notification controls, draggable system overlay, and lifecycle pauses.
- Audio playback with mouth animation measured from PCM16 WAVE audio and synchronized to the playback position. Other playable formats fall back to a still mouth.
- Included synthetic speech sample; installed Android TTS engine and voice selection; speed, engine-dependent pitch, optional reply reading and still-mouth mode.
- Push-to-talk through Android speech recognition, permission handling and a typing fallback.
- Local app discovery and launching, contact-name or phone-number call preparation, text drafts, and directions with a saved home destination. Background lookups are cancelled when leaving Velora; review dialogs cannot carry over to a new request.
- ChatGPT plan sign-in through the system browser: loopback callback restricted to 127.0.0.1, PKCE/state/nonce checks, signed identity verification, separate account registrations, account-specific models, protected token storage, serialized renewal and sign-out/revocation. Credentials use Android Keystore encryption in app-private, non-backed-up storage.
- Responses text streaming through the official public endpoint with complete history supplied by the app, no server-side conversation storage, explicit failures and request cancellation. Only completed replies enter model history and speech playback.
- The separate paid API key mode remains explicitly selectable in Settings. Its key is memory-only; no key is packaged or logged. Activity recreation clears this API key and local chat history, but preserves the protected ChatGPT connection.
- A strict local timer command such as `set a 45 second timer` requests confirmation, then opens the phone's Clock app. The app reports the handoff, not a verified timer start.
- Stop and leaving the main app stop its own audio/microphone capture and cancel its pending chat results. The separately enabled Hey V service listens outside the app under the conditions above. Device actions are never executed from model-generated text.

## Not implemented yet

Piper itself or a custom trained voice is **not bundled**. Compatible Piper models can be exposed through an installed Android TTS engine such as Sherpa-ONNX and selected in Settings, but that combination still needs a model-specific device test. Direct in-app model download/import and training controls are future work.

Also pending: screen-off/locked low-power wake detection, custom portrait import, default-assistant-role integration, native DND/flashlight/PC control, secure companion pairing, production backend credentials, conversation persistence, expression-specific talking frames and phoneme-accurate lip sync. The main app retains the original white-background artwork; the system overlay uses the generated transparent derivative.

## Verification on October 24, 2026

- APK compilation succeeded, targeting Android API 36 and compiling with platform 36.1.
- Twenty-one unit tests cover local commands, audio-to-mouth mapping, sign-in/stream protocol, phone-command validation and confidence-gated wake matching.
- Nineteen instrumentation tests cover UI/audio/storage/sign-in, phone-action review/cancel and intents, Contacts Provider fixtures, transparent portrait rendering, real bundled-model recognition of a synthetic Hey V sample, and foreground-service lifecycle. The lifecycle test starts actual microphone capture, attaches the overlay, verifies screen-off hides it and releases capture, verifies unlock resumes, verifies entering the app releases capture, and stops the service through its actual notification action. Tests run only on the API 37 emulator; phone-state tests refuse a non-emulator device. No calls or texts are sent; outgoing action intents are captured. Contacts and credentials used by tests are fabricated.
- Android lint passed with no errors. Remaining warnings concern English strings, newer dependency versions/API targets and backup configuration. The target remains API 36 for this device-focused preview.
- The updated APK was installed on Steve's connected Galaxy S25 Ultra. A real browser approval completed token exchange, identity signature verification and model discovery; the app reported ready, and Steve confirmed **Connected and ready**.
- Version 0.3 was installed over the connected build on the S25. Entering `open Settings` through Velora's actual composer opened `com.android.settings/.Settings`. Returning to Velora showed its action result and the existing connected ChatGPT plan. The phone-actions help dialog was visually checked on the S25. Named contacts and dialer/text/map intent details were tested on the emulator; real S25 call/text/map handoffs still need user trials.
- Version 0.4 installed successfully on the S25. After enabling the permissions and going to the Home screen, Steve confirmed that saying Hey V showed the portrait and the spoken command opened Settings. This verifies the real-device wake → overlay → local app-launch path. It is not a battery, noise or wake-accuracy benchmark.
- No paid OpenAI request or real ChatGPT inference was made during automated verification. Real streaming replies, renewal/revocation, third-party TTS engines and Clock handoff still require live-device validation.

The S25 initially reported a DNS lookup failure (`UnknownHostException`) during token exchange after browser approval. The update waits for Velora to return to the foreground before finishing sign-in and makes bounded retries for DNS failures only. A subsequent real sign-in succeeded. The precise underlying network or operating-system cause was not established. An emulator test also verifies that token exchange waits while the app is in the background.

Additional fixes include a clickable app return, accurate approval-versus-verification wording, persistent connection errors, and a connected-account screen that no longer offers the initial sign-in prompt. Signature, state, nonce and permission checks remain enforced.

## Connection behavior

Use **Add another ChatGPT account** for a different account or workspace, and **Choose saved account** to switch. Old credentials are not replaced until a new sign-in is validated. Model context is cleared when switching accounts. Signing out clears the phone's selected-account tokens and attempts to revoke the renewable session. If the network prevents revocation, the app says so; remove Velora in ChatGPT Settings → Security and login to disconnect it remotely. Account/client labels and the stable device host ID are retained for later sign-in.

The sign-in listener lasts only for one attempt and does not bind to your Wi-Fi interface. The app never reads another app's login files, browser cookies or ChatGPT passwords. No secrets are stored in the source ZIP. Clear app data/uninstall removes local account storage; disconnect in ChatGPT to revoke authorization too.

Developer references checked October 24, 2026: [registration](https://developers.openai.com/siwc/token-sharing-open-source/sign-in), [sessions](https://developers.openai.com/siwc/token-sharing-open-source/profiles-and-sessions), [models and inference](https://developers.openai.com/siwc/token-sharing-open-source/models-and-inference), [limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations).

Phone integration references checked October 24, 2026: [Android common intents](https://developer.android.com/guide/components/intents-common), [package visibility](https://developer.android.com/training/package-visibility/declaring), [Contacts Provider](https://developer.android.com/identity/providers/contacts-provider), and [Maps directions URLs](https://developers.google.com/maps/documentation/urls/get-started).

## Build from source

Open this folder as an Android project, or run `Build-Android.cmd` on Windows with JDK 17+ and Android SDK platform 36.1/build-tools 36.0.0 installed. The Gradle wrapper is included; initial dependency resolution needs internet access. This project uses AGP 9.1.1 with its built-in Kotlin support and Gradle 9.3.1.

```powershell
.\gradlew.bat assembleDebug testDebugUnitTest lintDebug
```

The APK is produced at `app/build/outputs/apk/debug/app-debug.apk`. `connectedDebugAndroidTest` runs device instrumentation tests; select only a test emulator when invoking it. It installs test builds and may remove them afterward.

See `PIPER-VOICE-PLAN.md` for the voice customization path.

See `THIRD-PARTY-NOTICES.md` and packaged `assets/licenses/` for Vosk, its bundled English model, and JNA attribution/license texts.
