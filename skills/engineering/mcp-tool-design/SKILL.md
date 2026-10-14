---
name: mcp-tool-design
description: "Design and audit MCP tools for agent compatibility. Use when defining tool names, descriptions, input or output schemas, errors, resource access, writing compatibility cards, or validating an MCP server alongside overlapping tools. Applies to building tool contracts, not routine use of existing tools."
---

# MCP Tool Design

## Objective

Produce a small, unambiguous MCP tool contract with bounded results, predictable
failure behavior, and compatibility evidence for its intended clients.

## Scope

- Apply to new tool contracts or a focused audit of an existing MCP server.
- Keep audit requests read-only; implement changes only when requested.
- Routine operation of existing tools belongs to an agent tool-use workflow.
- Preserve published contracts unless a breaking change is explicitly authorized.
  This workflow is not a complete protocol implementation or security audit.

## Procedure

1. **Frame the use case.** Identify the task, affected tools, target clients and
   models, SDK version, protocol version, transport, and authorization boundary.
   Inspect available project configuration before asking questions. Check the
   official specification for the target version when defining protocol behavior.
   Record unknown compatibility targets rather than inventing support. Finish
   when the required result and supported environment are explicit, or missing
   information that blocks implementation is reported.
2. **Shape discovery.** Inventory only the affected capabilities and likely
   competitors. Define task-level tools that are easy to distinguish, with names
   unique within the server. Describe what each tool does, when to choose it,
   side effects, and meaningful limits. For multi-server aggregation, test how
   the client disambiguates providers; do not assume server display names are
   globally unique. Prefer a relevant subset or grouping when supported; measure
   tool-selection quality instead of imposing a fixed catalog size. Finish when
   representative tasks map to an identifiable tool, including a no-match case.
3. **Define inputs and state.** Use the supported JSON Schema dialect and existing
   SDK validation utilities. Describe required fields, units, accepted values,
   defaults, and constraints. Prefer shallow inputs when they retain the domain
   meaning; simplify existing schemas only without silently changing contracts.
   Make relevant account, repository, environment, and state handles explicit.
   Document state lifetime and expiry behavior. Enforce input validation and
   per-resource authorization on the server; annotations and model obedience
   are not access controls. Finish when valid, invalid, and unauthorized calls
   have defined behavior.
4. **Define bounded outputs and resource access.** Set task-appropriate result
   limits, filtering, pagination, and explicit indicators of incomplete results.
   Prefer compact structured data when supported. If advertising `outputSchema`,
   return conforming `structuredContent` and check client compatibility, including
   the specified text fallback. Use resource links for large content only with
   a working retrieval path for target clients. Specify URI meaning, access,
   lifetime, and any client-to-server transfer mechanism; a server-local file path
   does not imply access to a client's file. Treat retrieved content as untrusted
   data and keep secrets out of results. Finish when size bounds and successful
   retrieval can be verified, including empty and partial responses.
5. **Define failure and retry semantics.** Follow the target specification's
   distinction between protocol/request errors and tool execution errors.
   Signal execution failures with `isError: true` and actionable, sanitized
   explanations; a success-shaped error string is insufficient. State which
   failures are retryable and how callers reconcile unknown write outcomes.
   For retryable writes, define and test the actual deduplication or idempotency
   mechanism; an annotation alone does not make a write safe to retry. Finish
   when callers can distinguish success, failure, and uncertain completion.
6. **Test compatibility.** Reuse the repository's test framework and approved
   fixtures. Test schema validation, success, empty and large results, pagination
   or truncation, inaccessible resources, execution errors, and uncertain writes
   where applicable. Run task-selection trials with the intended client/model
   alongside similar tools, plus an out-of-scope task. Use a sandbox for effects;
   live credentials and production writes require explicit authorization. Compare
   the same tasks before and after a change when a baseline exists. Finish when
   tests pass, or failures and unavailable checks are recorded without claiming
   untested compatibility.
7. **Document the contract.** Update existing server documentation with a compact
   compatibility card: tool purpose and side effects, schema and state limits,
   output size and latency measurements, authentication requirements, tested
   client/model/SDK/protocol versions, test tasks, and known incompatibilities.
   Separate measured results from estimates and pending checks. Finish when a
   consumer can determine whether the tool fits its environment.

## Completion Criteria

- Names and descriptions support selection alongside competing tools.
- Inputs, state, outputs, errors, and resource access have explicit contracts.
- Permissions and retry safety are enforced by implementation, not metadata alone.
- Compatibility claims identify the versions and tests that support them.
- For an audit, findings identify evidence, impact, and a concrete next check;
  unexecuted tests remain clearly marked.

## Output Format

For design work, return the proposed contract, key tradeoffs, and required tests.
For an audit, list findings by severity with evidence and the smallest useful fix.
For implementation, summarize changes, checks run, and remaining compatibility
limits. Include a compatibility card only when useful to the requested work.

## Sources And Limits

Consult these at the step where their details are needed:

- Step 2 and compatibility rationale: Microsoft Research,
  [Tool-space interference in the MCP era](https://www.microsoft.com/en-us/research/blog/tool-space-interference-in-the-mcp-era-designing-for-agent-compatibility-at-scale/),
  September 11, 2025.
- Steps 1 and 3-5: [MCP tools specification](https://modelcontextprotocol.io/specification/latest/server/tools).
- Step 4: [MCP resources specification](https://modelcontextprotocol.io/specification/latest/server/resources).

The research is an ecosystem snapshot, not a protocol requirement or a universal
performance guarantee. The specification links track the latest version; resolve
the documentation for the actual target version before implementing its behavior.
Tool names are server-scoped; cross-server disambiguation is a client integration
concern. Resource loading and tool grouping depend on client support. This skill
adds conservative engineering checks to the research recommendations rather than
presenting every practice as a finding from the article.