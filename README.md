# 🎙️ Voice Assistant with OpenAI GPT & IBM Watson

A full-stack voice assistant that lets you **speak or type** a question and get a **spoken answer** back. Built with a Flask backend, OpenAI's GPT model, and IBM Watson Speech-to-Text & Text-to-Speech, all packaged with Docker.

---
## 📸 Screenshot

<img width="821" height="625" alt="Voice Assistant" src="https://github.com/user-attachments/assets/91380a02-3194-418c-a936-247894fb1293" />

## ✨ Features

- 🎤 **Voice input**: record your question with the microphone
- ⌨️ **Text input**: or simply type your message
- 🧠 **AI responses**: powered by OpenAI GPT
- 🔊 **Spoken output**: answers are read aloud by Watson Text-to-Speech
- 🗣️ **Multiple voices**: choose from different English/French voices
- 🌗 **Light / dark mode** toggle
- 🐳 **Dockerized**: runs the same way everywhere

---

## 🔄 How It Works

```
 🎤 User speaks
      │
      ▼
 Watson Speech-to-Text  ──►  transcribed text
      │
      ▼
 OpenAI GPT  ──►  response text
      │
      ▼
 Watson Text-to-Speech  ──►  audio (WAV, base64)
      │
      ▼
 🔊 Browser plays the answer
```

1. The browser records audio and sends it to the `/speech-to-text` endpoint.
2. Flask forwards the audio to **Watson STT**, which returns the transcribed text.
3. The text is sent to `/process-message`, where **OpenAI GPT** generates a reply.
4. The reply is converted to speech with **Watson TTS**, encoded in base64, and returned to the browser together with the text.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript, Bootstrap, jQuery, Font Awesome |
| Backend | Python, Flask, Flask-CORS |
| AI | OpenAI GPT (`gpt-4.1-mini`) |
| Speech | IBM Watson Speech-to-Text & Text-to-Speech |
| Deployment | Docker |

---

## 📁 Project Structure

```
.
├── server.py            # Flask app & API routes
├── worker.py            # STT, OpenAI and TTS helper functions
├── templates/
│   └── index.html       # Main web page
├── static/
│   ├── script.js        # Frontend logic (recording, messaging, playback)
│   └── style.css        # Styling & loading animation
├── models/              # Optional configs for self-hosting Watson STT/TTS
├── Dockerfile
└── requirements.txt
```

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Serves the web interface |
| `POST` | `/speech-to-text` | Receives raw audio, returns `{ "text": "..." }` |
| `POST` | `/process-message` | Receives `{ "userMessage", "voice" }`, returns `{ "openaiResponseText", "openaiResponseSpeech" }` |

---

## 🚀 Running the App

### In the IBM Skills Network lab environment

```bash
docker build . -t voice-chatapp-powered-by-openai
docker run -p 8000:8000 voice-chatapp-powered-by-openai
```

Then open the app on port **8000**.

### Running outside the lab

This project was built in the IBM Skills Network lab, which provides hosted Watson services and an OpenAI proxy. To run it elsewhere:

1. Get an **OpenAI API key** and set it as an environment variable:
   ```bash
   export OPENAI_API_KEY="your-key-here"
   ```
2. Create **Watson Speech-to-Text** and **Text-to-Speech** instances on IBM Cloud (or self-host them using the configs in `models/`).
3. Update the `base_url` values in `worker.py` with your own service URLs (and add authentication if required).
4. Remove the lab-specific certificate and `OPENAI_API_KEY` lines from the `Dockerfile`.
5. Build and run with Docker as shown above.

---

## 📝 Notes

- The lab instructions use `gpt-5-nano`, which is not supported by the lab's OpenAI proxy. This project uses **`gpt-4.1-mini`** instead, which is fast and well suited for real-time voice replies.
- The assistant's personality can be customized by editing the `prompt` in `openai_process_message()` inside `worker.py`.

---

## 🙏 Acknowledgements

This project is based on the guided project **"Create a Voice Assistant with OpenAI's GPT Model and IBM Watson"** from [IBM Skills Network](https://skills.network/).

## 📄 License

Licensed under the [Apache License 2.0](LICENSE).
