---
name: vibe-prd
description: Turn rough requirements, original PRDs, Feishu/Lark document links, competitor analysis, or the explicit trigger "帮我写一个用来vibe coding的PRD" into a concise Markdown PRD that can be sent directly to Claude Code, Codex, or another coding agent. Use when the user wants a Vibe coding PRD, coding-agent-ready product spec, feature breakdown, MVP scope, tech-stack recommendation, wireframe-level screen design, AI agent workflow/prompt/tool design, or concrete acceptance criteria.
---

# Vibe PRD

## Goal

Produce a compact, coding-agent-ready PRD from messy product input. The output should tell a coding agent what to build, what not to build, what stack to use, what screens and workflows exist, and how each feature will be accepted.

## Core Rules

- Do not infer what the user wants when the requirement is ambiguous. Ask for confirmation before moving to the next design stage.
- Prefer confirmation tools such as `request_user_input` when available. If no popup/confirmation tool is available, ask concise multiple-choice questions in chat and wait.
- Ask at most 3 questions at a time. Prefer options over open-ended questions; include an "Other / I will describe" option when useful.
- Keep the PRD concise and free of business jargon. Optimize for what Claude Code, Codex, or another coding agent needs to implement.
- Remove business-value, market-size, OKR, stakeholder, and strategy prose unless it changes product behavior, permissions, constraints, or acceptance criteria.
- Treat the user as nontechnical when recommending tech. Explain tradeoffs plainly, make a recommendation, and ask for confirmation.
- Keep unresolved items in `Open Questions`; do not hide assumptions inside the spec.

## Input Classification

First classify the user's input:

1. **Clear requirement**: Has a clear user, scenario, pain/problem, and desired product shape. Continue to requirement definition.
2. **Original PRD**: Looks like an engineer-facing PRD, product spec, or document link. Extract only coding-relevant information: users, goals, flows, rules, states, data, dependencies, edge cases, permissions, constraints, non-goals, and acceptance criteria.
3. **Feishu/Lark PRD link**: If the input contains a Feishu/Lark/Doubao doc, wiki, or docx link/token, use the relevant Lark document skill to read it before analysis. Usually use `lark-doc`; use `lark-wiki` if a wiki node must be resolved, and `lark-drive` only for drive files.
4. **Competitor analysis**: Extract observed workflows, feature patterns, gaps, and possible user jobs. Do not turn observations into requirements until the user confirms the target user, scenario, and problem.
5. **Unclear / other**: If there is no clear user, scenario, pain, or product shape, guide the user with multiple-choice questions before drafting.

## Clarity Gate

Before designing functionality, confirm the minimum requirement:

- Who is the target user?
- In what scenario will they use it?
- What pain, job, or problem should it solve?
- What product shape is expected: web app, mobile app, browser extension, internal tool, AI agent, automation, plugin, API, or something else?
- Are there constraints: deadline, preferred platform, existing codebase, required integrations, data source, privacy/security limits, or budget?

If any answer is missing or fuzzy, ask targeted choices. Example:

```markdown
我还差一个关键信息：这个产品主要给谁用？

A. 我自己使用的个人效率工具
B. 团队内部使用的业务工具
C. 面向外部用户的 SaaS / 小程序 / 网站
D. 还不确定，我先描述场景
```

Only proceed after the user confirms enough detail to avoid guessing.

## Workflow

### 1. Requirement Definition

Draft and confirm a short definition:

- **For whom**: target users and roles
- **Scenario**: when/where the product is used
- **Problem**: pain, job, or unmet need
- **Product form**: app/tool/agent/automation/API/etc.
- **Success outcome**: observable result, not vague satisfaction

Ask the user to confirm or correct this definition before feature design.

### 2. Feature Module Design

Create a feature list with MVP thinking:

```markdown
| Priority | Level 1 Module | Level 2 Feature | Description | MVP? | Dependency / Notes |
|---|---|---|---|---|---|
```

Then ask the user to confirm:

- Which features must be built now?
- Which features should be excluded?
- Which features can wait for later?
- Whether the priority order matches the user's real urgency.

Prioritize features that unlock the main user flow, data creation/consumption, required permissions, and visible acceptance first.

### 3. Tech Stack Recommendation

Recommend an implementation stack based on product size, complexity, deadline, and the user's technical level.

Include:

- recommended frontend, backend, database/storage, auth, hosting/deployment, AI model/tooling if applicable
- which features each technology supports
- why this stack is simplest or safest for the product
- a fallback simpler stack if the project is small

Ask for confirmation before locking the stack.

### 4. Wireframe-Level Prototype

Design core screens in framework/wireframe form, not high-fidelity visual design.

Include:

- screen list
- primary user flow
- key UI regions and controls
- empty/loading/error states when important

Use compact text wireframes when helpful:

```markdown
[Top Bar: Product Name | Settings]
[Left Nav: Module A / Module B]
[Main: List / Detail / Primary Action]
[Footer or Status: Save state / Errors]
```

Ask the user to confirm the core screens and flow before detailed design.

### 5. Detailed Functional Design

Choose the appropriate detail mode:

**For AI products**, include:

- agent goal and boundaries
- agent workflow steps
- prompt structure and key instructions
- tool system: tool names, purpose, inputs, outputs, failure handling
- memory/context strategy
- human confirmation points
- safety, privacy, and escalation behavior

**For traditional products**, include:

- module behavior
- data model or key entities
- page/component behavior
- API or integration requirements
- permissions and roles
- state transitions
- validation, errors, and edge cases

### 6. Acceptance Criteria

Write concrete acceptance criteria for every feature. Use observable behavior:

- Good: "When the user clicks `Generate PRD`, the system shows a loading state, then displays an editable Markdown PRD."
- Good: "If the Feishu link cannot be read, show an error message and ask the user to paste the content manually."
- Bad: "The experience should be smooth."
- Bad: "The output should be high quality."

Prefer `Given / When / Then` or step-based criteria. Include negative and edge cases for important flows.

## Final Output Format

Return the final PRD as Markdown only, using this structure:

```markdown
# Vibe Coding PRD: {Product / Feature Name}

## 1. Input Summary
- Input type:
- Source material:
- Confirmed requirement:

## 2. Requirement Definition
- Target user:
- Scenario:
- Problem:
- Product form:
- Success outcome:

## 3. Scope and Priority
| Priority | Level 1 Module | Level 2 Feature | Description | MVP? | Build Now / Later / Not Now |
|---|---|---|---|---|---|

## 4. User Flow
1. ...

## 5. Wireframe-Level Screens
### Screen 1: ...

## 6. Recommended Tech Stack
| Area | Recommendation | Why | Supports |
|---|---|---|---|

## 7. Detailed Functional Design
### Module: ...

## 8. AI Agent Design
Only include this section for AI products.
- Agent workflow:
- Prompt design:
- Tools:
- Human confirmation points:
- Failure handling:

## 9. Data, Integrations, and Permissions
- Data entities:
- External integrations:
- Roles/permissions:

## 10. Acceptance Criteria
### Feature: ...
- Given ...
- When ...
- Then ...

## 11. Out of Scope
- ...

## 12. Open Questions
- ...
```

If no open questions remain, write `None`.
