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

## Task 2: Secure Variant & Diff

### Risk count comparison
| Severity | Baseline | Secure | Δ |
|----------|---------:|-------:|--:|
| Critical | 0 | 0 | 0 |
| High | 0 | 0 | 0 |
| Elevated | 4 | 2 | 2 |
| Medium | 14 | 13 | 1 |
| Low | 5 | 5 | 0 |
| **Total** | 23 | 20 | 3 |

### Which rules are GONE in the secure variant?
List 3 rule IDs that fired in baseline but not in secure-variant:
1. missing authentification — fixed by adding authentification on app communication link
2. unencrypted communication (proxy and app) - fixed by adding encryption (tls)
3. unencrypted communication (browser and app) - fixed by changing http to https

### Which rules are STILL THERE in the secure variant?
1. cross site scripting. - app has XSS vulnerabilities, there is still no explicit control.
2. unnecessary data transfer - i did not change the asset for that. i simply do not know how i can fix that in config.

### Honesty check
Total dropped only about 13%. it was a result of a few changes, i could not change more with confidence.
