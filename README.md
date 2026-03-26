# finance

A web-based personal finance (stocks) tracker built with Flask — the CS50-style "finance" application.

This repository contains a Flask application that lets users register and log in, look up stock quotes, buy and sell shares, view their portfolio and transaction history, and track cash balances. Data is stored in a local SQLite database (`finance.db`).

## Features
- User registration, login, and logout
- Look up real-time stock quotes (via helpers.lookup)
- Buy and sell shares, with price and transaction history
- Portfolio view with per-stock totals and total account value
- Transaction history (timestamped)
- Server-side session handling (filesystem) and basic security (password hashing)

## Requirements
- Python 3.8+
- SQLite (for local DB file)
- Python packages:
  - Flask
  - Flask-Session
  - cs50
  - Werkzeug

You can install Python dependencies with pip (example):

```sh
python -m pip install -r requirements.txt
```

If this repo doesn't include a requirements.txt, the minimal requirements are:
```
Flask
Flask-Session
cs50
Werkzeug
```

## Quickstart (local development)
1. Clone the repository and change into it.
2. Create and activate a virtual environment (recommended):
   ```sh
   python -m venv venv
   source venv/bin/activate   # macOS / Linux
   venv\Scripts\activate      # Windows (PowerShell)
   ```
3. Install dependencies:
   ```sh
   pip install -r requirements.txt
   ```
   or:
   ```sh
   pip install Flask Flask-Session cs50 Werkzeug
   ```

4. Configure environment variables:
   - If `helpers.lookup` uses an external stock API (as in CS50), set the API key:
     ```sh
     export API_KEY="your_api_key_here"
     ```
     (On Windows PowerShell: `setx API_KEY "your_api_key_here"`)

5. Initialize the database:
   - Create `finance.db` and the required tables. Example schema (adapt if you already have a migration or schema file):

   ```sql
   -- users table
   CREATE TABLE users (
       id INTEGER PRIMARY KEY AUTOINCREMENT,
       username TEXT NOT NULL UNIQUE,
       hash TEXT NOT NULL,
       cash NUMERIC NOT NULL DEFAULT 10000.00
   );

   -- transactions table
   CREATE TABLE transactions (
       id INTEGER PRIMARY KEY AUTOINCREMENT,
       user_id INTEGER NOT NULL,
       symbol TEXT NOT NULL,
       shares INTEGER NOT NULL,
       price NUMERIC NOT NULL,
       timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
       FOREIGN KEY(user_id) REFERENCES users(id)
   );
   ```

   Save the SQL to a file like `schema.sql` and run:
   ```sh
   sqlite3 finance.db < schema.sql
   ```

6. Run the app:
   - If the main Flask script is `application.py` (or `app.py`), set FLASK_APP and run:
     ```sh
     export FLASK_APP=application.py
     flask run
     ```
     Or run directly:
     ```sh
     python application.py
     ```
   - Open http://127.0.0.1:5000/ in your browser.

## Usage (routes)
- GET / — portfolio (requires login)
- GET/POST /register — create an account
- GET/POST /login — log in
- GET /logout — log out
- GET/POST /quote — fetch quote for a symbol
- GET/POST /buy — buy shares
- GET/POST /sell — sell shares
- GET /history — view transaction history

Forms expect typical fields:
- register: username, password, confirmation
- login: username, password
- buy/sell: symbol, shares
- quote: symbol

## CSV / Data export
This implementation stores transactions in SQLite. If you want CSV import/export, you can add routes that read from/write to CSV files, or export query results to a `.csv` download.

## Security & Notes
- Passwords are hashed using Werkzeug's `generate_password_hash` / `check_password_hash`.
- Sessions are stored on the filesystem (`Flask-Session`) and `SESSION_PERMANENT` is False.
- The app disables caching in responses to avoid showing stale data.
- Validate any API keys and be careful not to commit secrets to the repository.
- Consider adding CSRF protection (e.g., `Flask-WTF`) for production use.

## Tests
Add integration tests that:
- register a user
- log in
- perform buy/sell operations against a controlled lookup function (mock API)
- verify the DB state (users.cash and transactions)

## Project layout (suggested)
- application.py (or app.py) — main Flask app
- helpers.py — helper functions (lookup, usd, apology, login_required)
- templates/ — Jinja2 templates (index.html, login.html, register.html, buy.html, sell.html, quote.html, history.html)
- static/ — CSS/JS files
- finance.db — SQLite database (ignored by git)
- schema.sql — SQL to create tables
- requirements.txt
- README.md

## Contributing
- Open issues for bugs or feature requests.
- Make small, focused pull requests with tests where applicable.
- Keep credentials out of commits and use environment variables for secrets.

## License
Add a license file (e.g., MIT) if you want this project to be open-source.
