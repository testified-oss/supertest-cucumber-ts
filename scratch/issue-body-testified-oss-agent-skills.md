# agent-skills: add Postmortem Documentation skill documentation improvements

## Summary

This issue requests improvements to the Postmortem Documentation skill entry in the Agent Skills repository to better align with incident postmortem best practices and improve clarity for users.

## Motivation

The existing "Postmortem Documentation" skill provides a basic template for postmortem documents. However, teams using it have reported that it lacks guidance on:

- Explicit ownership and RACI for postmortem activities
- Concrete artifacts and artifacts storage conventions
- Timeboxing and communication cadence during postmortem
- Action item tracking and follow-up mechanisms

## Proposed changes

The Postmortem Documentation skill should be enhanced to include:

1. **Ownership & RACI** — clearly assign roles (Author, Reviewer, Approver, Executor) and decision rights.
2. **Artifacts & Storage** — list expected artifacts (timeline, timeline with evidence, timeline with evidence and actions) and recommended storage location/tagging.
3. **Cadence & Communication** — define postmortem timeline (detection → analysis → remediation) with suggested timeboxes and communication expectations.
4. **Action Items** — require explicit tracking with owners, deadlines, and verification criteria; recommend linking to work items/tickets.

### Example additions to the skill template

- Add a RACI table under "Roles & Responsibilities"
- Add an "Artifacts" section naming expected outputs and storage guidance
- Add a "Postmortem Cadence" subsection with suggested schedule and communication channels
- Add an "Action Items & Follow-up" section describing how to track and close remediation items

## Acceptance criteria

- The updated Postmortem Documentation skill includes the four sections above.
- The guidance is concise and actionable for teams adopting the skill.
- The updated template is documented in the repository's skills/ directory.

## Suggested commit message

```
feat(skills): enhance Postmortem Documentation skill with ownership, artifacts, cadence, and action items

- Add RACI table and owners for postmortem activities
- Define expected artifacts and storage guidance
- Specify postmortem cadence and communication expectations
- Add actionable item tracking guidance
```
