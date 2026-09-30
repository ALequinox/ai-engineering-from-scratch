# My Agent Skills Path
<!-- Managed by the learn-agent-skills tutor.
     Source: learning-paths/agent-skills.json -->

## Route
- Started: 2026-09-30
- Required time: about 9 hours 30 minutes
- Current: 3 of 5

## Prerequisite check
- Files, Python, and command line: Confirmed
- Node.js and npx: Confirmed
- Selected skill-capable host: Codex
- Install scope: Project (`agent-skills-first-run`, selected by learner)
- Phase 13 Lesson 01 refresher: Pending
- Phase 13 Lesson 05 refresher: Pending
- `tool-poisoning-and-untrusted-instructions`: Pending

## Progress
| Order | Lesson | Status | Evidence | Completed |
|---:|---|---|---|---|
| 1 | 13/22 Portable contract and runtime boundary | Done | Learner created a skill; Codex discovered and explicitly invoked the complete reviewer bundle. Report: valid, zero errors, exit 0, Agent Skill selected; script path, target, cwd, and argv recorded below. Reviewer removed; fresh Codex session showed it absent and explicit invocation unavailable. Quiz: 2/6. | 2026-09-30 |
| 2 | 13/24 Discovery and progressive disclosure | Done | Learner ran demo, traced discovery/catalog/activation/conditional reference load in `agent-skills-first-run/discovery-trace.md`, and explained the reference trigger. Project scope won; two catalog entries; 461/70/46 characters. Tutor verified 23 tests. Quiz: 6/6. | 2026-09-30 |
| 3 | 13/25 Invocation and routing | Next | | |
| 4 | 13/26 Permissions, sandboxes, and trust | Locked | | |
| 5 | 13/27 Evals, packaging, and portability | Locked | | |

## Notes
- Lesson 13/24 pre-question: A (correct, 1/1). Scope precedence is host policy, not a portable rule of `SKILL.md`.
- Lesson 13/24 course demo and 23 tests also passed in tutor verification. Learner reported `project`, `evidence-report`, and 461/70/46 character counts.
- Lesson 13/24 learner identified second catalog entry `meeting-brief` and observed 23 tests. Check question 1 answered B (correct, 1/1).
- Lesson 13/24 learner's initial trace captured the overall order; tutor clarified that discovery and catalog construction are separate and references load only for a needed branch. Check question 2 answered D (correct, 1/1).
- Learner created and read back `agent-skills-first-run/discovery-trace.md`. It separately records three discovery candidates, two catalog entries and project scope win, selected body activation, and `references/format.md` loading only for report formatting; counts 461/70/46. Learner confirmed the Level 3 trigger. Checkpoint trace observed.
- Lesson 13/24 check question 3 answered A (correct, 1/1); equal-precedence duplicates must not resolve by incidental directory order.
- Lesson 13/24 post question 1 answered C (correct, 1/1); this bundle's one-level reference contract rejects `references/archive/schema.md`.
- Lesson 13/24 post question 2 answered A (correct, 1/1). Quiz complete: 6/6 overall (pre 1/1, check 3/3, post 2/2).
- Course demo evidence: cwd `/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/24-skill-discovery-and-progressive-disclosure`; argv `["python3", "code/main.py"]`, exit 0, `catalog_chars: 461`, `report_chars: 607`, Level 2 body 70, Level 3 reference 46. Tests argv `["python3", "-m", "unittest", "discover", "-s", "code/tests", "-v"]`, exit 0, 23 tests passed. No `outputs/` script was installed or run in this lesson.
- Lesson 13/22 pre-question: C (incorrect, 0/1). Reviewed the boundary between repository instructions, a reusable method, and an external API capability.
- Installed-script execution: `SKILL_ROOT=/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/.agents/skills/skill-contract-reviewer`; `TARGET_ROOT=/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run`; `TARGET_SKILL=/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/my-first-skill`; cwd `/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run`; argv `["python3", "/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/.agents/skills/skill-contract-reviewer/scripts/check_skill.py", "/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/my-first-skill"]`; exit 0; report `valid: true`, `errors: []`. This direct script run does not verify Codex skill activation.
- Learner reports the reviewer appeared in a fresh Codex session and was invoked; awaiting the host's report to verify the checkpoint. Check question 1 answered A (incorrect; correct: B), 0/1.
- Check question 2 answered D (incorrect; correct: B), 0/1. Host-specific metadata requires an explicit adapter; other hosts may ignore or reject it.
- Check question 3 answered D (correct), 1/1. A lifecycle hook handles a required after-tool event.
- Post question 1 answered D (incorrect; correct: C), 0/1. Repository-wide conventions belong in `AGENTS.md`; rare task workflows fit a skill.
- Learner explanation (Chinese): "agents.md是对agent的规范，learn-agent-skills 负责引导学习agent". Tutor clarified the scope: repository-wide standing conventions versus a task-specific Agent Skills learning workflow.
- Post question 2 answered B (correct), 1/1. Quiz complete: 2/6 overall (pre 0/1, check 1/3, post 1/2).
- Learner-provided fresh Codex report: reviewer appeared and was invoked explicitly; `valid: true`, `name: my-first-skill`, `errors: []`; primitive `Agent Skill` for a reusable decision-record method. Script `/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/.agents/skills/skill-contract-reviewer/scripts/check_skill.py`; target `/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/my-first-skill`; cwd `/Users/liwen.an/self-pratice/ai-engineering-from-scratch`; argv `["python3", "/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/.agents/skills/skill-contract-reviewer/scripts/check_skill.py", "/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/my-first-skill"]`; exit 0. Cwd differed from lab root; absolute paths kept the run unambiguous.
- Learner confirmed removal. From the lab directory, `skills remove skill-contract-reviewer --yes` exited 0 and reported one skill removed; `skills list --json` returned `[]`; installed reviewer path is absent; learner's `my-first-skill/SKILL.md` remains.
- Learner verified from a fresh Codex session that `skill-contract-reviewer` disappeared from `/skills` and explicit invocation reported it unavailable. Lesson 13/22 checkpoint complete. Review needed: quiz errors centered on package identity, host extensions, and repository-wide instructions.
- Removed the installer's now-empty lab `skills-lock.json`; retained the learner's `my-first-skill/SKILL.md` for later lessons.
