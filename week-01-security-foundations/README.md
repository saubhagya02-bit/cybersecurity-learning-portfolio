# Week 1 - Cybersecurity Foundations

## Overview

This week introduces the fundamental concepts of cybersecurity and information security. The focus is on understanding how valuable information and systems can be protected from threats and vulnerabilities.

---

## Objectives

By the end of Week 1, the following concepts should be understood:

* Understand the CIA Triad
* Understand assets, threats, vulnerabilities and risks
* Understand security controls
* Understand authentication and authorization
* Understand defence in depth
* Perform a basic security risk analysis
* Identify potential security weaknesses
* Recommend appropriate security controls
* Document security findings clearly

---

## Practical Exercise

### University Student Portal Security Analysis

The practical exercise analyses a university student portal from a cybersecurity perspective.

The analysis considers:

* Important system assets
* Possible threats
* Existing or potential vulnerabilities
* Security impacts
* CIA Triad properties affected
* Appropriate security controls

The objective is to understand how security concepts can be applied to a realistic software system.

---

# Key Concepts

## CIA Triad

The CIA Triad is a fundamental information-security model consisting of:

1. Confidentiality
2. Integrity
3. Availability

These three principles help identify what security properties need to be protected.

---

## Confidentiality

**Confidentiality** means protecting information from unauthorized access or disclosure.

For a university student portal, confidential information may include:

* Student personal information
* Student login credentials
* Academic records
* Contact information
* Private university information

Security controls such as access control, authentication, encryption and appropriate permissions can help protect confidentiality.

---

## Integrity

**Integrity** means ensuring that information remains accurate, complete and protected from unauthorized modification.

For example, student academic records should not be modified by unauthorized users.

Possible controls include:

* Access control
* Authorization
* Input validation
* Audit logging
* Database permissions
* Data validation

Integrity is important because unauthorized changes to information can result in incorrect or unreliable data.

---

## Availability

**Availability** means ensuring that systems and information are accessible to authorized users when required.

For a university student portal, availability is important because students and staff may depend on the system to:

* Access academic information
* Register for courses
* View results
* Manage university information
* Access other student services

Possible controls include:

* Backups
* Monitoring
* Redundancy
* Rate limiting
* Disaster recovery procedures
* Protection against denial-of-service attacks

---

# Asset

An **asset** is something valuable that needs protection.

Examples of assets in a university student portal include:

| Asset                      | Description                                         |
| -------------------------- | --------------------------------------------------- |
| Student accounts           | Accounts used by students to access the portal      |
| Personal data              | Information belonging to students                   |
| Academic records           | Student grades and academic information             |
| Authentication credentials | Information used to authenticate users              |
| Database                   | Stores important application information            |
| Student portal             | The application used to provide university services |

Assets should be identified before security risks can be properly analysed.

---

# Threat

A **threat** is something that could cause harm to an asset.

Examples include:

* Account takeover
* Data theft
* Malware
* Phishing
* Unauthorized access
* Data modification
* Denial-of-service attacks

A threat does not necessarily mean that an attack has occurred. It represents a potential source of harm.

---

# Vulnerability

A **vulnerability** is a weakness that could be exploited by a threat.

Examples include:

* Weak or reused passwords
* Missing access controls
* Poor input validation
* Insecure authentication
* Outdated software
* Excessive user permissions
* Missing security monitoring

Identifying vulnerabilities helps determine where security improvements are required.

---

# Risk

**Risk** is the possibility and impact of a threat exploiting a vulnerability.

A simplified way of thinking about risk is:

**Risk = Likelihood × Impact**

For example:

> A student account with a weak password may be vulnerable to account takeover.

If the account contains sensitive student information, the impact of successful unauthorized access could be significant.

Risk analysis helps prioritize security improvements.

---

# Security Controls

A **security control** is a safeguard used to prevent, detect or recover from security problems.

Examples include:

### Preventive Controls

Designed to prevent security incidents.

Examples:

* Strong password policies
* Multi-factor authentication
* Access control
* Encryption
* Input validation

### Detective Controls

Designed to identify security incidents.

Examples:

* Security monitoring
* Audit logs
* Intrusion detection
* Login monitoring

### Corrective Controls

Designed to help recover from security incidents.

Examples:

* Backups
* Incident response procedures
* Account recovery
* Disaster recovery

---

# Authentication vs Authorization

## Authentication

**Authentication** verifies who a user is.

For example, a student may provide:

* Username
* Password
* Multi-factor authentication code

The system uses these mechanisms to verify the user's identity.

### Example

```text
Student → Login credentials → Authentication → Identity verified
```

---

## Authorization

**Authorization** determines what an authenticated user is allowed to do.

For example:

* A student may view their own academic records.
* A lecturer may manage appropriate academic information.
* An administrator may manage users and system settings.

### Example

```text
Authenticated User → Authorization → Allowed resources/actions
```

Authentication answers:

> **Who are you?**

Authorization answers:

> **What are you allowed to do?**

---

# Defence in Depth

**Defence in depth** is a security strategy that uses multiple layers of protection rather than relying on a single security control.

For example, a university portal could use:

```text
User
  ↓
Authentication
  ↓
Multi-Factor Authentication
  ↓
Authorization
  ↓
Input Validation
  ↓
Application Security
  ↓
Database Access Control
  ↓
Encryption
  ↓
Logging & Monitoring
  ↓
Backups
```

If one security layer fails, additional layers can still provide protection.

This reduces dependence on a single security mechanism.

---


# Conclusion

Week 1 established the fundamental concepts required for further cybersecurity learning.

The practical security analysis demonstrated how the CIA Triad, assets, threats, vulnerabilities, risks and security controls can be applied to a real-world software system.

These foundations will support the investigation of more advanced cybersecurity topics in the following weeks.

---
