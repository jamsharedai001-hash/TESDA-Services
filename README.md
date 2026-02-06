# TESDA Services System

## Overview
This repository contains the specification for a **local, kiosk-based, LAN-only information
system** for **TESDA Provincial Training Center – Daanbantayan**. The system digitizes
frontline TESDA forms while preserving the **official wording, structure, and intent** of
the paper versions. It reduces handwriting and manual encoding, improves legibility and
retrieval, and produces **print-ready official forms** for staff processing.

## Deployment & Environment
- Runs on **Windows** using **WAMP Server (Apache + PHP + MySQL)**.
- Developed in **Visual Studio Code** using **PHP + HTML/CSS/JavaScript**.
- Operates **offline / local only** (no public internet exposure).
- Accessible within the **TESDA LAN** for admin staff.
- A dedicated kiosk/front desk PC runs the client side in **Microsoft Edge Kiosk Mode /
  Assigned Access**.

## Interfaces (Strict Separation)

### Public Client Interface (Kiosk Mode)
**Goal:** Allow walk-in clients to fill out forms only.

**Flow:**
1. Kiosk PC is idle/off until a key press.
2. Shows **TESDA logo splash screen** for a few seconds.
3. Goes to a **main menu** with **3 big horizontal service buttons/cards**:
   - Learner Registration (Training Application)
   - Assessment Application
   - Client Satisfaction Measurement (CSM)

Each card has:
- **Details button** → shows a popup/modal explaining the form and requirements.
- **Start/Fill Up button** → opens the form.

**Client permissions:**
- Can fill out a form.
- Can review before submit (optional review step).
- Can submit.

**Client restrictions:**
- Cannot view other submissions.
- Cannot edit after submission.
- Cannot print or download.
- Cannot access admin portal or system settings.
- No browser controls (kiosk lock).

**After submission:**
- Shows a thank-you / confirmation screen (may show reference).
- Automatically returns to home screen.

**No uploads:**
- ID photos and supporting documents are **not uploaded**.
- They are attached physically after printing.
- Signatures are **written physically** on printed forms.

### Admin Portal (LAN-only, Staff-only)
**Goal:** Allow TESDA staff to process, verify, correct, print, and manage records.

**Access rules:**
- Admin portal is **not accessible from kiosk mode**.
- Access is **LAN-only** (admin PCs).
- Protected by **login credentials**.

**Admin capabilities:**
- View all submissions (Learner, Assessment, CSM).
- Search/filter/sort records.
- Open a record for review.
- **Edit records during verification**, including:
  - learner/applicant details (for corrections)
  - **Reference Number**
  - **ULI**
  (Editing reference/ULI is allowed to avoid duplication because existing real reference
  numbers already exist.)
- Print official forms:
  - Records auto-fill into **A4 print-ready templates** matching official TESDA forms.
  - Printing is admin-only.
- Export records (optional PDF/CSV).
- Maintain processing status (e.g., Under Review / Finalized).
- Track physical requirements using a **Requirements Checklist**.

## Forms Included (Scope)
1. **Learner Registration (Training Application)**
   - Digital version of Learner Profile / Registration form.
   - Stores all fields with the same wording/order as official form.
   - Admin prints and attaches ID photo + wet signature physically.
   - Admin uses checklist to confirm required attachments are complete.

2. **Assessment Application**
   - Digital version of Application Form for Assessment.
   - Client submits data; admin verifies and may correct fields.
   - Admin prints; wet signature + attachments completed physically.
   - Has its own requirements checklist separate from learner registration.

3. **Client Satisfaction Measurement (CSM)**
   - Mirrors official TESDA CSM structure (client profile + CC1-CC3 + SQD0-8 +
     suggestions).
   - **Name is optional** (not strictly anonymous).
   - If name is blank, treat as **“Anonymous by default.”**
   - Email/contact is optional (if included).
   - Admin views results and can generate summaries/reports.
   - Printing (if needed) is admin-only.

## Reference Number + ULI Rules
- The system can auto-generate Reference Numbers and ULI.
- Admin can set starting numbers (because TESDA already has existing numbers).
- Admin is allowed to edit Reference/ULI to prevent duplicates.
- Prefer database uniqueness checks to warn if duplicates happen.

## Requirements Checklist (Dynamic)
Instead of hardcoding “Requirement1, Requirement2…”, the system uses a flexible checklist:
- Learner Registration checklist (requirements vary).
- Assessment Application checklist (requirements vary).
- Admin can add/edit checklist items in admin settings.
- Each record shows checklist status (e.g., 4/6 completed).
- This tracks physical items like:
  - wet signature present
  - ID photo attached
  - payment/OR verified
  - other documents submitted

## Printing & Outputs
- Printing is restricted to admins.
- Forms are generated in **A4 print layouts**.
- Output is auto-filled from saved data and ready for physical signing/attachments.
- ID photos are not stored digitally; only attached after printing.

## Security, Privacy, and Reliability
- LAN-only access for admin.
- Role separation: kiosk vs admin.
- Data stored locally in MySQL.
- CSRF protection for form submission.
- Complies with RA 10173 principles:
  - data minimization
  - optional identity in CSM
  - controlled access
- Backup plan: scheduled local backups + optional encrypted cloud backup.
- UPS protection and auto-restart considerations for brownouts.

## Visual/UI Requirement
- Theme is **blue and white**.
- Kiosk UI is clean, focused, and distraction-free.
