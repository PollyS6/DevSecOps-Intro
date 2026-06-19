lab3 signing test
# Lab 3 — Submission

## Task 1: SSH Commit Signing

### Local configuration
- `git config --global gpg.format` → ssh
- `git config --global user.signingkey` → /home/polina/.ssh/id_ed25519.pub
- `git config --global commit.gpgsign` → true

### Local verification
Output of `git log --show-signature -1`:
commit 5f7f997be3dc7d3201c2cee26c894ef4afd5850f (HEAD -> feature/lab3, origin/feature/lab3)
Good "git" signature for pollysych.06@gmail.com with ED25519 key SHA256:oUrfmt5FE5vzeoKRtQ1Hu2O+lDfVA0iNPsylTnscDEg
Author: PollyS6 <pollysych.06@gmail.com>
Date:   Fri Jun 19 17:56:42 2026 +0300

    test: first signed commit

### GitHub verification
- Direct link to your most recent commit on GitHub: https://github.com/inno-devops-labs/DevSecOps-Intro/commit/5f7f997be3dc7d3201c2cee26c894ef4afd5850f
- Screenshot of the Verified badge: https://github.com/PollyS6/DevSecOps-Intro/blob/feature/lab3/Screenshot%20from%202026-06-19%2018-02-36.png

### One-paragraph reflection (2-3 sentences)
The forged-author commit would enable a Repudiation attack, where vulnerable code can be injected in the codebase, the person who did it could impersonate a trusted worker. And since name and email are spoofable, there is no proof of real attacker. The verified badge ties authors SSH key and the commit, so if key or signature are not correct or missing then it will be displayed as "unverified".

## Task 2: Pre-commit + gitleaks

### `.pre-commit-config.yaml` (paste the full content)
```yaml
repos:
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.30.1
    hooks:
      - id: gitleaks

  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v5.0.0
    hooks:
      - id: detect-private-key
      - id: check-added-large-files

### pre-commit install output
pre-commit installed at .git/hooks/pre-commit

### the blocked output
Detect hardcoded secrets.................................................Failed
- hook id: gitleaks
- exit code: 1

○
    │╲
    │ ○
    ○ ░
    ░    gitleaks

Finding:     GH_PAT=REDACTED
Secret:      REDACTED
RuleID:      github-pat
Entropy:     4.143943
File:        submissions/leak-attempt.txt
Line:        2
Fingerprint: submissions/leak-attempt.txt:github-pat:2

6:21PM INF 0 commits scanned.
6:21PM INF scanned ~101 bytes (101 bytes) in 38.6ms
6:21PM WRN leaks found: 1

detect private key.......................................................Passed
check for added large files..............................................Passed
