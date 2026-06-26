# Lab 6 — Submission

## Task 1: Checkov on Terraform + Pulumi

### Terraform scan
- Total checks: 127
- Passed: 49
- Failed: 78

| Severity | Count |
|----------|------:|
| Critical | <n> |
| High | <n> |
| Medium | <n> |
| Low | <n> |

There is no severity (the output is null for severity):
jq '[.[].results.failed_checks[].severity] | group_by(.) | map({severity: .[0], count: length})' \
  labs/lab6/results/checkov-terraform/results_json.json
[
  {
    "severity": null,
    "count": 80
  }
]

and inside the check:
...
"caller_file_path": null,
  "caller_file_line_range": null,
  "resource_address": null,
  "severity": null,
  "bc_category": null,
  "benchmarks": null,
  "description": null,
...

therefore I am not sure what to list here.


### Top 5 rule IDs (by frequency)
| Rule ID | Count | What it checks |
|---------|------:|----------------|
| CKV_AWS_289 | 4 | Ensure IAM policies does not allow permissions management / resource exposure without constraints |
| CKV_AWS_355 | 4 | Ensure no IAM policies documents allow \"*\" as a statement's resource for restrictable actions |
| CKV_AWS_23 | 3 | Ensure every security group and rule has a description |
| CKV_AWS_288 | 3 | Ensure IAM policies does not allow data exfiltration |
| CKV_AWS_290 | 3 | Ensure IAM policies does not allow write access without constraints |

### Pulumi scan
| Severity | Count |
|----------|------:|
| CRITICAL | 1 |
| HIGH | 2 |
| INFO | 2 |
| MEDIUM | 1 |

### Module-leverage analysis (Lecture 6 slide 17)
Looking at your top-5 Terraform rules, which ONE fix would eliminate the most findings if applied
at the module level? (2-3 sentences. e.g., "If the S3 module had `block_public_acls = true` as default,
the 8 findings of CKV_AWS_56 would all go away.") - The fix would be of the IAM module. If it used least privilege rule then 14 findings would be fixed.

## Task 2: KICS on Ansible

### Severity breakdown
| Severity | Count |
|----------|------:|
| HIGH | 3 |
| MEDIUM | 0 |
| LOW | 1 |
| INFO | 0 |

### Top 5 KICS queries (by frequency)
| Query | Severity | Files |
|-------|----------|------:|
| Passwords And Secrets - Generic Password | HIGH | 6 |
| Passwords And Secrets - Password in URL | HIGH | 2 |
| Passwords And Secrets - Generic Secret | HIGH | 1 |
| Unpinned Package Version | LOW | 1 |

### Checkov vs KICS — when to use which? (Lecture 6 slide 10)
2-3 sentences each:
- One thing Checkov did **better** for the Terraform sample: provided clear rule IDs. With clear mapping of rules to resources - therefore its easy to identify what needs fixing.

- One thing KICS did **better** for the Ansible sample: KICS did not pre compile files. So, it found mistakes in different languages more easily.
