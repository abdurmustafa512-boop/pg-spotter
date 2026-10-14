# StayWise AI — PG & Hostel Agreement Red-Flag Spotter

> **Tagline:** *"Understand before you sign."*  
> **Required Notice:** **"This is awareness guidance, not legal advice."**

StayWise AI is a student-focused, hackathon-ready AI web application built to help college students and young professionals moving to Bengaluru (and other Indian tech/education hubs) understand PG (Paying Guest), hostel, and rental agreements before committing to long leases and handing over large security deposits.

---

## 1. Problem Statement

Students moving to Bengaluru often sign agreements hastily without understanding the fine print. Months later, they encounter:
- Non-refundable deposit deductions (such as automatic ₹3,000–₹5,000 painting or checkout charges).
- Excessive refund delays (60 to 90 business days or held until July).
- Unfair notice period traps (e.g., notice strictly on the 1st of the month, or 2 months notice).
- Lock-in periods with total deposit forfeiture.
- Inflated commercial electricity billing (₹12 to ₹15/unit without sub-meters vs standard BESCOM ₹6 to ₹9 domestic rates).
- Intrusive room searches without notice or resident presence.
- Summary 24-hour eviction clauses without refund.
- Bans on visiting parents and severe late curfew fines (₹500 to ₹1,000).

StayWise AI empowers students with instant awareness, plain-language explanations, and tailored negotiation questions before signing.

---

## 2. Core Features

### 🔍 Single Agreement Analyser
- **Dual Input Options:** Paste agreement text or upload text-based PDF documents (in-memory parsing up to 5MB).
- **One-Click Realistic Samples:** Pre-loaded with genuine Bengaluru PG, student hostel, and co-living studio contracts.
- **Severity-Coded Risk Badges:** Clauses classified as **HIGH RISK** (red), **MEDIUM CONCERN** (amber), or **LOW / INFO** (teal/green) with distinct icons and readable text labels.
- **Comprehensive Clause Breakdown:**
  - Exact verbatim excerpt from the document.
  - Plain-language explanation.
  - Why it matters to students.
  - What information is missing or ambiguous.
  - Suggested question to ask the property owner (with one-click "Copy Question" button).
  - Recommended next step.
- **Overall Summary:** Flagged clauses count, risk breakdown, crucial missing clauses (e.g. absent refund timeline or missing notice duration), and important points to clarify before signing.

### ⚖️ Side-by-Side PG & Hostel Comparison
- Compare two accommodation options simultaneously (e.g., "Koramangala PG" vs "HSR Layout Studio").
- Side-by-side matrix extracting: Monthly rent, Security deposit, Refund conditions, Notice period, Lock-in tenure, Utility/extra charges, Rent increase terms, and Eviction conditions.
- Strict anti-hallucination extraction: Absent fields are marked `"Not specified in text"`.
- Highlights documented advantages, documented concerns, and unknowns for each property.
- Generates a **Master Owner Checklist** with questions to ask both landlords before paying a deposit.

### 🌐 Multilingual Awareness Support
- Full interface, explanations, questions, and recommendations translated into:
  - **English**
  - **Hindi (हिंदी)**
  - **Kannada (ಕನ್ನಡ)**
- **Verbatim Excerpt Integrity:** Document quotations always remain in their original contract language so students can verify findings against the source text.

### 📘 Bengaluru Student Rights Guide
- Guidance on **BESCOM electricity sub-meters** and avoiding overcharging.
- Legal awareness on security deposits and non-refundable deductions under Indian contract law.
- The **Move-in Video Inventory Protocol** (step-by-step method to prevent false damage claims).
- Rent payment receipts, bank transfer evidence, and HRA tax declarations.
- BBMP and Bengaluru City Police safety regulations for Paying Guest facilities (CCTV, police verification, fire exits).

### 🛡️ Privacy & Ephemeral Processing
- Agreemeents often contain personal student details (names, Aadhaar numbers, phone numbers).
- StayWise AI processes agreements **ephemerally in server memory**.
- Agreements and uploaded PDFs are **never permanently stored on disk or logged in databases**.
- Zero API keys or secrets are exposed to the client browser.

---

## 3. Technology Stack

- **Frontend:** React 18, TypeScript, Vite, Tailwind CSS, Lucide Icons.
- **Backend:** Node.js, Express, Multer (in-memory buffer storage), `pdf-parse` (PDF extraction), Zod (schema validation), Dotenv, CORS.
- **AI Integration:** Google Gemini API (`gemini-1.5-flash` via `@google/generative-ai`) with structured JSON schema outputs.
- **Demo Mode Engine:** High-fidelity deterministic rule-based pattern engine mirroring Bengaluru tenancy precedents. Operates offline without external API dependencies.

---

## 4. Architecture Diagram

```
[ User Browser (React + TypeScript + Tailwind) ]
                     │
          HTTP / API Proxy (Port 5173)
                     ▼
[ Express API Server (Port 5000) ]
       │              │               │
       ├─ /api/upload ├─ /api/samples ├─ /api/analyze & /api/compare
       │  (pdf-parse) │  (Samples)    │
       │              │               ▼
       │                      ┌───────────────────────┐
       │                      │  AI Router & Fallback │
       │                      └───────────┬───────────┘
       │                                  │
                  ┌───────────────────────┴───────────────────────┐
                  ▼                                               ▼
       [ Google Gemini API ]                         [ StayWise Rule Engine ]
       (When GEMINI_API_KEY is set)                  (Demo Mode / Offline Fallback)
       - Structured JSON schema                      - Exact Regex & Excerpt extraction
       - Multilingual reasoning                      - Bengaluru tenancy heuristics
       - Anti-hallucination rules                    - English, Hindi, Kannada output
```

---

## 5. Live Mode vs. Demo Mode

| Feature | Demo Mode (Rule-Based Engine) | Live AI Mode (Google Gemini) |
|---|---|---|
| **API Key Required** | None (works 100% offline out-of-the-box) | Yes (`GEMINI_API_KEY` in `server/.env`) |
| **Engine Used** | `StayWise Rule & Pattern Engine` | `Google Gemini 1.5 Flash` |
| **Reliability** | Guaranteed 100% uptime, zero rate-limits | Subject to API quota and connectivity |
| **Transparency** | Clearly labelled with amber **"DEMO MODE (Rule-Based)"** pill | Labelled with teal **"LIVE AI MODE (Gemini)"** pill |
| **Accuracy** | Pinpoint exact excerpts and regex flags | Context-aware reasoning with structured JSON |
| **Multilingual** | English, Hindi (हिंदी), Kannada (ಕನ್ನಡ) | English, Hindi (हिंदी), Kannada (ಕನ್ನಡ) |

> The application clearly discloses when Demo Mode is active and never claims demo results came from a live AI model.

---

## 6. Setup & Installation

### Prerequisites
- Node.js (v18 or v20+) and npm.

### Quickstart

1. **Clone or open the repository:**
   ```bash
   cd Hackathon
   ```

2. **Install all dependencies (Root, Server, Client):**
   ```bash
   npm run install:all
   ```

3. **Configure Environment Variables (Optional):**
   Copy the example environment file:
   ```bash
   cp server/.env.example server/.env
   ```
   *If you wish to use Live Gemini AI, add your API key in `server/.env`:*
   ```env
   PORT=5000
   GEMINI_API_KEY=your_actual_gemini_api_key_here
   NODE_ENV=development
   CORS_ORIGIN=http://localhost:5173
   ```
   *If `GEMINI_API_KEY` is left blank, the app runs automatically in high-fidelity Demo Mode.*

4. **Run the Backend Server:**
   ```bash
   npm run dev:server
   ```
   *Server starts at: `http://localhost:5000`*

5. **Run the Frontend Client (in a separate terminal):**
   ```bash
   npm run dev:client
   ```
   *Client opens at: `http://localhost:5173`*

6. **Run Backend Test Suite:**
   ```bash
   npm run test:server
   ```

---

## 7. How to Demonstrate During a Hackathon (3-Minute Walkthrough)

### Step 1: The Hook & Landing Page (30s)
1. Open `http://localhost:5173`.
2. Point out the product branding (**StayWise AI**), the tagline (*"Understand before you sign"*), and the prominent disclaimer banner: **"This is awareness guidance, not legal advice."**
3. Highlight the transparency badge in the top navbar: **"DEMO MODE (Rule-Based)"** or **"LIVE AI MODE (Gemini)"**. Click on it to show the transparency modal explaining how the system works without deceptive claims.

### Step 2: Single Agreement Analysis (60s)
1. Click **"Analyse My Agreement"** (or choose "Agreement Analyser" in the navbar).
2. Click the quick button **"GreenView PG"** (or **"Sai Balaji HOSTEL"**).
3. The sample agreement loads into the editor. Click **"Analyse Agreement"**.
4. Showcase the results:
   - **Summary Cards:** Total flagged clauses, High Risk count (e.g. 3 High Risk), Medium Concern, Low Risk.
   - **Critical Missing Information Banner:** Notice that the agreement omitted a specific deposit refund timeline and move-in inventory SLA.
   - **Clause Cards:**
     - Point out the **Security Deposit Deductions** card: show the exact verbatim excerpt (`"INR 4,500/- will be deducted automatically from the security deposit"`).
     - Point out the plain explanation and the student risk.
     - Click **"Copy Question"** to demonstrate copying the prepared question for the landlord.
   - Switch the language selector in the navbar to **"हिंदी (Hindi)"** or **"ಕನ್ನಡ (Kannada)"** to showcase that all explanations, questions, and action steps update immediately into natural regional languages while keeping the original legal excerpt intact!

### Step 3: Side-by-Side PG Comparison (60s)
1. Navigate to **"Compare Two PGs"**.
2. Click **"Load Sample Comparison (PG vs Studio)"**.
3. Point out that Option A is a PG in Koramangala and Option B is a Co-Living Studio in HSR Layout.
4. Click **"Compare Both Agreements"**.
5. Walk through the **Side-by-Side Contract Term Matrix**:
   - Compare Monthly rent and Security deposit.
   - Highlight Deposit Refund: Option A delays refund for 60-90 days, while Option B refunds within 15 working days.
   - Highlight Lock-in: Option A has 6 months lock-in with total forfeiture, while Option B has 3 months stay commitment.
   - Highlight Documented Advantages vs Documented Concerns.
   - Point out that missing details are marked `"Not specified"` rather than hallucinated.
   - Show the **Master Owner Checklist** and click **"Copy All Questions"**.

### Step 4: Student Tenant Rights Guide (30s)
1. Navigate to **"Student Rights Guide"**.
2. Show the Bengaluru-specific guidance: BESCOM electricity tariffs, the Day-1 Video Inventory Protocol, and BBMP Paying Guest guidelines.
3. Conclude by re-iterating the core mission: protecting student savings and safety across Bengaluru.

---

## 8. Limitations & Future Roadmap

- **Informational Guidance Only:** The application does not provide formal legal representation, statutory tenant advocacy, or contract enforcement guarantees.
- **Scanned Image PDFs:** The current PDF processor extracts text from text-based PDFs. Image-only scans require an OCR pipeline (planned for v2.0 using Tesseract / Google Document AI).
- **Regional Languages:** Initial release supports English, Hindi, and Kannada. Support for Tamil, Telugu, and Malayalam is on the future roadmap.

---

## 9. License

MIT License. Designed and developed for student awareness and safety.
