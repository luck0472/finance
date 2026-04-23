# Finance — Stock Trading Simulator

A full-stack web application that simulates stock trading. Users can register, log in, look up real-time stock quotes, buy/sell shares, and track a virtual portfolio with transaction history.

## Live Demo

- https://finance-ldz4.onrender.com/

## Features

- User registration and login
- Session-based authentication
- Real-time stock quote lookup
- Buy and sell shares with cash balance checks
- Portfolio dashboard (holdings, current prices, total value)
- Transaction history tracking
- Server-side validation and error handling

## Tech Stack

- **Backend:** Python, Flask  
- **Database:** SQLite (via `cs50` SQL helper)  
- **Templating:** Jinja2  
- **Authentication:** Werkzeug (password hashing), Flask-Session (server-side sessions)  
- **API Consumption:** Requests (external quote API)  
- **Frontend:** HTML, CSS, Bootstrap 5  

## Getting Started (Local)

### 1) Install dependencies
```bash
pip install -r requirements.txt
```

### 2) Run the app (development)
```bash
flask run
```

### 3) Run with Gunicorn (production-like)
```bash
gunicorn app:app
```

## Project Structure (high level)

- `app.py` — Flask routes (portfolio, buy/sell, quote, history, auth) + database queries
- `helpers.py` — helpers (login_required, apology, quote lookup, USD formatting)
- `templates/` — Jinja templates (layout + pages)
- `static/` — CSS and other static assets
- `finance.db` — SQLite database (users + transactions)

## Notes

This project is intended for learning full-stack fundamentals: routing, templates, database persistence, authentication, and API integration. It can be extended with features like input constraints, better error messaging, pagination for history, or switching to Postgres for production use.
