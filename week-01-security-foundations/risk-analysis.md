# Cybersecurity Risk Assessment Table

| Asset | Threat | Vulnerability | Impact | CIA | Security Control |
|---|---|---|---|---|---|
| Student accounts | Account takeover | Weak/reused passwords | Unauthorised access to accounts and data | C/I | MFA, password policy, login monitoring |
| Personal data | Data theft | Inadequate access control | Exposure of student information | C | RBAC, encryption, access logging |
| Grade records | Unauthorised modification | Poor authorization | Incorrect academic results | I | RBAC, audit logging, approval controls |
| Student portal | Service disruption | Insufficient resilience | Students cannot access services | A | Rate limiting, redundancy, monitoring |
| Course registration data | Unauthorised modification | Missing input validation/access controls | Incorrect course registrations | I | Input validation, RBAC, audit logs |