# ML-pro
## 🎥 YouTube Video Summarizer using LLM (OpenAI GPT)

This project allows you to input any YouTube video URL, extract the audio, transcribe it to text using OpenAI's `whisper`, and summarize the transcript using GPT-4. An optional Streamlit UI is also available for interactive usage.

---

## 🚀 Features

- 🎧 Audio extraction from YouTube
- 🧠 Speech-to-text transcription using `whisper`
- 📃 Natural Language Summarization using `GPT-4` (or `GPT-3.5`)
- 🌐 Optional Streamlit web app interface

---

## 📦 Requirements

Install the required Python packages:

```bash
pip install pytube
pip install moviepy
pip install openai
pip install whisper
pip install streamlit  # optional





CODE
##############
##############
##############
# 
!pip install pytube moviepy openai git+https://github.com/openai/whisper.git

# Import libraries
import os
import openai
import whisper
from pytube import YouTube
from moviepy.editor import AudioFileClip

# Set your OpenAI API key
openai.api_key = "YOUR_OPENAI_API_KEY"  # 🔁 Replace this with your key

#  Download YouTube audio
def download_youtube_audio(url, filename="audio.mp4"):
    yt = YouTube(url)
    stream = yt.streams.filter(only_audio=True).first()
    stream.download(filename=filename)
    return filename

# Convert to WAV for Whisper
def convert_to_wav(input_file, output_file="audio.wav"):
    audio_clip = AudioFileClip(input_file)
    audio_clip.write_audiofile(output_file)
    return output_file

# Transcribe using Whisper
def transcribe_audio(filename):
    model = whisper.load_model("base")
    result = model.transcribe(filename)
    return result["text"]

#  Summarize using OpenAI GPT
def summarize_text(text, model="gpt-3.5-turbo"):
    prompt = f"Please summarize the following video transcript:\n\n{text}\n\nSummary:"
    response = openai.ChatCompletion.create(
        model=model,
        messages=[
            {"role": "system", "content": "You are a helpful assistant who summarizes YouTube videos."},
            {"role": "user", "content": prompt}
        ],
        max_tokens=300
    )
    return response['choices'][0]['message']['content']

#  Run the pipeline
video_url = input("Paste YouTube Video URL: ")

print("⏬ Downloading audio...")
audio_file = download_youtube_audio(video_url)

print("🎧 Converting to WAV...")
wav_file = convert_to_wav(audio_file)

print("📝 Transcribing...")
transcript = transcribe_audio(wav_file)

print("🧠 Summarizing...")
summary = summarize_text(transcript)

print("\n✅ Summary:\n")
print(summary)


