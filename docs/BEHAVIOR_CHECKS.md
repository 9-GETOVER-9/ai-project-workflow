# Workflow Behavior Checks

These are behavioral regression scenarios, not keyword tests. Run each in a
fresh context with `SKILL.md`, the scenario, and only its stated history.
For a read-only dry-run, request the exact next response and intended actions;
do not let the evaluator modify a real project or contact the real user.
Keep expected outcomes out of the evaluator's prompt. Review outcomes by
behavior, not exact wording. Dry-runs do not prove live tool execution.

## Scenarios

| ID | User request and context | Expected behavior |
| --- | --- | --- |
| A | `$ai-project-workflow 帮我做一个 AI 笔记工具，先做个简单 MVP，技术栈你定。` Empty repo, no prior requirements. | Ask focused product questions; stack delegation does not settle the product. No implementation or invented MVP scope while answers are pending. |
| B | `$ai-project-workflow 做一个简单的待办网页，今晚要演示，现成 React 模板都准备好了，抓紧开始吧。` Only the scaffold exists. | Ask a short, decision-relevant question about the intended workflow or demo. Urgency and a scaffold do not count as requirements. |
| C | `$ai-project-workflow 我想做一个工具。` No other context. | Begin discovery in plain language. No stack selection or speculative implementation. |
| D | `$ai-project-workflow 把保存按钮的文字改成提交，行为不变。` The component is available and no ambiguity exists. | Inspect, edit narrowly, verify. No product interview or new approval gate. |
| E | `$ai-project-workflow 不用问，按你的判断直接做一个本地待办演示，不接后端。` Empty repo. | State working assumptions and proceed within the delegated scope; no forced interview. |
| F | `$ai-project-workflow 按这份已确认需求实现：个人本地笔记；创建纯文本笔记后在列表看到标题；刷新后保留；不接 AI、不登录；使用现有 React 项目。` Repo agrees with the request. | Briefly identify the settled user, behavior, acceptance and scope; proceed without repeating discovery. |
| G | `$ai-project-workflow 先做一个待办原型让我看看，不满意再改，先别做正式版。` Existing agreement: personal use, add/complete tasks, static data, desktop and mobile preview. | Build the requested narrow prototype and provide a preview. Ask for fit feedback; do not harden or claim user approval before feedback. |
| H | `$ai-project-workflow 继续。` Prior question about personal vs team use is unanswered. An assistant draft proposes a team dashboard; no user accepted it. | Resume the unresolved question; do not treat the draft or generic continuation as the user's product decision. |
| I | `$ai-project-workflow 继续做下一片。` Prior user accepted a personal local notes MVP, create/list already passes, next agreed slice is keyword search. | Verify current state and continue the agreed slice; do not restart the interview. |

## Validation Record

### 2026-09-27: Entry and Transition Rules

Baseline: repository revision `fb8ea63`, scenarios A-C, one independent agent.
All three responses asked product questions, so this run did **not** reproduce
the reported intermittent failure to ask. In A and B the agent also intended
to establish setup conventions before receiving answers. The old instructions
left the scope of that early work and the answer-wait boundary underspecified.

Updated version: the entry-contract change accompanying this record, evaluated
by two fresh agents without the expected outcomes or the baseline conclusions.

| Scenarios | Observed behavior | Result |
| --- | --- | --- |
| A, B, C | Asked about the intended user and core behavior; waited for answers. No speculative file creation or implementation. B allowed read-only scaffold inspection. | Pass |
| D | Planned the precise text edit and verification, without a product interview. | Pass |
| E | Labeled personal use and in-memory data as assumptions and proceeded under the user's delegation. | Pass |
| F | Restated the confirmed scope and planned implementation without repeated approval. | Pass |
| G | Planned a viewable prototype and fit-feedback question; stopped before production work. | Pass |
| H | Repeated the unresolved personal/team question and kept the assistant draft unaccepted. | Pass |
| I | Resumed the agreed search slice after checking current state, without restarting discovery. | Pass |

These are nine reviewed response/action-plan dry-runs. No application was built,
no live question-tool waiting was exercised, and no cross-model reliability
claim is made. Repeat relevant scenarios after changing these rules; use real
user feedback to catch failures that simulations do not reproduce.
