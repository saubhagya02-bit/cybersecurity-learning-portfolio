# Week 5 — HTTP and Web Application Architecture

## Overview

Week 5 focuses on how a browser communicates with a web application, and how HTTP, browser security mechanisms, an API server and a data store work together. I investigated **EliteMart**, a MERN e-commerce application (frontend on Vercel, API on Railway), by tracing one ordinary action, **searching for "shirt"**, in Microsoft Edge Developer Tools.

## Learning Objectives

- Explain HTTP requests and responses, URLs, methods, status codes, headers and bodies.
- Distinguish authentication from authorization.
- Understand cookies, sessions and bearer tokens.
- Explain the same-origin policy, CORS, CSRF and Content Security Policy.
- Describe what HTTPS/TLS protects and what it does not.
- Inspect browser network activity and trace one action end to end.
- Draw architecture and sequence diagrams and identify trust boundaries.
- Separate directly observed evidence from inferred behaviour.

## Laboratory Environment

| Item | Value |
| --- | --- |
| Operating system | Microsoft Windows |
| Browser | Microsoft Edge 156 |
| Tool | Developer Tools — Network panel |
| Application | EliteMart (MERN e-commerce application) |
| Frontend | `mern-ecommerce-app-v2.vercel.app` |
| API | `mernecommerceapp-production-2c05.up.railway.app` |
| Date | 24 October 2026 |
| Scope | Ordinary, non-destructive use of public pages and search on an application I am authorised to examine |

## Key Results

| Question | Result |
| --- | --- |
| Action investigated | Search for `shirt` |
| Request | `GET /api/v1/product/search/shirt` (type `xhr`, cross-origin) |
| First response | `200 OK`, `application/json; charset=utf-8`, gzip, `count: 2` |
| Repeated response | `304 Not Modified`; request sent `If-None-Match`, response returned the identical `ETag` |
| What triggered it | `onSubmit` → `search` → `xhr` in `index-DxGZg1ZC.js` |
| CORS | `Access-Control-Allow-Origin` set to the Vercel origin, `Vary: Origin`, credentials allowed |
| Transport | HTTPS with HSTS (`max-age=31536000; includeSubDomains`) |
| Authentication seen | None on the search request (public catalogue endpoint) |
| Total time | 3.53 s for the first request (2.57 s waiting, 958 ms download); 8.38 s for the repeated `304` (2.74 s waiting, 5.60 s download) |

## Repository Contents

- `README.md` — overview, results and completion checklist.
- `http-investigation.md` — full request/response investigation with evidence tables and findings.
- `web-architecture.md` — architecture diagram, components and trust boundaries.
- `sequence-diagram.md` — sequence of page load, search and repeated request, including the identity/session lane.
- `discussion-prompts.md` — answers to the Week 5 discussion prompts and the transport trace, grounded in the EliteMart evidence.
- `learning-record.md` — one-page record: annotated request/response, one diagram and two trust boundaries.
- `screenshots/` — sanitised evidence captured during the investigation.

## Key Security Lessons

- A `200 OK` response does not prove an operation was authorised or validated correctly.
- CORS controls which sites can read a response; it is not authentication or authorization.
- HTTPS protects data in transit but does not fix application logic flaws.
- A `304 Not Modified` is cache validation, not an error.
- A missing header on one response is not automatically a vulnerability.
- Browser evidence cannot prove database queries or server-side authorization.

## Evidence and Limitations

Screenshots are real captures from my own session. Cookie values and similar sensitive values are redacted. Server-side behaviour (validation, database queries, authorization on protected endpoints) is marked as inferred. See the Limitations section in `http-investigation.md`.

## Final Outcome

An evidence-based explanation of how a browser interacts with EliteMart's frontend and API, which security boundaries apply, and what additional evidence (source code, server logs, authenticated testing) would be needed to verify server-side security behaviour.