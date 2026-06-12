# Lab 1 — Submission

## Triage Report: OWASP Juice Shop

### Scope & Asset
- Asset: OWASP Juice Shop (local lab instance)
- Image: `bkimminich/juice-shop:v20.0.0`
- Image digest: sha256:99779f57113bd47312e8fe7b264ff402ee41da76ddda7f2fc842a92ad51827ce
- Host OS: Ubuntu 24.04.4
- Docker version: Docker version 29.5.0, build 98f1464

### Deployment Details
- Run command used: `docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop:v20.0.0`
- Access URL: http://127.0.0.1:3000
- Network exposure: 127.0.0.1 only? Yes
- Container restart policy: no

### Health Check
- HTTP code on `/`: 200
- API check (first 200 chars of `/api/Products`):
  ```
  {"status":"success","data":[{"id":1,"name":"Apple Juice (1000ml)","description":"The all-time classic.","price":1.99,"deluxePrice":0.99,"image":"apple_juice.jpg","createdAt":"2026-06-12T18:02:41.613Z"
  ```
- Container uptime: CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS          PORTS                      NAMES
1a3200c7cd8d   bkimminich/juice-shop:v20.0.0   "/nodejs/bin/node /j…"   12 minutes ago   Up 12 minutes   127.0.0.1:3000->3000/tcp   juice-shop

### Initial Surface Snapshot (from browser exploration)
- Login/Registration visible: Yes
- Product listing/search present: Yes
- Admin or account area discoverable: Yes
- Client-side errors in DevTools console: No
- Pre-populated local storage / cookies: cookies: cookieconsent_status, language, token, welcomebanner_status; local storage: key token

### Security Headers (Quick Look)
Run: `curl -I http://127.0.0.1:3000 2>&1 | head -20`. Paste output:
```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  9903    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Feature-Policy: payment 'self'
X-Recruiting: /#/jobs
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Fri, 12 Jun 2026 18:02:42 GMT
ETag: W/"26af-19ebd001f00"
Content-Type: text/html; charset=UTF-8
Content-Length: 9903
Vary: Accept-Encoding
Date: Fri, 12 Jun 2026 18:26:31 GMT
Connection: keep-alive
Keep-Alive: timeout=5

```
Which of these are MISSING? (cross-reference Lecture 1 OWASP Top 10:2025 — A06)
- [MISSING] `Content-Security-Policy`
- [MISSING] `Strict-Transport-Security`
- [ ] `X-Content-Type-Options: nosniff`
- [ ] `X-Frame-Options`

### Top 3 Risks Observed (2-3 sentences each, in your own words)
1. **Missing CSP** — Malicious javascript could be injected through user input.This is A02 - security misconfiguration 
2. **Information disclosure via API version endpoint** — no login(or authentification) for version or product list. Attacker can use this information. This is A01 - Broken Access Control 
3. **Missing transport security** — no strict transport security - attacker can steal data, since it is transmitted clearly. This is A07 - authentication failures

## PR Template Setup

- File: `.github/PULL_REQUEST_TEMPLATE.md`
- Sections included: Goal / Changes / Testing / Artifacts & Screenshots
- Checklist items: Title is clear (feat(labN): <topic> style), No secrets/large temp files committed, Submission file at submissions/labN.md exists
- Auto-fill verified: [ ] Yes — PR description showed my template (screenshot or link to draft PR)
