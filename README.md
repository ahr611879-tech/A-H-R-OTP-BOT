# INS — Telegram Bot (v2)

Local Telegram bot that connects to your OTP Manager VPS via the Agent API.

## What it does

**Users**
- `/start` → Main menu
- 🌍 **Get a number** → Pick **country** → Pick **platform** → Bot hands out **ONE** number
- Each number card has 🔁 **Change number** and ❌ **Release** buttons
- When an SMS arrives for the held number, the bot pushes a styled OTP card automatically

**Admin** (Telegram IDs in `ADMIN_TELEGRAM_IDS`)
- `/upload` → 3-step wizard: pick **country** → pick **platform** → paste numbers (one per line)
- Numbers are checked against the agent account via API, then only matched numbers
  are saved locally in `bot_state.json` with `{country, platform}`.
- The bot does **not** upload these Telegram inventory numbers to the main panel/server.

## Setup (Windows / macOS / Linux)

```bash
cd telegram-bot

# Windows:
py -m venv venv
venv\Scripts\activate

# macOS / Linux:
python3 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
cp .env.example .env       # Windows: copy .env.example .env
# edit .env with your real values, then:
py bot.py                  # macOS/Linux: python3 bot.py
```

## .env values

| Key | Where to get it |
|---|---|
| `TELEGRAM_BOT_TOKEN` 8923672599:AAEv2JOQWDpMRqlB85iR6-Fcy2geGpVZft4
| `ADMIN_TELEGRAM_IDS`6897116774
| `BOT_BRAND`A H R OTP BOT
| `API_BASE_URL` | http://203.161.58.20:3001/api/functions/agent-api`
| `AGENT_API_KEY` | sk_251a0294c70bb86c56ecec77d4dd5cd93753596d2c061b11b5bfd357f460b089.
| `POLL_INTERVAL_SECONDS` | How often to poll for new SMS (default `5`) |
| `NUMBER_HOLD_SECONDS` | How long a user keeps a number (default `600` = 10 min) |

## Files

- `bot.py` — Telegram handlers, UI, SMS poller
- `api_client.py` — async HTTP client for the VPS Agent API; no upload endpoint is used
- `storage.py` — local JSON store (local inventory, holds + per-number platform/country tags)
- `countries.py` — country code → flag/name mapping
- `bot_state.json` — auto-created on first run; safe to delete to reset state
