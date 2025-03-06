# 🗣️ Speech & Development Companion (SDC)

## 📌 Overview
The **Speech & Development Companion (SDC)** is a digital tool designed to **analyze and track speech development in children with Autism Spectrum Disorder (ASD)**. It leverages **machine learning, speech-to-text transcription, NLP (Natural Language Processing), and cloud-based AI models** to extract meaningful insights from children's speech data.

## 🚀 Project Goals
- **Transcribe speech recordings** from children with ASD.
- **Extract linguistic and acoustic features** (speech rate, pitch, fluency, emotional tone, etc.).
- **Analyze speech using NLP and machine learning**.
- **Generate insights and progress reports** for parents and therapists.
- **Store and visualize data securely**.

---

## 📂 Data Sources
We use the following **public datasets** to train and analyze speech:

### 🔹 Primary ASD Speech Datasets:
- **CPSD (Child Pathological Speech Database)** – ASD & language impairment speech recordings.
- **CHILDES Database (TalkBank Project)** – ASD, Down Syndrome, and neurotypical speech samples.
- **TORGO Database** – Speech & articulatory data from children with Cerebral Palsy.
- **VoiceBank Dataset** – Clinical speech synthesis dataset for personalized speech analysis.

### 🔹 Supporting Speech Disorder Datasets:
- **PROCSA Dataset (Aphasia Speech Samples)** – Auditory-perceptual speech features (for analysis comparison).
- **Saarbruecken Voice Database** – Large dataset of disordered speech recordings.
- **ALS Voice Dataset** – Voice recordings from ALS patients (useful for comparison with ASD speech patterns).

---

## 🔍 Step-by-Step Process

### **1️⃣ Data Storage & Organization (AWS S3 + MongoDB)**
- Store raw **audio files** in **AWS S3** (`s3://sdc-asd-speech-data/raw_audio/`).
- Store **processed transcripts, extracted features, and insights** in **MongoDB Atlas**.
- Suggested **S3 hierarchy**:
  ```
  s3://sdc-asd-speech-data/
  ├── raw_audio/
  │   ├── ASD_speech_samples/
  │   ├── Neurotypical_children/
  │   ├── CP_speech_samples/
  ├── transcripts/
  ├── processed_audio/
  ├── model_outputs/
  ```

### **2️⃣ Speech-to-Text Transcription (OpenAI Whisper / AWS Transcribe)**
- Convert speech to text using **OpenAI Whisper**:
  ```python
  import whisper
  model = whisper.load_model("large")
  result = model.transcribe("child_speech.wav")
  print(result["text"])
  ```
- Alternative: Use **AWS Transcribe** for scalable transcription.

### **3️⃣ Feature Extraction (Librosa + Praat)**
- Extract **speech rate, pitch, pauses, articulation rate**:
  ```python
  import librosa
  import numpy as np

  def extract_features(audio_path):
      y, sr = librosa.load(audio_path)
      return {
          "duration": librosa.get_duration(y=y, sr=sr),
          "mean_pitch": np.mean(librosa.piptrack(y=y, sr=sr)),
          "speech_rate": len(librosa.effects.split(y)) / librosa.get_duration(y=y, sr=sr)
      }
  ```
- Save extracted features to **MongoDB Atlas**.

### **4️⃣ NLP Analysis (OpenAI GPT-4 for Speech Insights)**
- Analyze speech transcriptions for **lexical diversity, sentence complexity, and emotion**:
  ```python
  import openai

  openai.api_key = "YOUR_OPENAI_API_KEY"
  prompt = "Analyze this child's speech for complexity and emotion:\n\n" + result["text"]

  response = openai.ChatCompletion.create(
      model="gpt-4",
      messages=[{"role": "system", "content": prompt}]
  )
  print(response["choices"][0]["message"]["content"])
  ```

### **5️⃣ Data Visualization & Reporting (Chart.js + PDF Reports)**
- **Web Dashboard (React.js + Chart.js)** to show **progress over time**.
- **Generate reports** (PDF/CSV) for parents & therapists:
  ```python
  from reportlab.pdfgen import canvas

  def generate_report(text_analysis, speech_features, file_path="speech_report.pdf"):
      c = canvas.Canvas(file_path)
      c.drawString(100, 750, "Speech Analysis Report")
      c.drawString(100, 730, f"Lexical Complexity: {text_analysis['complexity']}")
      c.drawString(100, 710, f"Speech Rate: {speech_features['speech_rate']}")
      c.save()
  ```

---

## 🛠️ Tech Stack
| **Component**          | **Tool**            |
|----------------------|------------------|
| **Storage**          | AWS S3, MongoDB  |
| **Speech-to-Text**   | OpenAI Whisper, AWS Transcribe  |
| **Feature Extraction** | Librosa, Praat  |
| **NLP Analysis**     | OpenAI GPT-4, spaCy, NLTK  |
| **Frontend**         | React.js, Chart.js  |
| **Hosting**          | AWS EC2, Lambda  |

---

## 🚀 Future Enhancements
- **Personalized Speech Therapy Models** (AI-driven recommendations).
- **Real-time Speech Feedback** (web-based speech assessment tool).
- **Multi-language Support** for ASD speech assessment.

---

## 🤝 Get Involved
We welcome contributions from **developers, speech therapists, and researchers** passionate about improving speech therapy with AI.

### 🔹 How to Contribute:
- **Fork the repository** & submit pull requests.
- **Submit issues** for feature requests or bugs.
- **Join discussions** about ASD speech research & technology.

---

## 📜 License
This project is open-source under the **MIT License**.

---

## 📌 Acknowledgments
Special thanks to researchers and open-source contributors who have provided datasets and tools for advancing speech analysis in ASD research. 🙌
