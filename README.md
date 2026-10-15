# ATS CV Scorer

Paste your CV and get an ATS (Applicant Tracking System) score out of 100, a breakdown by category, missing keywords, and specific fixes. Optionally paste a job description for a more accurate keyword score.

Built with [Streamlit](https://streamlit.io) and a Google Gemini Flash model (via the `google-genai` SDK).

## How the score works

The AI rates five categories from 0 to 100. The app then computes the overall score as a fixed weighted average, so the headline number is consistent:

| Category | Weight |
|---|---|
| Keywords & relevance | 30% |
| Experience & impact | 25% |
| ATS-friendly formatting | 20% |
| Completeness | 15% |
| Readability & language | 10% |

This is an estimate, not the output of a real ATS. Treat it as guidance for improving your CV.

## Run locally

1. Get a free API key from [Google AI Studio](https://aistudio.google.com/app/apikey).
2. Install and run:

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

export GEMINI_API_KEY="your-key"   # Windows PowerShell: $env:GEMINI_API_KEY="your-key"
streamlit run app.py
```

Alternatively, create `.streamlit/secrets.toml` (never commit it):

```toml
GEMINI_API_KEY = "your-key"
```

You can also paste the key into the app's sidebar.

## Configuration

| Setting | Where | Default |
|---|---|---|
| `GEMINI_API_KEY` | Streamlit secrets or environment variable | none (required) |
| `GEMINI_MODEL` | Streamlit secrets or environment variable | `gemini-3.5-flash` |

If the default model name is rejected, set `GEMINI_MODEL` to another Flash model listed in the [Gemini models docs](https://ai.google.dev/gemini-api/docs/models).

## Deploy on Streamlit Community Cloud

1. Push this repo to GitHub (keep `app.py` and `requirements.txt` in the repo root).
2. Go to [share.streamlit.io](https://share.streamlit.io), sign in with GitHub, and click **Create app**.
3. Pick the repo, branch `main`, and main file `app.py`.
4. Open **Advanced settings > Secrets** and add: `GEMINI_API_KEY = "your-key"`
5. Click **Deploy**.

## Privacy

CV text is sent to the Gemini API for analysis. This app does not store it. Do not paste ID numbers or other highly sensitive data.

## Files

- `app.py` - the Streamlit app
- `requirements.txt` - Python dependencies
- `README.md` - this file
