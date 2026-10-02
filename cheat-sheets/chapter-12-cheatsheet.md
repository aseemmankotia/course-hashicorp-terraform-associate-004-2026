## Chapter 12 Cheat Sheet: Full Practice Exam & Time Management

### Pacing Budget (60 min / ~57 questions)
| Phase | Time | Action |
|---|---|---|
| First pass | ~45-50 min | ~1 min/question; answer or flag, never leave blank |
| Buffer | ~10-15 min | Revisit flagged questions only |
| Final check | last 1-2 min | Confirm no question is left unanswered before submit |

### Question Formats on TA-004
| Format | Note |
|---|---|
| Multiple choice | One correct answer |
| Multiple response | ALL correct options required, no partial credit |
| True/False | Read carefully — absolutes ('always/never') are traps |
| Fill-in-the-blank | Exact syntax/keyword expected |
| Text-match | Case/spelling sensitivity possible |

### Flagging Strategy
1. Answer with best guess immediately (never blank).
2. Flag if unsure or time-consuming.
3. Move on — do not exceed ~1.5x average time per question.
4. Revisit flags only after all questions answered.

### High-Yield Last-Minute Topics
| Topic | Key Fact |
|---|---|
| count vs for_each | count = numeric index (list shift risk); for_each = map/set key (stable identity) |
| moved block | Declares rename/refactor of resource address; avoids destroy/recreate |
| removed block (1.7+) | Declares Terraform should stop managing a resource; can forget without destroying |
| .terraform.lock.hcl | Locks provider versions/checksums; commit to VCS for consistency |
| Community Edition vs HCP Terraform | CE = free local CLI; HCP Terraform = managed service (remote state, remote runs, private registry, Sentinel policy) |

### Domain-Weighted Final Review
- Re-check the official exam guide's domain weight percentages.
- Allocate final study time proportional to weight, not personal comfort.
- Do a quick pass on every domain — don't fully skip low-weight domains.

### Certiverse Proctoring Reminders
- Government-issued photo ID required.
- Webcam room scan: clear desk, no notes, no second monitor.
- No printed or digital reference material during the exam.
- Confirm stable internet and quiet, private space before starting.

### Submission Checklist
- [ ] All ~57 questions have an answer
- [ ] Flagged questions reviewed at least once
- [ ] No accidental blank multiple-response selections
- [ ] Time remaining used for a final flagged-question pass, not re-reading everything
