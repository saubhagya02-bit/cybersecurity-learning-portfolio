# HTTP Investigation — EliteMart Product Search

## 1. Objective

The objective of this investigation was to understand how a browser communicates with a web application by tracing one ordinary action — searching for a product — from the browser action to the HTTP request, the server response, and the security implications of the observed evidence.

The goal was to interpret the evidence rather than assume that a successful request proves an application is secure.

## 2. Environment

| Item | Value |
|---|---|
| Operating system | Microsoft Windows |
| Browser | Microsoft Edge 156 (Chromium 156) |
| Tool | Edge Developer Tools — Network panel |
| Network settings | Preserve log enabled, no throttling, Disable cache off |
| Application | EliteMart, a MERN e-commerce application |
| Frontend | `https://mern-ecommerce-app-v2.vercel.app` |
| API | `https://mernecommerceapp-production-2c05.up.railway.app` |
| Investigation date | 24 October 2026 |
| Scope | Ordinary, non-destructive use of public product pages and search. No login, checkout, or data modification was performed. |

The investigation was limited to ordinary browser interactions with the application. The observations in this report describe the evidence available through Developer Tools and do not establish how the application is implemented internally.

## 3. Procedure

1. Opened the application in Microsoft Edge and opened Developer Tools using `F12`.
2. Selected the Network panel, enabled **Preserve log**, and reloaded the page.
3. Identified the main document request and associated resources, including scripts, stylesheets, fonts, images, and XHR requests.
4. Entered `shirt` into the product search form and submitted it.
5. Located the matching request using the **Fetch/XHR** filter.
6. Inspected the request's Headers, Preview, Response, Initiator, and Timing tabs.
7. Repeated the same search later and compared the two requests.
8. Reviewed the observed response headers, request headers, cookies, and CORS-related behaviour.
9. Captured screenshots for evidence and identified screenshots that require redaction before publication.

## 4. Initial Page Load and Resource Dependencies

### Page-level summary

The Network panel showed the following page-load summary:

| Metric | Observation |
|---|---:|
| Total requests | 57 |
| Transferred data | 7.2 kB |
| Total resource size | 13.0 MB |
| DOMContentLoaded | 443 ms |
| Load event | 1.22 s |

### Selected resource observations

| Resource | Type | Status | Initiator | Size / time | Interpretation |
|---|---|---|---|---|---|
| `mern-ecommerce-app-v2.vercel.app` | Document | 304 | Other | 204 B / 276 ms | Main HTML document was revalidated. The browser could reuse its cached representation. |
| `index-DxGZg1ZC.js` | Script | 304 | `(index):8` | 82 B / 84 ms | Application JavaScript bundle referenced by the HTML. |
| `index-CmaeycFF.css` | Stylesheet | 304 | `(index):9` | 105 B / 84 ms | Stylesheet referenced by the HTML. |
| Google Fonts `css2?family=Playfair+Display...` | Stylesheet | 200 (memory cache) | CSS file | 0 B / 0 ms | Font stylesheet was served from memory cache. |
| `*.woff2` font files | Font | 200 (memory cache) | Google Fonts CSS | 0 B / 0–1 ms | Font files used for typography. |
| `stripe.js` | Script | 304 | `index-DxGZg1ZC.js:377` | 305 B / 217 ms | Third-party payment-related script requested by the application. |
| `logo.png`, `*-banner-*.png` | PNG | 304 | `index-DxGZg1ZC.js:9` | Approximately 81 B / 65–69 ms | Image resources requested by the application. |
| `get-category` | XHR | 304 | `index-DxGZg1ZC.js:348` | 483 B / 781 ms | API request initiated by frontend code. |
| `product-count` | XHR | 304 | `index-DxGZg1ZC.js:348` | 81 B / 790 ms | API request initiated by frontend code. |

### Reading the evidence

- A `304 Not Modified` response indicates that the server considered the cached representation valid for the conditional request. The response does not normally include a new representation body.
- Resources served from memory or disk cache can contribute to the displayed resource size without requiring the same amount of data to be transferred over the network.
- The difference between 13.0 MB of resources and 7.2 kB transferred is consistent with caching and conditional revalidation. The available summary does not establish that every byte of this difference was caused by `304` responses.
- The Initiator column helps connect requests to the resources or scripts that initiated them. It does not establish every dependency or internal operation.

### Third-party requests observed

Requests to `js.stripe.com` included a document such as `controller-with-preconnect-*.html`, which showed `200 OK (from disk cache)` and a Content-Security-Policy header.

Several Fetch/XHR entries named `b` were also observed for `r.stripe.com`. Some showed no status and 0 B in the displayed list. The cause was not established. These entries may require further investigation in the Network and Console panels; the available evidence is insufficient to conclude that they represent a security problem.

## 5. Investigated Action: Search for "shirt"

### 5.1 Action

- **Action:** Entered `shirt` in the product search form and submitted it.
- **Expected behaviour:** The application returns products matching the search term.
- **Identification method:** The Fetch/XHR filter showed a request named `shirt`. Its Initiator call stack began at `onSubmit` in the search handler.

### 5.2 Observed request

| Field | Observation |
|---|---|
| Method | `GET` |
| URL | `https://mernecommerceapp-production-2c05.up.railway.app/api/v1/product/search/shirt` |
| Search term location | URL path segment: `/search/shirt` |
| Query string | None |
| Resource type | `xhr` |
| Remote address | `69.46.46.85:443` |
| Referrer policy | `strict-origin-when-cross-origin` |
| Request body | None observed; the request used GET |

#### Relevant request headers

| Header | Value |
|---|---|
| `:scheme` / `:method` / `:path` | `https` / `GET` / `/api/v1/product/search/shirt` |
| `Accept` | `application/json, text/plain, */*` |
| `Accept-Encoding` | `gzip, deflate, br, zstd` |
| `Accept-Language` | `en-US,en;q=0.9` |
| `Origin` | `https://mern-ecommerce-app-v2.vercel.app` |
| `Referer` | `https://mern-ecommerce-app-v2.vercel.app/` |
| `Sec-Fetch-Dest` / `Sec-Fetch-Mode` / `Sec-Fetch-Site` | `empty` / `cors` / `cross-site` |
| `Sec-Ch-Ua` | Edge 156 / Chromium 156, platform Windows |
| `Authorization` | Not present in the captured request headers |
| `Cookie` | Not present in the captured search request headers |

The absence of `Cookie` and `Authorization` applies to the two captured search requests. It does not prove that the application has no authentication or session mechanisms elsewhere.

### 5.3 Observed response

| Field | Observation |
|---|---|
| Status | `200 OK` |
| `Content-Type` | `application/json; charset=utf-8` |
| `Content-Encoding` | `gzip` |
| `ETag` | `W/"26746b-xyxItUyfyCcYTQV2ifjS//GHn+s"` (weak ETag) |
| `Date` | Sat, 24 Oct 2026 08:14:53 GMT |
| Body | JSON containing `success: true`, `count: 2`, and a `products` array |
| Example product | `Crew Neck T-Shirt` |
| Example product fields | `price: 1`, `quantity: 1`, `shipping: false` |
| Image data | `photo.url` and `photo.public_id` were empty; `photo.data` contained base64 PNG data |

The observed product objects included fields such as `photo`, `averageRating`, `reviewCount`, `_id`, `name`, `slug`, `description`, `price`, `category`, `quantity`, `createdAt`, `updatedAt`, `__v`, and `shipping`.

#### Timing tab — first request (`200 OK`)

| Phase | Duration |
|---|---:|
| Queueing | 0.96 ms |
| Stalled | 1.86 ms |
| Request sent | 0.34 ms |
| Waiting for server response (TTFB) | 2.57 s |
| Content download | 958.14 ms |
| **Total duration** | **3.53 s** |

The timing values are observations from Developer Tools. They should not be treated as a controlled measurement of the API's normal performance.

### 5.4 Initiator

The observed call stack was:

`onSubmit` → `search` → `cM.<computed>` → `request` / `_request` → `Promise.then` → `xhr`

The stack appeared in `index-DxGZg1ZC.js`, around the displayed lines 8–9 and 346–354.

This supports the conclusion that the search form's submit handler initiated an XHR-based request through the frontend's HTTP-request code. The minified call stack alone does not confirm the exact HTTP client library or the server-side implementation.

### 5.5 Interpretation

When I submitted the search term `shirt`, the frontend's `onSubmit` handler initiated a cross-origin `GET` request to `/api/v1/product/search/shirt` on the Railway-hosted API.

The API returned `200 OK` with a JSON response containing `success: true` and `count: 2`. The response included a product named `Crew Neck T-Shirt`. The response headers labelled the content as JSON, and the Preview and Response tabs showed the returned data.

The captured request headers contained neither a `Cookie` header nor an `Authorization` header. This is consistent with the observed request being made without those explicit credentials. It does not prove that every endpoint is unauthenticated or that the server has no other authentication mechanisms.

The first request took 3.53 seconds, including 2.57 seconds waiting for the response and 958.14 ms in the Content download phase. The response included base64 image data within the JSON, which could increase the response size. However, the payload size was not captured, so its contribution to the observed timing cannot be quantified.

The browser evidence does not reveal how the server validated the search term, matched products, or queried the database.

## 6. Response Headers and Browser Security

### 6.1 API response headers

The following headers were observed on the search API response.

| Header | Observed value | Interpretation |
|---|---|---|
| `Access-Control-Allow-Origin` | `https://mern-ecommerce-app-v2.vercel.app` | The response permits this specific origin to access the response through browser CORS. |
| `Access-Control-Allow-Credentials` | `true` | Indicates that credentialed cross-origin requests are permitted for the allowed origin under CORS rules. It does not prove credentials were sent in this request. |
| `Vary` | `Origin, accept-encoding` | Indicates that the response may vary according to these request headers, which matters for caching. |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` | Instructs supporting browsers to use HTTPS for the stated period and scope after receiving the policy over a secure connection. |
| `X-Content-Type-Options` | `nosniff` | Helps prevent browsers from interpreting a response as a different MIME type. |
| `X-Frame-Options` | `SAMEORIGIN` | Restricts framing to the same origin in supporting browsers. |
| `Cross-Origin-Opener-Policy` | `same-origin` | Provides browsing-context isolation behaviour. |
| `Cross-Origin-Resource-Policy` | `cross-origin` | Allows cross-origin contexts to request the resource, subject to applicable browser rules. |
| `Referrer-Policy` | `no-referrer` | Specifies that referrer information should not be sent in applicable requests initiated from the response's context. Its practical effect on this JSON response was not tested. |
| `X-XSS-Protection` | `0` | Disables the legacy XSS auditor; this is not, by itself, evidence of an XSS vulnerability. |
| `X-DNS-Prefetch-Control` / `X-Download-Options` / `X-Permitted-Cross-Domain-Policies` | `off` / `noopen` / `none` | Additional browser-related hardening headers. |
| `Server` | `railway-hikari` | Identifies a server or hosting-layer value; it does not establish the application's internal architecture. |
| `ETag` | Weak ETag | Provides a validator that can be used for conditional requests. |

**CORS interpretation:** The response allowed the observed frontend origin. This is a positive observation, but a single response does not prove that the server's complete CORS configuration contains only one permitted origin.

CORS controls whether browser JavaScript from another origin can read a response under the browser's cross-origin rules. It is not a substitute for authentication, authorization, or server-side access control.

`Access-Control-Allow-Credentials: true` does not mean that credentials were included in the captured search request. The captured request had no `Cookie` or `Authorization` header.

### 6.2 Frontend document response headers

The main document's observed `304` response included:

- `Cache-Control: public, max-age=0, must-revalidate`
- `ETag`
- `Server: Vercel`
- `X-Vercel-Cache: HIT`
- `X-Vercel-Id`

These are observations from the captured document response. Because it was a `304` response, it does not provide a complete basis for evaluating every security header on the original HTML document.

### 6.3 Missing or unverified headers

- No `Content-Security-Policy` header was visible on the captured API JSON response. For this response, its absence alone is not evidence of a vulnerability.
- `Strict-Transport-Security` and `Content-Security-Policy` were not visible in the captured frontend document's `304` response. This does not prove that they are absent from a full `200` document response. A full document response should be checked before drawing a conclusion.
- No `Cache-Control` header was visible on the captured API response. The presence of an ETag supports conditional validation, but does not by itself establish the full caching policy.

### 6.4 Cookies

The main document request included a `Cookie` header containing `__stripe_mid` and `__stripe_sid`; their values were redacted. These names are associated with Stripe-related browser functionality.

No application session cookie or `Authorization` header was observed on the two captured product-search requests. The document's cookies and the search request's headers are different pieces of evidence and should not be treated as if they were the same request.

**Privacy note:** Any screenshots showing cookie values, session identifiers, tokens, or other sensitive values must be redacted before being committed to GitHub.

## 7. Repeated Request Comparison

The same search was repeated approximately seven minutes after the first request.

| Field | First request | Repeated request |
|---|---|---|
| Method | `GET` | `GET` |
| Endpoint | `/api/v1/product/search/shirt` | `/api/v1/product/search/shirt` |
| Status | `200 OK` | `304 Not Modified` |
| `Date` | Sat, 24 Oct 2026 08:14:53 GMT | Sat, 24 Oct 2026 08:21:58 GMT |
| `ETag` | `W/"26746b-xyxItUyfyCcYTQV2ifjS//GHn+s"` | Same value |
| `Content-Type` / `Content-Encoding` | `application/json; charset=utf-8` / `gzip` | Not listed in the captured `304` headers |
| CORS headers | `Access-Control-Allow-Origin` and `Access-Control-Allow-Credentials: true` | Same observed values |
| `X-Hikari-Trace` | `sin1.d1nj` | `sin1.98a6` |
| Request `If-None-Match` | Not present | Same weak ETag as the first response |
| `X-Railway-Request-Id` | Unique value | Different unique value |
| Queueing / Stalled | 0.96 ms / 1.86 ms | 1.28 ms / 29.96 ms |
| Waiting for server response | 2.57 s | 2.74 s |
| Content download | 958.14 ms | 5.60 s |
| **Total duration** | **3.53 s** | **8.38 s** |

### Interpretation

The repeated request included `If-None-Match` with the ETag from the first response. The server returned `304 Not Modified` with the same ETag.

This is consistent with HTTP conditional revalidation: the browser asks whether its stored representation is still valid, and the server responds that it does not need to send a new representation body for that conditional request.

The matching ETag indicates that the validator matched for this request. It does not prove that every underlying application record remained unchanged or that no other data changed elsewhere.

The response contained Railway-related trace information and a different request ID. This is consistent with the request being handled by the hosting infrastructure rather than being satisfied entirely from the browser's local cache.

The repeated request was slower in the displayed timing summary: 8.38 seconds compared with 3.53 seconds. The Timing tab reported 5.60 seconds in the Content download phase even though a `304` response normally does not carry a new representation body.

This timing observation should be reported as displayed. The available evidence does not establish why that phase took 5.60 seconds, and it does not prove that a full response body was downloaded. Possible explanations cannot be distinguished from these two observations alone.

The two requests were made during separate page loads rather than under controlled conditions. Additional repeated measurements would be needed before making claims about typical API performance.

## 8. Authentication, Authorization and Sessions

- **Authentication evidence:** No `Authorization` header or application session cookie was observed in either captured product-search request.
- **Authorization evidence:** The public product search returned product data. This is consistent with a public catalogue endpoint, but server-side authorization logic was not inspected.
- **Session and cookie evidence:** Stripe-related cookie names were observed on the document request. No application session cookie was observed on the captured search requests.
- **CORS observation:** `Access-Control-Allow-Credentials: true` was present, although the captured search request did not include a `Cookie` or `Authorization` header.
- **Limitations:** Protected flows such as cart, orders, and administrative functions were not tested. Their access controls cannot be assessed from this investigation.

The presence or absence of a cookie or token in a browser request does not, by itself, prove whether the server verifies identity or enforces authorization. Protected endpoints require separate, authorised testing and, where available, server-side code or configuration review.

## 9. Trust Boundaries

### 9.1 Boundary 1 — Browser to API

**Data crossing the boundary:**

- Search term in the URL path.
- Request headers such as `Origin`, `Accept`, and `Referer`.
- Credentials, if a particular request uses them.

**Observed controls:**

- HTTPS was used for the captured request.
- The API response included an HSTS header.
- The response allowed the observed frontend origin through CORS.

**Controls to verify:**

- Server-side validation of search terms.
- Safe construction of database queries.
- Rate limiting and abuse protection.
- Logging and safe error handling.
- Authentication and authorization on protected endpoints.

### 9.2 Boundary 2 — Application server to database (inferred)

The product-search response suggests that the application retrieves product information from a backend data source. However, the database operation itself was not observed.

**Controls to verify:**

- Safe query construction, particularly if search uses NoSQL operators or regular expressions.
- Least-privilege database access.
- Protection of database connection strings and secrets.
- Appropriate handling of errors and returned fields.

These are general security considerations, not claims that EliteMart contains these weaknesses.

## 10. Findings and Recommendations

| # | Finding | Risk or significance | Recommended follow-up |
|---|---|---|---|
| 1 | The observed API response allowed the specific frontend origin and included `Vary: Origin`. | Positive observation; the full allow-list was not verified. | Keep the CORS allow-list restricted to trusted origins. Verify the server configuration rather than assuming this single response proves the complete policy. |
| 2 | `Access-Control-Allow-Credentials: true` was present on the public search response. | May be unnecessary for this endpoint if credentialed requests are not required; not a confirmed vulnerability. | Review whether credentialed cross-origin access is needed and disable it where unnecessary. |
| 3 | The search term was included in the URL path. | Not inherently insecure; impact depends on server-side handling. | Review validation and safe query construction in an authorised code or test environment. |
| 4 | Product image data was returned as base64 inside JSON, while `photo.url` was empty. | May increase response size and processing overhead; size was not measured. | Measure response size and performance. Consider serving images separately and returning URLs, and use pagination if appropriate. |
| 5 | `_id` and `__v` appeared in product objects. | Informational; these fields may reveal implementation details but are not automatically vulnerabilities. | Return only fields needed by the frontend, according to the application's data-exposure requirements. |
| 6 | No CSP was observed on the API JSON response; frontend security headers were not fully verified. | No vulnerability established by the API observation. Frontend policy remains unverified. | Inspect a full `200` frontend document response and assess its applicable security headers. |
| 7 | Third-party resources, including Stripe scripts and Google Fonts, were requested. | Third-party dependencies introduce availability and supply-chain considerations. | Review dependencies, load only necessary resources, and assess appropriate controls such as a suitable Content Security Policy for the frontend. |
| 8 | Some Stripe telemetry requests showed no status and 0 B in the Network list; Console messages were not investigated. | Cause and impact are unknown. | Review the relevant request details and Console messages. Avoid assuming the entries are security incidents without further evidence. |
| 9 | The first search took 3.53 seconds; the repeated conditional request took 8.38 seconds. | A timing difference was observed, but its cause is unconfirmed. | Repeat measurements under comparable conditions and inspect timing details before drawing conclusions about performance. |

These findings distinguish observed behaviour from possible risks. None of the observations above, on its own, establishes that the application is vulnerable.

## 11. Evidence Index

The following filenames describe the evidence referenced during the investigation. Confirm that each file exists in the repository and that its content matches the description before submitting the report.

Files with a `_1` suffix were identified as the first image of their series. If you rename them when saving to `screenshots/`, update the names in this table to match the actual files.

| File | Evidence |
|---|---|
| `01-network-panel-overview.png` | Network panel overview showing requests, document, scripts, CSS, fonts, images, and XHR entries. |
| `02-document-and-resources1.png` | Stripe-related document showing `200 OK (from disk cache)` and response headers, including CSP. |
| `02-document-and-resources2.png` | Timing tab for the Stripe document. |
| `02-document-and-resources3.png` | Initiator tab showing the call stack for the Stripe request. |
| `02-document-and-resources4.png` | Response tab for the Stripe document. |
| `03-selected-request-headers1.png` | Main document `GET /` showing `304 Not Modified` and Vercel response headers. |
| `03-selected-request-headers2.png` | Main document request headers. **Redact cookie values before committing.** |
| `04-action-response.png` | First search response showing `200 OK`, CORS headers, and security headers. |
| `04-action-response2.png` | Search request headers, including `Origin`, `Accept`, and `Sec-Fetch-*`; no `Cookie` or `Authorization` header observed. |
| `04-action-response3.png` | JSON product data in the Response tab. |
| `04-action-response4.png` | Initiator stack: `onSubmit` → `search` → `xhr`. |
| `04-action-response5.png` | Timing tab showing 3.53 seconds total. |
| `04-action-response6.png` | Preview showing `success: true` and `count: 2`. |
| `05-repeated-request-comparison.png` | Repeated request showing `304 Not Modified` and the same ETag. |
| `05-repeated-request-comparison2.png` | Repeated request headers showing `If-None-Match`. |
| `05-repeated-request-comparison3.png`–`6.png` | Repeated-request response, Initiator, Timing, and Preview evidence. Keep the screenshots needed to support claims in the report. |
| `JS List.png` | JavaScript filter showing scripts, initiators, and cache-related information. |

**Evidence hygiene:** Do not commit unredacted cookie values, access tokens, session identifiers, passwords, or other sensitive data. Redact them in screenshots and verify the saved copies before pushing to GitHub.

## 12. Limitations

- Developer Tools exposed client-side requests, responses, headers, and timing information only.
- Database queries, internal services, server logs, and the full authorization logic were not observed.
- Only one user action and one endpoint were investigated.
- No authenticated flows were tested.
- The size of the first `200 OK` search response was not captured, so the payload size and its effect on timing cannot be quantified.
- The two request timings came from separate page loads and were not a controlled performance comparison.
- Console errors and warnings were observed but not investigated in detail.
- The cause of the 5.60-second Content download phase displayed for the repeated `304` response remains unconfirmed.
- A successful HTTP response does not prove that all security requirements were enforced.
- The response headers establish what was observed on the captured responses, not the application's complete server configuration.

## 13. What I Learned

- A normal product search produces a traceable chain: form submission → JavaScript handler → XHR request → cross-origin API → JSON response → page update.
- The Initiator tab helps connect a network request to the frontend code that triggered it.
- A `304 Not Modified` response is a conditional-cache validation result, not an HTTP error.
- Browser caching can reduce transferred data compared with the total size of resources used by a page.
- CORS settings control browser access to cross-origin responses; they do not replace server-side authentication or authorization.
- A missing header on one captured response is not automatically a vulnerability. The response type, context, and security impact matter.
- A request without a visible cookie or authorization header does not establish that all application endpoints are public.
- Timing values should be reported as observations. Additional controlled measurements are needed to establish performance patterns.
- It is important to separate what browser evidence demonstrates from what would require server code, configuration, or logs to verify.

## 14. Conclusion

This investigation traced an ordinary EliteMart product search from the browser action to the API response using evidence from Microsoft Edge Developer Tools. The captured request used HTTPS, and the API response included a specific CORS origin, security-related headers, and an ETag used in a later conditional request.

The repeated search returned `304 Not Modified`, demonstrating conditional revalidation. However, the repeated request's displayed duration was longer than the first request, and the available evidence does not establish the reason.

The main follow-up areas are to verify server-side input handling and authorization on protected endpoints, measure the search response size, assess the handling of base64 image data, investigate the unexplained timing difference, and check the frontend's security headers on a full `200 OK` document response.

Overall, the investigation reinforced the importance of documenting observed evidence accurately, distinguishing findings from assumptions, and avoiding claims that go beyond what the available evidence supports.
