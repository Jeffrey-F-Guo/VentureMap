# 🗺️ Venture Map

**Venture Map** is a full-stack web application that helps users discover activities and places based on their location, interests, and schedule.

Built during a 6-hour hackathon.

---

## Demo

🎥 **Watch the demo:** [YouTube](https://www.youtube.com/watch?v=ww4J3DjF8Eo)

> User selects a location, activity type, time, and date → the app returns the **top 4 recommended places** with names, addresses, ratings, and categories.

---

## How It Works

1. The user submits their preferences (location, interest, time of day, date, and optional notes) through the React frontend.
2. The Flask backend receives the request and passes it to a **LlamaIndex ReAct Agent** powered by **Google Gemini 1.5 Pro**.
3. The agent reasons about the input, calls the **Google Places API** (Text Search) to find real-world results, and formats the output into a structured JSON response.
4. The frontend renders the recommendations on an interactive map.

---

## Tech Stack

### Languages
| Language | Usage |
|---|---|
| Python 3 | Backend server, AI agents, API integration |
| TypeScript | Frontend UI components |
| CSS | Styling and responsive layout |
| HTML | Root page template |

### Frontend
- **React 19** — UI framework
- **Vite 6** — Build tool and dev server with HMR
- **SWC** (via `@vitejs/plugin-react-swc`) — Fast React refresh
- **ESLint 9** — Linting with `react-hooks` and `react-refresh` plugins
- **TypeScript ~5.7** — Static typing

### Backend
- **Flask** — Lightweight Python web framework
- **Flask-CORS** — Cross-origin resource sharing
- **python-dotenv** — Environment variable management
- **Requests** — HTTP client for external API calls

### AI / ML
- **Google Gemini 1.5 Pro** — Large language model for natural language understanding and structured data extraction
- **google-generativeai** — Python SDK for the Gemini API
- **LlamaIndex Core** — ReAct agent framework with tool-use capabilities
- **llama-index-llms-gemini** — LlamaIndex wrapper for Gemini

### APIs
- **Google Places API** (Text Search) — Real-world place discovery
- **Google Gemini API** — AI-powered preference parsing and reasoning

---

## Architecture

```
┌─────────────┐     POST /handle_preferences      ┌──────────────┐
│   React UI  │ ──────────────────────────────▶   │  Flask API   │
│  (Vite/TS)  │ ◀──────────────────────────────   │  (Python)    │
└─────────────┘        JSON recommendations       └──────┬───────┘
                                                         │
                                                         ▼
                                                  ┌──────────────┐
                                                  │  ReAct Agent │
                                                  │  (LlamaIndex │
                                                  │  + Gemini)   │
                                                  └──────┬───────┘
                                                         │
                                          ┌──────────────┼──────────────┐
                                          ▼              ▼              ▼
                                   ┌────────────┐ ┌───────────┐ ┌────────────┐
                                   │ find_places│ │  format_  │ │  Gemini    │
                                   │   (tool)   │ │  response │ │  1.5 Pro   │
                                   └─────┬──────┘ │  (tool)   │ │  (LLM)     │
                                         │        └───────────┘ └────────────┘
                                         ▼
                                  ┌──────────────┐
                                  │ Google Places│
                                  │     API      │
                                  └──────────────┘
```
