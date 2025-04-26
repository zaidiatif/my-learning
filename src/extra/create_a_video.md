# Step-by-Step Guide: Convert Text to YouTube Video with Open-Source AI Tools

We'll go through:

### 1. Convert Text to Speech (TTS)

### 2. Generate Video & Animation

### 3. Edit & Enhance Video

### 4. Add AI-Generated Music

### 5. Generate Subtitles

### 6. Upload & Optimize for YouTube

---

## Step 1: Convert Text to Speech (TTS)

We’ll use `Coqui TTS` (open-source) to generate AI voiceover

#### Install Coqui TTS

```bash
pip install TTS
```

#### Convert Text to Speech

```bash
TTS --text "Hello, welcome to my YouTube channel!" --out_path voiceover.wav
```

👉 This creates a voiceover.wav file, which we will add to the video.

## Step 2: Generate Video & Animation

We’ll use `Manim` (Mathematical Animation Engine) or OpenShot for video creation.

#### Install Manim (for animations)

```bash
pip install manim
```

#### Create a Simple Animation

Save this as video.py:

```python
from manim import *

class MyScene(Scene):
    def construct(self):
        text = Text("Hello, YouTube!").scale(1.5)
        self.play(Write(text))
        self.wait(2)

MyScene().render()
```

Run the script:

```bash
manim -pql video.py
```

👉 This will generate an animated text video.

## Step 3: Edit & Enhance Video

We’ll use `ffmpeg` (command-line video editor) to add the AI voiceover.

#### Install ffmpeg

```bash
sudo apt install ffmpeg
```

#### Merge AI Voiceover with Video

```bash
ffmpeg -i MyScene.mp4 -i voiceover.wav -c:v copy -c:a aac final_video.mp4
```

👉 This combines the animation and AI voice into one video.

## Step 4: Add AI-Generated Music

We’ll use `Riffusion` (open-source AI music generator).

#### Install Riffusion

```bash
git clone https://github.com/riffusion/riffusion.git
cd riffusion
pip install -r requirements.txt
```

#### Generate AI Music

```bash
python riffusion.py --text "Background cinematic music" --output music.wav
```

#### Merge Music with Video

```bash
ffmpeg -i final_video.mp4 -i music.wav -filter_complex "[0:a][1:a]amix=inputs=2:duration=first:dropout_transition=3" final_video_with_music.mp4
```

👉 This adds background music to your video.

## Step 5: Generate Subtitles

We’ll use `Whisper` (OpenAI's speech-to-text).

#### Install Whisper

```bash
pip install openai-whisper
```

#### Generate Subtitles

```bash
whisper final_video_with_music.mp4 --model small
```

👉 This will create a .srt subtitle file.

## Step 6: Upload & Optimize for YouTube

We’ll use `yt-dlp` and `OpenTAS` to automate video uploads.

#### Install yt-dlp

```bash
pip install yt-dlp
```

#### Upload Video with OpenTAS

```bash
opentas --upload final_video_with_music.mp4 --title "My AI-Generated Video" --description "Made with open-source AI!"
```

👉 This uploads your video to YouTube.

## ======================================================

Perfect! Please do one of the following so I can start creating your video:

Paste the text you want turned into a video
OR

Upload a file (like a README or script)

Also, let me know a few quick things to customize the video:

🎞️ Video format: Horizontal (YouTube), Vertical (Shorts/TikTok), or Square (Instagram)?

🎨 Style: Minimal, cinematic, playful, professional?

🔊 Sound: Do you want background music or voiceover?
