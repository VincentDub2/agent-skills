---
name: agent-good-practices
description: "Use tools reliably in agent workflows. Use when choosing between overlapping MCP, terminal, or browser tools, coordinating state across tools or agents, handling large tool results, or recovering from ambiguous tool errors. Applies to operating existing tools, not designing MCP servers."
---

# Agent Good Practices

## Objective

Complete a tool-assisted task with an unambiguous execution path, bounded context,
and evidence that the intended result was achieved.

## Scope

- Apply this workflow when tool overlap, context growth, state handoffs, or unclear
  errors create a concrete reliability risk.
- For tool or server implementation, use a design workflow instead.
- For a simple task with one clear tool, proceed directly; keep the checks brief.
- Follow the host's tool policies and permissions. A skill guides tool use; it
  cannot enable tools, change authorization, or enforce client behavior.

## Procedure

1. **Frame the result.** Identify the requested outcome, target resource, permitted
   actions, and a cheap check that would distinguish success from failure. Ask a
   focused question only when missing information blocks execution. Finish when
   the target and verification method are clear.
2. **Choose a tool path.** Discover only capabilities needed for the next step,
   where the client supports selective discovery. Read the selected tool's schema,
   description, and provider identity. For overlapping capabilities, choose the
   tool whose state, access, and output fit the task, subject to host policies.
   Keep a stable path; switch only for an identified limitation. Finish when the
   selected tool and its required arguments are unambiguous.
3. **Establish state.** Check the relevant workspace, repository, branch, account,
   environment, or session in the tool being used. Treat browser, terminal, and
   MCP state as separate until verified. Before a handoff, convey the resource
   identifier, state already observed, actions taken, and remaining work; the
   receiving tool or agent must verify the state it depends on. Parallelize only
   independent operations; serialize dependent or conflicting writes. Finish
   when each operation has a known target and no unresolved state dependency.
4. **Execute with bounded context.** Use documented filters, pagination, limits,
   and field selection to request only the evidence needed. Retrieve linked
   resources on demand. Keep identifiers, errors, and verification evidence when
   summarizing large results. A truncated response is incomplete evidence: narrow
   the request or retrieve the missing portion. Before passing a resource between
   tools, confirm the recipient can access it; a local path is not automatically
   usable by a remote server. Obtain approval before a transfer outside the
   authorized boundary. Finish when the relevant result is available without
   relying on inaccessible or omitted data.
5. **Interpret and recover.** Inspect both status fields and returned content;
   a success flag may accompany an error message. On failure, identify whether
   arguments, authorization, resource access, or execution failed. Correct a
   supported cause before retrying. Retry transient failures only when safe and
   bounded; reconcile uncertain write outcomes before another attempt to avoid
   duplicate effects. Report a blocker when recovery would require unavailable
   access or a changed task scope. Finish when the outcome is known or the blocker
   is explicit.
6. **Verify and report.** Run the check chosen in step 1 against the resulting
   state. Distinguish completed actions, verified outcomes, and unresolved limits.
   Keep the report proportional to the task. Finish when the evidence supports
   completion or clearly explains what remains unverified.

## Completion Criteria

- Tool selection identifies the provider and actual target, not just a similar name.
- Relevant state and resource access are verified across any tool handoff.
- Errors, truncation, and uncertain write outcomes are resolved or reported.
- The requested outcome has been checked, or the missing check is disclosed.

## Output Format

Return the result, the check performed, and any remaining blocker. Explain tool
choice or a tool switch only when it affects the result or needs a user decision.

## Source And Limits

Adapted from Microsoft Research's
[Tool-space interference in the MCP era: Designing for agent compatibility at scale](https://www.microsoft.com/en-us/research/blog/tool-space-interference-in-the-mcp-era-designing-for-agent-compatibility-at-scale/)
(September 11, 2025). Consult it when the rationale or research evidence is needed;
routine execution is self-contained.

The article identifies tool overlap, output size, parameter complexity, ambiguous
errors, and resource-sharing assumptions as compatibility risks. This skill turns
those observations into operating practices; it does not promise a performance
gain or impose a universal tool-count or token limit. Client capabilities and
model limits must be checked in the actual environment.