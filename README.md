# 🍔 Salma's Bite — Restaurant Customer Support Bot (n8n)

An AI-powered Telegram bot built with **n8n** that acts as a restaurant's customer support team — routing each customer message to the right "specialist" AI agent, answering menu questions using a knowledge base, and managing orders end-to-end.

---

## 🧩 What it does

1. **Triggers** on any new message sent to the restaurant's Telegram bot.
2. A **routing LLM chain** classifies the message into one of three roles:
   - **Questions Expert** — menu or restaurant-related questions
   - **Orders Expert** — placing, checking, or cancelling an order
   - **Generalist** — casual greetings (e.g. "Hi", "Good morning")
3. Based on the classification, a matching **system prompt** is set, then passed to a shared **AI Agent** that actually replies to the user — with access to different tools depending on the task:
   - **Questions Expert** → searches a **Supabase vector store** (RAG) containing restaurant/menu information
   - **Orders Expert** → can look up an order, create a new order, or cancel one, using connected **Google Sheets** tools + a **Code Tool** that generates a unique 4-digit order ID
   - **Generalist** → replies with a simple friendly message
4. A **memory buffer** keeps track of the conversation per Telegram chat, so context isn't lost between messages.
5. All orders are logged to a **Google Sheet** with columns: Time, Item, Price, Order ID, Name, Phone Number, Address, Status.

---

## 🛠️ Tools & Nodes Used

| Node | Purpose |
|---|---|
| Telegram Trigger | Listens for incoming customer messages |
| Basic LLM Chain + Structured Output Parser | Classifies the message into a role (Questions/Orders/Generalist) |
| Switch | Routes the flow based on the classified role |
| Set nodes (system prompts) | Defines the AI Agent's role-specific instructions |
| AI Agent (LangChain) | Core conversational logic, shared across all roles |
| Simple Memory (Buffer Window) | Keeps per-chat conversation context |
| Supabase Vector Store + OpenAI Embeddings | RAG-based knowledge base for menu/restaurant questions |
| Google Sheets (Get / Append / Append-or-Update) | Order lookup, creation, and cancellation |
| Code Tool | Generates a unique 4-digit order ID |
| Telegram (Send Message) | Sends the AI's reply back to the customer |

---

## ⚙️ How to Use This Workflow

1. Import `Salma_Bite.json` into your n8n instance (**Workflows → Import from File**).
2. Set up your own credentials for:
   - Telegram Bot API
   - OpenRouter API (for the routing + reply LLMs)
   - Google Sheets OAuth2
   - Supabase API + OpenAI API (for the menu knowledge base)
3. Replace the placeholder values in the workflow:
   - `YOUR_GOOGLE_SHEET_ID` — your own Google Sheet ID for storing orders
4. Create a Google Sheet named "Orders" with these columns: `Time`, `Item`, `Price`, `Order ID`, `Name`, `Phone Number`, `Address`, `Status`.
5. Set up a Supabase table called `documents` with your restaurant's menu/info as embeddings (via the OpenAI Embeddings node).
6. Activate the workflow and start chatting with your restaurant's Telegram bot 🚀

---

## 💡 Why I built this

Built as a hands-on project during an **AI Automation** course, to practice building a **multi-role AI agent system** — where a single conversation is intelligently routed to different specialized prompts and tool sets, combined with a RAG-based knowledge base for real business use cases.

---

## 📌 Note

All credentials and personal identifiers have been removed/replaced with placeholders in this repo for security. You'll need to plug in your own.
