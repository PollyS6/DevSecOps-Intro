# Lab 5 — Submission

## Task 1: DAST with OWASP ZAP

### Baseline (unauthenticated) scan
- Duration: 1 minute
- Total alerts: 10
| Severity | Count |
|----------|------:|
| High | 0 |
| Medium | 2 |
| Low | 5 |
| Informational | 3 |

### Authenticated full scan
- Duration: 5.5 minutes
- Total alerts: 12
| Severity | Count |
|----------|------:|
| High | 1 |
| Medium | 4 |
| Low | 3 |
| Informational | 4 |

### The "10–20× more" claim (Lecture 5 slide 11)
- Ratio 1.2×
- Did your run match the lecture's ratio? No it did not. However i see that some false positives were filtered out and higher severity alerts were found.
- Pick **two specific alerts** that only the authenticated scan found. For each:
  1. Alert title + severity
  2. Why was it unreachable to the unauthenticated scan? (1 sentence)
  
  1. SQL Injection, High severity. It was unreachable to the unauthenticated scan because it is in authenticated endpoints: search and login. 
  2. Session ID in URL Rewrite, Medium severity. It was unreachable to the unauthenticated scan because it is in WebSocket connections and they are established during authenticated sessions. 
  
  ## Task 2: SAST with Semgrep

### Semgrep severity breakdown
| Severity | Count |
|----------|------:|
| ERROR | 12 |
| WARNING | 10 |
| INFO | 0 |
| **Total** | 22 |

### Top 10 rules by frequency
| Rule ID | Count | OWASP category |
|---------|------:|----------------|
| javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection | 6 | A03 |
| yaml.github-actions.security.run-shell-injection.run-shell-injection | 5 | A03 |
| javascript.express.security.audit.express-check-directory-listing.express-check-directory-listing | 4 | A01 |
| javascript.express.security.audit.express-res-sendfile.express-res-sendfile | 4 | A01 |
| javascript.express.security.audit.express-open-redirect.express-open-redirect | 1 | A01 |
| javascript.jsonwebtoken.security.jwt-hardcode.hardcoded-jwt-secret | 1 | A07 |
| javascript.lang.security.audit.code-string-concat.code-string-concat | 1 | A03 |

### Triage shortcut (Lecture 5 slide 8)
Looking at the top 10 — which **one rule** would you fix first if you had time for only one? - I would fix javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection .
Why? - It has the highest frequency and it is a critical vulnerability. Fixing it would mean closing 6 alerts at once and it would be a big improvement.

### False-positive sample
Pick **one** finding you'd suppress as a false positive after review. Quote the file path +
rule + 1-sentence reason. (NOT generic — must reference the specific code.)

file path: labs/lab5/semgrep/juice-shop/data/static/codefixes/unionSqlInjectionChallenge_1.ts
rule: javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection
reason: Looks like it is some challenge, it is called union Sql Injection Challenge. 

