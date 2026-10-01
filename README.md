# Cyber_Shield
## Detect the Scam. Understand the Threat. Protect Yourself.

**Real-Time Scam Detection and Victim Protection Assistant for AMS Hackathon 2026**

---

## Problem

India faces a surge in fast, social, and real-time financial scams — OTP theft, vishing, phishing, fake bank/police calls, digital arrest scams, and other social engineering attacks. Victims often lack a quick, reliable tool to assess whether a message, call, or URL is dangerous before acting.

## Proposed Solution

CyberShield is a low-friction, AI-powered real-time scam detection platform that analyses user inputs (text, screenshots, URLs, conversations) and delivers:
- Clear scam/suspicious/safe verdicts
- 0-100 risk scores with explainable factors
- Social engineering indicators
- Immediate safety instructions
- Recommended next actions

## Innovation

Unlike simple keyword filters, CyberShield uses a layered rule-based engine with social engineering pattern detection, conversation escalation analysis, URL structural analysis, and optional OCR — all wrapped in a professional, multilingual (English/Tamil/Tanglish) interface with an "Easy Mode" for elderly users.

## Architecture

```
Input
  ↓
Preprocessing
  ↓
OCR / URL extraction
  ↓
Rule-based signal detection
  ↓
ML scam classifier (scikit-learn / TF-IDF + Logistic Regression — ready for transformer upgrade)
  ↓
Optional LLM reasoning layer
  ↓
Risk engine
  ↓
Explainability layer
  ↓
Safety recommendation engine
  ↓
Final result
```

The system works fully even without the optional LLM.

## Features

### A. Multi-Modal Scam Detection
- Text messages
- Screenshots (OCR via Tesseract/EasyOCR)
- URLs
- Conversations
- Call transcripts

### B. Social Engineering Detection
Detects urgency, fear, authority impersonation, secrecy, OTP/password harvesting, payment requests, reward baiting, account suspension threats, legal/police threats, and emotional manipulation.

### C. Scam Category Classification
OTP fraud, bank impersonation, UPI fraud, phishing, digital arrest scam, fake police scam, KYC scam, job scam, investment scam, loan scam, courier scam, fake customer support, prize/reward scam, identity theft, credential harvesting, other suspicious activity.

### D. Explainable Risk Score
0-100 risk score with clearly labeled "detected signals" or "risk factors" (never arbitrary internal math presented as fact).
- 0-30: LOW
- 31-60: MEDIUM
- 61-85: HIGH
- 86-100: CRITICAL

### E. Conversation Analysis
Paste entire conversations to see attack chain progression (Stage 1: Impersonation → Stage 2: Fear → Stage 3: Urgency → Stage 4: OTP request → Stage 5: Payment request).

### F. URL Analysis
Structural signals: HTTPS presence, lookalike domains, suspicious TLDs, IP-based URLs, sensitive paths, unusually long domains.

### G. Protect Me Now Mode
Immediate safety instructions for high-risk results.

### H. Tamil + English + Tanglish
Full language support with language switching.

### I. Elderly / Simple Mode
Easy Mode with large buttons and plain language.

## Technology Stack

**Frontend:** React, Vite, HTML, CSS, JavaScript, React Router, Axios, Framer Motion, Recharts, Lucide React

**Backend:** Python, Flask, REST API, Flask-CORS, python-dotenv

**Database:** MongoDB (via PyMongo)

**AI/ML:** Python, scikit-learn, TF-IDF, Logistic Regression (baseline), ready for transformer upgrade

**OCR:** Tesseract / EasyOCR (optional)

**Optional:** LLM API for explanation/classification assistance

**URL Analysis:** Python URL parsing, DNS/HTTP checks where safe, reputation API via environment variables

## Project Structure

```
cybershield/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   │   ├── Home.jsx
│   │   │   ├── Home.css
│   │   │   ├── AnalysisResult.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Dashboard.css
│   │   │   └── Demo.jsx
│   │   ├── services/
│   │   │   └── api.js
│   │   ├── utils/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── package.json
│   ├── vite.config.js
│   ├── Dockerfile
│   └── nginx.conf
├── backend/
│   ├── app.py
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── analyze.py
│   │   ├── analysis.py
│   │   ├── feedback.py
│   │   ├── dashboard.py
│   │   └── demo.py
│   ├── services/
│   │   ├── __init__.py
│   │   └── scam_detector.py
│   ├── models/
│   │   └── __init__.py
│   ├── ml/
│   │   └── __init__.py
│   ├── ocr/
│   │   └── __init__.py
│   ├── url_scanner/
│   │   └── __init__.py
│   ├── rules/
│   │   └── __init__.py
│   ├── utils/
│   │   └── __init__.py
│   ├── requirements.txt
│   └── Dockerfile
├── dataset/
├── model/
├── tests/
├── .env.example
├── .env
├── docker-compose.yml
└── README.md
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/health` | Health check |
| GET | `/api/` | API root with endpoint list |
| POST | `/api/analyze/text` | Analyse text message |
| POST | `/api/analyze/url` | Analyse URL |
| POST | `/api/analyze/screenshot` | Analyse screenshot (OCR) |
| POST | `/api/analyze/conversation` | Analyse conversation |
| GET | `/api/analysis/:id` | Get analysis by ID |
| POST | `/api/feedback` | Submit feedback |
| GET | `/api/dashboard` | Get dashboard stats |
| GET | `/api/demo/examples` | List demo examples |
| GET | `/api/demo/examples/:id` | Get demo example |

## Security

- Input validation and sanitisation
- File type and size validation
- Secure headers (CSP, X-Frame-Options, X-XSS-Protection)
- CORS configuration
- Environment variables for secrets
- No hardcoded API keys
- Safe URL handling with SSRF protection
- Rate limiting ready
- Privacy-by-design: no permanent storage of sensitive screenshots without consent

## Installation

### Prerequisites
- Python 3.11+
- Node.js 18+
- MongoDB (optional — falls back to in-memory storage)

### Backend Setup

```bash
cd backend
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Backend runs on `http://localhost:5000`

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

Frontend runs on `http://localhost:3000`

### Docker Setup

```bash
docker-compose up --build
```

Frontend: `http://localhost:3000`
Backend: `http://localhost:5000`
MongoDB: `localhost:27017`

### Environment Variables

Copy `.env.example` to `.env` and configure:

```env
FLASK_APP=app.py
FLASK_ENV=development
SECRET_KEY=your-secret-key
MONGODB_URI=mongodb://localhost:27017/cybershield
PORT=5000
```

Optional:
```env
LLM_API_KEY=
URL_SAFE_BROWSING_API_KEY=
```

## Demo Mode

Access `/demo` or click "Demo" in the nav to try preloaded examples:
1. Fake Bank OTP Scam
2. Digital Arrest Scam
3. UPI Fraud
4. Fake Job Offer
5. Legitimate Transaction
6. Tamil Bank Scam
7. Tanglish Bank Scam

## Testing

```bash
# Backend tests
cd backend
pytest

# Frontend build check
cd frontend
npm run build
```

## AI Safety

- Never guarantees safety
- Never invents threat intelligence
- Never invents official contacts
- Uses cautious language: "Likely scam", "High-risk indicators detected", "Insufficient evidence"
- Every verdict includes: Risk, Confidence, Evidence, Limitations, Recommended action

## Limitations

- Rule-based detection may miss novel scam patterns
- URL analysis is structural only — does not query live reputation databases unless configured
- OCR accuracy depends on image quality
- LLM layer is optional and requires API key
- Not a substitute for official legal/financial advice

## Future Scope

- Transformer-based classifier fine-tuned on scam datasets
- Real-time browser extension
- Voice call analysis
- Community-driven scam database
- Multilingual expansion (Hindi, Telugu, Kannada, Malayalam)
- Mobile app (React Native)
- Integration with official reporting APIs

## Installation Summary

1. Clone the repository
2. Install backend dependencies: `cd backend && pip install -r requirements.txt`
3. Install frontend dependencies: `cd frontend && npm install`
4. Configure `.env` file
5. Start backend: `python backend/app.py`
6. Start frontend: `npm run dev` (in frontend directory)
7. Open `http://localhost:3000`

## Demo Instructions for Hackathon Judges

1. Open `http://localhost:3000`
2. Click "Check Message" → paste a scam message → Analyse
3. Click "Demo" → try preloaded examples
4. Switch language to Tamil/Tanglish
5. Try URL analysis with suspicious URLs
6. View Dashboard for statistics

## License

MIT License — Built for AMS Hackathon 2026

---

**CyberShield — Detect the Scam. Understand the Threat. Protect Yourself.**
