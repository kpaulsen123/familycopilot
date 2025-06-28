# AI Finances - Secure Family-Centered Financial Planning with AI

Welcome to AI Finances, a private, secure, and AI-powered Streamlit application designed to help individuals and families understand, forecast, and plan their financial lives using natural language interfaces, embedded policy documents, and customizable copilots.

---

## 🧭 Project Vision
Empower families to:
- Understand real-time finances using their own bank data
- Plan for retirement, taxes, and other long-term goals with guidance from AI copilots
- Interact naturally through text or voice to ask questions like:
  > "How much would I save in taxes if I increase my 401(k) contributions?"
  > "What happens to my Social Security benefits if I die at age 67 with kids under 18?"

---

## 🔐 Core Principles
- **Private by design**: All data remains in your own secure Render-hosted PostgreSQL database.
- **AI-enhanced, not AI-dependent**: Uses GPT-4 for insight and synthesis, but always grounded in real data and user-controlled actions.
- **Family-centric**: Built to account for dependents, spouses, and scenarios that matter most to real people.

---

## 🧱 Architecture Overview
- **Streamlit frontend** (voice/text interaction, page builder, copilots)
- **PostgreSQL backend** (Render-hosted)
- **OpenAI GPT-4/3.5 API** for SQL generation, explanations, and copilots
- **Plaid/Yodlee/Teller APIs** to pull live financial data (user controlled)

---

## 🚀 Features
- 🔎 **AI Assistant**: Ask questions in natural language; gets translated to safe SQL queries.
- 📊 **Dashboard Builder**: Generate live Python/SQL dashboards with edit and deploy options.
- 📄 **Reference Docs AI**: Upload and embed SSA, IRS, or other policy documents to inform copilots.
- 🧠 **Copilots**: Custom planners like Retirement Copilot or Child Benefit Advisor.
- 🎤 **Voice Input**: Ask your question by voice (via Whisper).
- 🔒 **Google Auth**: Secure login system for family-only access.

---

## 👥 Ideal Contributors
- Python / Streamlit / SQL developers
- AI engineers interested in prompt design and copilots
- Security-minded engineers (Render + Google Auth + database protection)
- UI/UX designers for better dashboards and mobile experiences
- Financial advisors or enthusiasts interested in tax, retirement, or benefit planning

---

## 📌 Roadmap
### v1.0 (Stable core)
- [x] AI assistant for finance Q&A
- [x] Dashboard editor using GPT
- [x] Secure login (Google)
- [x] Postgres schema with Plaid integration

### v1.1
- [ ] Reference doc ingestion (SSA, IRS)
- [ ] Retirement Copilot (w/ family roles)
- [ ] Improved page layout and mobile support

### v1.2
- [ ] Voice-to-AI chatbot
- [ ] Budgeting assistant copilot
- [ ] Scenario simulator (What-if)

---
## 🛠 Kick the Tires

Try the hosted demo: **[ai-sandbox.onrender.com](https://ai-sandbox.onrender.com)**  
Use test credentials (shared privately) to log in.

This demo app connects to:
- A **PostgreSQL sandbox database** hosted on Render.com
- A **PLAID sandbox account** with live sample financial data

---

## 🛠 Setup Instructions (Run Locally)

To run the app on your laptop or server:

1. **Clone this repository**
   ```bash
   git clone https://github.com/your-username/ai-finances.git
   cd ai-finances
   ```

2. **Create a `.env` file** in the root directory:
   ```env
   DATABASE_URL=postgresql://your_user:your_password@your_host/your_db
   OPENAI_API_KEY=your_openai_key
   GOOGLE_CLIENT_ID=your_google_client_id
   GOOGLE_CLIENT_SECRET=your_google_client_secret
   REDIRECT_URI=http://localhost:8501
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the app**
   ```bash
   streamlit run app.py
   ```

---

## 🛠 Setup Instructions (Host on Render.com)

### 🔐 Prerequisites

- **Google OAuth** (for login):
  - Create a Google Cloud OAuth app
  - Save the Client ID and Client Secret

- **OpenAI API Key**:
  - Create an account at [https://platform.openai.com](https://platform.openai.com)
  - Your financial data is *not* sent to OpenAI — only reference documents and prompts

- *(Optional)* **PLAID + Yodlee** for real bank data:
  - Create accounts at [https://plaid.com](https://plaid.com) and [https://yodlee.com](https://yodlee.com)

---

### 🌐 Render.com Deployment

#### 1. Create a PostgreSQL Database
- Service type: PostgreSQL
- Name: `finance-db`
- User: `finance_user`
- Version: 16
- Plan: Free or Basic ($6/month)

#### 2. Deploy the `plaid_flask_app`
- Create → Web Service → Git → `kpaulsen123/plaid_flask_app`
- Name: `plaid-flask-app`
- Build command:
  ```bash
  pip install -r requirements.txt
  ```
- Start command:
  ```bash
  gunicorn -w 4 -b 0.0.0.0:5000 plaid_app:app
  ```
- Plan: Free

#### 3. Set Up Cron Job to Sync Data
- Create → Cron Job → Git → `kpaulsen123/plaid_flask_app`
- Name: `plaid-sync-cron`
- Schedule: `0 0 * * *` (daily at midnight)
- Command:
  ```bash
  curl -X GET https://plaid-flask-app.onrender.com/sync
  ```
- Plan: Starter (billed per minute)

#### 4. Deploy the Main `ai-finances` App
- Create → Web Service → Git → `kpaulsen123/ai-finances`
- Name: `ai-finances`
- Build command:
  ```bash
  pip install -r requirements.txt
  ```
- Start command:
  ```bash
  streamlit run app.py --server.port=$PORT
  ```
- Plan: Starter ($7/month) or Free

#### 5. Set Environment Variables for `ai-finances`
```env
DATABASE_URL=postgresql://finance_user:your_password@your_host/your_database
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
OPENAI_API_KEY=your_openai_key
REDIRECT_URI=https://ai-finances.onrender.com
YODLEE_APP_URL=https://yodlee-flask-app.onrender.com  # optional
---

## 💬 Want to Help?
This app was built to help real families. If you're interested in:
- Adding a copilot
- Improving security
- Building better dashboards
- Making financial planning less overwhelming...

You're invited to join. Contact Keith Paulsen or submit a PR or idea.

---

*Developed in Centerville, Utah. Inspired by real needs. Powered by open tools and collaboration.*
