# AI Yelp Review Management System

An AI-powered review management workflow built with **n8n, Gmail, OpenAI, and Google Sheets**.

The system automatically captures Yelp review notifications, extracts structured review data, generates brand-consistent draft responses, and places them into a centralized approval queue for restaurant staff.

The workflow is intentionally designed with a **human approval step** before anything is published to Yelp.

---

## Overview

Restaurant teams often spend time manually:

- Reading incoming Yelp reviews
- Drafting responses
- Tracking which reviews have been handled
- Maintaining consistent messaging
- Following up on negative customer feedback

This project automates the repetitive parts of that process while keeping final publishing under human control.

---

## Architecture

The system consists of two n8n workflows:

```text
                    Yelp Review Email
                           │
                           ▼
                     Gmail Trigger
                           │
                           ▼
                     Review Intake
                           │
                  ┌────────┴────────┐
                  ▼                 ▼
            Review Data        AI Response
                  │                 │
                  └────────┬────────┘
                           ▼
                    Google Sheets
                    Review Queue
                           │
                           ▼
                    Human Approval
                           │
                           ▼
                 Manual Yelp Publishing
```

### Workflow 1 — Review Intake

**Purpose:** Automatically process new Yelp review notifications.

**Process:**

1. Gmail detects a new Yelp review notification.
2. Review information is extracted from the email.
3. OpenAI processes the review and generates a brand-consistent response.
4. The review and AI-generated response are stored in Google Sheets.
5. The processed email is marked as read.

The workflow is designed to prevent duplicate processing and ensure each review enters the system with an initial approval status.

### Workflow 2 — Review Approval

**Purpose:** Manage the human approval stage.

1. The review database is periodically checked.
2. Reviews marked as `Approved` are identified.
3. Approval metadata is recorded.
4. The review is marked as `Completed` after staff manually publish the response on Yelp.

This separates **AI-assisted response generation** from the final customer-facing action.

---

## Data Flow

### Input

- Yelp review notification email
- Review text
- Customer name
- Star rating
- Restaurant location

### Processing

- Email parsing
- Review extraction
- AI response generation

### Output

- Structured review record
- AI-generated response
- Review status
- Timestamp
- Approval metadata

---

## Technologies

- **n8n** — Workflow automation
- **OpenAI API** — AI response generation
- **Gmail** — Review notification intake
- **Google Sheets** — Lightweight review database
- **JavaScript** — Data transformation and parsing
- **OAuth2** — Service authentication

---

## Repository Contents

```text
Yelp-Review-Management/
│
├── README.md
├── Yelp_ AI Review Management Store Reviews - GITHUB.json
└── Yelp_ AI Review Approval Workflow - GITHUB.json
```

The two JSON files contain the sanitized n8n workflow exports for the intake and approval workflows.

> **Note:** Credentials and environment-specific identifiers have been removed from the GitHub versions. Credentials must be configured when importing the workflows into an n8n instance.

---

## Human-in-the-Loop Design

The system does **not** automatically publish responses to Yelp.

The AI generates a draft, but restaurant staff remain responsible for:

1. Reviewing the generated response
2. Editing it when necessary
3. Approving the response
4. Manually publishing it through Yelp

This provides automation without removing human oversight from customer communications.

---

## Key Benefits

- Faster review response preparation
- Consistent brand voice
- Centralized review tracking
- Reduced manual data entry
- Less repetitive administrative work
- Human oversight before publication
- Lightweight implementation using existing SaaS tools

---

## Assumptions

The MVP assumes:

- Yelp sends email notifications for new reviews.
- The connected Gmail account receives those notifications.
- Yelp's email format remains sufficiently consistent for parsing.
- OpenAI is available for response generation.
- Google Sheets is sufficient as the review database for the MVP.
- Staff handle final review approval and Yelp publication.

---

## Known Limitations

### Yelp Publishing

Yelp does not provide a public API for automatically publishing owner responses. Final publication therefore remains a manual step.

### Email Parsing

The parser depends on Yelp maintaining a consistent notification format.

### Database

Google Sheets is used as a lightweight database for the MVP and may not be appropriate for high-volume deployments.

---

## Future Enhancements

Potential production improvements include:

- Replace Google Sheets with Supabase or PostgreSQL
- Build a dedicated approval dashboard
- Support multiple restaurant brands and locations
- Detect duplicate or edited reviews
- Add Slack or Microsoft Teams approval notifications
- Add review and response analytics
- Extend support to Google Reviews, Facebook Reviews, and Trustpilot
- Make AI prompts configurable for different brand voices

---

## Project Status

**Proof of Concept / MVP**

The workflow demonstrates an end-to-end review management process from Yelp notification intake through AI-assisted response generation, centralized tracking, human approval, and completion tracking.

The current implementation focuses on demonstrating the automation architecture rather than a fully productionized review management platform.
