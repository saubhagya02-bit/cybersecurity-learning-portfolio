# Week 7 — Burp Suite and Hypothesis-Driven Request Analysis

## Overview

This week focuses on using Burp Suite Community Edition as an intercepting proxy to observe, replay, and compare HTTP requests in an authorised training environment.

The objective is to understand application behaviour through controlled request analysis rather than treating the proxy as an automatic vulnerability detector.

## Learning objectives

- Understand intercepting proxy architecture.
- Capture and inspect HTTP requests using Proxy HTTP history.
- Identify request methods, headers, cookies, and parameters.
- Replay requests using Repeater.
- Establish a baseline before modifying a request.
- Compare one controlled variable at a time.
- Interpret response differences carefully.
- Understand the limitations of request-based observations.

## Tools

- Burp Suite Community Edition
- Burp's built-in browser
- PortSwigger Web Security Academy
- Browser HTTP request and response analysis

## Repository contents

- `learning-record.md` — Concepts learned and reflection.
- `proxy-investigation.md` — Proxy investigation journal.
- `discussion-prompts.md` — Answers to discussion questions.
- `screenshots/` — Sanitised investigation evidence.

## Investigation methodology

Configure → Scope → Capture → Inspect → Replay → Compare → Interpret → Document

## Safety and scope

All practical activities are restricted to explicitly authorised training environments. The target scope is limited to the selected lab host. No uncontrolled automated scans or unrelated personal browsing are performed.

Sensitive session cookies, authentication tokens, and personal information are excluded from published evidence.

## Key takeaway

Burp Suite is an instrument for observing and comparing application behaviour. A reliable security conclusion requires a reproducible baseline, controlled changes, sufficient evidence, and careful interpretation.
