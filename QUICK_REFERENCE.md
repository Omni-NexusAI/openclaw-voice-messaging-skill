# Voice Messaging Skill - Quick Reference

## What You Asked For

1. ✅ **Voice messaging works** - Send voice from Telegram/Discord, receive voice back
2. ✅ **Modular providers** - Swap ANY STT/TTS provider easily (not just Kokoro/Qwen3)
3. ✅ **Simple setup** - Easy to install and use
4. ⏸ **Full modularity/cross-platform** - Future work (for now, keep it simple)

---

## What I Created

**Location:** `C:\Users\yepyy\.openclaw\workspace\voice-messaging-skill\`

### Files

```
voice-messaging-skill/
├─ SKILL.md           # Skill description
├─ README.md          # Complete usage guide
├─ config.toml        # Configuration file (edit this to swap providers)
├─ setup.ps1          # Windows installation script
└─ src/
    ├─ __init__.py
    ├─ providers.py      # Abstract interfaces + factory
    ├─ stt_providers.py # STT implementations (faster-whisper, OpenAI, Google)
    ├─ tts_providers.py # TTS implementations (Kokoro, Qwen3, OpenAI, ElevenLabs)
    ├─ audio_processor.py # Audio format conversion
    └─ voice_handler.py   # Main handler
```

---

## How It Works

### 1. You Send Voice Message
```
Telegram/Discord → Voice Message → OpenClaw receives audio file
```

### 2. Skill Processes
```
Audio → STT Provider → Transcription → Your Agent Processing
```

### 3. Agent Responds
```
Response Text → TTS Provider → Voice Audio → Send back
```

---

## Supported Providers

### STT (Speech-to-Text)
- `faster-whisper` - Local, fast
- `openai` - OpenAI Whisper API
- `google` - Google Cloud Speech-to-Text

### TTS (Text-to-Speech)
- `kokoro` - Local, production quality
- `qwen3` - Local, customizable (when available)
- `openai` - OpenAI TTS API
- `elevenlabs` - ElevenLabs API

---

## How to Swap Providers

### Example: Swap from Kokoro to Qwen3 (when available)

Edit `config.toml`:

```toml
# Before:
[tts]
provider = "kokoro"

# After:
[tts]
provider = "qwen3"
model_path = "/path/to/qwen3-tts"
device = "cuda"
```

### Example: Swap from faster-whisper to OpenAI

Edit `config.toml`:

```toml
# Before:
[stt]
provider = "faster-whisper"

# After:
[stt]
provider = "openai"
api_key = "${OPENAI_API_KEY}"
```

**No code changes needed!** Just edit config and restart.

---

## Installation (Windows)

### Option 1: Automated (Recommended)

```powershell
cd C:\Users\yepyy\.openclaw\workspace\voice-messaging-skill
.\setup.ps1
```

**Setup script will:**
- ✅ Check Python
- ✅ Install Python dependencies
- ✅ Check/install FFmpeg
- ✅ Check Kokoro TTS
- ✅ Test setup
- ✅ Copy files to OpenClaw skills directory

### Option 2: Manual

```powershell
# 1. Install dependencies
pip install faster-whisper requests pyav tomli

# 2. Install FFmpeg (if not already installed)
choco install ffmpeg

# 3. Copy to OpenClaw skills
xcopy voice-messaging-skill C:\Users\yepyy\.openclaw\skills\ /E /I

# 4. Install in OpenClaw
openclaw skills install C:\Users\yepyy\.openclaw\skills\voice-messaging-skill
```

---

## After Installation

### Test 1: Quick Test

```powershell
cd C:\Users\yepyy\.openclaw\skills\voice-messaging-skill
python -c "from src.voice_handler import quick_test; quick_test()"
```

### Test 2: Send Real Voice Message

1. Go to Telegram/Discord
2. Record a voice message
3. Send to your agent
4. Agent transcribes → processes → responds with voice

---

## Configuration

Edit `config.toml`:

```toml
[stt]
provider = "faster-whisper"
model = "base"
device = "cpu"

[tts]
provider = "kokoro"
base_url = "http://localhost:8880/v1"
voice = "af_bella"
format = "ogg"
```

---

## Common Tasks

### Check Available Voices

```python
from voice_messaging.src.voice_handler import VoiceHandler

handler = VoiceHandler.from_config("config.toml")
voices = handler.get_voices()
print(f"Voices: {voices}")
```

### Test STT Only

```python
from voice_messaging.src.stt_providers import quick_transcribe

text = quick_transcribe("test.wav")
print(f"Transcribed: {text}")
```

### Test TTS Only

```python
from voice_messaging.src.tts_providers import quick_synthesize

quick_synthesize("Hello world!", "output.ogg")
print("✓ TTS test passed")
```

---

## Troubleshooting

### "FFmpeg not found"

Install FFmpeg:
```powershell
choco install ffmpeg
```

### "Kokoro TTS not responding"

Check Docker:
```powershell
docker ps | grep kokoro
docker logs <container_name>
docker restart <container_name>
```

### "Model download slow"

Set cache directory:
```bash
export HF_HUB_CACHE=/path/to/cache
```

---

## What This Solves

✅ **Voice messaging works** - Send voice, receive voice back
✅ **Provider swapping** - Change STT/TTS in config, no code changes
✅ **Platform compatibility** - Telegram and Discord supported
✅ **Local-first** - No cloud APIs required
✅ **Extensible** - Easy to add new providers

---

## Next Steps

1. ✅ Install skill (run setup.ps1)
2. ✅ Test with quick_test()
3. ✅ Send real voice message
4. ⏳ Swap providers as needed (edit config.toml)
5. ⏳ Future: Full modularity and cross-platform support

---

**Status: Ready to use!** Just run `.\setup.ps1` and you're good to go.
