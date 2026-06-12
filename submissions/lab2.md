## Task 1: Baseline Threat Model

### Risk count by severity
| Severity | Count |
|----------|------:|
| Critical | 0 |
| High | 0 |
| Elevated | 4 |
| Medium | 14 |
| Low | 5 |
| **Total** | 23 |

### Top 5 risks (paste from `jq` output)`
[
  {
    "severity": "elevated",
    "category": "missing-authentication",
    "title": "<b>Missing Authentication</b> covering communication link <b>To App</b> from <b>Reverse Proxy</b> to <b>Juice Shop Application</b>",
    "technical_asset": "juice-shop"
  },
  {
    "severity": "elevated",
    "category": "unencrypted-communication",
    "title": "<b>Unencrypted Communication</b> named <b>Direct to App (no proxy)</b> between <b>User Browser</b> and <b>Juice Shop Application</b> transferring authentication data (like credentials, token, session-id, etc.)",
    "technical_asset": "user-browser"
  },
  {
    "severity": "elevated",
    "category": "unencrypted-communication",
    "title": "<b>Unencrypted Communication</b> named <b>To App</b> between <b>Reverse Proxy</b> and <b>Juice Shop Application</b>",
    "technical_asset": "reverse-proxy"
  },
  {
    "severity": "elevated",
    "category": "cross-site-scripting",
    "title": "<b>Cross-Site Scripting (XSS)</b> risk at <b>Juice Shop Application</b>",
    "technical_asset": "juice-shop"
  },
  {
    "severity": "low",
    "category": "unnecessary-data-transfer",
    "title": "<b>Unnecessary Data Transfer</b> of <b>Tokens & Sessions</b> data at <b>User Browser</b> from/to <b>Juice Shop Application</b>",
    "technical_asset": "user-browser"
  }
]

### STRIDE mapping (Lecture 2 slide 7)
For each top-5 risk, name the STRIDE letter(s) it primarily violates:
- Risk 1: **S** — Attacker can send malicious requests directly to application, because no authentification is required
- Risk 2: **I** - Credentials and tokens are in text, they can be stolen
- Risk 3: **I** - If attacker attacks internal network - data between proxy and app can be stolen
- Risk 4: **T** - Attacker can modify page, steal data from cookies
- Risk 5: **R** - The more data - the harder to understand which is truly intended by user

### Trust boundary observation
There is an arrow between Reverse Proxy and Juice shop application. This arrow is attractive for an attacker since it crosses into application zone. It also is unencrypted and lacks authentification. The application itself is quite vulnerable (since RAA is 70) therefore it would be easy for an attacker to exploit it.
