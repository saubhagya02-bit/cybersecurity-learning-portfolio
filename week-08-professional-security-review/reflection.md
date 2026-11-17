# Week 8 — Reflection

## 1. Introduction

Week 8 focused on integrating the knowledge and practical skills developed throughout the cybersecurity learning programme into a small professional web security review.

The main objective was to understand how security investigations are planned, conducted, documented, and communicated within an authorised testing environment.

## 2. What I Learned

### Defining scope

I learned that a security review should begin with clearly defined boundaries. Identifying the authorised application, relevant features, and excluded activities helps keep an investigation controlled and responsible.

### Understanding application architecture

I learned to consider how a browser communicates with a web application and how user-controlled information may travel between application components.

I also learned to distinguish directly observed behaviour from assumptions about internal server-side implementation.

### Identifying security weaknesses

The review helped me connect vulnerability concepts with application behaviour.

These included:

- SQL injection and unsafe database query construction.
- Cross-site scripting and unsafe content rendering.
- Broken access control and object-level authorisation.
- Authentication and session management.
- Security misconfiguration and dependency risks.

### Collecting and evaluating evidence

I learned that a suspicious response or unusual request does not automatically prove a vulnerability.

A reliable finding requires reproducible evidence, an understanding of expected behaviour, and consideration of alternative explanations.

### Recommending secure engineering solutions

I learned that remediation should address the underlying cause.

Examples include parameterised queries, server-side authorisation, safe DOM APIs, appropriate session controls, and secure configuration.

## 3. Discussion Questions

### What is the most important lesson from this review?

The most important lesson is that a security review must be evidence-based. Identifying a suspicious parameter or response is only the beginning of an investigation. A conclusion requires sufficient evidence and an understanding of the relevant security boundary.

### What is the difference between observed behaviour and inferred root cause?

Observed behaviour is directly supported by evidence, such as an HTTP request or response.

An inferred root cause is an explanation of why the behaviour occurred. It should be labelled as an inference unless further evidence confirms it.

### Which remediation should be prioritised?

The remediation should directly address the underlying design or implementation weakness.

For SQL injection, parameterised queries are a primary mitigation. For broken access control, server-side object-level authorisation checks are essential.

### What uncertainty could change the conclusion?

The internal server-side implementation was not fully visible during the browser-level investigation. Source-code inspection or additional controlled evidence could provide further information about how input is processed.

## 4. Connection to Weeks 1–7

| Week | Main learning |
|---|---|
| Week 1 | Security foundations, CIA triad, threats, vulnerabilities, and risks |
| Week 2 | Network traffic analysis using Wireshark |
| Week 3 | Linux commands, permissions, and system observation |
| Week 4 | Network discovery and traffic analysis using Nmap and Wireshark |
| Week 5 | HTTP requests, responses, web architecture, and browser investigation |
| Week 6 | Vulnerability mechanisms and secure engineering |
| Week 7 | Burp Suite, Proxy history, Repeater, and controlled request comparison |
| Week 8 | Professional web security review and evidence-based reporting |

## 5. Overall Reflection

Throughout these eight weeks, I developed a stronger understanding of how cybersecurity concepts relate to real application and network behaviour.

I progressed from learning fundamental security principles to observing network traffic, investigating web requests, understanding common vulnerability mechanisms, and using Burp Suite for controlled request analysis.

Week 8 helped me bring these skills together in a structured review process.

I also learned that professional security work is not simply about identifying vulnerabilities. It involves defining scope, respecting authorisation, collecting reliable evidence, explaining uncertainty, and recommending practical engineering improvements.

These skills provide a foundation for further learning in web application security and secure software development.

