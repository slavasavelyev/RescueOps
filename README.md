# RescueOps 🐾  
**Privacy-aware volunteer dispatch for stray animal aid**  
FastAPI + SQLite + Leaflet (no build tools)

![CI](https://github.com/slavasavelyev/rescueops/actions/workflows/ci.yml/badge.svg)
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Python](https://img.shields.io/badge/Python-3.11%2B-blue)
![UI demo](docs/ui.png)

> **Goal:** make it easy for volunteers to **collect cases**, **prioritize help**, and **build a route** — without exposing private addresses.

---

## What this demo includes

- ✅ **Case intake**: create a new case (animal type, problem, severity, notes, point on map)
- ✅ **Urgency score (0–100)**: simple and explainable (severity + issue + time open)
- ✅ **Route planning**: builds a practical visit order for top urgent open cases  
  (Nearest Neighbor + 2‑opt)
- ✅ **Privacy by design**: the public map shows a **blurred point** (stable jitter)
- ✅ **API docs**: automatic Swagger UI at `/docs`
- ✅ **Tests + CI**: pytest + ruff, GitHub Actions workflow included

---

## Quick start (local)

### 1) Install
- Python **3.11+**
- Git (optional but recommended)

### 2) Download
```bash
git clone https://github.com/slavasavelyev/rescueops.git
cd rescueops
```

### 3) Create virtual environment
**Windows (PowerShell):**
```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 4) Install dependencies
```bash
pip install -r requirements.txt
```

### 5) Run
```bash
python -m rescueops
```

Open:
- Web UI: http://127.0.0.1:8000/
- API docs: http://127.0.0.1:8000/docs

---

## How the privacy blur works

Public endpoints never expose exact coordinates.

Instead, we return a **stable blurred** point:
- close to the real point (default ~120 meters)
- stable per case (same case → same blurred point)

See: [`docs/PRIVACY.md`](docs/PRIVACY.md)

---

## Project structure

```
rescueops/
  rescueops/          # backend (FastAPI)
  web/                # frontend (static files)
  tests/              # pytest tests
  docs/               # architecture + privacy + algorithms
  .github/workflows/  # CI
```

---

## Environment variables (optional)

You can run without any config. Defaults are safe for local demo.

| Variable | Meaning | Default |
|---|---|---|
| `RESCUEOPS_DB_URL` | DB connection string | `sqlite:///./rescueops.db` |
| `RESCUEOPS_ADMIN_TOKEN` | admin token for protected endpoints | `change-me` |
| `RESCUEOPS_PUBLIC_JITTER_METERS` | blur radius in meters | `120` |
| `RESCUEOPS_PUBLIC_JITTER_SECRET` | secret for stable blur | `rescueops-demo` |

Example:
```bash
RESCUEOPS_ADMIN_TOKEN="my-long-random-token" python -m rescueops
```

---

## Admin API (optional)

This demo includes minimal admin endpoints (token header `X-Admin-Token`):

- `GET /api/cases` (shows exact coordinates + reporter_contact)
- `PATCH /api/cases/{id}/status`

This is for demo only. For real use, implement proper authentication.

---

## Tests & style

Install dev tools:
```bash
pip install -r requirements-dev.txt
```

Run tests:
```bash
pytest
```

Format + lint:
```bash
ruff format .
ruff check .
```

---

## Safety disclaimer

RescueOps is an educational demo. Do not use it to publish private addresses or sensitive locations.  
If a situation is dangerous, contact local emergency services.

---

## License
MIT (see [LICENSE](LICENSE)).
