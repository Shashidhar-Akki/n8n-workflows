#  Jarvis — Personal AI Assistant via Telegram

##  Overview

A **production-style AI assistant prototype** built with n8n that operates entirely through Telegram.
Users can send text or voice messages, and Jarvis intelligently handles tasks like email management, scheduling, web search, social media posting, and contact management — then responds with a **sarcastic “Jarvis-style” voice output**.


## Features

*  **Security Layer**
  Restricts access using Telegram chat ID and name verification

*  **Voice + Text Input**

  * Accepts Telegram messages and voice notes
  * Voice input is transcribed using OpenAI Whisper

*  **AI Agent with Tool Access**

  * Powered by GPT-4o
  * Dynamically selects and uses tools based on user intent

*  **Tool-Based Agent Architecture**
  Modular sub-workflows act as tools:

  * Gmail (email operations)
  * Calendar (event management)
  * Search (web + Wikipedia + HackerNews)
  * X/Twitter (posting)
  * Contacts (storage and retrieval)

*  **Session Memory**
  Maintains recent conversation context per user

* **Voice Output (TTS)**
  Converts responses into audio using OpenAI TTS

*  **Jarvis Personality Layer**
  Generates short sarcastic responses for a unique user experience(CAN BE ALTERED)



##  Architecture

### 1. Security Layer

Telegram Trigger → IF node verifies chat ID + name
→ Only authorised user proceeds

### 2. Input Routing

Switch Node:

* Text → Directly sent to AI agent
* Voice → Retrieved → Transcribed → Sent to AI agent

### 3. AI Agent

* GPT-4o-based agent with tool access
* Uses modular sub-workflows:

  * Gmail_Tool
  * Calendar_Tool
  * Search_Tool
  * X_posts
  * Contact_agent

### 4. Response Pipeline

Agent Output →
→ Sent as text to Telegram
→ Passed to LLM Chain (Jarvis personality)
→ Converted to audio (TTS)
→ Sent back as voice message


## 🛠️ Tech Stack

* **Automation:** n8n
* **AI/LLM:** OpenAI GPT-4o
* **Speech-to-Text:** OpenAI Whisper
* **Text-to-Speech:** OpenAI TTS (Shimmer voice)
* **Messaging Interface:** Telegram Bot API
* **Integrations:** Gmail API, Google Calendar API, X (Twitter) API
* **Architecture:** Tool-based agent using modular sub-workflows



##  Setup Instructions

1. Import the workflow JSON into n8n
2. Configure credentials:

   * Telegram API
   * OpenAI API
   * Gmail API
   * Google Calendar API
3. Replace placeholders:

   * `YOUR_CHAT_ID`
   * `YOUR_NAME`
   * `REPLACE_WORKFLOW_ID` (connect sub-workflows)
4. Activate all sub-workflows
5. Run the main workflow
6. Send a message to your Telegram bot



##  Security

* No API keys are stored in this repository
* All credentials are managed securely within n8n
* Sensitive identifiers are replaced with placeholders
* Basic access control implemented via chat ID verification



**Challenges & Solutions**

| Problem                       | Solution                                           |
| ----------------------------- | -------------------------------------------------- |
| Unauthorised access risk      | Implemented IF node with chat ID + name validation |
| Voice messages not processing | Used Telegram “Get File” + OPENAI transcription   |
| Sub-workflows not executing   | Activated workflows + used `$fromAI()` mapping     |
| Calendar events failing       | Enforced ISO 8601 format in prompt                 |
| Tool input handling issues    | Standardised `$fromAI()` for all tool inputs       |
| Generic AI responses          | Designed structured system prompt                  |



##  What This Project Demonstrates

* ✅ AI agent design with real-world tools
* ✅ Multi-modal input/output pipeline
* ✅ 5+ external API integrations
* ✅ Modular sub-workflow architecture
* ✅ Context-aware conversational memory
* ✅ End-to-end automation system design

