# 🩺 AI Health Coach: A Mini Doctor in Your Pocket

## 1. Project Title
**AI Health Coach: Intelligent Personalized Health Monitoring System**

## 2. Project Overview
The **AI Health Coach** is a comprehensive health monitoring system that bridges the gap between fragmented health data sources. It integrates real-time **wearable data**, **live weather conditions**, and **medical parameters** into a single "Holistic Health Checkup." Unlike standard fitness apps, it uses Machine Learning to predict disease risks and generative AI (RAG) to provide medically grounded lifestyle advice 24/7.

## 3. Problem Statement
**"The Disconnected Health Information Problem"**
Currently, your health data is siloed:
*   Your **Smartwatch** knows your heart rate and sleep.
*   Your **Weather App** knows it’s 40°C outside.
*   Your **Doctor** knows your medical history.

**The Gap:** If you have high blood pressure and it's extremely hot, running is dangerous. No existing app connects these dots to warn you.
**Our Solution:** A unified system that fuses all these inputs to provide context-aware, safety-first health guidance.

## 4. Solution Approach
We built a centralized intelligence layer that:
1.  **Ingests Data:** Simulates real-time wearable streams and fetches live weather via APIs.
2.  **Predicts Risk:** Uses specialized ML models for chronic diseases and wellness states.
3.  **Contextualizes:** Uses a Retrieval-Augmented Generation (RAG) system to "look up" medical guidelines before answering user questions.
4.  **Acts:** An "Observer Agent" monitors user state to trigger alerts for invisible burnout or health threats.

## 5. Key Features
*   **🏥 Holistic Health Checkup:** A unified scan that checks your Biological (Disease), Mental (Burnout), and Environmental (Weather) risks simultaneously.
*   **🔥 Invisible Burnout Detector:** Uses a Random Forest model to detect stress patterns before you feel exhausted.
*   **💤 Sleep Root-Cause Explainer:** Uses LightGBM to not just score sleep, but explain *why* it was poor (e.g., "High Stress," "Low HRV").
*   **💬 RAG-Powered AI Doctor:** A chatbot that answers questions using a verified local medical database (`health_guidlines.pdf`), ensuring safer answers than generic ChatGPT.
*   **🏋️ Dynamic Workout Planner:** Generates workout plans that adapt to your real-time energy levels and local weather.
*   **⚡ Smart Data Fusion:** Automatically prioritizes precise wearable data over manual inputs when a device is "connected."

## 6. Technologies Used
*   **Core:** Python 3.9+ 🐍
*   **Frontend:** Streamlit (for the interactive dashboard).
*   **Database:** SQLite (Local user storage).
*   **Machine Learning:**
    *   **Scikit-Learn:** Logistic Regression (Disease Models), Random Forest (Burnout).
    *   **LightGBM:** Sleep Quality Analysis.
    *   **FAISS & SentenceTransformers:** Vector database for RAG.
    *   **PDFPlumber:** Parsing medical documents.
*   **AI/LLM:** Mistral AI API (Reasoning Engine).
*   **External APIs:** Open-Meteo (Real-time Weather).
*   **Visualization:** Plotly (Interactive Charts).

## 7. System Architecture
The application follows a **Monolithic Architecture** for simplicity and performance:
1.  **Presentation Layer:** Streamlit UI with "Bento-Grid" design.
2.  **Service Layer (`backend/services`):**
    *   `analysis_service.py`: Orchestrator that calls all models.
    *   `wearable_service.py`: Simulates continuous data streams.
    *   `weather_service.py`: Handles API calls and location data.
3.  **Intelligence Layer (`ML_models` & `backend/rag`):** Contains trained `.pkl` models and the FAISS vector index.
4.  **Persistance Layer (`users.db`):** Stores user profiles and encrypted passwords.

## 8. Machine Learning / AI Models Used
We chose models based on the nature of the data:

| Model | Algorithm | Task | Why this model? |
| :--- | :--- | :--- | :--- |
| **Diabetes Risk** | Logistic Regression | Classification | Interpretable coefficients for medical risk factors. |
| **Heart Disease** | Logistic Regression | Classification | High reliability for binary medical outcomes. |
| **Stroke Risk** | Logistic Regression | Classification | Proven performance on the specific dataset used. |
| **Invisible Burnout** | **Random Forest** | Regression (0-100) | Captures non-linear interactions between Sleep, HRV, and Stress. |
| **Sleep Quality** | **LightGBM** | Regression (0-100) | Efficient gradient boosting for complex tabular patterns. |

## 9. Data Sources
*   **Disease Training Data:** Public datasets (PIMA Indians Diabetes, Heart Disease UCI).
*   **RAG Knowledge Base:** `health_guidlines.pdf` (Medical protocols) and `Dynamic_Workout_Planner.pdf`.
*   **Simulated Wearables:** We created a simulator to mimic Apple Watch/Garmin data streams (Heart Rate, SpO2, HRV) to demonstrate real-time logic without needing physical hardware integration.

## 10. AI Agents & RAG Flow
To make the system "proactive" rather than "passive," we implemented **Autonomous AI Agents**.

### A. "Observer Agent" (Event-Driven)
*   **Role:** Runs in the background (Sidebar) essentially as a sophisticated monitor.
*   **Logic:** It continuously scans the fused data stream. If `Stress > 80` OR `Heart Risk == High`, it interrupts the UI to display a **red alert**.
*   **Code Reference:** `st.session_state.active_alerts` in `app.py`.

### B. "Planner Agent" (Goal-Oriented)
*   **Role:** Actively constructs a path to a goal (Health Improvement).
*   **Logic:**
    1.  **Perceives:** Weather (Rainy), Energy (Low Steps), and Profile (Age 30).
    2.  **Reasons:** "User cannot run outside due to rain. User is tired. Yoga is the safest option."
    3.  **Acts:** Generates a JSON-structured workout plan.
*   **Code Reference:** `generate_workout_plan()` function using Mistral LLM.

## 11. Installation Steps

### Prerequisites
*   Python 3.9 or higher.
*   A Mistral AI API Key (Get one from [console.mistral.ai](https://console.mistral.ai)).

### Setup Guide
1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/your-username/AI-Health-Coach.git
    cd AI-Health-Coach
    ```

2.  **Create Virtual Environment:**
    ```bash
    python -m venv venv
    # Windows:
    .\venv\Scripts\activate
    # Mac/Linux:
    source venv/bin/activate
    ```

3.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Setup Environment Variables:**
    Create a `.env` file in the root folder and add:
    ```ini
    MISTRAL_API_KEY=your_actual_api_key_here
    ```

5.  **Build the RAG Database (Crucial Step):**
    You must build the local knowledge base (vector index) before running the app.
    ```bash
    python build_rag.py
    ```
    *This creates the `data/health_rag_db` folder.*

## 12. How to Run the Project
Start the application using Streamlit:
```bash
streamlit run app.py
```
The app will automatically open in your default browser at `http://localhost:8501`.

## 13. Use Cases
1.  **Proactive wellness:** Detecting "Low HRV" (Heart Rate Variability) days before a user gets sick.
2.  **Safety First:** Preventing users from high-intensity workouts during dangerous weather conditions (High AQI / Heatwaves).
3.  **Accessible Advice:** Answering complex medical questions in simple language based on verified documents.

## 14. Folder Structure (Detailed)
```bash
📂 AI_Health_Coach
├── 📄 app.py                  # 🚀 Main Application (Frontend & Orchestrator)
├── 📄 build_rag.py            # 🛠 Script to build RAG Vector Database
├── 📄 requirements.txt        # 📦 Project Dependencies
├── � .env                    # 🔑 API Keys (Not committed)
├── 📂 ML_models               # 🧠 Trained Machine Learning Models
│   ├── diabetes_model.pkl
│   ├── heart_model.pkl
│   ├── invisible_burnout.pkl
│   └── ...
├── 📂 Dataset                 # 📊 Raw CSV Data for Training
├── 📂 backend                 # ⚙️ Logic Core
│   ├── 📄 database.py         # User Auth & SQLite Management
│   ├── 📂 services            # Service Layer
│   │   ├── analysis_service.py # Smart Data Fusion Logic
│   │   ├── wearable_service.py # Simulated Device Streams
│   │   ├── weather_service.py  # Open-Meteo API Handler
│   │   ├── ml_*.py             # Wrappers for specific ML models
│   │   └── ...
│   └── 📂 rag                 # RAG Intelligence
│       ├── rag_service.py     # Search & Retrieve Logic
│       ├── health_guidlines.pdf
│       └── ...
└── 📂 data                    # 💾 Local Data Storage
    └── health_rag_db          # Generated FAISS Index (Vector DB)
```

## 15. Limitations
*   **Simulation:** The "Wearable" connection is simulated for demonstration purposes.
*   **Cold Start:** The RAG system requires the `build_rag.py` script to be run once initially.
*   **LLM Latency:** Responses depend on the speed of the Mistral AI API.

## 16. Future Scope
1.  **Hardware Integration:** Replace the simulator with real Bluetooth Low Energy (BLE) integration for Fitbit/Apple Watch using the Google Fit API.
2.  **Voice-First Interface:** Add speech-to-text (STT) and text-to-speech (TTS) to allow elderly users to talk to the AI Doctor via voice commands.


## 17. Ethical Considerations
*   **Data Privacy:** All personal data is stored in a local SQLite file (`users.db`), ensuring user privacy.
*   **No Diagnosis:** The app explicitly states (via System Prompt) that it provides *guidance*, not medical diagnoses.
*   **Bias Awareness:** We acknowledge that the training datasets (e.g., PIMA) may have demographic biases.

## 18. Conclusion
The AI Health Coach demonstrates the power of **Agentic AI** in healthcare. By moving from "Reactive" treatment to "Proactive" monitoring, we can improve quality of life using accessible technology.
