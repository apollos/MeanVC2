# ComfyUI / ROCm compatibility notes

This branch records the MeanVC2 changes validated alongside the local ComfyUI
installation on 2026-09-30.  MeanVC2 is imported and run in the same Python
environment as ComfyUI; it is not exposed through a separate service.

## Saved change

`runtime/run_rt.py` accepts `--record-output PATH` in real-time mode.  Audio
written to the WAV file is the exact mono signal sent to the playback device.
Disk writes run on a background thread so the PortAudio callback is not blocked.
The file is 16 kHz, mono, 24-bit PCM WAV.

Example:

```bash
python runtime/run_rt.py \
  --mode realtime \
  --model 120ms \
  --device cuda \
  --target-spk example/s3p2.wav \
  --record-output recordings/meanvc2.wav
```

The Linux `default` or `pipewire` device is preferred.  A raw ALSA `hw:*`
device may reject MeanVC2's 16 kHz stream if the hardware does not expose that
sample rate directly.

## Related ComfyUI node

The offline and real-time control nodes live in
[`apollos/local-comfyui-nodes`](https://github.com/apollos/local-comfyui-nodes/tree/main/ComfyUI-VoiceConversion).
Model checkpoints, reference audio, generated recordings, and virtual
environments are deliberately excluded from Git.
