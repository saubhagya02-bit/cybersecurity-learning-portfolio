# Week 8 — Professional Web Security Review

## Overview

Week 8 integrates the concepts and practical skills developed throughout the cybersecurity learning programme into a small professional web security review.

The review focuses on authorised testing, application architecture, HTTP communication, vulnerability analysis, evidence collection, risk assessment, and secure engineering recommendations.

## Objectives

- Define a clear and authorised testing scope.
- Understand application architecture and normal workflows.
- Identify relevant security questions.
- Conduct controlled and reproducible investigations.
- Distinguish confirmed observations from potential risks.
- Recommend appropriate engineering fixes.
- Communicate security findings professionally.

## Training Environment

- **Platform:** PortSwigger Web Security Academy
- **Lab:** SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- **Feature:** Product-category filtering
- **Tool:** Burp Suite Community Edition
- **Browser:** Burp built-in browser

## Review Methodology

The review follows this process:

**Define scope → Understand → Identify → Hypothesise → Validate → Evaluate → Remediate → Report**

## Repository Structure

```text
week-08-professional-security-review/
├── README.md
├── architecture-and-scope.md
├── review-checklist.md
├── security-review-report.md
├── reflection.md
└── screenshots/
    ├── 01-application-overview.png
    ├── 02-architecture-diagram.png
    ├── 03-normal-request.png
    ├── 04-security-investigation.png
    └── 05-evidence-and-results.png
```

## Documentation

| File | Description |
|---|---|
| `architecture-and-scope.md` | Application context, architecture assumptions, and testing boundaries |
| `review-checklist.md` | Security review questions and investigation checklist |
| `security-review-report.md` | Main professional security review |
| `reflection.md` | Learning reflection and discussion questions |
| `screenshots/` | Sanitised investigation evidence |

## Tools and Resources

- Burp Suite Community Edition
- PortSwigger Web Security Academy
- Browser Developer Tools
- OWASP Web Security Testing Guide
- OWASP Top 10
- OWASP Cheat Sheet Series

## Safety and Authorisation

All practical activities are restricted to explicitly authorised training environments.

No unrelated websites, third-party assets, real user data, or unauthorised accounts are included.

Raw proxy histories, credentials, session tokens, and potentially sensitive information are not published.

## Key Learning Outcome

A professional security review is not about finding the largest number of vulnerabilities.

It is about producing accurate, reproducible, and actionable security observations supported by evidence, clear reasoning, and practical engineering recommendations.