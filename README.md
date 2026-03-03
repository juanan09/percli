# 🔍 PerCLI — Perplexity CLI

A command-line AI assistant that combines **Google Gemini** with **Tavily Search** to provide intelligent, source-backed answers directly from your terminal. Built with [Genkit](https://firebase.google.com/docs/genkit).

## ✨ Features

- 🤖 **AI-Powered Answers** — Uses Google Gemini 2.5 Pro for natural language understanding and generation
- 🔎 **Web Search Integration** — Leverages Tavily API to ground responses with real-time web data
- 💬 **Interactive Chat Mode** — Conversational interface with session-based chat history
- 🎨 **Beautiful Terminal UI** — Colored output, spinners, and formatted responses using Chalk and Ora
- 🛠️ **Genkit Dev Tools** — Built on Firebase Genkit for observability and debugging

## 📋 Prerequisites

- [Node.js](https://nodejs.org/) v18 or higher
- A [Tavily API Key](https://tavily.com)
- A [Google AI API Key](https://aistudio.google.com/app/apikey)

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone <repo-url>
cd percli
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Copy the example file and add your API keys:

```bash
cp .env.example .env
```

Edit `.env` with your keys:

```env
TAVILY_API_KEY=your_tavily_api_key_here
GOOGLE_API_KEY=your_google_api_key_here
```

### 4. Run the application

```bash
# Production mode
npm start

# Development mode (with Genkit Dev UI)
npm run dev
```

## 🎮 Usage

Once running, you'll see an interactive prompt:

```
╔════════════════════════════════════════════════════╗
║   Welcome to Perplexity CLI - Interactive Mode     ║
╚════════════════════════════════════════════════════╝
Type your questions and get AI-powered answers with sources!
Chat history is maintained during this session.
Commands: exit, quit, or press Ctrl+C to leave

💬 Ask a question (or type "exit" to quit):
```

Simply type your question and press Enter. The AI will search the web and provide a comprehensive answer with sources.

**Commands:**
- Type `exit` or `quit` to leave
- Press `Ctrl+C` to force quit

## 📁 Project Structure

```
percli/
├── index.js           # Entry point — interactive CLI loop
├── src/
│   ├── agent.js       # Chat agent configuration (WIP)
│   └── search.js      # Search tool integration (WIP)
├── .env.example       # Environment variables template
├── .gitignore         # Git ignore rules
└── package.json       # Dependencies and scripts
```

## 📦 Dependencies

| Package | Description |
|---|---|
| [genkit](https://www.npmjs.com/package/genkit) | Firebase Genkit framework for AI apps |
| [@genkit-ai/google-genai](https://www.npmjs.com/package/@genkit-ai/google-genai) | Google Gemini plugin for Genkit |
| [@tavily/core](https://www.npmjs.com/package/@tavily/core) | Tavily search API client |
| [chalk](https://www.npmjs.com/package/chalk) | Terminal string styling |
| [ora](https://www.npmjs.com/package/ora) | Terminal spinner for loading states |
| [dotenv](https://www.npmjs.com/package/dotenv) | Environment variable management |

## 📜 Scripts

| Script | Command | Description |
|---|---|---|
| `start` | `npm start` | Runs the CLI with Node.js |
| `dev` | `npm run dev` | Runs with Genkit Dev UI for debugging |

## 📄 License

ISC
