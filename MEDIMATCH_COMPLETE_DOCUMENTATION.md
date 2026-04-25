# MEDIMATCH - Complete System Detailed Documentation
**Powered by COGNIMED AI**

---

## TABLE OF CONTENTS

1. [Executive Summary](#section-1-executive-summary)
2. [System Overview](#section-2-system-overview)
3. [Core Modules (4 Portals)](#section-3-core-modules-4-portals)
4. [COGNIMED AI Engine - Complete Details] fggddfvsd(#section-4-cognimed-ai-engine---complete-details)
5. [Database Design - Full Schema](#section-5-database-design---full-schema)
6. [API Specifications - Complete Endpoints](#section-6-api-specifications---complete-endpoints)
7. [Security & Compliance Framework](#section-7-security--compliance-framework)
8. [Deployment Architecture](#section-8-deployment-architecture)
9. [Cost Breakdown](#section-9-cost-breakdown)
10. [Development Roadmap](#section-10-development-roadmap)
11. [Risk Management](#section-11-risk-management)
12. [Future Roadmap](#section-12-future-roadmap)

---

## SECTION 1: EXECUTIVE SUMMARY

### 1.1 What is MediMatch?
MediMatch is a comprehensive, AI-powered telemedicine and hospital management system that connects patients, doctors, hospitals, and administrators through an intelligent platform. The system is powered by COGNIMED, a cognitive AI engine that understands symptoms, asks intelligent follow-up questions, predicts diseases with >75% confidence, and continuously learns from every interaction.

### 1.2 Core Problem Solved

| Problem | MediMatch Solution |
|---------|-------------------|
| Patients unsure about symptoms | AI symptom checker with 75%+ confidence |
| Language barriers | English + Amharic voice/text support |
| Finding right doctor | Smart matching by disease expertise |
| Manual booking | Online booking + payment |
| Hospitals lack visibility | Management dashboard with analytics |
| Doctors need assistance | AI doctor assistant for diagnosis |
| No system-wide control | Admin portal with full governance |
| AI doesn't improve | Continuous learning from feedback |

### 1.3 Key Metrics

| Metric | Target |
|--------|--------|
| AI Prediction Confidence | ≥75% (ethical threshold) |
| User Response Time | <3 seconds |
| System Uptime | 99.9% |
| AI Accuracy Improvement | +5% per month |
| User Satisfaction | 4.5/5 stars |

---

## SECTION 2: SYSTEM OVERVIEW

### 2.1 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PRESENTATION LAYER                              │
├──────────────────┬──────────────────┬──────────────────┬───────────────────┤
│   PATIENT WEB    │  PATIENT MOBILE   │   FACILITY WEB   │    ADMIN WEB      │
│   (Vite/React)   │   (Flutter)       │   (Vite/React)   │   (Vite/React)    │
│   Port: 3000     │   Android APK     │   Port: 3001     │   Port: 3002      │
└────────┬─────────┴────────┬─────────┴────────┬─────────┴─────────┬─────────┘
         │                  │                  │                   │
         └──────────────────┼──────────────────┼───────────────────┘
                            │                  │
                            ▼                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              API GATEWAY LAYER                               │
│                         (Nginx / Kong / Express Gateway)                     │
│                    Rate Limiting | Authentication | Logging                  │
└─────────────────────────────────────────────────────────────────────────────┘
                            │                  │
                            ▼                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           APPLICATION LAYER                                  │
│                         (Node.js + Express / Python FastAPI)                 │
├──────────────────┬──────────────────┬──────────────────┬───────────────────┤
│   ADMIN MODULE   │  FACILITY MODULE  │  PATIENT MODULE  │   AI MODULE       │
│  • Hospital Mgmt │ • Auth (Role)     │ • AI Chat        │ • COGNIMED        │
│  • Doctor Mgmt   │ • Dashboard       │ • Booking        │ • Symptom Check   │
│  • Payment Mgmt  │ • Appointments    │ • Payment        │ • Learning        │
│  • AI Control    │ • Doctor AI       │ • Reports        │ • Personalization │
│  • Audit Logs    │ • Surgery Schedule│ • History        │ • Ethics Filter   │
└──────────────────┴──────────────────┴──────────────────┴───────────────────┘
                            │                  │
                            ▼                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              DATA LAYER                                      │
├──────────────────┬──────────────────┬──────────────────┬───────────────────┤
│   POSTGRESQL     │      REDIS       │    AWS S3        │    DATASETS       │
│   (Primary DB)   │    (Cache)       │   (Storage)      │   (CSV/JSON)      │
│   Port: 5432     │    Port: 6379    │   Reports/Images │   Training Data   │
└──────────────────┴──────────────────┴──────────────────┴───────────────────┘
```

### 2.2 User Types & Access Levels

| User Type | Access Level | Portals Accessible |
|-----------|--------------|-------------------|
| Patient | Low | Patient Web + Mobile |
| Doctor | Medium | Facility Web (Doctor Dashboard) |
| Hospital Management | Medium-High | Facility Web (Management Dashboard) |
| Admin | Full | Admin Web |

### 2.3 Platform Support Matrix

| Feature | Patient Web | Patient Android | Facility Web | Admin Web |
|---------|-------------|-----------------|--------------|-----------|
| AI Symptom Checker | ✅ | ✅ | ❌ | ❌ |
| Voice Input (En/Amh) | ✅ | ✅ | ❌ | ❌ |
| Doctor Booking | ✅ | ✅ | ✅ (view only) | ✅ (override) |
| Payment Processing | ✅ | ✅ | ✅ (view) | ✅ (refund) |
| Medical Reports | ✅ | ✅ | ✅ (view patient) | ✅ (view all) |
| Consultation History | ✅ | ✅ | ✅ (patient-specific) | ✅ (all) |
| Hospital Management | ❌ | ❌ | ✅ | ✅ |
| Doctor Management | ❌ | ❌ | ✅ | ✅ |
| AI Doctor Assistant | ❌ | ❌ | ✅ | ❌ |
| Surgery Scheduling | ❌ | ❌ | ✅ | ✅ (approve) |
| AI Model Control | ❌ | ❌ | ❌ | ✅ |
| Block/Unblock Users | ❌ | ❌ | ❌ | ✅ |
| Payment Monitoring | ❌ | ❌ | ✅ (own) | ✅ (all) |
| External API Control | ❌ | ❌ | ❌ | ✅ |

---

## SECTION 3: CORE MODULES (4 PORTALS)

### 3.1 PATIENT PORTAL (Web + Android)

#### 3.1.1 Complete Feature List

| Feature | Sub-Features | Platform | Status |
|---------|--------------|----------|--------|
| AI Symptom Checker | Text input, Voice input (En/Amh), Follow-up questions | Both | ✅ |
| Disease Prediction | Confidence score, Recommendation, Disclaimer | Both | ✅ |
| Patient Information | Name, Age, Sex, Onset date, Contact info | Both | ✅ |
| Medical Report | PDF generation, Download, Share | Both | ✅ |
| Hospital Listing | By disease, By location, By rating, By fee | Both | ✅ |
| Doctor Booking | Select doctor, Choose time, Confirm booking | Both | ✅ |
| Payment | Card, Mobile money (Telebirr), Bank transfer | Both | ✅ |
| Consultation History | Past sessions, Old reports, Feedback given | Both | ✅ |
| Profile Management | Update info, Change password, Language preference | Both | ✅ |
| Settings | Notifications, Privacy, Data sharing consent | Both | ✅ |
| Emergency Alert | One-tap emergency, Redirect to 911 | Both | ✅ |
| Feedback | Rate AI, Report issue, Suggest improvement | Both | ✅ |

#### 3.1.2 Patient Journey Flow

```
START
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 1. User opens MediMatch (Web or Android)                        │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 2. COGNIMED AI greets: "Hello! Describe your symptoms"          │
│    → Language detected (English/Amharic)                        │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 3. User inputs symptoms (voice or text)                         │
│    Examples: "Fever and headache for 2 days"                    │
│              "ትኩሳት እና ራስ ምታት ለ2 ቀናት"                              │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 4. COGNIMED asks follow-up questions (2-5 rounds)               │
│    Q1: "Do you have a cough?"                                   │
│    Q2: "Any body pain?"                                         │
│    Q3: "Shortness of breath?"                                   │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 5. COGNIMED predicts disease                                    │
│    → Viral Infection (85% confidence)                           │
│    → Recommendation: Rest, hydration, paracetamol               │
│    → Disclaimer: "AI suggestion, not medical diagnosis"         │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 6. If confidence ≥75% → Continue                                │
│    If confidence <75% → "Please consult a doctor directly"      │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 7. Collect patient information                                  │
│    → Full name, Age, Sex, When symptoms started, Phone number   │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 8. Generate medical report (PDF)                                │
│    → Saves to database and cloud storage                        │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 9. Show recommended hospitals & doctors                         │
│    → Filtered by predicted disease expertise                    │
│    → Show: Name, address, fee, rating, availability             │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 10. User selects doctor → Books appointment                     │
│     → Select date/time → Confirm                                │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 11. Payment processing                                          │
│     → Consultation fee: $5-50 depending on doctor               │
│     → Methods: Card, Telebirr, Bank transfer                    │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 12. Booking confirmation                                        │
│     → SMS/Email notification                                     │
│     → QR code for hospital check-in                             │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 13. Feedback request                                            │
│     → "Was our prediction correct?"                             │
│     → Rate AI 1-5 stars                                         │
│     → COGNIMED learns and improves                              │
└─────────────────────────────────────────────────────────────────┘
  │
  ▼
END
```

### 3.2 FACILITY PORTAL (Hospital + Doctor)

#### 3.2.1 Authentication & Role Management

```
┌─────────────────────────────────────────────────────────────────┐
│                    FACILITY LOGIN PAGE                          │
├─────────────────────────────────────────────────────────────────┤
│  Username: [__________________]                                 │
│  Password: [__________________]                                 │
│                                                                 │
│  Role:  ○ Doctor    ○ Management                                │
│                                                                 │
│  [LOGIN]                                    [Forgot Password?]  │
└─────────────────────────────────────────────────────────────────┘
```

#### 3.2.2 Management Dashboard (Hospital Admin)

| Feature | Description | Actions |
|---------|-------------|---------|
| Hospital Profile | Name, address, facilities, departments | Edit, Update |
| Doctor Management | List all doctors, add, remove, edit | Add, Edit, Delete, Block |
| Appointment Overview | Today, upcoming, completed, cancelled | View, Reschedule, Cancel |
| Booking Calendar | Visual calendar of all appointments | Filter by doctor, date |
| Revenue Dashboard | Daily/weekly/monthly earnings | Export reports |
| Payment Transactions | List of all payments received | View, Refund |
| Schedule Settings | Hospital operating hours, holidays | Set, Modify |
| Patient Feedback | Ratings and comments about hospital | View, Respond |
| Analytics | Patient volume, popular doctors, peak hours | Charts, Export |

#### 3.2.3 Doctor Dashboard

| Feature | Description | Actions |
|---------|-------------|---------|
| My Appointments | Today's schedule, upcoming, past | View, Confirm, Cancel |
| Patient List | Assigned patients with details | View profile, Medical history |
| Patient Medical History | Past symptoms, AI predictions, previous diagnoses | View, Add notes |
| COGNIMED AI Assistant | Dictate symptoms, get AI suggestions | Voice input, Text input |
| Prescription Writer | Digital prescriptions | Create, Print, Share |
| Surgery Scheduling | Book OT, date, time, duration | Create, Reschedule, Cancel |
| Consultation Notes | Add medical notes per patient | Save, Edit, Delete |
| Availability Toggle | Set available/unavailable status | On/Off, Set hours |
| Profile | Specialization, fees, experience | Edit, Update |
| Earnings | Consultation fees collected | View, Withdraw request |

#### 3.2.4 Doctor-AI Assistant (COGNIMED for Doctors)

```
┌─────────────────────────────────────────────────────────────────┐
│                    COGNIMED DOCTOR ASSISTANT                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  [🎤] Speak or Type symptoms...                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ "Patient has dry cough, wheezing, and chest tightness"  │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ COGNIMED ANALYSIS:                                       │   │
│  │                                                          │   │
│  │ Possible Diagnoses:                                      │   │
│  │ • Asthma - 82% confidence                                │   │
│  │ • Allergic Reaction - 76% confidence                     │   │
│  │ • GERD - 68% (below threshold)                           │   │
│  │                                                          │   │
│  │ Suggested Questions for Patient:                         │   │
│  │ 1. "When does chest tightness worsen?"                   │   │
│  │ 2. "Any known allergies?"                                │   │
│  │ 3. "Does it worsen at night?"                            │   │
│  │                                                          │   │
│  │ Recommended Tests:                                       │   │
│  │ • Peak flow meter                                        │   │
│  │ • Allergy test                                           │   │
│  │                                                          │   │
│  │ [Add to Notes] [Schedule Follow-up] [Prescribe]          │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 3.3 ADMIN PORTAL

#### 3.3.1 Complete Admin Controls

| Category | Feature | Description |
|----------|---------|-------------|
| Hospital Management | View all hospitals | List, search, filter |
| | Block/Unblock hospital | Suspend operations |
| | Add hospital manually | Create new hospital profile |
| | Edit hospital details | Update any information |
| Doctor Management | View all doctors | List, search, filter by hospital |
| | Block/Unblock doctor | Suspend license |
| | Verify doctor credentials | Approve/reject applications |
| | Transfer doctor | Move to different hospital |
| Patient Management | View all patients | List, search |
| | Suspend account | Block user access |
| | View medical history | Access any patient report |
| Payment Monitoring | All transactions | Complete payment history |
| | Issue refunds | Process refund requests |
| | Payment analytics | Charts, reports, export |
| AI Model Control | Switch AI provider | Internal/OpenAI/Google |
| | Set confidence threshold | Default 75%, can adjust |
| | View model accuracy | Current performance metrics |
| | Trigger retraining | Force model update |
| Dataset Management | Upload training data | CSV/JSON files |
| | View dataset stats | Size, distribution, quality |
| | Export anonymized data | For research/backup |
| Ethical Filter | Blocked topics | Add/remove blocked keywords |
| | Emergency keywords | Configure emergency triggers |
| | Disclaimer text | Edit medical disclaimer |
| External Integrations | Payment gateway | Enable/disable, configure |
| | SMS service | Enable/disable, API keys |
| | Email service | SMTP settings |
| | Speech-to-text | Google STT API keys |
| Audit Logs | All admin actions | Complete history |
| | User activity logs | Track usage patterns |
| | System errors | Error logs, debugging |
| System Settings | Maintenance mode | Turn on/off |
| | Backup | Manual backup trigger |
| | Health check | System status dashboard |

#### 3.3.2 Admin Dashboard Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  ADMIN DASHBOARD - MEDIMATCH                                    [Admin Name] │
├─────────────────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│ │ HOSPITALS   │ │  DOCTORS    │ │  PATIENTS   │ │  PAYMENTS   │            │
│ │     24      │ │    156      │ │   12,847    │ │  $45,231    │            │
│ │ +2 this week│ │ +8 this week│ │ +340 today  │ │ +$5,620     │            │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘            │
│                                                                             │
│ ┌──────────────────────────────────┐ ┌──────────────────────────────────┐  │
│ │ AI MODEL STATUS                   │ │ SYSTEM HEALTH                     │  │
│ ├──────────────────────────────────┤ ├──────────────────────────────────┤  │
│ │ Model: COGNIMED v2.3             │ │ API: 🟢 Operational               │  │
│ │ Provider: Internal               │ │ DB: 🟢 Connected                  │  │
│ │ Accuracy: 84.7% ▲ +2.1%          │ │ Redis: 🟢 Connected               │  │
│ │ Confidence Threshold: 75%        │ │ Storage: 🟢 234GB/500GB           │  │
│ │ Last Trained: 2025-04-20         │ │ Uptime: 99.97%                    │  │
│ │ [Retrain Now] [Switch Provider]  │ │ [Run Diagnostics]                 │  │
│ └──────────────────────────────────┘ └──────────────────────────────────┘  │
│                                                                             │
│ ┌──────────────────────────────────────────────────────────────────────────┐ │
│ │ RECENT ACTIVITY                                                          │ │
│ ├──────────────────────────────────────────────────────────────────────────┤ │
│ │ 10:32 - Admin blocked hospital "City Medical Center" - Violation         │ │
│ │ 09:15 - New AI training completed - Accuracy improved to 84.7%          │ │
│ │ 08:45 - Payment gateway switched to production mode                      │ │
│ │ Yesterday - 342 new patient registrations                                │ │
│ └──────────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## SECTION 4: COGNIMED AI ENGINE - COMPLETE DETAILS

### 4.1 AI Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         COGNIMED AI ARCHITECTURE                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                        INPUT PROCESSING LAYER                        │   │
│  ├─────────────────┬─────────────────┬─────────────────────────────────┤   │
│  │ Language        │ Grammar         │ Intent                          │   │
│  │ Detection       │ Correction      │ Understanding                   │   │
│  │ (FastText)      │ (Hunspell)      │ (spaCy + Custom NER)            │   │
│  └─────────────────┴─────────────────┴─────────────────────────────────┘   │
│                                    │                                        │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      CONVERSATION MANAGEMENT                         │   │
│  ├─────────────────────────────────────────────────────────────────────┤   │
│  │ • Context tracking (Rasa / LangChain)                               │   │
│  │ • Follow-up question generation                                     │   │
│  │ • Turn management (2-5 rounds)                                      │   │
│  │ • Session memory (Redis)                                            │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      PREDICTION ENGINE                               │   │
│  ├─────────────────────────────────────────────────────────────────────┤   │
│  │ • Symptom → Disease mapping (Random Forest/XGBoost)                 │   │
│  │ • Confidence scoring (Softmax probability)                          │   │
│  │ • Ensemble model (Multiple classifiers)                             │   │
│  │ • Ethical threshold enforcement (≥75%)                              │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      LEARNING & PERSONALIZATION                      │   │
│  ├─────────────────────────────────────────────────────────────────────┤   │
│  │ • User history storage (PostgreSQL)                                 │   │
│  │ • Feedback integration (ratings, corrections)                       │   │
│  │ • Retraining pipeline (Airflow/Celery)                              │   │
│  │ • Personalized weighting                                            │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      OUTPUT GENERATION                               │   │
│  ├─────────────────────────────────────────────────────────────────────┤   │
│  │ • Diagnosis text                                                    │   │
│  │ • Recommendations (medication, rest, hydration)                     │   │
│  │ • Follow-up suggestions                                             │   │
│  │ • Hospital/doctor matching                                          │   │
│  │ • Medical report (PDF)                                              │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Dataset Structure

#### 4.2.1 File 1: symptom_disease.csv

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| symptom_list | string | Comma-separated symptoms | "fever,cough,sore throat" |
| disease | string | Predicted disease | "Viral infection" |
| confidence_weight | float | Base confidence (0-1) | 0.85 |
| severity | integer | 1-10 scale | 3 |
| urgency | string | Low/Medium/High | "Low" |
| recommended_specialty | string | Doctor type | "General Physician" |
| common_medications | string | OTC drugs | "Paracetamol, Rest" |
| home_remedies | string | Non-medical care | "Hydration, Warm fluids" |

#### 4.2.2 File 2: disease_questions.csv

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| primary_symptom | string | Main symptom | "fever" |
| next_question | string | Follow-up question | "How many days have you had fever?" |
| question_order | integer | Sequence number | 1 |
| expected_answer_type | string | numeric/boolean/text | "numeric" |
| weight_in_prediction | float | Importance for diagnosis | 0.7 |

#### 4.2.3 File 3: ethical_constraints.json

```json
{
  "blocked_topics": [
    "politics",
    "religion", 
    "violence",
    "finance",
    "gambling",
    "entertainment",
    "sports"
  ],
  "min_confidence_percent": 75,
  "emergency_keywords": [
    "heart attack",
    "stroke", 
    "severe bleeding",
    "unconscious",
    "difficulty breathing",
    "chest pain",
    "severe burn",
    "seizure"
  ],
  "disclaimer": "This is an AI-generated suggestion and not a medical diagnosis. Please consult a qualified doctor for proper medical advice.",
  "emergency_message": "⚠️ Your symptoms require immediate medical attention. Please call emergency services (911) or go to the nearest hospital immediately.",
  "low_confidence_message": "I'm not confident enough (below 75%) about your condition. Please consult a doctor directly for accurate diagnosis."
}
```

#### 4.2.4 File 4: doctor_ai_responses.json

```json
{
  "symptom_patterns": [
    {
      "symptoms": ["dry cough", "wheezing", "chest tightness"],
      "suggested_diagnosis": ["Asthma", "Allergic Reaction"],
      "follow_up_questions": [
        "When does chest tightness worsen?",
        "Any known allergies?",
        "Does exercise trigger symptoms?"
      ],
      "recommended_tests": ["Peak flow meter", "Allergy test"],
      "suggested_medications": ["Inhaler", "Antihistamines"]
    }
  ]
}
```

### 4.3 Confidence Calculation Logic

```python
def calculate_confidence(symptoms, user_history, conversation_turns):
    """
    Calculate confidence score for disease prediction
    
    Factors:
    1. Symptom match strength (60% weight)
    2. Follow-up question answers (25% weight)
    3. User history relevance (10% weight)
    4. Data quality (5% weight)
    """
    
    base_confidence = model.predict_proba(symptoms)
    
    # Adjust based on follow-up answers
    answer_quality = calculate_answer_relevance(conversation_turns)
    
    # Personalization factor (returning users)
    personalization = 0
    if user_history:
        similar_symptoms = find_similar_past_cases(user_history, symptoms)
        personalization = calculate_similarity_score(similar_symptoms)
    
    final_confidence = (
        base_confidence * 0.60 +
        answer_quality * 0.25 +
        personalization * 0.10 +
        data_quality_score * 0.05
    )
    
    return final_confidence * 100  # Return as percentage
```

### 4.4 Learning & Retraining Pipeline

#### 4.4.1 Data Collection Points

| Source | Data Collected | Frequency |
|--------|----------------|-----------|
| Patient chats | Symptoms, AI questions, user answers | Real-time |
| User feedback | Ratings, correct/incorrect, actual condition | After each session |
| Doctor corrections | Final diagnosis vs AI prediction | After consultation |
| Booking data | If user booked after AI recommendation | Real-time |

#### 4.4.2 Retraining Triggers

| Trigger | Threshold | Action |
|---------|-----------|--------|
| Feedback count | 100 new entries | Incremental training |
| Accuracy drop | <70% | Full retraining |
| Time-based | Every 7 days | Scheduled training |
| Admin manual | Button click | Force retraining |
| Dataset upload | New CSV added | Immediate retraining |

#### 4.4.3 Retraining Pipeline Steps

```
Step 1: Collect new data
   ↓
Step 2: Data cleaning & validation
   ↓
Step 3: Feature extraction
   ↓
Step 4: Train/validation split (80/20)
   ↓
Step 5: Model training (Random Forest/XGBoost)
   ↓
Step 6: Evaluate accuracy on validation set
   ↓
Step 7: If accuracy improved → Deploy model
   ↓
Step 8: Log new model version
   ↓
Step 9: Update version in production
```

### 4.5 Personalization Logic

```python
def personalize_prediction(user_id, current_symptoms):
    """
    Adjust prediction based on user's historical data
    """
    
    # Fetch user history (last 90 days)
    user_history = db.get_user_history(user_id, days=90)
    
    if len(user_history) < 3:
        return base_prediction  # Not enough data
    
    # Calculate symptom frequency for this user
    symptom_frequency = {}
    for session in user_history:
        for symptom in session.symptoms:
            symptom_frequency[symptom] = symptom_frequency.get(symptom, 0) + 1
    
    # Weight common symptoms higher
    weighted_symptoms = {}
    for symptom in current_symptoms:
        weight = 1 + (symptom_frequency.get(symptom, 0) / len(user_history))
        weighted_symptoms[symptom] = weight
    
    # Adjust prediction based on weighted symptoms
    personalized_prediction = model.predict_weighted(weighted_symptoms)
    
    # Increase confidence if pattern matches history
    if personalized_prediction.disease in user_history.common_diseases:
        personalized_prediction.confidence += 5
    
    return personalized_prediction
```

---

## SECTION 5: DATABASE DESIGN - FULL SCHEMA

### 5.1 PostgreSQL Schema

#### 5.1.1 Users Table

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(20) NOT NULL CHECK (role IN ('patient', 'doctor', 'hospital_management', 'admin')),
    language_pref VARCHAR(2) DEFAULT 'en',
    is_active BOOLEAN DEFAULT TRUE,
    is_blocked BOOLEAN DEFAULT FALSE,
    blocked_reason TEXT,
    last_login TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
CREATE INDEX idx_users_is_blocked ON users(is_blocked);
```

#### 5.1.2 Patients Table

```sql
CREATE TABLE patients (
    id UUID PRIMARY KEY REFERENCES users(id) ON DELETE CASCADE,
    full_name VARCHAR(255) NOT NULL,
    age INTEGER CHECK (age >= 0 AND age <= 150),
    sex VARCHAR(10) CHECK (sex IN ('male', 'female', 'other')),
    phone VARCHAR(20),
    emergency_contact VARCHAR(20),
    medical_history TEXT,
    allergies TEXT[],
    chronic_conditions TEXT[],
    blood_type VARCHAR(3),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_patients_full_name ON patients(full_name);
CREATE INDEX idx_patients_phone ON patients(phone);
```

#### 5.1.3 Hospitals Table

```sql
CREATE TABLE hospitals (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    address TEXT NOT NULL,
    city VARCHAR(100),
    latitude DECIMAL(10,8),
    longitude DECIMAL(11,8),
    phone VARCHAR(20),
    email VARCHAR(255),
    facilities TEXT[],
    departments TEXT[],
    operating_hours JSONB,
    rating DECIMAL(3,2) DEFAULT 0,
    total_reviews INTEGER DEFAULT 0,
    is_blocked BOOLEAN DEFAULT FALSE,
    blocked_reason TEXT,
    payment_details JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_hospitals_name ON hospitals(name);
CREATE INDEX idx_hospitals_city ON hospitals(city);
CREATE INDEX idx_hospitals_is_blocked ON hospitals(is_blocked);
CREATE INDEX idx_hospitals_location ON hospitals(latitude, longitude);
```

#### 5.1.4 Doctors Table

```sql
CREATE TABLE doctors (
    id UUID PRIMARY KEY REFERENCES users(id) ON DELETE CASCADE,
    hospital_id UUID REFERENCES hospitals(id) ON DELETE SET NULL,
    full_name VARCHAR(255) NOT NULL,
    specialization VARCHAR(100) NOT NULL,
    sub_specialties TEXT[],
    consultation_fee DECIMAL(10,2) NOT NULL,
    experience_years INTEGER DEFAULT 0,
    qualification TEXT,
    biography TEXT,
    available_slots JSONB, -- Daily time slots
    is_available BOOLEAN DEFAULT TRUE,
    is_verified BOOLEAN DEFAULT FALSE,
    is_blocked BOOLEAN DEFAULT FALSE,
    blocked_reason TEXT,
    rating DECIMAL(3,2) DEFAULT 0,
    total_reviews INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_doctors_hospital_id ON doctors(hospital_id);
CREATE INDEX idx_doctors_specialization ON doctors(specialization);
CREATE INDEX idx_doctors_is_available ON doctors(is_available);
CREATE INDEX idx_doctors_is_blocked ON doctors(is_blocked);
```

#### 5.1.5 Symptom Sessions Table

```sql
CREATE TABLE symptom_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    patient_id UUID REFERENCES patients(id) ON DELETE CASCADE,
    session_id VARCHAR(100) UNIQUE NOT NULL,
    ai_provider_used VARCHAR(50),
    language_used VARCHAR(2) DEFAULT 'en',
    symptoms_text TEXT,
    predicted_disease VARCHAR(255),
    confidence DECIMAL(5,2) CHECK (confidence >= 0 AND confidence <= 100),
    recommendation TEXT,
    final_diagnosis VARCHAR(255), -- From doctor feedback
    severity VARCHAR(20),
    was_emergency BOOLEAN DEFAULT FALSE,
    completed_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_sessions_patient_id ON symptom_sessions(patient_id);
CREATE INDEX idx_sessions_session_id ON symptom_sessions(session_id);
CREATE INDEX idx_sessions_created_at ON symptom_sessions(created_at);
```

#### 5.1.6 Conversation Turns Table

```sql
CREATE TABLE conversation_turns (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id UUID REFERENCES symptom_sessions(id) ON DELETE CASCADE,
    turn_number INTEGER NOT NULL,
    ai_question TEXT,
    patient_answer TEXT,
    answer_type VARCHAR(20), -- text, numeric, boolean
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_conversation_session_id ON conversation_turns(session_id);
```

#### 5.1.7 Appointments Table

```sql
CREATE TABLE appointments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    patient_id UUID REFERENCES patients(id) ON DELETE CASCADE,
    doctor_id UUID REFERENCES doctors(id) ON DELETE CASCADE,
    hospital_id UUID REFERENCES hospitals(id) ON DELETE CASCADE,
    session_id UUID REFERENCES symptom_sessions(id),
    scheduled_time TIMESTAMP NOT NULL,
    duration_minutes INTEGER DEFAULT 30,
    status VARCHAR(20) DEFAULT 'pending' CHECK (status IN ('pending', 'confirmed', 'completed', 'cancelled', 'no_show')),
    payment_status VARCHAR(20) DEFAULT 'pending' CHECK (payment_status IN ('pending', 'paid', 'refunded', 'failed')),
    fee_paid DECIMAL(10,2),
    payment_method VARCHAR(50),
    transaction_id VARCHAR(255),
    cancellation_reason TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_appointments_patient_id ON appointments(patient_id);
CREATE INDEX idx_appointments_doctor_id ON appointments(doctor_id);
CREATE INDEX idx_appointments_hospital_id ON appointments(hospital_id);
CREATE INDEX idx_appointments_scheduled_time ON appointments(scheduled_time);
CREATE INDEX idx_appointments_status ON appointments(status);
```

#### 5.1.8 Surgeries Table

```sql
CREATE TABLE surgeries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    appointment_id UUID REFERENCES appointments(id) ON DELETE CASCADE,
    doctor_id UUID REFERENCES doctors(id),
    patient_id UUID REFERENCES patients(id),
    surgery_type VARCHAR(255) NOT NULL,
    ot_room VARCHAR(50),
    scheduled_date DATE NOT NULL,
    scheduled_time TIME NOT NULL,
    duration_hours DECIMAL(3,1),
    status VARCHAR(20) DEFAULT 'scheduled' CHECK (status IN ('scheduled', 'in_progress', 'completed', 'cancelled')),
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_surgeries_appointment_id ON surgeries(appointment_id);
CREATE INDEX idx_surgeries_scheduled_date ON surgeries(scheduled_date);
```

#### 5.1.9 Reports Table

```sql
CREATE TABLE reports (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id UUID REFERENCES symptom_sessions(id) ON DELETE CASCADE,
    appointment_id UUID REFERENCES appointments(id),
    pdf_url VARCHAR(500),
    report_data JSONB,
    file_size_bytes INTEGER,
    downloaded_count INTEGER DEFAULT 0,
    generated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_reports_session_id ON reports(session_id);
CREATE INDEX idx_reports_appointment_id ON reports(appointment_id);
```

#### 5.1.10 Feedback Table

```sql
CREATE TABLE feedback (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id UUID REFERENCES symptom_sessions(id) ON DELETE CASCADE,
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    was_correct BOOLEAN,
    actual_condition VARCHAR(255),
    rating INTEGER CHECK (rating >= 1 AND rating <= 5),
    comment TEXT,
    used_for_training BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_feedback_session_id ON feedback(session_id);
CREATE INDEX idx_feedback_user_id ON feedback(user_id);
CREATE INDEX idx_feedback_rating ON feedback(rating);
CREATE INDEX idx_feedback_used_for_training ON feedback(used_for_training);
```

#### 5.1.11 Payments Table

```sql
CREATE TABLE payments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    appointment_id UUID REFERENCES appointments(id) ON DELETE CASCADE,
    user_id UUID REFERENCES users(id),
    amount DECIMAL(10,2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'ETB',
    payment_method VARCHAR(50),
    gateway_transaction_id VARCHAR(255),
    gateway_response JSONB,
    status VARCHAR(20) DEFAULT 'pending' CHECK (status IN ('pending', 'success', 'failed', 'refunded')),
    refund_id VARCHAR(255),
    refund_reason TEXT,
    processed_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_payments_appointment_id ON payments(appointment_id);
CREATE INDEX idx_payments_status ON payments(status);
CREATE INDEX idx_payments_gateway_transaction_id ON payments(gateway_transaction_id);
```

#### 5.1.12 External Integrations Log Table

```sql
CREATE TABLE external_integrations_log (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    api_name VARCHAR(100) NOT NULL,
    endpoint VARCHAR(255),
    request_payload JSONB,
    response_payload JSONB,
    latency_ms INTEGER,
    status_code INTEGER,
    error_message TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_external_log_api_name ON external_integrations_log(api_name);
CREATE INDEX idx_external_log_created_at ON external_integrations_log(created_at);
```

#### 5.1.13 Audit Logs Table (Admin)

```sql
CREATE TABLE audit_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    admin_id UUID REFERENCES users(id),
    action VARCHAR(255) NOT NULL,
    target_type VARCHAR(50), -- hospital, doctor, patient, payment
    target_id UUID,
    old_values JSONB,
    new_values JSONB,
    ip_address INET,
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_audit_admin_id ON audit_logs(admin_id);
CREATE INDEX idx_audit_created_at ON audit_logs(created_at);
CREATE INDEX idx_audit_action ON audit_logs(action);
```

### 5.2 Redis Cache Schema

| Key Pattern | Type | TTL | Purpose |
|-------------|------|-----|---------|
| session:{sessionId} | Hash | 24h | Active AI chat session |
| user:{userId}:history | List | 1h | Cached user consultation history |
| hospital:{hospitalId}:doctors | Set | 15min | Available doctors list |
| appointment:lock:{appointmentId} | String | 5min | Prevent double booking |
| ai:model:version | String | Permanent | Current AI model version |
| feedback:queue | List | - | Pending feedback for training |
| rate_limit:{ip} | String | 1min | API rate limiting |
| otp:{phone} | String | 5min | OTP verification codes |

---

## SECTION 6: API SPECIFICATIONS - COMPLETE ENDPOINTS

### 6.1 Patient Module APIs

#### 6.1.1 AI Chat

```
POST /api/v1/patient/ai/chat

Description: Send message to COGNIMED AI and get response

Request Headers:
  Authorization: Bearer {jwt_token}
  Content-Type: application/json

Request Body:
{
  "message": "I have fever and headache for 2 days",
  "sessionId": "session_123456",
  "language": "en",
  "conversationHistory": [
    {
      "role": "user",
      "content": "I have fever",
      "timestamp": "2025-04-25T10:00:00Z"
    },
    {
      "role": "assistant", 
      "content": "How long have you had fever?",
      "timestamp": "2025-04-25T10:00:05Z"
    }
  ],
  "userId": "user_123",
  "learningMode": true
}

Response (Success - 200):
{
  "success": true,
  "reply": "Based on fever and headache for 2 days, you may have a viral infection (85% confidence). Do you have any cough?",
  "confidence": 85,
  "hasPrediction": false,
  "needsMoreInfo": true,
  "followUpQuestion": "Do you have any cough?",
  "sessionId": "session_123456",
  "usedPersonalized": false
}

Response (Prediction Ready - 200):
{
  "success": true,
  "reply": "I am 85% confident that you have Viral Infection. I recommend rest, hydration, and paracetamol.",
  "confidence": 85,
  "hasPrediction": true,
  "disease": "Viral Infection",
  "recommendation": "Rest, hydration, paracetamol",
  "severity": "Low",
  "sessionId": "session_123456"
}

Response (Low Confidence - 200):
{
  "success": true,
  "reply": "I'm not confident enough (below 75%) about your condition. Please consult a doctor directly.",
  "confidence": 65,
  "hasPrediction": false,
  "showHospitals": true,
  "sessionId": "session_123456"
}

Response (Emergency - 200):
{
  "success": true,
  "reply": "⚠️ Your symptoms require immediate medical attention. Please call emergency services (911) immediately.",
  "isEmergency": true,
  "emergencyMessage": "Symptoms indicate possible cardiac issue",
  "sessionId": "session_123456"
}

Error Responses:
400 Bad Request: Invalid input
401 Unauthorized: Invalid or missing token
429 Too Many Requests: Rate limit exceeded
500 Internal Server Error: AI service unavailable
```

#### 6.1.2 Generate Report

```
POST /api/v1/patient/ai/generate-report

Description: Generate medical report after AI prediction

Request Headers:
  Authorization: Bearer {jwt_token}
  Content-Type: application/json

Request Body:
{
  "prediction": {
    "disease": "Viral Infection",
    "confidence": 85,
    "recommendation": "Rest, hydration, paracetamol"
  },
  "patientInfo": {
    "fullName": "John Doe",
    "age": 30,
    "sex": "male",
    "onsetDate": "2025-04-23",
    "contact": "+251912345678"
  },
  "sessionId": "session_123456",
  "userId": "user_123",
  "conversationHistory": []
}

Response (Success - 200):
{
  "success": true,
  "reportId": "report_123456",
  "pdfUrl": "https://storage.medimatch.com/reports/report_123456.pdf",
  "reportData": {
    "patientName": "John Doe",
    "age": 30,
    "sex": "male",
    "predictedDisease": "Viral Infection",
    "confidence": 85,
    "recommendation": "Rest, hydration, paracetamol",
    "generatedDate": "2025-04-25T10:30:00Z",
    "disclaimer": "This is an AI-generated suggestion, not a medical diagnosis."
  },
  "shareableLink": "https://medimatch.com/report/report_123456"
}

Error Responses:
400 Bad Request: Missing required fields
401 Unauthorized: Invalid token
500 Internal Server Error: Report generation failed
```

#### 6.1.3 Get Hospitals by Disease

```
GET /api/v1/patient/hospitals/disease/{disease}

Description: Get list of hospitals with doctors specializing in given disease

Request Headers:
  Authorization: Bearer {jwt_token}

Path Parameters:
  disease: string (URL encoded)

Query Parameters:
  lat: float (optional) - User latitude for distance sorting
  lng: float (optional) - User longitude
  limit: integer (default: 20)
  offset: integer (default: 0)

Response (Success - 200):
{
  "success": true,
  "total": 15,
  "hospitals": [
    {
      "id": "hospital_123",
      "name": "City General Hospital",
      "address": "123 Main St, Addis Ababa",
      "distance": 2.5,
      "rating": 4.5,
      "consultationFee": 500,
      "doctors": [
        {
          "id": "doctor_456",
          "name": "Dr. Sarah Abebe",
          "specialization": "Infectious Disease",
          "experience": 8,
          "availability": ["10:00", "11:00", "14:00"],
          "fee": 500
        }
      ]
    }
  ]
}
```

#### 6.1.4 Book Appointment

```
POST /api/v1/patient/appointments/book

Request Body:
{
  "doctorId": "doctor_456",
  "hospitalId": "hospital_123",
  "scheduledTime": "2025-04-26T10:00:00Z",
  "sessionId": "session_123456",
  "patientId": "patient_789"
}

Response:
{
  "success": true,
  "appointmentId": "apt_123456",
  "status": "pending",
  "fee": 500,
  "paymentRequired": true,
  "paymentLink": "https://payment.medimatch.com/pay/apt_123456"
}
```

#### 6.1.5 Initiate Payment

```
POST /api/v1/patient/payments/initiate

Request Body:
{
  "appointmentId": "apt_123456",
  "paymentMethod": "telebirr",
  "amount": 500
}

Response:
{
  "success": true,
  "transactionId": "txn_123456",
  "paymentUrl": "https://telebirr.com/pay/...",
  "expiresAt": "2025-04-25T11:00:00Z"
}
```

#### 6.1.6 Verify Payment

```
GET /api/v1/patient/payments/verify/{transactionId}

Response:
{
  "success": true,
  "status": "completed",
  "appointmentId": "apt_123456",
  "paymentStatus": "paid",
  "receiptUrl": "https://storage.medimatch.com/receipts/receipt_123456.pdf"
}
```

### 6.2 Facility Module APIs

#### 6.2.1 Facility Login

```
POST /api/v1/facility/auth/login

Request Body:
{
  "username": "doctor@cityhospital.com",
  "password": "********",
  "role": "doctor"
}

Response:
{
  "success": true,
  "token": "jwt_token_here",
  "user": {
    "id": "user_123",
    "name": "Dr. Sarah Abebe",
    "email": "doctor@cityhospital.com",
    "role": "doctor",
    "hospitalId": "hospital_123",
    "hospitalName": "City General Hospital"
  },
  "permissions": ["view_appointments", "view_patients", "use_ai_assistant"]
}
```

#### 6.2.2 Get Doctor Appointments

```
GET /api/v1/facility/doctor/appointments

Query Parameters:
  date: string (YYYY-MM-DD) - Optional
  status: string (pending/confirmed/completed) - Optional

Response:
{
  "success": true,
  "appointments": [
    {
      "id": "apt_123",
      "patientName": "John Doe",
      "patientAge": 30,
      "scheduledTime": "2025-04-26T10:00:00Z",
      "status": "confirmed",
      "symptoms": "Fever, cough, headache",
      "aiPrediction": "Viral Infection (85%)",
      "reports": ["report_123.pdf"]
    }
  ]
}
```

#### 6.2.3 Doctor AI Assistant

```
POST /api/v1/facility/doctor/ai/assist

Request Body:
{
  "symptoms": "Patient has dry cough, wheezing, chest tightness",
  "patientHistory": "Previous asthma diagnosis",
  "mode": "dictation"
}

Response:
{
  "success": true,
  "suggestedDiagnoses": [
    {"disease": "Asthma", "confidence": 82},
    {"disease": "Allergic Reaction", "confidence": 76}
  ],
  "followUpQuestions": [
    "When does chest tightness worsen?",
    "Any known allergies?"
  ],
  "recommendedTests": ["Peak flow meter", "Allergy test"],
  "suggestedMedications": ["Inhaler", "Antihistamines"],
  "notesToAdd": "Patient presenting with classic asthma symptoms"
}
```

#### 6.2.4 Schedule Surgery

```
POST /api/v1/facility/doctor/surgery/schedule

Request Body:
{
  "appointmentId": "apt_123",
  "patientId": "patient_456",
  "surgeryType": "Appendectomy",
  "otRoom": "OT-3",
  "scheduledDate": "2025-04-28",
  "scheduledTime": "09:00",
  "durationHours": 1.5,
  "notes": "Pre-op tests completed"
}

Response:
{
  "success": true,
  "surgeryId": "surg_123456",
  "status": "scheduled",
  "confirmationMessage": "Surgery scheduled successfully"
}
```

### 6.3 Admin Module APIs

#### 6.3.1 Get All Hospitals

```
GET /api/v1/admin/hospitals

Query Parameters:
  page: integer (default: 1)
  limit: integer (default: 20)
  is_blocked: boolean (optional)
  search: string (optional)

Response:
{
  "success": true,
  "total": 50,
  "hospitals": [
    {
      "id": "hospital_123",
      "name": "City General Hospital",
      "address": "123 Main St",
      "doctorsCount": 25,
      "totalAppointments": 1250,
      "totalRevenue": 625000,
      "is_blocked": false,
      "created_at": "2024-01-15T00:00:00Z"
    }
  ]
}
```

#### 6.3.2 Block/Unblock Hospital

```
PUT /api/v1/admin/hospitals/{hospitalId}/block

Request Body:
{
  "is_blocked": true,
  "reason": "License expired - waiting for renewal"
}

Response:
{
  "success": true,
  "message": "Hospital blocked successfully",
  "hospitalId": "hospital_123",
  "is_blocked": true
}
```

#### 6.3.3 Switch AI Provider

```
PUT /api/v1/admin/ai/model/switch

Request Body:
{
  "provider": "openai",  // Options: internal, openai, google
  "apiKey": "sk-...",    // Required for external providers
  "model": "gpt-4-turbo", // Optional
  "fallbackToInternal": true
}

Response:
{
  "success": true,
  "activeProvider": "openai",
  "status": "connected",
  "latency_ms": 245,
  "message": "AI provider switched to OpenAI"
}
```

#### 6.3.4 Upload Training Dataset

```
POST /api/v1/admin/ai/dataset/upload

Content-Type: multipart/form-data

Request Body:
  file: (CSV file)
  dataset_type: "symptoms" or "questions"
  replace_existing: false

Response:
{
  "success": true,
  "rows_imported": 1250,
  "validation_status": "passed",
  "retraining_triggered": true,
  "estimated_completion": "2025-04-25T15:00:00Z"
}
```

#### 6.3.5 Get AI Statistics

```
GET /api/v1/admin/ai/stats

Response:
{
  "success": true,
  "stats": {
    "model_version": "v2.3",
    "provider": "internal",
    "accuracy": 84.7,
    "avg_confidence": 82.3,
    "total_predictions": 12500,
    "predictions_above_threshold": 10875,
    "predictions_below_threshold": 1625,
    "feedback_received": 3420,
    "avg_rating": 4.2,
    "last_trained": "2025-04-20T00:00:00Z",
    "next_scheduled_training": "2025-04-27T00:00:00Z"
  }
}
```

### 6.4 AI Learning Module APIs

#### 6.4.1 Submit Feedback

```
POST /api/v1/ai-learning/feedback

Request Body:
{
  "sessionId": "session_123456",
  "predictionId": "Viral Infection",
  "wasCorrect": true,
  "actualCondition": "Viral Infection",
  "rating": 5,
  "comment": "Very accurate prediction",
  "userId": "user_123"
}

Response:
{
  "success": true,
  "feedbackId": "fb_123456",
  "addedToTrainingQueue": true,
  "message": "Thank you for your feedback! COGNIMED will learn from this."
}
```

#### 6.4.2 Get User History

```
GET /api/v1/ai-learning/user/{userId}/history

Response:
{
  "success": true,
  "history": [
    {
      "sessionId": "session_123",
      "date": "2025-04-20",
      "symptoms": "fever, cough",
      "predictedCondition": "Viral Infection",
      "confidence": 85,
      "wasCorrect": true,
      "rating": 5,
      "bookingMade": true
    }
  ],
  "totalSessions": 12,
  "commonSymptoms": ["fever", "cough", "headache"],
  "commonDiagnoses": ["Viral Infection", "Common Cold"]
}
```

#### 6.4.3 Trigger Retraining

```
POST /api/v1/ai-learning/retrain

Request Body: {} (empty)

Response:
{
  "success": true,
  "trainingId": "train_123456",
  "samplesUsed": 1250,
  "estimatedDuration": "30 minutes",
  "status": "started"
}
```

---

## SECTION 7: SECURITY & COMPLIANCE FRAMEWORK

### 7.1 Authentication & Authorization

| Layer | Implementation |
|-------|----------------|
| Password Hashing | bcrypt (salt rounds: 12) |
| JWT Tokens | Expiry: 24 hours, Refresh token: 7 days |
| Session Management | Redis storage, automatic logout after inactivity |
| MFA | Optional SMS/Email OTP for sensitive actions |
| Role-Based Access | Predefined roles with granular permissions |

#### 7.1.1 Role Permissions Matrix

| Permission | Patient | Doctor | Management | Admin |
|------------|---------|--------|------------|-------|
| View own profile | ✅ | ✅ | ✅ | ✅ |
| Edit own profile | ✅ | ✅ | ✅ | ✅ |
| View medical history | ✅ (own) | ✅ (patients) | ❌ | ✅ (all) |
| Use AI symptom checker | ✅ | ❌ | ❌ | ❌ |
| Use AI doctor assistant | ❌ | ✅ | ❌ | ✅ |
| Book appointment | ✅ | ❌ | ❌ | ❌ |
| Cancel appointment | ✅ (own) | ✅ (assigned) | ✅ (hospital) | ✅ (any) |
| View hospital revenue | ❌ | ❌ | ✅ | ✅ |
| Manage doctors | ❌ | ❌ | ✅ | ✅ |
| Block/unblock users | ❌ | ❌ | ❌ | ✅ |
| View audit logs | ❌ | ❌ | ❌ | ✅ |
| Modify AI model | ❌ | ❌ | ❌ | ✅ |

### 7.2 Data Encryption

| Data Type | Encryption | Key Management |
|-----------|------------|----------------|
| Passwords | bcrypt | Application-level |
| PII (names, emails) | AES-256 | AWS KMS |
| Medical records | AES-256 | Customer-managed key |
| Payment info | PCI DSS compliant | Payment gateway |
| API keys | AES-256 | Environment variables |
| Backups | AES-256 | AWS S3 server-side |

### 7.3 API Security

```
┌─────────────────────────────────────────────────────────────────┐
│                      API SECURITY LAYERS                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. RATE LIMITING                                               │
│     • 100 requests per minute per IP                            │
│     • 1000 requests per hour per user                           │
│     • 10 AI chat requests per minute                            │
│                                                                 │
│  2. INPUT VALIDATION                                            │
│     • Sanitize all user inputs                                  │
│     • Validate data types                                       │
│     • Max length limits                                         │
│     • SQL injection prevention                                  │
│                                                                 │
│  3. CORS CONFIGURATION                                          │
│     • Whitelist: medimatch.com, app.medimatch.com               │
│     • Methods: GET, POST, PUT, DELETE                           │
│     • Credentials: allowed                                      │
│                                                                 │
│  4. REQUEST/LOGGING                                             │
│     • Log all API requests                                      │
│     • Mask sensitive data                                       │
│     • 90-day retention                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 7.4 Compliance Checklist (HIPAA/DPDP)

| Requirement | Status | Implementation |
|-------------|--------|----------------|
| Data encryption at rest | ✅ | AES-256 on database |
| Data encryption in transit | ✅ | TLS 1.3 |
| Access logs | ✅ | Audit logs table |
| User consent management | ✅ | Consent checkbox + settings |
| Right to access data | ✅ | Export feature in profile |
| Right to deletion | ✅ | Account deletion API |
| Breach notification | ✅ | Automated alerts + admin email |
| Business associate agreement | ✅ | Signed with cloud providers |
| Data minimization | ✅ | Only collect necessary fields |
| Retention policy | ✅ | 7 years for medical records |

---

## SECTION 8: DEPLOYMENT ARCHITECTURE

### 8.1 Infrastructure Setup

#### 8.1.1 Production Environment

```
┌─────────────────────────────────────────────────────────────────────┐
│                         AWS/GCP CLOUD                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────┐    ┌─────────────────┐                        │
│  │   LOAD BALANCER │    │   CDN (CloudFront)                       │
│  │   (Application  │    │   Static Assets │                        │
│  │    Load Balancer│    │                 │                        │
│  └────────┬────────┘    └─────────────────┘                        │
│           │                                                        │
│  ┌────────┴────────────────────────────────────────┐               │
│  │                                                  │               │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐      │               │
│  │  │ API      │  │ API      │  │ API      │      │               │
│  │  │ Server 1 │  │ Server 2 │  │ Server 3 │      │               │
│  │  │ (Node.js)│  │ (Node.js)│  │ (Node.js)│      │               │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘      │               │
│  │       │             │             │            │               │
│  └───────┼─────────────┼─────────────┼────────────┘               │
│          │             │             │                            │
│          └─────────────┼─────────────┘                            │
│                        │                                          │
│  ┌─────────────────────┼─────────────────────────────────────┐   │
│  │                     ▼                                      │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │   │
│  │  │  PostgreSQL  │  │    Redis     │  │     S3       │    │   │
│  │  │  (Primary)   │  │   (Cache)    │  │  (Storage)   │    │   │
│  │  │  Multi-AZ    │  │  Cluster     │  │    Bucket    │    │   │
│  │  └──────────────┘  └──────────────┘  └──────────────┘    │   │
│  │                                                           │   │
│  │  ┌──────────────┐  ┌──────────────┐                      │   │
│  │  │   AI Server  │  │   Backups    │                      │   │
│  │  │   (GPU)      │  │   (S3 Glacier)│                      │   │
│  │  │  Python/TF   │  │   Daily      │                      │   │
│  │  └──────────────┘  └──────────────┘                      │   │
│  └───────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

#### 8.1.2 Server Specifications

| Service | Instance Type | vCPU | RAM | Storage | Count |
|---------|---------------|------|-----|---------|-------|
| API Server | t3.large (AWS) | 2 | 8GB | 100GB SSD | 3 (min) |
| PostgreSQL | db.t3.large | 2 | 8GB | 200GB SSD | 1 primary + 1 replica |
| Redis | cache.t3.medium | 2 | 4GB | - | 1 cluster |
| AI Server | g4dn.xlarge | 4 | 16GB | 100GB | 1 (GPU) |
| Load Balancer | Application LB | - | - | - | 1 |

#### 8.1.3 Environment Variables

```env
# Database
DATABASE_URL=postgresql://user:pass@host:5432/medimatch
DATABASE_POOL_SIZE=20
DATABASE_SSL=true

# Redis
REDIS_URL=redis://:password@host:6379
REDIS_TTL=86400

# JWT
JWT_SECRET=your-secret-key-here
JWT_EXPIRY=24h
REFRESH_TOKEN_EXPIRY=7d

# AWS
AWS_ACCESS_KEY_ID=AKIA...
AWS_SECRET_ACCESS_KEY=...
AWS_REGION=us-east-1
AWS_S3_BUCKET=medimatch-storage

# AI Provider (Internal)
AI_MODEL_PATH=/models/cognimed_v2.3.pkl
AI_CONFIDENCE_THRESHOLD=75
AI_TRAINING_SCHEDULE=0 0 * * 0  # Weekly Sunday at midnight

# OpenAI (Optional)
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4-turbo

# Payment Gateway (Telebirr)
TELEBIRR_API_KEY=...
TELEBIRR_SECRET_KEY=...
TELEBIRR_WEBHOOK_SECRET=...

# SMS Service
SMS_PROVIDER=twilio
TWILIO_ACCOUNT_SID=...
TWILIO_AUTH_TOKEN=...
TWILIO_PHONE_NUMBER=+251...

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=notifications@medimatch.com
SMTP_PASSWORD=...

# Admin
ADMIN_EMAIL=admin@medimatch.com
ADMIN_PHONE=+251911111111

# Monitoring
SENTRY_DSN=https://...
DATADOG_API_KEY=...
```

### 8.2 CI/CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy MediMatch

on:
  push:
    branches: [main, staging]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run backend tests
        run: npm test
      - name: Run frontend tests
        run: npm run test:frontend
      - name: Run AI model validation
        run: python tests/validate_model.py

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Build Docker images
        run: docker-compose build
      - name: Push to ECR
        run: docker push $ECR_REGISTRY/medimatch:latest

  deploy-staging:
    needs: build
    if: github.ref == 'refs/heads/staging'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to staging
        run: kubectl apply -f k8s/staging/

  deploy-production:
    needs: build
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: kubectl apply -f k8s/production/
      - name: Run database migrations
        run: npm run migrate:production
      - name: Clear Redis cache
        run: redis-cli FLUSHALL
```

---

## SECTION 9: COST BREAKDOWN

### 9.1 Monthly Infrastructure Costs (USD)

| Service | Specification | Cost |
|---------|---------------|------|
| **Compute (AWS)** | | |
| API Servers (3 x t3.large) | 2 vCPU, 8GB RAM each | $210 |
| AI Server (1 x g4dn.xlarge) | 4 vCPU, 16GB RAM, GPU | $450 |
| Load Balancer | Application LB | $25 |
| **Database** | | |
| PostgreSQL (db.t3.large) | 2 vCPU, 8GB RAM | $150 |
| Read Replica | db.t3.large | $150 |
| **Cache** | | |
| Redis (cache.t3.medium) | 2 vCPU, 4GB RAM | $50 |
| **Storage** | | |
| S3 (500GB) | Reports, images, backups | $15 |
| S3 Glacier (1TB) | Long-term backups | $10 |
| **CDN** | | |
| CloudFront (1TB) | Static assets | $85 |
| **Monitoring** | | |
| CloudWatch, Datadog | Logs, metrics, alerts | $50 |
| **Network** | | |
| Data Transfer (500GB) | In/out traffic | $45 |
| **Subtotal** | | **$1,240** |

### 9.2 Third-Party Services (Monthly)

| Service | Usage Estimate | Cost |
|---------|----------------|------|
| OpenAI API (if used) | 10,000 requests | $200-500 |
| SMS (Twilio) | 5,000 messages | $40 |
| Email (SendGrid) | 10,000 emails | $15 |
| Payment Gateway (Telebirr) | 2% transaction fee | Variable |
| **Subtotal** | | **$255-555** |

### 9.3 One-Time Costs

| Item | Cost |
|------|------|
| Domain registration (medimatch.com) | $15/year |
| SSL Certificate | Free (Let's Encrypt) |
| App Store (Google Play) | $25 one-time |
| Logo & Branding | $500-2,000 |
| Legal/Compliance review | $1,000-3,000 |

### 9.4 Total Monthly Operating Cost

| Environment | Cost |
|-------------|------|
| Development | $200-400 |
| Staging | $300-500 |
| Production | $1,500-1,800 |
| **Total** | **$2,000-2,700** |

---

## SECTION 10: DEVELOPMENT ROADMAP

### 10.1 Phase 1: Patient Web Portal (4 weeks)

| Week | Tasks | Deliverables |
|------|-------|--------------|
| Week 1 | Setup Vite + React, Authentication, Routing | Working prototype |
| Week 2 | AI Chat UI, Voice input integration | Functional chat |
| Week 3 | Hospital listing, Booking UI | Booking flow |
| Week 4 | Payment integration, Reports, Testing | Complete portal |

**Team:** Frontend Developer (1), UI/UX Designer (0.5)

### 10.2 Phase 2: Patient Android App (4 weeks)

| Week | Tasks | Deliverables |
|------|-------|--------------|
| Week 5 | Flutter setup, Screens navigation | App skeleton |
| Week 6 | AI Chat with voice | Voice-enabled chat |
| Week 7 | Booking, Payment integration | Full app flow |
| Week 8 | Offline support, Testing, Store submission | APK published |

**Team:** Mobile Developer (1)

### 10.3 Phase 3: Facility Portal (3 weeks)

| Week | Tasks | Deliverables |
|------|-------|--------------|
| Week 9 | Login, Role-based dashboard | Management dashboard |
| Week 10 | Doctor dashboard, AI assistant | Doctor tools |
| Week 11 | Surgery scheduling, Testing | Complete portal |

**Team:** Frontend Developer (1), Backend Developer (0.5)

### 10.4 Phase 4: Admin Portal + AI Engine (3 weeks)

| Week | Tasks | Deliverables |
|------|-------|--------------|
| Week 12 | Admin dashboard, CRUD operations | Admin controls |
| Week 13 | COGNIMED AI integration, Dataset loading | AI engine |
| Week 14 | Learning pipeline, Feedback system | Continuous learning |

**Team:** Backend Developer (1), AI/ML Engineer (1)

### 10.5 Phase 5: Integration & Testing (2 weeks)

| Week | Tasks | Deliverables |
|------|-------|--------------|
| Week 15 | API integration, End-to-end testing | Integrated system |
| Week 16 | Load testing, Security audit, Bug fixes | Stable release |

**Team:** QA Engineer (1), DevOps Engineer (0.5)

### 10.6 Phase 6: Deployment & Documentation (1 week)

| Week | Tasks | Deliverables |
|------|-------|--------------|
| Week 17 | Production deployment, Monitoring setup | Live system |
| Week 17 | User docs, API docs, Training | Complete documentation |

### 10.7 Team Summary

| Role | Allocation | Weeks |
|------|------------|-------|
| Frontend Dev | 1 FTE | 1-11 |
| Mobile Dev | 1 FTE | 5-8 |
| Backend Dev | 1 FTE | 1-17 |
| AI/ML Engineer | 1 FTE | 12-17 |
| UI/UX Designer | 0.5 FTE | 1-4 |
| QA Engineer | 1 FTE | 15-16 |
| DevOps Engineer | 0.5 FTE | 15-17 |
| Project Manager | 0.5 FTE | 1-17 |

**Total Effort:** ~9 person-months

---

## SECTION 11: RISK MANAGEMENT

### 11.1 Risk Register

| ID | Risk | Probability | Impact | Mitigation | Owner |
|----|------|-------------|--------|------------|-------|
| R1 | AI misdiagnosis | Medium (30%) | Critical | 75% confidence threshold, disclaimer, human review | AI Engineer |
| R2 | Data breach | Low (5%) | Critical | Encryption, audits, HIPAA compliance | Security Lead |
| R3 | Low user adoption | Medium (40%) | High | Feedback loop, UI improvements, marketing | Product Manager |
| R4 | Payment failure | Medium (25%) | Medium | Multiple gateways, retry logic, refunds | Backend Dev |
| R5 | Voice recognition errors | High (60%) | Low | Fallback to text input | Frontend Dev |
| R6 | External API downtime | Medium (20%) | Medium | Fallback to internal AI | DevOps |
| R7 | Scalability issues | Low (10%) | High | Horizontal scaling, load balancing | DevOps |
| R8 | Regulatory compliance | Low (5%) | Critical | Legal review, regular audits | Legal Team |
| R9 | Server downtime | Low (5%) | High | Multi-AZ, auto-scaling, backups | DevOps |
| R10 | Late delivery | Medium (35%) | Medium | Agile methodology, buffer time | PM |

### 11.2 Mitigation Strategies

#### R1: AI Misdiagnosis
- **Prevention:** 75% confidence threshold, ensemble models
- **Detection:** User feedback, doctor corrections, accuracy monitoring
- **Response:** Flag low-confidence predictions, notify admin, trigger retraining

#### R2: Data Breach
- **Prevention:** AES-256 encryption, network isolation, least-privilege access
- **Detection:** Intrusion detection, anomaly alerts, audit logs
- **Response:** Immediate isolation, breach notification, forensic analysis

#### R3: Low User Adoption
- **Prevention:** User research, beta testing, continuous feedback
- **Detection:** Daily active users, retention metrics, NPS scores
- **Response:** Feature improvements, onboarding enhancements, support

---

## SECTION 12: FUTURE ROADMAP

### 12.1 Phase 2 Features (Months 4-6)

| Feature | Description | Priority |
|---------|-------------|----------|
| iOS App | Flutter iOS build for App Store | High |
| Video Consultation | In-app video calls with doctors | High |
| WhatsApp Bot | AI symptom checking via WhatsApp | Medium |
| Pharmacy Integration | Order medications online | Medium |
| Ambulance Dispatch | One-tap emergency ambulance | High |

### 12.2 Phase 3 Features (Months 7-9)

| Feature | Description | Priority |
|---------|-------------|----------|
| Wearable Integration | Connect Fitbit, Apple Watch | Medium |
| Health Analytics | Personal health trends dashboard | Medium |
| AI Image Recognition | Upload rash/wound photos | High |
| Multi-language Support | Add Oromo, Tigrinya, Somali | High |
| Insurance Integration | Claim processing | Low |

### 12.3 Phase 4 Features (Months 10-12)

| Feature | Description | Priority |
|---------|-------------|----------|
| Predictive Analytics | Hospital demand forecasting | Medium |
| Telemedicine Kiosks | Rural health kiosks | Medium |
| Medical IoT Integration | Blood pressure, glucose monitors | Low |
| Blockchain Records | Decentralized health records | Low |
| International Expansion | Kenya, Nigeria, South Africa | High |

---

## APPENDIX

### A. Glossary

| Term | Definition |
|------|------------|
| COGNIMED | The AI engine powering MediMatch |
| Confidence Threshold | Minimum 75% confidence to show prediction |
| Continuously Learning | AI improves from every user interaction |
| Ethical AI | Only responds with ≥75% confidence, blocks non-health topics |
| Role-Based Access | Permissions based on user type |
| Session | One AI consultation conversation |

### B. Contact Information

| Role | Contact |
|------|---------|
| Technical Support | support@medimatch.com |
| Security Concerns | security@medimatch.com |
| Partnership | partnerships@medimatch.com |
| Emergency | emergency@medimatch.com |

---

## END OF DOCUMENTATION

```
╔═══════════════════════════════════════════════════════════════════╗
║                                                                   ║
║                         MEDIMATCH                                 ║
║                   Powered by COGNIMED AI                          ║
║                                                                   ║
║           "Cognitive AI for Smarter Healthcare"                  ║
║                                                                   ║
║                     Version 1.0 - Complete                        ║
║                    Documented on April 2025                       ║
║                                                                   ║
╚═══════════════════════════════════════════════════════════════════╝
```

This document contains the complete, detailed system design for MediMatch with COGNIMED AI. All sections are ready for implementation.

*This response is AI-generated, for reference only.*
