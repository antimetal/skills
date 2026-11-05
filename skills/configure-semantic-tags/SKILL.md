---
name: configure-semantic-tags
description: Define the organization's investigation or remediation semantic tags and when agents should select them. Use when the user wants recurring issue categories, reusable remediation labels, or changes to the available tag vocabulary. If the user only wants to find a type of issue, search first and offer configuration only when they want a lasting preference.
---

# Configure semantic tags

Semantic tags describe findings or remediation characteristics. Organizations define
separate vocabularies for investigation and remediation. Agents may select zero to
many tags from the configured vocabulary. These settings apply to future runs;
they do not retag existing issues or change the system's fix outcome types.

## Read, edit, save

1. Identify the source: `investigation` for findings and root causes, or
   `remediation` for proposed fixes. Ask only if the user's intent does not resolve it.
2. Call `get_semantic_tag_configuration` with `source`. Keep the returned `entryId`
   alongside the existing `tags`.
3. Make the requested edit to the full list. Preserve unrelated definitions.
   Each tag has a `name` and an optional `description` explaining **when to select it**.
   Describe evidence or conditions, not commands to run or changes to agent permissions.
4. When the intended lasting change is clear, call `update_semantic_tag_configuration`
   with `source`, the complete `tags` list, and `expected_entry_id` copied from
   `entryId` (including `null`). Do not write solely because the user mentioned a topic.
5. Summarize what was saved, which source it affects, and that it applies to future runs.

If the tools are unavailable, explain that this workflow requires the MCP
classification settings tools. Do not substitute `update_rule`: general agent
instructions are a different setting.

## Add a category

User: "Going forward, label investigations caused by resource exhaustion Capacity."

Read the investigation configuration. Append this definition if it does not
already exist, or edit the matching definition if that is what the user requested:

```json
{ "name": "Capacity", "description": "The evidence identifies exhausted compute, memory, storage, or connection capacity as a cause." }
```

Save the complete list with the read token. Never assume the existing list is empty.
For "Show me capacity issues", use issue search/filtering instead; that request
alone does not authorize changing the organization's vocabulary.

## Change remediation criteria

User: "Tag fixes that need data repair so we can track them separately."

Read the remediation configuration. Add or refine a `Data repair` definition whose
description says when a fix requires correcting stored records. Preserve the other
definitions, then save. Do not replace execution outcomes such as code or operational
fixes with these semantic tags.

## Clear or recover

- `tags: []` clears only the chosen source's vocabulary. Clear it only when requested.
- Names must be nonblank and unique ignoring case. Respect the tool's count and
  text limits; ask the user to narrow an oversized vocabulary rather than silently
  dropping definitions.
- A conflict means another edit won. Read again, show the competing change when it
  affects the user's intent, and reapply only the intended edit. Do not blindly retry
  the old full list with a new token.
- A failed read is not an empty vocabulary. Do not overwrite settings while reads fail.
