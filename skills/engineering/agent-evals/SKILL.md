---
name: agent-evals
description: "Build and diagnose evaluations for AI agents. Use when creating agent eval tasks or graders, comparing agent changes or models, investigating unreliable eval scores, interpreting repeated-trial metrics, or maintaining capability and regression suites. Not for ordinary application unit tests or routine use of tools."
---

# Agent Evals

## Objective

Build or improve an agent evaluation whose scores reflect real task outcomes,
with evidence that its tasks, graders, and execution environment are trustworthy.

## Scope

- Apply to evaluations of coding, conversational, research, or computer-use agents.
- Keep analysis and audit requests read-only. Add a harness or change an existing
  suite only when requested; reuse the project's evaluation tools first.
- Ordinary application testing and operating existing tools are separate workflows.
- An evaluation supports a specific decision; it is not proof of general agent
  reliability or a replacement for production feedback and human review.

## Working Terms

- **Task:** the request, starting environment, and criteria for a successful result.
- **Trial:** one attempt at a task, starting from an isolated initial state.
- **Grader:** a check of the resulting state, output, or a required behavior.
- **Trace:** the available record of outputs, tool calls, and intermediate results.
  Record only information the system exposes, with sensitive data redacted.
- **Harnesses:** the agent harness determines how the model acts; the evaluation
  harness prepares trials, captures evidence, grades, and aggregates results.
  The evaluated system includes the model and its agent harness together.

## Procedure

1. **Frame the decision.** Identify the agent behavior, affected users, intended
   comparison, and existing eval infrastructure. Separate capability exploration
   from regression protection. Establish the execution budget, trial count, and
   acceptance criteria before running. Preserve existing quality gates; ask when
   an unresolved decision blocks a meaningful test. Finish when the evaluation
   question, scope, and permitted execution are explicit.
2. **Specify representative tasks.** Start with observed failures and manual
   release checks, prioritizing user impact. Cover both when a behavior should
   occur and when it should not, plus relevant boundaries. Define observable
   outcomes and supply the information needed to meet every graded requirement.
   Establish a working reference solution or independently validated outcome for
   each task. Keep reference answers and grading artifacts out of the agent's
   trial environment. Finish when tasks are solvable and independent reviewers
   could reach the same verdict; a small useful set beats an arbitrary task quota.
3. **Choose graders.** Prefer executable state checks and deterministic tests for
   objective requirements. Grade the outcome, not a prescribed tool sequence,
   unless the sequence itself enforces a task or safety requirement. Add separate
   quality dimensions or partial credit where useful, while keeping mandatory
   safety and correctness gates explicit. Use model judges for subjective criteria
   with concrete rubrics, representative human calibration, and an `Unknown`
   option for insufficient evidence. Treat candidate outputs as untrusted judge
   input, not grading instructions. Finish when each criterion has a suitable
   grader and the score aggregation and unknown-result policy are defined.
4. **Control the environment.** Match the relevant production behavior, including
   agent instructions, tool access, and limits. Reset each trial using approved
   isolated fixtures or sandboxes; prevent leakage through leftover files, caches,
   shared accounts, or previous-trial history. Resetting never means deleting a
   user's working tree or production data. Protect graders from candidate changes.
   Use approved datasets and credentials; obtain authorization for external costs,
   data transfers, or live effects. Finish when setup, reset, and evidence capture
   work without contaminating other trials or the user's environment.
5. **Validate the evaluation first.** Run reference solutions through the graders
   and check that known incorrect outcomes fail. Inspect representative traces,
   scores, and judge disagreements to find hidden assumptions, inappropriate exact
   matches, missing evidence, or grader bypasses. A very low score is a reason to
   inspect task solvability and harness limits, not immediate proof of incapability.
   Repair demonstrated evaluation defects without relaxing the agreed behavior.
   Finish when pilot verdicts are justified by evidence, or remaining uncertainty
   is explicit and the evaluation is not presented as validated.
6. **Run comparable trials.** Repeat tasks from clean states within the agreed
   budget. Record task and suite revisions, model and harness versions, prompts,
   tool configuration, grader versions, trial counts, and available sampling
   settings. Capture outcomes and accessible traces, plus relevant cost, latency,
   or token measurements. Keep baseline and candidate conditions comparable.
   Classify agent failures separately from infrastructure failures and inconclusive
   grading; preserve all attempts and report how each category affects denominators.
   Finish when the planned trials are recorded or an execution blocker is reported;
   proposed runs and simulated reviews are not completed agent evaluations.
7. **Interpret and maintain.** Report per-task and aggregate results with sample
   counts, variability, and limits on conclusions. Choose metrics for the product:
   `pass@k` concerns at least one success in k attempts; `pass^k` concerns all k
   attempts succeeding. Declare k and the estimation method; distinguish empirical
   observations from estimates requiring independence or distribution assumptions.
   A best-of-k result does not establish reliable first-attempt behavior. Inspect
   failure traces before attributing a change to the agent, grader, or environment.
   Keep solved capability tasks as regression coverage and add harder realistic
   tasks when a capability suite saturates. Assign suite ownership and feed new
   user failures back into tasks. Finish when the decision is supported, disputed
   outcomes are visible, and follow-up work has an owner or an explicit blocker.

## Completion Criteria

- Tasks have clear, achievable criteria, including relevant negative cases.
- Reference and known-bad outcomes exercise the graders, with human calibration
  recorded or still pending for subjective judgments.
- Trials are isolated and reproducible enough for the intended comparison.
- Reported scores have explicit samples, metrics, uncertainty, and failure categories.
- Claimed evaluations have execution evidence; unrun or simulated checks are labeled.

## Output Format

For a design, provide the evaluation question, task set, graders, isolation plan,
trial budget, and acceptance criteria. For an audit, lead with evidence-backed
findings and the smallest corrective checks. For a completed run, report the
configuration, per-task and aggregate results, failure examples, and remaining
limits. Identify proposed work separately from executed checks.

## Source And Limits

Adapted from Anthropic's
[Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
(January 9, 2026). Consult the article when methodology or rationale needs review;
the operating procedure is self-contained.

The article's examples illustrate possible evaluation designs, not a mandatory
schema, framework, task count, or score threshold. This skill adds conservative
execution and reporting safeguards; it does not claim that a small offline suite
predicts every production failure or that a model judge replaces expert judgment.