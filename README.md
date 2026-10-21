# Resume ATS Checker

A Streamlit app that extracts text from a resume and uses Google's Gemini Flash model to provide an **estimated ATS-readiness review**, improvement suggestions, and job-description keyword guidance.

> Important: This is a heuristic AI review, not a real score from an employer's Applicant Tracking System (ATS), and it cannot predict hiring outcomes. It analyzes extracted text, not the resume's full visual layout.

## Features

- Upload PDF, DOCX, or TXT resumes (10 MB maximum).
- Optionally enter a target role and paste the job description.
- Gemini returns a score estimate, category breakdown, strengths, improvements, keyword suggestions, and example bullet rewrites.
- Download the review as JSON.
- API key is read from Streamlit secrets or the `GEMINI_API_KEY` environment variable.

## Files

- `app.py` — application code.
- `requirements.txt` — Python dependencies.

## Run locally

Python 3.10+ is recommended.

1. Download/clone this repository.
2. Open a terminal in the project folder.
3. Create and activate a virtual environment (optional but recommended):

   **Windows**
   ```powershell
   py -m venv .venv
   .venv\Scripts\activate
   ```

   **macOS/Linux**
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

4. Install packages:
   ```bash
   pip install -r requirements.txt
   ```
5. Set your Gemini API key.

   **Windows PowerShell (current terminal only)**
   ```powershell
   $env:GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
   ```

   **macOS/Linux**
   ```bash
   export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
   ```

6. Start the app:
   ```bash
   streamlit run app.py
   ```

Get a Gemini API key from Google AI Studio: https://aistudio.google.com/apikey

## Deploy on Streamlit Community Cloud

1. Push `app.py`, `requirements.txt`, and `README.md` to a GitHub repository.
2. Sign in at https://share.streamlit.io/ using GitHub.
3. Select **Create app**, choose your repository, branch (usually `main`), and main file path `app.py`.
4. Before deploying, open **Advanced settings / Secrets** (or add secrets in the app's settings after creation) and add:

   ```toml
   GEMINI_API_KEY = "YOUR_GEMINI_API_KEY"
   ```

5. Deploy. Streamlit installs the packages listed in `requirements.txt`.
6. If the app fails to start, check the app's **Manage app → Logs** page.

Never commit your API key to GitHub. If a key was exposed, revoke it and create a new one.

## Push to GitHub using the website UI

1. Sign in to https://github.com/ and click **+ → New repository**.
2. Give it a name such as `resume-ats-checker`. Choose Public or Private, then create it.
3. In the repository, choose **Add file → Create new file**.
4. Name the file `app.py`, paste the complete code from `app.py`, then click **Commit changes**.
5. Repeat for `requirements.txt` and `README.md`.
6. Confirm all three files are visible at the repository root.
7. Do not upload `.env`, API keys, real resumes, or private user data.

## Supported files and limitations

- PDF: text is extracted with `pypdf`. Image-only/scanned PDFs may need OCR or a text-based export.
- DOCX: paragraphs and table cells are extracted.
- TXT: decoded as UTF-8.
- PDF columns, tables, graphics, and visual layout may not extract in reading order. The app explicitly avoids claiming it can fully verify visual formatting.
- Resume content is sent to Google's Gemini API for analysis. Review Google's current terms and privacy settings before using sensitive personal information.
- AI feedback may be inaccurate. Verify every suggestion and only use bullet rewrites that truthfully describe your experience.

## Troubleshooting

- **Missing API key:** Add `GEMINI_API_KEY` in Streamlit Secrets or your environment.
- **Invalid key / quota:** Check the key and current Google AI Studio API quota.
- **Model unavailable:** Check the current Gemini API model names and access in Google AI Studio; check Google AI Studio for the currently available Gemini Flash model name and update the `model` value in `app.py` if needed.
- **PDF has no text:** Try a text-based PDF, DOCX, or TXT.
