# Velora implementation handoff — October 24, 2026

## Source and current status

Input: the user-supplied October 23, 11:50:53 PM architecture studio ZIP. Original archives and Desktop images were not modified. The previous artifact was a React/Express simulator containing illustrative Kotlin/C# snippets, not native application projects. This release preserves the useful sandbox and fixes its execution paths while adding the conversation portrait.

The active app, API paths, module names, text prefix, storage keys and sample voice use Velora. Native blueprint identifiers have also been renamed. Existing old-name browser preferences are intentionally separate from this release.

## Repairs

1. Removed the public companion-token getter and unauthenticated action paths. Local session checks cover administrative routes as well as commands. Host, Origin and cross-site checks are enforced.
2. Replaced self-authentication with server-verified local sessions. Client-provided screen-state overrides cannot bypass server policy.
3. Bound approvals to immutable action arguments and session identity. Approvals expire, cannot be replayed, and are invalidated by cancellation or relevant policy changes. A boolean alone cannot approve an action.
4. Required timestamps/nonces, rejected unknown action arguments, retained explicit timer durations, and made DND/torch settings idempotent. Unsupported phone volume, unlock, alarms and unknown compound steps do not silently target another device.
5. Connected chat confirmation, direct action buttons and routines to the same execution flow. Routines record each result and stop after cancellation or denial. Gated compound commands execute no steps and request separate commands.
6. Removed fake initial conversation and misleading implemented-voice claims. Vision is synthetic only. Optional OpenAI failures are explicit; model text is never treated as an action plan.
7. Repaired the dependency lock and removed unused Gemini/OpenAI SDK packages and the conflicting direct esbuild dependency. Build/runtime are Node 22.12+.
8. Added the user artwork, audio-reactive mouth selection, emotes, device speech, local audio preview, portrait selection, saved appearance settings, a stop control and reduced-motion handling.

## Verification

- TypeScript check passes (`npm run lint`).
- 21 automated tests pass: parser semantics, parameter bounds, session/CSRF/Host checks, replay rejection, immutable session-bound approvals, cancellation/revocation, end-to-end simulated phone state, locked-screen policy, compound gating, synthetic vision, explicit provider failures, bounded chat history and mouth energy thresholds.
- Vite production build passes.
- Browser verified: portrait framing and new name, sample audio enters speaking state, Stop resets it, emote selection, DND command, chat lock approval, and cancellation during the Morning Sync routine skips its later action.
- Real paid OpenAI calls, Android installation, physical device actions, microphone capture and real companion pairing have not been tested because those integrations are not part of this local prototype.

## Suggested next milestones

1. **Native Android shell:** Create a real Kotlin/Compose Gradle project for the S25 Ultra. Port the portrait stage, settings and transcript, beginning with an explicit push-to-talk button and interruptible audio playback. Produce and test an installable debug APK.
2. **Voice path:** Choose and verify current speech input/output APIs. Drive lip animation from the actual playback clock and PCM amplitude; add timed visemes only when the selected voice path supplies reliable timing. Handle interruptions, headphones, lifecycle changes and playback errors.
3. **Avatar refinement:** Create transparent, perfectly aligned character layers and distinct mouth poses if desired. Add blink frames and emotion-specific talking frames without changing face placement. Keep a still-portrait fallback.
4. **Phone actions:** Implement each approved action behind real Android permission and result handling. Test actual behavior on the phone; a simulated success must never be promoted to native success.
5. **PC companion:** Build a Windows user-session agent for interactive desktop operations. Design authenticated pairing, encrypted transport, device revocation and the same payload-bound approval model before enabling remote access.
6. **Assistant activation:** Evaluate assistant-role and wake-word capabilities on the actual stock phone. Do not promise custom DSP enrollment or screen-off survival based on these illustrative blueprints.

The name Velora is provisional for product branding; no name-availability review has been performed.
