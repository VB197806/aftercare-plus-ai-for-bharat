# requirements.md
# AfterCare+ — Requirements (AWS AI for Bharat Hackathon)

## 1) Problem
Post-discharge care breaks down in predictable ways: patients miss medicines, forget restrictions, delay follow-ups, and ignore red-flag symptoms. Discharge instructions are typically unstructured (PDFs/images), hard to understand, and not operationalized into a daily plan. The result is avoidable complications and unnecessary re-visits.

## 2) Solution Summary
AfterCare+ is a **post-discharge continuity intelligence layer** that converts discharge documents into a structured care workflow, drives adherence via reminders, enables follow-up booking, triggers **rule-based red-flag escalation**, and generates clinician-ready follow-up summaries.  
**Non-diagnostic. Uses synthetic/public data only for demo.**

## 3) Primary Users
- **Patient / Caregiver**: receives care plan, reminders, logs adherence, reports symptoms, books follow-up.
- **Clinician / Hospital Escalation Desk**: receives structured summaries and escalations; acknowledges/acts.

## 4) Objectives
- Convert discharge summaries to a structured care plan within minutes.
- Improve adherence through timely reminders and simple logging.
- Reduce delays in escalation for red-flag symptoms (triage logic only).
- Make follow-up coordination frictionless via local provider discovery and booking.
- Provide a clear clinician summary to accelerate review and decision-making.

## 5) Scope (Hackathon MVP)
In-scope:
- Upload discharge/prescription document (PDF/image).
- OCR + structured extraction to care-plan schema.
- Medication schedule + reminders (SMS/WhatsApp-ready).
- Adherence + symptom logging.
- Rule-based escalation (tiered).
- Provider discovery (local region) + follow-up booking.
- Clinician follow-up summary (PDF/structured view).

Out-of-scope (explicit):
- Medical diagnosis or treatment recommendations.
- Integration with hospital EMR/EHR as a dependency (future).
- Claims/reimbursement processing (removed).

## 6) Functional Requirements

### FR-1 Document Intake
- Patient/caregiver can upload discharge/prescription documents (PDF/image).
- System stores the original document securely and tracks processing status.

### FR-2 OCR & Structuring
- System extracts text from documents (OCR).
- System converts extracted text into a **structured Care Plan JSON** with:
  - medications (name, dosage, frequency, duration, instructions)
  - follow-up needs (specialty, timeframe)
  - restrictions (diet/activity)
  - red-flag symptoms list (non-diagnostic)
  - education notes (plain-language)

### FR-3 Care Plan Delivery
- System renders the care plan in a patient-friendly format.
- Caregiver sharing is supported (view-only access is acceptable for MVP).

### FR-4 Reminders & Adherence Logging
- Reminders are scheduled per medication cadence and follow-up milestones.
- Patient can mark medication as Taken/Missed and add optional notes.
- System logs adherence events and computes an adherence score.

### FR-5 Symptom Check-in
- Patient can record symptoms using simple guided inputs (free text + optional structured fields).
- Symptom entries are logged for trend and summary.

### FR-6 Rule-based Escalation
- Escalation engine evaluates symptom entries and adherence risk signals against rules.
- Escalation is tiered:
  - Tier 1: patient guidance + “watch” prompt
  - Tier 2: notify caregiver
  - Tier 3: notify hospital escalation desk
- System records escalation events and acknowledgement status.

### FR-7 Follow-up Booking
- System resolves user location (or user-selected locality).
- System provides list of nearby doctors/hospitals by specialty and availability (MVP can be seeded/synthetic directory).
- Patient can request/confirm a follow-up slot; appointment is stored and reminders scheduled.

### FR-8 Clinician Follow-up Summary
- System generates clinician-oriented summary:
  - key instructions from discharge
  - adherence snapshot
  - symptom timeline highlights
  - escalations and actions taken
- Export as PDF and/or structured view.

## 7) Non-Functional Requirements

### NFR-1 Security
- Role-based access control (patient/caregiver/clinician/admin).
- Encryption at rest for stored documents and records.
- HTTPS in transit.
- Audit-friendly logging (access + critical workflow events).

### NFR-2 Privacy & Data Policy (Hackathon)
- Demo uses **synthetic/public data only**.
- No real PHI is required for judging.

### NFR-3 Reliability & Resilience
- Retry and failure handling for OCR/LLM calls.
- Idempotent processing of document workflows.

### NFR-4 Performance
- Document-to-care-plan time target: **≤ 3 minutes** for typical discharge PDF.
- API responsiveness target: **P95 ≤ 1 second** for basic reads/writes (MVP target).

### NFR-5 Scalability & Cost
- Serverless-first, pay-per-use design.
- Works for pilot hospital and scales to multi-hospital without redesign.

## 8) Assumptions
- Provider directory can be seeded via public listings or synthetic dataset for demo.
- Reminders can be delivered via SMS/WhatsApp; for MVP, SMS is sufficient.
- Clinician desk is represented as notification endpoint + basic acknowledgement (no deep workflow tooling required).

## 9) Constraints
- Non-diagnostic; no medical decisioning or prescribing.
- Synthetic/public data only for demo and documentation.
- Platform remains independent of current government systems (no dependency).

## 10) Success Metrics 
- % discharge docs successfully structured into care plan (target: ≥ 90% on demo set)
- Median document-to-plan time (target: ≤ 3 minutes)
- Adherence event completion rate (target: ≥ 70% in pilot simulation)
- Escalation delivery latency (target: ≤ 2 minutes for Tier 3)
- Follow-up booking completion rate (target: ≥ 50% of “follow-up needed” cases)
- Clinician review time reduction (proxy): summary available in one screen/PDF

## 11) Deliverables for Submission
- `requirements.md` (this file)
- `design.md` (system design)
- Deck (PDF) per hackathon template
- GitHub repo with both markdown files at root
