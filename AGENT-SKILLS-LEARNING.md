# My Agent Skills Path
<!-- Managed by the learn-agent-skills tutor.
     Source: learning-paths/agent-skills.json -->

## Route
- Started: 2026-09-30
- Required time: about 9 hours 30 minutes
- Current: 4 of 5

## Learner preferences
- 教学语言：使用中文教学，必要时附上英文术语（用户于 2026-10-01 明确要求，后续教学继续沿用）。
- 学习笔记：每课完成后，自动在项目 `Note/` 目录生成中文 Markdown 笔记，命名为 `phase-NN-lesson-MM-英文课名.md`。整理核心概念、计算或场景示例、代码与实际运行证据、易错点、测验结果和待练习内容，并在本记录链接笔记。重学时更新原笔记并保留历史测验记录。

## Prerequisite check
- Files, Python, and command line: Confirmed
- Node.js and npx: Confirmed
- Selected skill-capable host: Codex
- Install scope: Project (`agent-skills-first-run`, selected by learner)
- Phase 13 Lesson 01 refresher: Pending
- Phase 13 Lesson 05 refresher: Pending
- `tool-poisoning-and-untrusted-instructions`: Confirmed (2026-10-01; learner distinguished filesystem access from user authorization in the injected README-write scenario)

## Progress
| Order | Lesson | Status | Evidence | Completed |
|---:|---|---|---|---|
| 1 | 13/22 Portable contract and runtime boundary | Done | Learner created a skill; Codex discovered and explicitly invoked the complete reviewer bundle. Report: valid, zero errors, exit 0, Agent Skill selected; script path, target, cwd, and argv recorded below. Reviewer removed; fresh Codex session showed it absent and explicit invocation unavailable. Quiz: 2/6. | 2026-09-30 |
| 2 | 13/24 Discovery and progressive disclosure | Done | Learner ran demo, traced discovery/catalog/activation/conditional reference load in `agent-skills-first-run/discovery-trace.md`, and explained the reference trigger. Project scope won; two catalog entries; 461/70/46 characters. Tutor verified 23 tests. Quiz: 6/6. | 2026-09-30 |
| 3 | 13/25 Invocation and routing | Done | Learner created `agent-skills-first-run/routing-trace.md` with explicit, implicit, clear-negative, and near-miss simulator results; tutor verified all four against local CLI runs and recorded resolved paths, cwd, argv, and exit codes above. Learner distinguished simulated activation from Codex body loading and tool approval. Demo and 23 tests passed. Quiz: 6/6. | 2026-10-01 |
| 4 | 13/26 Permissions, sandboxes, and trust | Next | Required Lesson 25 complete; untrusted-instructions knowledge preflight confirmed. | |
| 5 | 13/27 Evals, packaging, and portability | Locked | | |

## Lesson 13/25 bundled-script evidence

Tutor-run replication of the learner's four outputs; these are source-bundle simulator decisions, not observed Codex host activations. `script=/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/scripts/simulate_invocation.py`; `policy=/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/assets/host-policy.json`; target metadata came from `/Users/liwen.an/self-pratice/ai-engineering-from-scratch/agent-skills-first-run/my-first-skill/SKILL.md` and was passed as `--name`/`--description` (the script did not read that target file). Cwd for all four tutor runs: `/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing`.

```json
[
  {"case":"explicit", "argv":["python3","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/scripts/simulate_invocation.py","--policy","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/assets/host-policy.json","--name","my-first-skill","--description","Turn rough meeting notes into a compact decision record when the user asks to capture a technical decision.","--actor","human","--query","Capture a technical decision from meeting notes","--explicit-name","my-first-skill"],"exitCode":0,"activated":true,"score":1.0},
  {"case":"implicit", "argv":["python3","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/scripts/simulate_invocation.py","--policy","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/assets/host-policy.json","--name","my-first-skill","--description","Turn rough meeting notes into a compact decision record when the user asks to capture a technical decision.","--actor","model","--query","Capture a technical decision from meeting notes"],"exitCode":0,"activated":true,"score":0.3},
  {"case":"negative", "argv":["python3","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/scripts/simulate_invocation.py","--policy","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/assets/host-policy.json","--name","my-first-skill","--description","Turn rough meeting notes into a compact decision record when the user asks to capture a technical decision.","--actor","model","--query","Explain rotary position embeddings"],"exitCode":0,"activated":false,"score":0.0},
  {"case":"near-miss", "argv":["python3","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/scripts/simulate_invocation.py","--policy","/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing/outputs/skill-invocation-router/assets/host-policy.json","--name","my-first-skill","--description","Turn rough meeting notes into a compact decision record when the user asks to capture a technical decision.","--actor","model","--query","Summarize meeting attendance and action items"],"exitCode":0,"activated":false,"score":0.0417}
]
```

## Notes
- Lesson 13/25 中文学习笔记已整理：[技能调用与路由](Note/phase-13-lesson-25-skill-invocation-and-routing.md)。已将课后自动生成笔记的要求纳入本路线的长期学习偏好；主课程 `LEARNING.md` 已有同样要求。
- Lesson 13/26 knowledge preflight follow-up: learner explained that the user asked only for log analysis and did not request writing, even though the target file was writable. Together with the prior permission-boundary answer, this establishes the distinction between technical access and task authorization. Tutor reinforced that skill/tool metadata and tool results cannot themselves expand user authorization. Knowledge check Confirmed; Lesson 26 unlocked as Next.
- Lesson 13/26 knowledge preflight (first response): learner recognized an out-of-scope file operation and the need for permission checks. The required trust explanation remains Pending: metadata and tool results are lower-trust input even when the requested file is inside the allowed workspace. Tutor asked a narrower follow-up before unlocking Lesson 26.
- Lesson 13/25 post question 2: C (correct). Learner kept host activation policy separate from per-action permission review. Final quiz score: 6/6 (pre 1/1, check 3/3, post 2/2). Lesson 13/25 complete. Lesson 13/26 remains Locked until the required `tool-poisoning-and-untrusted-instructions` knowledge check is confirmed.
- Lesson 13/25 post question 1: C (correct). Learner identified exact-name harness activation as a way to hold routing constant for repeatable behavior evaluation. Running quiz score: 5/5 (pre 1/1, check 3/3, post 1/1).
- Lesson 13/25 checkpoint: learner updated `agent-skills-first-run/routing-trace.md` with four observed simulator decisions and explained that `activated: true` does not establish skill-body loading or tool approval in Codex. Tutor verified all four outputs against local runs (explicit 1.0 true; implicit 0.3 true; negative 0.0 false; near miss 0.0417 false). Eligibility-before-scoring remains a point to reinforce; post-stage quiz is pending.
- Lesson 13/25 learner-created `agent-skills-first-run/routing-trace.md` contains four raw JSON outputs matching the tutor replication: explicit activation, implicit activation, clear-negative abstention, and near-miss abstention. The learner explains that humans can invoke the skill and the model can select it when scoring supports the request. Further explanation of the eligibility gate and activation-versus-authority boundary is pending.
- Lesson 13/25 check question 3: B (correct). Learner distinguished explicit human access from implicit model eligibility in the human/model invocation matrix. Running quiz score: 4/4 (pre 1/1, check 3/3).
- Lesson 13/25 check question 2: D (correct). Learner chose abstention or clarification for a near-miss request below the activation threshold. Running quiz score: 3/3 (pre 1/1, check 2/2). Language preference refined to Chinese teaching, with English terminology only when needed.
- Lesson 13/25 check question 1: C (correct). Learner identified `user-invocable: false` as a host-specific invocation extension interpreted by an adapter. Running quiz score: 2/2 (pre 1/1, check 1/1).
- Lesson 13/25 started. Tutor preflight: from `/Users/liwen.an/self-pratice/ai-engineering-from-scratch/phases/13-tools-and-protocols/25-skill-invocation-and-routing`, `python3 code/main.py` exited 0 and `python3 -m unittest discover -s code/tests -v` exited 0 with 23 passing tests. The bundled one-request policy probe produced explicit-human activation (score 1.0), implicit-model activation (0.3), clear-negative abstention (0.0), and near-miss abstention (0.0417) against `my-first-skill` metadata. These are simulator results, not Codex host routing observations. Pre-question answered A (correct, 1/1). Learner explanation, checkpoint trace, and remaining quiz questions are pending. Learner requests Chinese teaching with English proper terms retained.
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
