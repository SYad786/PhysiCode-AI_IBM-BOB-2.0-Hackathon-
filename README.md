# PhysiCode AI

**Understand your research code — don't just run it.**

PhysiCode AI is a lightweight web app that explains research/scientific code
— numerical solvers, simulations, data-fitting scripts — in plain language,
tied to the physics or math it implements. Built for physics and STEM
students who write code for research but never had formal CS training.

Built for the **IBM Bob 2.0 Hackathon 2026**.

---

## How it works

1. Paste a Python snippet (an ODE solver, a Monte Carlo simulation, a
   curve-fitting routine, etc.) into the text box.
2. The backend sends it to **IBM watsonx.ai** (Granite model) with a
   structured prompt that asks for:
   - **Explanation** — a section-by-section walkthrough tied to the
     underlying physical/mathematical operation
   - **Issues** — numerical instability, unit mismatches, off-by-one
     errors, silent logic bugs
   - **Suggestions** — concrete, prioritized improvements
3. The result is rendered back as three clear panels.

If no watsonx.ai credentials are configured (or a live call fails), the app
automatically falls back to a **rule-based demo mode** — so it's always
responsive and demo-safe, never a blank error screen.

---

## Tech stack

| Layer      | Choice                                   |
|------------|-------------------------------------------|
| AI engine  | IBM watsonx.ai (Granite instruct model)   |
| Dev partner| IBM Bob 2.0 (used throughout development) |
| Backend    | Python + Flask                            |
| Frontend   | Vanilla HTML / CSS / JS (no framework)     |
| Tests      | pytest (or manual Flask test client)      |

---

## Project structure

```
physicode-ai/
├── app.py                 # Flask routes (/, /api/explain, /api/health)
├── ai_client.py           # watsonx.ai REST client + offline demo fallback
├── prompts.py             # Prompt template sent to the model
├── templates/
│   └── index.html         # Single-page frontend
├── static/
│   ├── style.css          # Dark/purple theme matching project branding
│   └── script.js          # Fetch call + rendering logic
├── tests/
│   └── test_app.py        # Smoke tests (run entirely in demo mode)
├── bob_sessions/          # <-- put your IBM Bob 2.0 session screenshots here
│   └── README.md
├── requirements.txt
├── .env.example
├── .gitignore
└── LICENSE                # MIT (required for hackathon submission)
```

---

## Setup

### 1. Clone and install

```bash
git clone https://github.com/<your-username>/physicode-ai.git
cd physicode-ai
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Configure environment variables

```bash
cp .env.example .env
```

Edit `.env` and fill in your IBM watsonx.ai credentials:

```
WATSONX_API_KEY=your_ibm_cloud_api_key
WATSONX_PROJECT_ID=your_watsonx_project_id
WATSONX_URL=https://us-south.ml.cloud.ibm.com   # match your region
WATSONX_MODEL_ID=ibm/granite-3-8b-instruct
```

> **Don't have watsonx credentials yet?** Leave `.env` blank — the app
> still runs fully in **demo mode** with rule-based feedback, so you can
> build and test the UI immediately.

### 3. Run it

```bash
python app.py
```

Visit **http://localhost:5000**.

### 4. Run tests

```bash
pip install pytest
pytest
```

---

## Deploying

Any host that runs Python works (Render, Railway, Fly.io, IBM Cloud Code
Engine, etc.). For production, run with gunicorn instead of the Flask dev
server:

```bash
gunicorn -w 2 -b 0.0.0.0:$PORT app:app
```

Remember to set the same environment variables on your host as in `.env`.

---

## IBM Bob 2.0 usage

This project was built with **IBM Bob 2.0** as the primary AI development
partner throughout the 48-hour build — scaffolding the Flask backend,
debugging the watsonx.ai REST integration, and refactoring the frontend.

Session screenshots proving this are in [`bob_sessions/`](./bob_sessions/).

---

## Roadmap

- Native support for common scientific libraries (NumPy/SciPy-aware
  explanations)
- Classroom mode — instructors assign scripts, students get guided
  explanations
- Direct Jupyter notebook integration for inline explanations

---

## License

MIT — see [LICENSE](./LICENSE).

## Author

**Syad Ali Raza** — MS Physics student, solo builder.
