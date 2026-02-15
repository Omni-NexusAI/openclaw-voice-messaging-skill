# Voice Messaging Skill

**Purpose**: Enable voice message communication through Telegram/Discord with local STT/TTS

**Features**:
- Receive voice messages → transcribe → process → respond with voice
- Modular provider swapping (STT: Whisper, OpenAI, Google; TTS: Kokoro, Qwen3, ElevenLabs)
- Automatic audio format conversion
- Platform-specific output formats

---

## Quick Start

### Step 1: Install Dependencies

```bash
pip install faster-whisper requests pyav
```

### Step 2: Configure Providers

Edit `config.toml`:

```toml
[stt]
provider = "faster-whisper"  # Options: faster-whisper, openai, google
model = "base"
device = "cpu"

[tts]
provider = "kokoro"  # Options: kokoro, qwen3, openai, elevenlabs
base_url = "http://localhost:8880/v1"
voice = "af_bella"
```

### Step 3: Install Skill

```bash
# Copy to OpenClaw skills directory
cp -r voice-messaging-skill ~/.openclaw/skills/

# Install
openclaw skills install ~/.openclaw/skills/voice-messaging-skill
```

### Step 4: Test

```python
from voice_messaging.src.voice_handler import VoiceHandler

handler = VoiceHandler.from_config("config.toml")

# Test STT
text = handler.transcribe("test.wav")
print(f"Transcribed: {text}")

# Test TTS
handler.synthesize("Hello world!", "output.ogg")
print("✓ TTS test passed")
```

---

## Supported Providers

### STT (Speech-to-Text)
- **faster-whisper**: Local, fast (default)
- **openai**: OpenAI Whisper API
- **google**: Google Cloud Speech-to-Text

### TTS (Text-to-Speech)
- **kokoro**: Local, fast (default)
- **qwen3**: Local, customizable
- **openai**: OpenAI TTS API
- **elevenlabs**: ElevenLabs API

---

## Usage in OpenClaw

When you receive a voice message, the skill automatically:
1. Transcribes the audio (using configured STT)
2. Processes the text (your agent's normal processing)
3. Generates voice response (using configured TTS)
4. Sends back the audio file

No additional code needed - it works automatically!
