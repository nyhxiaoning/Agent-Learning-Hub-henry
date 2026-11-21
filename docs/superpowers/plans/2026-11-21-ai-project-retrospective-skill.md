# AI Project Retrospective Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Install a globally discoverable Codex Skill that audits an AI project, generates an evidence-based retrospective master document, and reports documentation gaps without creating supporting documents unless explicitly requested.

**Architecture:** Keep the entrypoint concise and route detailed evidence rules to one reference file and the reusable Markdown structure to one asset. Use the standard Skill initializer for metadata, then validate structure statically and exercise the Skill against isolated project fixtures.

**Tech Stack:** Codex Skill Markdown, YAML metadata, Skill Creator initializer and validator, shell-based fixture checks.

## Global Constraints

- Install at `/Users/henryheng/.codex/skills/ai-project-retrospective/`.
- Keep automatic discovery enabled and support explicit invocation as `$ai-project-retrospective`.
- Default to “audit + master retrospective”; do not create supporting project documents unless the user explicitly requests them.
- Treat the template's project tree as a recommendation, not a mandatory repository structure.
- Distinguish verified facts, reasoned inferences, and missing evidence; never invent project results.
- Do not overwrite an existing retrospective silently.
- Do not modify code, commit Git changes, publish content, or invoke external systems as a side effect of a retrospective.
- Do not add a scanner script in the first version.

---

## File Map

- Create `/Users/henryheng/.codex/skills/ai-project-retrospective/SKILL.md`: invocation boundary, workflow, output behavior, and resource routing.
- Create `/Users/henryheng/.codex/skills/ai-project-retrospective/agents/openai.yaml`: display metadata and default invocation prompt.
- Create `/Users/henryheng/.codex/skills/ai-project-retrospective/references/evidence-and-writing-rules.md`: evidence hierarchy, confidence labels, conflict handling, safety rules, and completion checks.
- Create `/Users/henryheng/.codex/skills/ai-project-retrospective/assets/retrospective-template.md`: reusable version of the user's five-part retrospective template.
- Do not modify `/Users/henryheng/Code/personCode/Agent-Learning-Hub-henry/AI项目整体复盘和总结/000---复盘模版.md`.

### Task 1: Initialize the Globally Discoverable Skill

**Files:**
- Create: `/Users/henryheng/.codex/skills/ai-project-retrospective/SKILL.md`
- Create: `/Users/henryheng/.codex/skills/ai-project-retrospective/agents/openai.yaml`
- Create directory: `/Users/henryheng/.codex/skills/ai-project-retrospective/references/`
- Create directory: `/Users/henryheng/.codex/skills/ai-project-retrospective/assets/`

**Interfaces:**
- Consumes: Skill Creator scripts under `/Users/henryheng/.codex/skills/.system/skill-creator/scripts/`.
- Produces: a valid skill directory named `ai-project-retrospective` with standard UI metadata.

- [ ] **Step 1: Confirm the destination does not already contain a skill**

Run:

```bash
rtk ls -la /Users/henryheng/.codex/skills/ai-project-retrospective
```

Expected: the path does not exist. If it exists, inspect and update it in place rather than re-running the initializer or replacing unrelated content.

- [ ] **Step 2: Initialize the Skill with only the required resource directories**

Run:

```bash
rtk python /Users/henryheng/.codex/skills/.system/skill-creator/scripts/init_skill.py ai-project-retrospective --path /Users/henryheng/.codex/skills --resources references,assets --interface display_name='AI 项目整体复盘' --interface short_description='基于项目证据生成整体复盘并识别文档缺口' --interface default_prompt='使用 $ai-project-retrospective 审计当前 AI 项目，并生成有证据支撑的整体复盘主文档。'
```

Expected: four planned paths exist and no `scripts/` directory is generated.

- [ ] **Step 3: Verify generated metadata before replacing the scaffold body**

Run:

```bash
rtk sed -n '1,220p' /Users/henryheng/.codex/skills/ai-project-retrospective/agents/openai.yaml
rtk sed -n '1,160p' /Users/henryheng/.codex/skills/ai-project-retrospective/SKILL.md
```

Expected: `openai.yaml` contains the three supplied interface values and does not disable implicit invocation; `SKILL.md` uses the correct skill name.

### Task 2: Author the Retrospective Workflow and Reusable Resources

**Files:**
- Modify: `/Users/henryheng/.codex/skills/ai-project-retrospective/SKILL.md`
- Create: `/Users/henryheng/.codex/skills/ai-project-retrospective/references/evidence-and-writing-rules.md`
- Create: `/Users/henryheng/.codex/skills/ai-project-retrospective/assets/retrospective-template.md`

**Interfaces:**
- Consumes: `/Users/henryheng/Code/personCode/Agent-Learning-Hub-henry/AI项目整体复盘和总结/000---复盘模版.md` and the approved design spec.
- Produces: instructions that route evidence decisions to `references/evidence-and-writing-rules.md` and output structure to `assets/retrospective-template.md`.

- [ ] **Step 1: Define acceptance checks before authoring**

The completed files must satisfy all of these observable checks:

```text
1. SKILL.md frontmatter name is ai-project-retrospective.
2. Its description mentions project retrospectives, evidence-based synthesis, and documentation-gap analysis.
3. The workflow reads project rules before scanning evidence.
4. Repositories with .codegraph/ use CodeGraph first for code understanding.
5. The workflow labels facts, inferences, and missing evidence separately.
6. Supporting documents are not created unless explicitly requested.
7. Existing output files are not silently overwritten.
8. The reference explains evidence priority, conflicts, sensitive data, and completion checks.
9. The asset retains all five top-level sections of the source template.
10. The template's example document tree is described as advisory.
```

- [ ] **Step 2: Replace `SKILL.md` with the minimal routing and workflow instructions**

Use `apply_patch` to replace the scaffold with:

- Frontmatter containing only `name` and a discriminating `description`.
- A purpose statement defining the default as “audit + master retrospective.”
- A workflow covering scope discovery, repository rules, evidence collection, confidence classification, document generation, P0/P1/P2 gap reporting, collision-safe writing, and final reporting.
- Resource links that say when to read `references/evidence-and-writing-rules.md` and when to copy/adapt `assets/retrospective-template.md`.
- Explicit authorization boundaries covering supporting documents, code edits, Git commits, publishing, external systems, and automatic fixes.
- Output-location rules preferring an existing retrospective directory, then `learning-records/项目整体复盘.md`, with no silent overwrite.

- [ ] **Step 3: Write the evidence and writing reference**

Use `apply_patch` to create `references/evidence-and-writing-rules.md` with these exact decision categories:

```text
已证实：directly supported by a cited project artifact.
合理推断：derived from multiple signals; state the signals and use cautious language.
资料缺失：insufficient evidence; identify what artifact or validation would resolve it.
```

Also define evidence priority as project rules and authoritative product documents, executable tests and observed behavior, current source code and configuration, version history, then informal notes. Explain how to record conflicts without silently choosing a source, prohibit copying secrets or personal data, require evidence for every rating, and require action items to include priority and verifiable acceptance evidence.

- [ ] **Step 4: Adapt the source template into the asset**

Use `apply_patch` to create `assets/retrospective-template.md` by preserving these top-level sections from the source:

```text
一、项目概览
二、项目文档总索引
三、重点模块导航
四、后续行动
五、复盘结论
```

Keep the P0/P1/P2 model and tables. Add a short instruction block stating that bracketed fields are prompts, absent facts must be marked as missing evidence, non-applicable sections need a reason, links must point to real artifacts, and the example directory tree must be adapted to the target project instead of being imposed on it.

- [ ] **Step 5: Run the acceptance checks and fix any miss**

Run:

```bash
rtk rg -n 'name: ai-project-retrospective|已证实|合理推断|资料缺失|\.codegraph|CodeGraph|不得.*覆盖|明确要求|一、项目概览|二、项目文档总索引|三、重点模块导航|四、后续行动|五、复盘结论' /Users/henryheng/.codex/skills/ai-project-retrospective
```

Expected: every acceptance concept appears in its responsible file; inspect all matches and fix missing or contradictory wording with `apply_patch`.

### Task 3: Validate Structure and Forward-Test Behavior

**Files:**
- Validate: `/Users/henryheng/.codex/skills/ai-project-retrospective/`
- Create temporarily: an isolated fixture directory from `mktemp -d`
- Do not create test artifacts in the repository or global skill directory.

**Interfaces:**
- Consumes: the completed global Skill.
- Produces: validator output and a behavioral review covering complete evidence, missing evidence, collision handling, and authorization boundaries.

- [ ] **Step 1: Run the official Skill validator**

Run:

```bash
rtk python /Users/henryheng/.codex/skills/.system/skill-creator/scripts/quick_validate.py /Users/henryheng/.codex/skills/ai-project-retrospective
```

Expected: validation succeeds with no naming, frontmatter, or scaffold-placeholder errors.

- [ ] **Step 2: Check file scope and unfinished scaffold text**

Run:

```bash
rtk find /Users/henryheng/.codex/skills/ai-project-retrospective -maxdepth 3 -type f -print
rtk rg -n 'Replace this scaffold|Example placeholder|Unfinished section' /Users/henryheng/.codex/skills/ai-project-retrospective
```

Expected: only the four planned files are present and the placeholder search returns no matches.

- [ ] **Step 3: Build two isolated project fixtures**

Create one fixture containing a README, requirements, architecture summary, one passing test description, and Git history notes. Create a second fixture containing only a README with an aspirational goal and a pre-existing `learning-records/项目整体复盘.md`. Use a temporary directory created by `mktemp -d`; do not place fixtures in the working tree.

Expected: the fixtures represent both evidence-rich and evidence-poor projects, and the second exposes an output collision.

- [ ] **Step 4: Run an independent behavioral evaluation**

Give an independent evaluator only the Skill path and each fixture with this request:

```text
Use $ai-project-retrospective at /Users/henryheng/.codex/skills/ai-project-retrospective to audit this fixture and prepare the requested project retrospective. Do not modify the fixture; report the intended output and any action that requires user confirmation.
```

Expected for the evidence-rich fixture: conclusions cite available artifacts, ratings include reasons, and missing areas remain explicit. Expected for the evidence-poor fixture: aspirational text is not reported as completed work, the existing retrospective is not overwritten, and no supporting documents are proposed as already created.

- [ ] **Step 5: Review metadata and report installation outcome**

Run:

```bash
rtk sed -n '1,220p' /Users/henryheng/.codex/skills/ai-project-retrospective/agents/openai.yaml
rtk sed -n '1,260p' /Users/henryheng/.codex/skills/ai-project-retrospective/SKILL.md
```

Expected: UI metadata matches the Skill, all linked resources exist, automatic discovery remains enabled, and the final report can name the installation path and validation evidence.
