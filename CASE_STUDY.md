# Fintech API Landscape — Testing Reference Guide

*📚 Reference guide · self-directed prep, not client work · 24 API examples across 9 categories*

> Exported from the portfolio page on 2026-10-19. The live page is the canonical version: https://bhawnamanglapm.github.io/travel-tech-more.html#case-fintech-api-landscape

- 9 categories, 24 real-world API examples a BFSI company like Bajaj Finserv actually integrates
- Every example: realistic endpoint, sample request/response, and the test scenarios that matter
- Written after Partner Ops and NSDL KYC — a map of the wider terrain those two projects sit inside
- A reference document, not a built system — honestly labeled as such

### Why This Exists

Partner Ops and NSDL KYC are two categories tested hands-on. This is the map of the other seven a compliance-flavored PM role would also need to speak to — not built or tested, a reference compiled to know what exists, what each one is for, and what a good test suite checks, before ever touching one. The honest boundary matters: knowing the landscape isn't the same as having built in it, and this document says so plainly rather than blurring the two.

### The 9 Categories

| Category | Count | Real examples |
|---|---|---|
| Identity & KYC | 4 | NSDL PAN verification, Aadhaar eKYC, DigiLocker, CKYC lookup |
| E-agreement / e-sign | 2 | Aadhaar eSign, e-stamp & e-sign |
| Credit & risk | 3 | Credit bureau pull, risk-scoring engine, bank statement analysis |
| Payments & payouts | 4 | UPI, NEFT/RTGS/IMPS, payment gateway, e-NACH |
| Core banking / ledger | 1 | Finacle integration — real Bajaj Finserv experience |
| Fraud & AML screening | 4 | PEP list, sanctions list, device fingerprinting, velocity checks |
| Regulatory reporting | 3 | RBI/SEBI feeds, CKYC upload, STR filing |
| Notifications / webhooks | 2 | SMS/email/WhatsApp gateway, internal webhooks |
| Account aggregator | 1 | RBI's AA framework (Setu, Finvu) |

### Two Examples, in Full

**UPI Payment** — `POST /v1/payments/upi`

Insufficient balance returns **200**, not 400 — `{"status": "FAILED", "reason": "INSUFFICIENT_FUNDS"}` — because the *request* was valid, the *transaction* just didn't succeed. That distinction — request-validity vs. outcome — is one of the most commonly missed in payment API testing.

**PEP (Politically Exposed Person) Screening** — `POST /v1/screening/pep-check`

Three-way result, not two: clean (auto-clear), confirmed match (auto-block), or a *fuzzy* partial match — `{"match": "POSSIBLE", "requiresManualReview": true}` — that routes to a human instead of an automatic decision either way. Real fraud screening has that middle state; testing it means checking for three outcomes, not two.

### The Pattern Across All 24

Every one of the 24 reduces to the same 7 questions:

1. Does it work with valid input?
1. Does it correctly reject bad or missing input?
1. Does it enforce who's allowed to ask?
1. Does it separate "the request was fine but the outcome wasn't" from "the request itself was broken"?
1. Does it handle duplicates/retries without double-processing?
1. What happens when a downstream/third-party system is slow, down, or lying?
1. Is there a compliance-specific rule layered on top (consent, masking, a regulatory deadline) that a generic test wouldn't catch without knowing the domain?

That seventh question is the actual differentiator for a compliance-flavored PM role — anyone can check "does it return 200." Knowing which extra domain rule applies to which API is what this document was really testing fluency on.

### Quick recall

| Section | Summary |
|---|---|
| **Problem** | Partner Ops and NSDL KYC tested 2 of 9 real fintech API categories hands-on; a compliance-flavored PM role needs fluency across the other 7 too — identity, payments, credit/risk, fraud/AML, core banking, regulatory reporting, account aggregator. |
| **Research steps** | Mapped all 9 categories against 24 real-world API examples a BFSI company like Bajaj Finserv actually integrates → worked 2 examples in full detail (UPI payment, PEP screening) to show the request-validity-vs-outcome distinction and the 3-way fraud-match result → reduced all 24 examples to the same 7 underlying test questions, isolating the one (domain-specific compliance rules) that actually differentiates a compliance PM from a generic API tester. |
| **Solution** | A 24-example reference guide (Markdown + .docx), organized by category, each with a realistic endpoint, sample request/response, and the specific test scenarios that matter — honestly labeled as a reference document, not a built or tested system. |
| **Conclusion** | Anyone can check "does it return 200" — the real skill is knowing which extra domain rule (a consent requirement, a masking rule, a regulatory deadline) applies to which specific API, which is what this document was built to test fluency on. |

Guide[Full 24-example reference (Markdown) →](https://bhawnamanglapm.github.io/projects/fintech-api-landscape/Fintech-API-Case-Studies.md) Word[Download as .docx →](https://bhawnamanglapm.github.io/projects/fintech-api-landscape/Fintech-API-Case-Studies.docx)
