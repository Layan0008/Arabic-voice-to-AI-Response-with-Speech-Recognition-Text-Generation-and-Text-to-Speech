 Description


This project Accepts an arabic audio file and converts it to WAV (if needed).

Uses Google Speech Recognition to transcribe Arabic speech to text.

Uses a Hugging Face Arabic language model (aragpt2-base) to generate a relevant text response.

Converts the generated response into audio using gTTS (Google Text-to-Speech).

Plays the response audio directly in Google Colab.

🧰 Requirements
All required Python packages can be installed with:

bash
Copy
Edit
pip install transformers torch gTTS SpeechRecognition pydub

- Steps:
1. Upload an Audio File
Upload an Arabic audio file (e.g., MP3 or WAV):

python
Copy
Edit
from google.colab import files
uploaded = files.upload()
2. Convert Audio to Text
If the uploaded file isn’t WAV, it’s converted automatically. Then, Google’s Speech Recognition is used:

python
Copy
Edit
recognizer = sr.Recognizer()
with sr.AudioFile(wav_file) as source:
    audio_data = recognizer.record(source)
    text_input = recognizer.recognize_google(audio_data, language="ar")
    
3. Generate a Response
Use the Arabic GPT-2 model (aubmindlab/aragpt2-base) from Hugging Face:

python
Copy
Edit
from transformers import pipeline
generator = pipeline("text-generation", model="aubmindlab/aragpt2-base")

prompt = f"سؤال: {text_input}\nجواب:"
result = generator(prompt, max_new_tokens=100, do_sample=True, top_k=50, temperature=0.9)

4. Convert the Response to Speech
Use gTTS to convert the generated Arabic text to MP3:

python
Copy
Edit
tts = gTTS(text=generated_text, lang='ar')
tts.save("response.mp3")

5. Play the Audio in Colab
python
Copy
Edit
from IPython.display import Audio
Audio("response.mp3")
