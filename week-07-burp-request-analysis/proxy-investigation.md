# Week 7 — Burp Suite Proxy Investigation

## 1. Objective

The objective of this investigation was to understand how Burp Suite Community Edition can be used as an intercepting proxy to capture, inspect, replay, and compare HTTP requests.

The investigation focused on observing a normal product-category filtering operation in an authorised PortSwigger Web Security Academy training lab.

The purpose was to establish a baseline request, make one controlled change to a harmless parameter, and analyse the resulting application behaviour.

## 2. Environment

- **Tool:** Burp Suite Community Edition
- **Browser:** Burp's built-in browser
- **Platform:** PortSwigger Web Security Academy
- **Lab title:** SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- **Lab host:** `0a1b00510427922380e294b60063001f.web-security-academy.net`
- **Operating system:** Windows
- **Browser engine:** Chromium
- **Date:** 6 November 2026
- **Scope:** The authorised PortSwigger training lab only

All practical observations were limited to the official training environment. No unrelated websites or real user accounts were tested.

## 3. Application Feature

The selected application feature was the product-category filter.

The lab page displayed a product catalogue with category options, including:

- All
- Accessories
- Food & Drink
- Lifestyle
- Pets

I selected the **Accessories** category to observe how the browser communicated the filtering request to the server.

### Expected normal behaviour

When a user selects a product category, the application should send the selected category to the server and display products belonging to that category.

The application should process the category value as data and should not allow ordinary user input to change the intended database query structure.

## 4. Proxy Configuration and Scope

Burp Suite Community Edition was used as the intercepting proxy.

The following workflow was used:

1. Opened Burp Suite Community Edition.
2. Started a temporary project using Burp defaults.
3. Opened Burp's built-in browser.
4. Launched the authorised PortSwigger training lab.
5. Visited the product-category filtering page.
6. Used Proxy HTTP history to locate the filtering request.
7. Inspected the captured request.

The target scope was restricted to the authorised lab host.

No uncontrolled automated scans were performed.

## 5. Captured Baseline Request

The browser generated the following request when the Accessories category was selected.

### Request line

```http
GET /filter?category=Accessories HTTP/2
```

### Host

```http
0a1b00510427922380e294b60063001f.web-security-academy.net
```

### Relevant request information

| Field | Observed value |
|---|---|
| HTTP method | GET |
| Request path | `/filter` |
| Query parameter | `category` |
| Parameter value | `Accessories` |
| Protocol | HTTP/2 |
| Content-Type | Not observed in the supplied request |
| Request body | No body observed |
| Cookie | Session cookie present; value excluded |
| User-Agent | Chromium 156 on Windows |
| Request purpose | Product-category filtering |

The request used a query-string parameter to communicate the selected category.

The session cookie was present in the captured request. Its actual value has intentionally been excluded from this report because session cookies are sensitive authentication-related information.

## 6. Baseline Behaviour

The original category value was:

```text
category=Accessories
```

The browser displayed the Accessories category page and a list of products.

The visible page heading was:

```text
Accessories
```

This established the expected application behaviour for the original category selection.

### Baseline response details

- **HTTP status code:** 200 OK
- **Response length:** 4869
- **Visible behaviour:** Accessories category and its associated product list were displayed.
- **Request modification:** None.

The original request was sent to Repeater without intentional modification to establish a baseline response.

## 7. One-Variable Comparison

A controlled comparison was planned using the same category-filtering request.

Only the value of the `category` parameter was changed.

### Original request

```http
GET /filter?category=Accessories HTTP/2
```

### Comparison request

```http
GET /filter?category=Gifts HTTP/2
```

The comparison value was an ordinary category value, not an attack payload.

All other request components were intended to remain unchanged, including:

- HTTP method
- Host
- Request headers
- Session state
- Other request parameters

### Comparison results

| Field | Baseline | Comparison |
|---|---|---|
| Parameter | Accessories | Gifts |
| HTTP method | GET | GET |
| Status code | 200 OK | 200 OK |
| Response length | 4869 | 4854 |
| Visible page behaviour | Accessories products | Pets |

### Interpretation

Changing the category value is expected to affect which products are displayed.

If the application returns a different product list for the two category values, this is normal filtering behaviour.

A difference in response length or status code alone does not establish a security vulnerability.

The results must be interpreted in the context of the application feature and the specific request modification.

## 8. Request Replay

The original request was used as the baseline before comparing a modified request.

Replaying an unchanged request helps establish whether the original behaviour can be reproduced.

However, repeated requests may not always produce identical responses because of:

- Session expiry or changes in session state.
- Cached responses.
- Dynamic application content.
- Timestamps or other changing response values.
- Server-side application state.

Any differences must be recorded rather than automatically attributed to a security weakness.

## 9. Security Analysis

This lab is titled SQL injection vulnerability in WHERE clause allowing retrieval of hidden data.

The lab's stated vulnerability category concerns unsafe handling of user-controlled input in a database query.

However, this Week 7 investigation focused on Burp Suite request observation and a normal category comparison.

The captured request demonstrates that the browser sends the selected category as a URL query parameter:

```text
category=Accessories
```

The presence of this parameter does not, by itself, prove SQL injection.

To establish a SQL injection finding, additional controlled evidence from the lab would be required to demonstrate that user-controlled input affects SQL query behaviour.

### Security considerations

- URL encoding is a method of representing data in a URL; it is not a security validation mechanism.
- A parameter being visible in a request does not automatically make it security-sensitive.
- Server-side validation and parameterised database queries are important secure engineering controls.
- A changed response does not automatically indicate that the application has a vulnerability.

## 10. Alternative Explanations and Limitations

Several factors can explain differences between two HTTP responses.

### Alternative explanations

1. Different category values can naturally return different products.
2. Dynamic content can change the response length.
3. Session state can influence the server's response.
4. Caching can affect response behaviour.
5. Redirects can produce different intermediate or final responses.

### Investigation limitations

- The supplied browser screenshot confirms the Accessories category page but does not independently establish the Repeater response.
- The baseline and comparison status codes and response lengths must be taken from actual Repeater observations.
- The normal category comparison does not independently prove SQL injection.
- No production application or unrelated system was tested.
- No conclusion about the full database implementation can be made from the captured request alone.

## 11. Secure Engineering Considerations

Although the normal category comparison is not proof of a vulnerability, the lab topic provides an opportunity to understand SQL injection prevention.

### Primary mitigation

Use parameterised queries or prepared statements so user-controlled values are treated as data rather than executable SQL syntax.

### Defence in depth

- Validate input according to the application's expected data type and allowed values.
- Apply least-privilege database permissions.
- Avoid exposing detailed database error messages to users.
- Log relevant server-side errors securely.
- Keep database libraries and application dependencies maintained.

### Regression testing

A developer should verify that:

1. Valid category values return the expected products.
2. Unexpected values are handled safely.
3. User-controlled values cannot change the intended database query structure.
4. Unauthorised database information is not returned.
5. Existing category-filtering functionality continues to work after the fix.

## 12. Conclusion

This investigation provided practical experience using Burp Suite Community Edition to observe HTTP requests and understand how a browser communicates with a web application.

The product-category filtering feature generated a GET request containing the `category` query parameter.

The investigation demonstrated the importance of establishing an unchanged baseline before modifying a request and changing one variable at a time during a comparison.

It also highlighted an important distinction between observing an HTTP parameter and proving that the parameter creates a security vulnerability.

A reliable security conclusion requires sufficient evidence, consideration of alternative explanations, and an understanding of the underlying application behaviour.