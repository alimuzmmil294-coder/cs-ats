# ATS Score Checker

A Streamlit app where users paste their CV (and optionally a job description) and get an
ATS-style score out of 100, a breakdown by category, missing keywords and concrete
suggestions. The analysis is done by a Gemini Flash model through the `google-genai` SDK.

> Scores are AI-generated estimates, not the output of a real ATS.

## How scoring works

Gemini scores four categories from 0-100 and returns them as structured JSON. The overall
score is calculated in the app (not by the model) using these weights:

| Category                  | Weight |
|---------------------------|--------|
| Keyword match             | 35%    |
| Content quality & impact  | 30%    |
| Structure & formatting    | 20%    |
| Readability & length      | 15%    |

## Run locally

1. Get a free API key from [Google AI Studio](https://aistudio.google.com/apikey).
2. Install dependencies (Python 3.10+):

   ```bash
   python -m venv .venv
   source .venv/bin/activate        # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. Add your key. Create `.streamlit/secrets.toml`:

   ```toml
   GEMINI_API_KEY = "your-key-here"
   # Optional: pin a specific model instead of the default "gemini-flash-latest"
   # GEMINI_MODEL = "gemini-2.5-flash"
   ```

   Alternatively set an environment variable: `export GEMINI_API_KEY="your-key-here"`.

4. Start the app:

   ```bash
   streamlit run app.py
   ```

## Project structure

```
.
├── app.py
├── requirements.txt
├── README.md
├── .gitignore
└── .streamlit/
    └── secrets.toml     # local only, never commit this
```

## Push to GitHub

Make sure `.gitignore` contains `.streamlit/secrets.toml` and `.venv/` so your key is never
uploaded.

```bash
git init
git add .
git commit -m "Initial commit: ATS score checker"
git branch -M main
```

Create an empty repository on github.com (no README or .gitignore), then:

```bash
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

If you ever commit your API key by accident, revoke it in Google AI Studio and create a new one.

## Deploy on Streamlit Community Cloud

1. Go to [share.streamlit.io](https://share.streamlit.io) and sign in with GitHub.
2. Click **Create app** and choose your repository, the `main` branch and `app.py` as the
   main file path.
3. Open **Advanced settings > Secrets** and paste:

   ```toml
   GEMINI_API_KEY = "your-key-here"
   ```

4. Click **Deploy**. After a minute or two you get a public `*.streamlit.app` URL.

Updating the app later is just `git push`; Streamlit redeploys automatically.

## Notes

- CV text is sent to Google's Gemini API. Mention this in your privacy notice if you share
  the app publicly, and do not log CV content.
- On the free Gemini tier, requests are rate limited. Results are cached for an hour per
  identical input to save quota.
- Limits: CV 200 to 20,000 characters, job description up to 10,000 characters.
