# Master Task Checklist: [Feature / Project Name]

> **File Location**: `local/agents/brainstorming/plans/YYYY-MM-DD-<feature-name>.md`

## 1. Metadata

- **Overall Status**: [Pending / In Progress / Reviewing / Completed]
- **Lead / Reviewer**: [Tech Lead / Reviewer Agent]
- **Created Date**: [YYYY-MM-DD]

---

## 2. Task Delegation Matrix

| Task ID | Track / Scope | Assigned Agent | Track Plan (Task Spec) | Status | Walkthrough Artifact | Review Status |
| :---: | :--- | :---: | :--- | :---: | :--- | :---: |
| **TASK-01** | Backend (API, DAO, Service) | OpenCode Agent | `task_spec_be.md` | `PENDING` | `be_walkthrough.md` | `PENDING` |
| **TASK-02** | Frontend (Types, UI, Client) | OpenCode Agent | `task_spec_fe.md` | `PENDING` | `fe_walkthrough.md` | `PENDING` |
| **TASK-03** | Documentation & Changelog | OpenCode Agent | `task_spec_docs.md` | `PENDING` | `docs_walkthrough.md` | `PENDING` |

---

## 3. Status Legend

**Status** (execution progress):
- `PENDING`: Task created, execution not started.
- `IN_PROGRESS`: Assigned agent is implementing code and tests.
- `REVIEWING`: Walkthrough generated, awaiting Lead review.
- `REVISE`: Lead found errors or gaps; revisions requested.
- `DONE`: Changes approved and verified.

**Review Status** (reviewer verdict):
- `PENDING`: Not yet reviewed.
- `REVISE`: Issues found; agent must fix and re-submit.
- `APPROVED`: Lead validated `git diff`, re-ran verification, and approved. Set task Status to `DONE`.

---

## 4. Review & Quality Gate Log

### Review Log: [TASK-01]
- **Review Date**: [YYYY-MM-DD]
- **Reviewer**: [Name]
- **Verdict**: [APPROVED / REVISE]
- **Notes / Feedback**: [...]

### Review Log: [TASK-02]
- **Review Date**: [YYYY-MM-DD]
- **Reviewer**: [Name]
- **Verdict**: [APPROVED / REVISE]
- **Notes / Feedback**: [...]