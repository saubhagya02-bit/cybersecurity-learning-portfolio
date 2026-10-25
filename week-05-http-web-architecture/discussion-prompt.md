# Week 5 Discussion Prompts

Answers use the EliteMart investigation. "Observed" means seen in Edge Developer Tools. General protocol knowledge is labelled as such, because Developer Tools alone cannot show what a network observer sees.

## 1. What remains visible to a network observer under HTTPS? What can the browser user still see?

**Network observer (general protocol knowledge, not captured in this investigation):**

- Visible: source and destination IP addresses and ports (the browser connected to `64.29.17.195:443` for the Vercel frontend and `69.46.46.85:443` for the Railway API), packet sizes and timing, and usually the server name in the TLS handshake (SNI). DNS lookups are visible unless encrypted DNS is used.
- Not visible: the URL path and query (`/api/v1/product/search/shirt`), request and response headers, cookies, and the JSON body, because TLS encrypts them.

**Browser user (observed):** everything after decryption: full URL, request and response headers, status codes, JSON body, timing and the JavaScript that made the call. This is why nothing sent to the browser, such as the product JSON or the frontend code, can be treated as secret.

## 2. What does 401 imply compared with 403? Can a 200 still be an application error?

- `401`: the server needs authentication that is missing, invalid or expired. `403`: the server understood the request and refuses the operation, whether or not it knows who is asking. Some applications return `404` instead of `403` to hide a resource.
- **Observed in EliteMart:** the search endpoint returned `200` with no credentials, so it is public by design (or by omission; only the other endpoints' behaviour can tell).
- A `200` can still be an application-level failure. EliteMart's JSON includes its own `success` field (`"success": true`), which shows the application reports outcome separately from the HTTP status. I only saw the success case; I did not trigger a failing request, so I cannot say what EliteMart does on failure.
- A `304` is not an error either: it is a cache validation result.

## 3. Why is a client-side disabled button not an authorisation control?

The browser is controlled by the user. The disabled button only changes the page; the request it would send is an ordinary HTTP request that anyone can send by other means. EliteMart shows this clearly: the search is a plain `GET` to a separate public API origin, and the `Origin` and `Sec-Fetch-*` headers are added by the browser, not required by the server's logic. Only a server-side check on the request can enforce who may do what. CORS does not help here either: it only stops other websites' scripts from reading responses in a browser, not a direct request from another client.

## 4. Which observations would require source code or server logs?

| Question | Needs |
| --- | --- |
| Is the search term validated, and how is it used in the database query? | Source code, or authorised testing |
| Is the database MongoDB, and is the query built safely? | Source code, database logs |
| Are protected routes (cart, orders, admin) authorised correctly? | Source code and authorised tests with different roles |
| Why did the same request take 3.53 s, then 8.38 s? | Railway metrics and application logs |
| Why is `Access-Control-Allow-Credentials: true` set? | Server CORS configuration |
| Are the security headers set by Helmet or by hand? | Source code |
| Are product images really stored as base64 in the database? | Source code and database inspection |
| Was a request rate-limited or logged? | Server and proxy logs |

## 5. Transport trace: DNS → TCP → TLS → HTTP

**Observed in Developer Tools:**

- All requests used `https`, and request headers appear as HTTP/2 pseudo-headers (`:authority`, `:method`, `:path`, `:scheme`), which indicates HTTP/2.
- `Strict-Transport-Security: max-age=31536000; includeSubDomains` on the API response tells the browser to use HTTPS for that host for one year.
- The Timing tabs I captured show Queueing, Stalled, Request sent, Waiting and Content download, but **no DNS Lookup, Initial connection or SSL rows**. That means the browser reused connections it had already opened, so the lookup, TCP connection and TLS negotiation happened earlier and are not shown for these requests.

**What HTTPS protects here:** confidentiality, integrity and server authentication for the traffic between the browser and the Vercel and Railway hosts. **What it does not protect against:** flaws in the server's logic, such as weak validation or missing authorisation, which the browser would receive over HTTPS exactly as it would any other response.