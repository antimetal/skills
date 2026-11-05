---
name: configure-severity-rules
description: Read or update the organization's lasting severity classification rules. Use when the user defines which kinds of incidents should be critical, high, medium, or low, or wants future assessments to reflect business urgency. A request about the urgency of one issue alone is not a request to change organization policy.
---

# Configure severity rules

Severity rules add organization-specific guidance to future severity assessments.
They do not directly set the severity of an issue, rewrite past assessments, replace
the system prompt, or relax the classifier's evidence and output requirements.

## Read, edit, save

1. Determine whether the user wants a lasting rule. "This issue is urgent" is local
   context. "Treat checkout outages as critical going forward" is an organization rule.
   If the scope is ambiguous, clarify it before saving.
2. Call `get_severity_rules`. Keep both `prompt` and `entryId`.
3. Incorporate the requested classification criterion into the current prompt.
   Preserve unrelated rules. Use concrete business impact and evidence conditions;
   do not add tool instructions, credentials, or instructions to ignore other safeguards.
4. If the user already specified the lasting change, save it. Otherwise present the
   proposed wording and obtain agreement on the organization policy before writing.
5. Call `update_severity_rules` with the complete `prompt` and `expected_entry_id`
   copied from the read response's `entryId`, including `null` when returned.
6. Report the saved rule and explain that future assessments use it. Do not claim
   an existing issue's severity changed.

If the tools are unavailable, explain that this workflow requires the MCP
classification settings tools. General `update_rule` instructions and issue edits
are not substitutes for severity configuration.

## Add an urgency rule

User: "From now on, checkout failures that prevent purchases should be critical."

Read the current rules and incorporate:

> Classify checkout failures as critical when evidence shows customers cannot
> complete purchases.

Preserve existing guidance about other systems. Save the combined prompt with the
read token. Do not turn a single user's momentary urgency into a global rule.

## Revise or clear

- To revise a rule, read first and replace the matching criterion without losing others.
- `prompt: null` clears the organization's guidance and leaves default severity
  classification in effect. Use it only when the user asks to clear the rules.
- Respect the tool's text limit. Summarize with the user if the policy is too long;
  do not silently truncate it.
- On a conflict, read the latest rules and reconcile the intended edit. Do not blindly
  overwrite another user's change or automatically retry a failed write.
- If a read fails, do not treat it as `prompt: null`.

Successful changes are audited with actor and configuration version provenance.
The prompt is not copied into product analytics.
