# Week 8 — Professional Web Security Review Report

## 1. Executive Overview

### Application

PortSwigger Web Security Academy — SQL injection vulnerability in WHERE clause allowing retrieval of hidden data.

### Review purpose

The purpose of this review was to integrate the web security concepts and practical skills developed during Weeks 1–7 into a structured security review of an authorised training application.

The review focused on product-category filtering, HTTP request analysis, user-controlled input, security boundaries, and secure engineering recommendations.

### Main observation

The product-category filtering feature sends a GET request containing a category query parameter.

The captured request was:

```http
GET /filter?category=Accessories HTTP/2
```

This confirms that the browser communicates the selected category through the URL query string.

### Important limitation

The captured request does not independently establish whether the server constructs database queries safely. A confirmed SQL injection finding requires additional reproducible evidence from the authorised lab.

## 2. Scope and Methodology

### Authorisation

The review was restricted to the individual PortSwigger Web Security Academy training lab.

### In-scope feature

- Product-category filtering.
- HTTP request and response behaviour.
- Relevant query parameters.
- Observable application behaviour.

### Tools

- Burp Suite Community Edition.
- Burp built-in browser.
- PortSwigger Web Security Academy.

### Methodology

The review follows these stages:

1. Define the authorised scope.
2. Understand normal application behaviour.
3. Identify candidate security weaknesses.
4. Establish a baseline request.
5. Form a testable hypothesis.
6. Conduct controlled checks permitted by the lab.
7. Evaluate evidence and possible impact.
8. Recommend secure engineering corrections.
9. Document limitations and conclusions.

### Excluded activities

- Testing unrelated websites.
- Accessing real users' private information.
- Testing third-party systems.
- Using unauthorised accounts.
- Performing destructive or uncontrolled automated testing.

## 3. Application Context and Architecture

The application provides a product catalogue with category-filtering functionality.

When a user selects a category, the browser sends a request containing the selected category value.

Example:

```http
GET /filter?category=Accessories HTTP/2
```

The web application receives the request and processes the selected category.

The internal server-side query implementation and database configuration were not established from the captured browser request alone.

### Relevant trust boundaries

1. Browser to web application.
2. User-controlled input to server-side processing.
3. Web application to database.
4. Session information to server-side security controls.

### Architecture assumptions

The browser-to-application request is directly observable.

The server-side processing and database interaction are conceptual components of the architecture. Their exact implementation requires additional evidence.

## 4. Investigation and Observations

### Observation 01 — Product-category filtering

**Feature:** Product-category filtering.

**Expected behaviour:**

When a user selects a valid product category, the application should display products associated with that category.

**Observed behaviour:**

The browser generated the following request:

```http
GET /filter?category=Accessories HTTP/2
```

**Relevant request details:**

| Field | Observation |
|---|---|
| HTTP method | GET |
| Request path | `/filter` |
| Query parameter | `category` |
| Parameter value | `Accessories` |
| Protocol | HTTP/2 |
| Request body | No body observed |
| Cookie | Session cookie present; value excluded |

**Interpretation:**

The application uses a URL query parameter to communicate the selected category.

The parameter is user-controlled input and must be handled safely by the server.

**Evidence:**

- `03-normal-request.png`
- `01-application-overview.png`

**Limitation:**

The request alone does not prove SQL injection or reveal the server's database query implementation.

### Investigation 02 — SQL injection risk

**Security question:**

Does the application safely handle user-controlled category values when processing database queries?

**Expected behaviour:**

The application should treat the supplied category as data rather than executable SQL syntax.

**Root cause to investigate:**

Unsafe construction of database queries can allow user-controlled input to influence query structure.

**Primary mitigation:**

Use parameterised database queries or prepared statements.

**Current evidence status:**

The captured normal category request establishes the presence of a user-controlled parameter. It does not independently confirm SQL injection.

**Further evidence required:**

Record the actual authorised lab observations, including the relevant request, response, and lab result, before claiming a confirmed vulnerability.

## 5. Security Analysis

### Input validation and parameterisation

Input validation checks whether supplied data meets expected rules, such as type, format, or allowed values.

Parameterised queries separate SQL query structure from supplied data values.

Input validation is useful, but it is not a replacement for parameterised queries.

### Authentication and authorisation

Authentication verifies identity.

Authorisation determines what an identified user is allowed to access.

A valid session does not automatically mean that every requested resource is permitted.

### Session management

Session cookies must be protected because they can be associated with a user's authenticated session.

Actual cookie values must not be published in screenshots or reports.

### Error handling

Applications should avoid exposing unnecessary internal implementation details through error messages.

Errors should be logged securely and presented to users in a controlled way.

## 6. Findings and Risk Assessment

No production-wide vulnerability or severity rating is claimed by this report.

The current directly supported observation is that the product-category request includes a user-controlled `category` parameter.

The existence of that parameter is normal application behaviour and is not itself a vulnerability.

A confirmed security finding should include:

- Reproducible evidence.
- Expected behaviour.
- Actual behaviour.
- Root cause supported by evidence.
- Relevant prerequisites.
- Demonstrated impact.
- Remediation.
- Regression testing.

A successful demonstration in a training lab should not automatically be treated as the same impact or severity in a production system.

## 7. Recommendations

### Priority 1 — Safe database query construction

Use parameterised queries or prepared statements for database operations involving user-controlled input.

**Regression test:**

Verify that valid category values return the correct products and that user input cannot change the intended database query structure.

### Priority 2 — Server-side input validation

Validate input according to the application's expected data types and business rules.

**Regression test:**

Verify that unexpected values are handled safely without breaking legitimate filtering.

### Priority 3 — Object-level authorisation

Enforce permission checks for each protected resource request on the server.

**Regression test:**

Verify that a user cannot retrieve another user's protected resources without the required permission.

### Priority 4 — Session protection

Use appropriate session-cookie security settings and invalidate sessions when required.

**Regression test:**

Verify that invalidated sessions cannot continue to access protected resources.

### Priority 5 — Secure error handling

Avoid exposing unnecessary internal errors to application users.

**Regression test:**

Verify that unexpected application errors do not reveal sensitive implementation details.

## 8. Limitations and Uncertainty

This review was conducted in an intentionally vulnerable training environment.

The investigation did not comprehensively assess:

- The complete server-side source code.
- The full database configuration.
- Every application endpoint.
- All authentication and authorisation workflows.
- Third-party integrations.
- Production deployment infrastructure.

The exact server-side implementation cannot be established from the normal category request alone.

Areas not tested must not be reported as confirmed vulnerabilities.

## 9. Conclusion

This review integrated the practical skills developed during the cybersecurity learning programme.

It demonstrated the importance of defining an authorised scope, understanding normal application behaviour, inspecting HTTP requests, identifying user-controlled input, and making evidence-based security conclusions.

The investigation also reinforced the importance of distinguishing an observable behaviour from an inferred root cause.

Secure engineering recommendations should address the underlying weakness and include regression tests to verify that the correction works.

A professional security review should be accurate, reproducible, clearly communicated, and useful to developers.

