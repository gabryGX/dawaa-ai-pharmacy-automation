# 💊 Dawaa AI — Smart Pharmacy Automation System (n8n + Gemini 2.5 Flash)

**Engineered & Developed by: Eng. Mostafa Elgabry**

An end-to-end pharmacy order management, prescription OCR, voice-to-order AI, and inventory control system built on **n8n**, powered by **Google Gemini 2.5 Flash**, **WhatsApp Business Cloud API**, **Telegram Bot API**, and **Google Sheets**.
🔗 **Live Interactive Demo:** https://gabrygx.github.io/dawaa-ai-pharmacy-automation/---

## 🌟 Key Features

1. **Multi-Modal Customer Intake (WhatsApp):**
   - **Text Messages:** Extracts medicine names, strengths, dosage forms, and box quantities.
   - **Prescription Images (OCR & Vision):** Reads handwritten/printed Egyptian prescriptions and medicine boxes using Gemini 2.5 Flash without guessing unreadable names (`?`).
   - **Voice Notes (Audio Transcription):** Transcribes Egyptian Arabic & English drug names from WhatsApp voice notes (`audio/ogg`), evaluates confidence (`high` vs. `low`), and shares the transcript with both the customer and the pharmacist.

2. **Intelligent Catalog Matching & Safety Routing:**
   - **Levenshtein Fuzzy Matching:** Matches drug brand names and active ingredients while tolerating minor spelling mistakes.
   - **Human-in-the-Loop Pharmacist Escalation (Telegram):** Automatically routes prescriptions, controlled substances (`controlled = yes`), fuzzy matches, low-confidence voice notes, and out-of-stock items to a pharmacist group on Telegram with inline **Approve / Reject** buttons.
   - **Instant Auto-Reservation:** High-confidence text/voice orders for available, non-controlled items are reserved immediately with real-time stock deduction.

3. **Automated Operations, Cron Jobs & Error Handling:**
   - **15-Minute Expiry & Restock Cron:** Automatically expires uncollected reservations after 120 minutes, notifies the customer on WhatsApp, and returns reserved quantities to inventory.
   - **Daily Low-Stock Alert (22:00):** Scans inventory and sends a formatted restock report to Telegram.
   - **Centralized Error Classification:** Captures workflow errors, classifies severity (`network`, `validation`, `ai_output`, `config`), logs them to Google Sheets, alerts Telegram, and emails critical failures.

4. **Interactive Live Command Center (`demo/index.html`):**
   - Standalone interactive simulator demonstrating WhatsApp ↔ n8n execution logs ↔ Telegram pharmacist approvals ↔ live Google Sheets updates.

---

## 📁 Repository Structure

```text
├── workflows/
│   ├── dawaa_main_workflow.json         # Main WhatsApp, Gemini AI, Matching & Telegram Decision workflow (39 nodes)
│   └── dawaa_error_cron_workflow.json   # Error Handler, 15-min Expiry Restock & 22:00 Reorder Cron (22 nodes)
├── data/
│   └── dawaa_pharmacy_sample.xlsx       # Google Sheets template (Catalog, Reservations, Errors tabs)
├── demo/
│   └── index.html                       # Interactive Command Center & Workflow Simulator
└── README.md
```

---

## 🚀 Setup & Configuration

1. **Google Sheets Setup:**
   - Upload `data/dawaa_pharmacy_sample.xlsx` to Google Drive and convert it to a Google Sheet (keep the 3 tab names: `Catalog`, `Reservations`, `Errors`).
   - Copy the Sheet ID from the URL.

2. **Import Workflows into n8n:**
   - Import both JSON files from `workflows/` into your n8n instance.
   - Link the **Error Workflow** setting in `dawaa_main_workflow` to `dawaa_error_cron_workflow`.

3. **Configure Credentials & Placeholders:**
   Replace the placeholder values across nodes with your own IDs and credentials:
   - `YOUR_GOOGLE_SHEET_ID` → Your Google Sheet ID
   - `YOUR_WHATSAPP_PHONE_NUMBER_ID` → Your Meta WhatsApp Business Phone Number ID
   - `YOUR_TELEGRAM_CHAT_ID` → Your Pharmacist Telegram Group/Chat ID
   - `alerts@example.com` → Admin email for critical error notifications
   - Connect your **WhatsApp**, **Google Gemini (PaLM)**, **Google Sheets**, **Telegram**, and **Gmail** credentials in n8n.

---

## ⚠️ Disclaimer
This project is built for workflow automation demonstration and operational support. Final medical verification and dispensing decisions remain strictly with a licensed pharmacist.
