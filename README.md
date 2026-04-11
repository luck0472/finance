# Finance Web App

A Flask-based stock portfolio tracker (CS50 Finance style).

Users can register, log in, look up stock prices, buy/sell shares, and view portfolio + transaction history.

## Features

- User authentication (register/login/logout)
- Stock quote lookup
- Buy and sell shares
- Portfolio dashboard
- Transaction history
- Session handling with `Flask-Session`
- SQLite database via `cs50.SQL`

## Tech Stack

- Python
- Flask
- Flask-Session
- cs50 SQL library
- SQLite
- Bootstrap + custom CSS

## Requirements

Install dependencies from `requirements.txt`:

```bash
pip install -r requirements.txt
```

Current requirements:

- cs50
- Flask
- Flask-Session
- pytz
- requests
- gunicorn

## Environment Variable

This app uses a stock lookup helper. Set your API key before running:

### PowerShell (Windows)

```powershell
$env:API_KEY = "your_api_key_here"
```

### macOS/Linux

```bash
export API_KEY="your_api_key_here"
```

## Run the App

From the project folder:

```bash
python app.py
```

Then open:

`http://127.0.0.1:5000`

## Main Routes

- `/` - Portfolio (login required)
- `/quote` - Get stock quote
- `/buy` - Buy shares
- `/sell` - Sell shares
- `/history` - Transaction history
- `/register` - Create account
- `/login` - Log in
- `/logout` - Log out

## Project Structure

```text
finance/
	app.py
	helpers.py
	requirements.txt
	static/
		styles.css
	templates/
		layout.html
		index.html
		login.html
		register.html
		quote.html
		buy.html
		sell.html
		history.html
		apology.html
```

## Notes

- Development server is for local use only.
- Do not commit API keys or secrets.

## Deploy on Render

1. Push this repository to GitHub.
2. In Render, click New +, then Blueprint.
3. Connect your GitHub repo and select this project.
4. Render will read the deployment config from render.yaml.

If you prefer manual setup, use these settings:

- Runtime: Python 3
- Build Command: pip install -r requirements.txt
- Start Command: gunicorn app:app --bind 0.0.0.0:$PORT

5. Add environment variables in Render:

- API_KEY = your_stock_api_key
- SECRET_KEY = a_long_random_secret
- DATA_DIR = /var/data

6. Click Create Web Service and wait for first deploy.

Important for SQLite and filesystem sessions:

- Render root filesystem is ephemeral, so files like finance.db and flask_session data can reset on deploy/restart.
- For persistent SQLite data, attach a Render Disk and store the database/session files on that mounted path.
- If you do not attach a disk, account data and sessions may be lost.

Recommended next step for production:

- Move from SQLite/file sessions to a managed database + Redis-backed sessions for reliability.

