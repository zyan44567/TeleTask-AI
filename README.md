# TeleTask AI Workflow

An advanced **n8n** automation workflow powered by LangChain and OpenAI. This project transforms a Telegram chat interface into an intelligent, autonomous personal assistant capable of managing schedules, sending emails, and handling CRM/contact databases in Google Sheets.

The architectural layout of this automated system can be referenced in the provided image file: `TeleTask-AI-Workflow.png`.

The configuration can be fully restored or modified using the workflow file: `TeleTask-AI.json`.

---

## 🚀 Overview

This workflow intercepts messages sent via Telegram, routes them through a conversational AI agent using memory retention windows, and dynamically decides which productivity tool to execute based on the user's natural language request.

---

## 🛠️ Key Features

*   **Intelligent Conversational Agent:** Powered by OpenAI (`gpt-4.1-mini`) via LangChain to accurately interpret complex human intent.
*   **Contextual Memory Retention:** Integrated with a `Simple Memory` buffer window mapped directly to the individual Telegram Chat ID so the agent remembers previous conversation turns.
*   **Calendar Automation:** Automatically parses relative dates (e.g., *"tomorrow at 3 PM"*), creates Google Calendar events, and auto-generates Hangouts/Google Meet links.
*   **Contact Management (CRM):** Seamlessly matches, retrieves, or appends client contact information (Name and Email) inside a centralized Google Sheet ecosystem.
*   **Automated Email Dispatch:** Drafts and fires standard text emails with predefined, strict branding guidelines directly through Gmail.

---

## 📦 System Architecture & Nodes

Based on the JSON configuration and `TeleTask-AI-Workflow.png`, the workflow is composed of the following interconnected components:

### 1. Triggers & Channels
*   **Telegram Trigger1:** Listens for active updates (`message` or text `caption` over media) targeting the bot.
*   **Send a text message:** Returns the final parsed AI agent text output back to the specific initiating Telegram Chat ID.

### 2. Brain & Core Logic
*   **AI Agent1:** The LangChain core handling system rules, context injection, and autonomous tool routing.
*   **OpenAI Chat Model1:** Executes the language processing using the `gpt-4.1-mini` model.
*   **Simple Memory:** Tracks session data across custom keys dynamically extracted using:
    `={{ $('Telegram Trigger1').first().json.message.chat.id }}`

### 3. Integrated Action Tools
*   **Create an event in Google Calendar:** Allocates calendar entries defaulted to a 1-hour duration under the Malaysian timezone (`+08:00`).
*   **Send a message in Gmail:** Automatically issues messages finalized by a professional email signature rule.
*   **Gat Contact & Add Contact:** Tools utilizing conditional matching rules to view or append `ID`, `Name`, and `Email` in Google Sheets database `1NjnNZ1mSPtYX-4XY3P4jixHXTfYui5ibvAGsGbi8Y0A`.

---

## 📝 Embedded Agent Rules & Guardrails

The underlying AI agent operates under strict operational guardrails written explicitly inside its system prompt payload:

> *   **Timezone Lock:** Locked exclusively to Malaysia/Kuala Lumpur standard time (`+08:00`). Relative dates like "TMR" or "next Monday" are systematically transformed into absolute UTC/ISO strings.
> *   **Branding & Signatures:** Every outgoing email strictly signs off as:
>     ```text
>     Best regards,
>     Leezy
>     Founder   
