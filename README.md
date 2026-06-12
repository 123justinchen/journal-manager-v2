# Journal Manager V2

Web-based academic journal scraper with Chinese translation. Tracks 28 journals across optics and RF/microwave engineering.

## Features

- **Scrape** current issues from 28 journals across 7 publishers
- **Browser automation** via DrissionPage — bypasses Cloudflare on T&F, Elsevier, Wiley
- **IEEE journals** use shared DrissionPage TOC scraper (REST API → rendered TOC → detail pages)
- **Chinese translation** via DeepSeek API (titles + abstracts)
- **Category filter** — split into 🔬 Optics (20) and 📡 RF/Microwave (8)
- **Favorites** — bookmark articles; viewable in a dedicated favorites page
- **Markdown export** — select articles and export as `.md`
- **Delete / Mark-read** — manage your article collection

## Quick Start

```bash
# Install dependencies
pip install -r journal_app/requirements.txt

# Set environment variables
export DEEPSEEK_API_KEY="sk-xxx"       # required for translation
export BROWSER_ADDRESS="127.0.0.1:9222" # optional: connect to existing Chrome

# Start Chrome with remote debugging (optional)
chrome.exe --remote-debugging-port=9222

# Run
python -m journal_app.app
# Open http://127.0.0.1:5050
```

## Journals

| Category | Publisher | Journals |
|----------|-----------|----------|
| 🔬 Optics | OPG | AO, AOP, COL, OE, OL, OME, Optica, Optica Quantum, Optics Continuum, Photonics Research |
| 🔬 Optics | Nature | Nature Photonics, Light: Science & Applications, Nature Communications |
| 🔬 Optics | Wiley | Laser & Photonics Reviews, Nanophotonics, Advanced Optical Materials |
| 🔬 Optics | Elsevier | Optics & Laser Technology, Optics and Lasers in Engineering |
| 🔬 Optics | Springer | eLight |
| 🔬 Optics | OEA | Opto-Electronic Advances |
| 📡 RF/MW | IEEE | TAP, TMTT, MWTL, AWPL, THz, Microwave Magazine |
| 📡 RF/MW | Wiley | IET Microwaves, Antennas and Propagation |
| 📡 RF/MW | T&F | J. Electromagnetic Waves and Applications |

## Project Structure

```
journal_app/
├── app.py                  # Flask web app
├── config.py               # Configuration (env vars)
├── database.py             # SQLite database layer
├── translator.py           # DeepSeek translation
├── templates/
│   └── index.html          # Single-page frontend
├── scrapers/
│   ├── base.py             # Base scraper class
│   ├── __init__.py         # Scraper registry + categories
│   ├── opg/                # Optica Publishing Group
│   ├── nature/             # Nature Publishing Group
│   ├── wiley/              # Wiley journals (browser)
│   ├── elsevier/           # Elsevier ScienceDirect (browser)
│   ├── ieee/               # IEEE Xplore (DrissionPage TOC)
│   ├── tandf/              # Taylor & Francis (browser)
│   ├── springer/           # Springer journals
│   ├── rss/                # RSS-based scrapers (unused)
│   └── oea.py              # Opto-Electronic Advances
└── requirements.txt
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/journals` | List journals with stats |
| POST | `/api/scrape/<id>` | Scrape a journal |
| POST | `/api/scrape-all` | Scrape all enabled journals |
| GET | `/api/scrape-status` | Current scrape progress |
| GET | `/api/articles?journal_id=X` | List articles |
| GET | `/api/articles/by-volume?journal_id=X` | Articles grouped by volume |
| GET | `/api/articles/favorites` | Favorite articles |
| POST | `/api/articles/favorite` | Toggle favorite |
| POST | `/api/articles/delete` | Delete articles |
| POST | `/api/articles/export-md` | Export to Markdown |
| POST | `/api/translate` | On-demand translation |
| POST | `/api/journals/<id>/mark-read` | Mark all read |
