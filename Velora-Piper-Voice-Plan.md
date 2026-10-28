# Piper and Velora's voice

The active upstream is [OHF-Voice/piper1-gpl](https://github.com/OHF-Voice/piper1-gpl). The original rhasspy repository is archived and directs development there.

## What customization means

1. **Choose a voice model:** select an existing voice with the desired accent, range and delivery. Different models provide different voice identities.
2. **Tune delivery:** Piper exposes length scale (speed), volume and noise controls that vary synthesis. These are not guaranteed emotional-style controls and do not turn one voice into an arbitrary different speaker. [Python synthesis controls](https://github.com/OHF-Voice/piper1-gpl/blob/main/docs/API_PYTHON.md).
3. **Train or fine-tune a voice:** provide recordings and matching transcripts, train on a computer, then export the resulting ONNX model and configuration. This is a separate data/training workflow, not instant cloning from one short clip. [Training and export](https://github.com/OHF-Voice/piper1-gpl/blob/main/docs/TRAINING.md).

## Android integration path

[Sherpa-ONNX](https://github.com/k2-fsa/sherpa-onnx/blob/master/android/README.md) provides Android TTS applications and system speech engines. Its [Android TTS catalog](https://k2-fsa.github.io/sherpa/onnx/tts/apk.html) includes converted Piper models and distinguishes standalone demo APKs from TTS engine APKs.

Velora 0.1 already uses Android's engine/voice discovery and file synthesis. The first integration should install a compatible **TTS engine** with a chosen Piper voice, select it inside Velora, and verify synthesis, speed, interruption, memory and latency on the S25 Ultra. A standalone Piper demo app will not automatically appear in the speech-engine selector.

After that test, decide whether to keep the external engine or embed a model/runtime directly for a single-app experience. Track the engine and model licenses separately; upstream Piper currently identifies its code as GPL-3.0, while Sherpa-ONNX and individual voice models have their own terms. No Piper/Sherpa implementation or voice model is included in this APK.

## Animation

Piper produces waveform audio. The existing native PCM envelope can drive the supplied mouth poses using the actual playback clock. This gives amplitude-based mouth movement. Better lip articulation needs additional aligned mouth shapes and reliable phoneme/viseme timing; audio loudness alone does not provide that.

## Remaining choices

- Audition existing voices before choosing a base voice for Velora.
- Decide whether to train a unique voice and supply an appropriate recording/transcript dataset.
- Benchmark the selected runtime/model on the actual phone before promising response latency or all-day battery behavior.

Research checked October 24, 2026. A custom voice has not yet been selected, trained or installed.
