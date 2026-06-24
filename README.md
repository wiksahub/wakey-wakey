# ☕ Coffee – Wake Streamlit App with GitHub Actions + Selenium

This repository contains a simple automation setup that uses **GitHub Actions** and **Selenium** to keep a [Streamlit Community Cloud](https://streamlit.io/cloud) app awake.

Streamlit Community apps automatically go to sleep after inactivity. This project clicks the **“Yes, get this app back up!”** button for you every 4 hours, so your app doesn’t stay idle.

---

## 📂 Project Structure

```
Coffee/
│── main.py                 # Selenium script to wake up the Streamlit app
│── requirements.txt        # Python dependencies
│── .github/
│   └── workflows/
│       └── wake.yml        # GitHub Actions workflow
```

---

## 🚀 How it Works

1. **GitHub Actions** is configured to run every 4 hours (cron job).
2. The workflow spins up a Linux runner with Python installed.
3. Selenium + ChromeDriver (via `webdriver-manager`) launches a headless Chrome browser.
4. The browser visits your Streamlit app URL and:

   * Clicks the **wake-up button** if present.
   * Waits for the app container to load (`div[data-testid='stAppViewContainer']`).
   * Polls until the app is awake or times out.
5. Logs are saved in the **Actions tab** so you can verify success.

---

## 🛠 Setup

### 1. Fork or Clone

```bash
git clone https://github.com/Virgo-Alpha/Coffee.git
cd Coffee
```

### 2. Update Your Streamlit URL

By default, the script points to:

```python
STREAMLIT_URL = "https://benson-mugure-portfolio.streamlit.app/"
```

You can override this by setting an environment variable in your workflow:

```yaml
- name: Run Selenium script
  run: python main.py
  env:
    STREAMLIT_APP_URL: "https://your-streamlit-app.streamlit.app/"
```

---

## 📦 Requirements

Dependencies are managed via `requirements.txt`:

```
selenium
webdriver-manager
```

Install locally if you want to test the script:

```bash
pip install -r requirements.txt
```

---

## ⚡ GitHub Actions Workflow

The workflow file `.github/workflows/wake.yml` schedules the script to run every 4 hours:

```yaml
on:
  schedule:
    - cron: "0 */4 * * *"   # every 4 hours
  workflow_dispatch:         # allow manual trigger
```

You can also **manually trigger** it from the **Actions tab** in GitHub.

---

## 🖥 Running Locally (for testing)

```bash
python main.py
```

Make sure you have Chrome installed. `webdriver-manager` will automatically fetch the correct ChromeDriver.

---

## 📋 Logs & Debugging

* Check workflow runs under the **Actions tab** of your repo.
* Look for messages like:

  * `Button clicked successfully!`
  * `Application has fully loaded! ✅`
* If the app takes too long, you’ll see:

  * `App did not load within the expected time. ❌`

---

## ⚠️ Notes

* Streamlit Cloud cold starts can take **2–5 minutes**. The script retries with refreshes until it succeeds or times out.
* This doesn’t guarantee **100% uptime**, it just automates the manual wake-up click.
* For production reliability, consider hosting your Streamlit app on **always-on infrastructure** (AWS, GCP, Railway, Render, etc.).

---

## 🏷 License

MIT License.
