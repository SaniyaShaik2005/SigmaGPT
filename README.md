# SigmaGPT

A full-stack ChatGPT replica built from scratch using the MERN stack and OpenAI's API. Supports multi-thread conversations, persistent chat history, markdown rendering with code syntax highlighting, and a smooth word-by-word typing animation — all wrapped in a clean dark-mode UI.

---

## Features

- **Multi-thread conversations** — Create, switch between, and delete independent chat threads
- **Persistent history** — All conversations are saved to MongoDB and restored on reload
- **AI-powered replies** — Powered by OpenAI's `gpt-4o-mini` model
- **Markdown rendering** — AI responses support full markdown including headers, lists, and tables
- **Syntax-highlighted code blocks** — Code rendered with GitHub Dark theme via highlight.js
- **Typing animation** — AI replies appear word-by-word for a natural feel
- **Dark-mode UI** — Minimal, ChatGPT-inspired interface

---

## Tech Stack

| Layer            | Technology                          |
|------------------|-------------------------------------|
| Frontend         | React 19 + Vite 7                   |
| State Management | React Context API                   |
| Styling          | Custom CSS (dark theme)             |
| Backend          | Node.js + Express.js 5              |
| Database         | MongoDB + Mongoose 8                |
| AI Model         | OpenAI API (`gpt-4o-mini`)          |
| Markdown         | react-markdown + rehype-highlight   |
| Thread IDs       | uuid v11                            |
| Dev Tools        | Nodemon, ESLint                     |

---

## Project Structure

```
SigmaGPT/
├── README.md
├── .gitignore
├── Backend/
│   ├── server.js           # Express server entry point (port 8080)
│   ├── package.json
│   ├── models/
│   │   └── Thread.js       # MongoDB schema for threads & messages
│   ├── routes/
│   │   └── chat.js         # All API route handlers
│   └── utils/
│       └── openai.js       # OpenAI API integration
└── Frontend/
    ├── index.html
    ├── vite.config.js
    ├── package.json
    └── src/
        ├── main.jsx         # React entry point
        ├── App.jsx          # Root component & global state
        ├── MyContext.jsx    # React Context definition
        ├── Sidebar.jsx      # Thread list & navigation
        ├── ChatWindow.jsx   # Main chat interface & input
        ├── Chat.jsx         # Message display & typing animation
        └── assets/
            └── blacklogo.png
```

---

## Prerequisites

- [Node.js](https://nodejs.org/) v18+
- [npm](https://www.npmjs.com/) v9+
- [MongoDB](https://www.mongodb.com/) (local instance or MongoDB Atlas)
- An [OpenAI API key](https://platform.openai.com/api-keys)

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/mh-purna/SigmaGPT.git
cd SigmaGPT
```

### 2. Set up the Backend

```bash
cd Backend
npm install
```

Create a `.env` file inside the `Backend/` directory:

```env
MONGODB_URI=mongodb://localhost:27017/sigmagpt
OPENAI_API_KEY=sk-your-openai-api-key-here
```

Start the backend server:

```bash
npm start
```

The API will be available at `http://localhost:8080`.

### 3. Set up the Frontend

Open a new terminal:

```bash
cd Frontend
npm install
npm run dev
```

The app will open at `http://localhost:5173`.

---

## Environment Variables

| Variable        | Description                                  | Required |
|-----------------|----------------------------------------------|----------|
| `MONGODB_URI`   | MongoDB connection string                    | Yes      |
| `OPENAI_API_KEY`| Your OpenAI secret API key                   | Yes      |

> These variables go in `Backend/.env`. Never commit this file — it is already listed in `.gitignore`.

---

## API Reference

All endpoints are prefixed with `/api` and the server runs on port `8080`.

| Method   | Endpoint                  | Body / Params                        | Description                            |
|----------|---------------------------|--------------------------------------|----------------------------------------|
| `POST`   | `/api/chat`               | `{ message, threadId }`              | Send a message and receive an AI reply |
| `GET`    | `/api/thread`             | —                                    | Fetch all threads (most recent first)  |
| `GET`    | `/api/thread/:threadId`   | `threadId` (URL param)               | Fetch all messages for a thread        |
| `DELETE` | `/api/thread/:threadId`   | `threadId` (URL param)               | Delete a thread and its messages       |

### Example — Send a message

**Request:**
```http
POST http://localhost:8080/api/chat
Content-Type: application/json

{
  "message": "Explain recursion in Python.",
  "threadId": "abc-123-uuid"
}
```

**Response:**
```json
{
  "reply": "Recursion is when a function calls itself..."
}
```

---

## How It Works

1. **User sends a message** — The frontend POSTs the message and current `threadId` to `/api/chat`.
2. **Backend finds or creates the thread** — If no thread exists for that `threadId`, a new MongoDB document is created with the title "New Chat".
3. **Message is stored** — The user's message is appended to the thread's `messages` array.
4. **OpenAI is called** — The backend sends the message to OpenAI's `gpt-4o-mini` model and awaits the response.
5. **Reply is stored** — The AI's reply is saved to the same thread document and `updatedAt` is refreshed.
6. **Frontend receives the reply** — The response is returned to the frontend and added to the chat state.
7. **Typing animation plays** — The `Chat` component reveals the reply one word at a time (40ms per word) for a natural feel.

---

## Built With

- [React](https://react.dev/) — UI framework
- [Vite](https://vitejs.dev/) — Frontend build tool
- [Express.js](https://expressjs.com/) — Backend web framework
- [MongoDB](https://www.mongodb.com/) — NoSQL database
- [Mongoose](https://mongoosejs.com/) — MongoDB object modeling
- [OpenAI Node SDK](https://github.com/openai/openai-node) — AI API integration
- [react-markdown](https://github.com/remarkjs/react-markdown) — Markdown rendering
- [rehype-highlight](https://github.com/rehypejs/rehype-highlight) — Code syntax highlighting
- [react-spinners](https://www.davidhu.io/react-spinners/) — Loading animations

---

## Acknowledgements

Built with guidance from [ApnaCollege](https://apnacollege.in/).

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
