## Summary

- Problem: The "Delete Session" button in the web dashboard relied on a native browser `window.confirm` dialog, which can be confusing and provides a poor, blocking UX.
- Why it matters: Native alerts interrupt the app flow and can be permanently dismissed by users checking "prevent this page from creating additional dialogs," potentially leading to accidental unconfirmed deletions.
- What changed: Replaced `window.confirm` in `sessions.ts` with an inline button state. Clicking "Delete" now changes the button to "Are you sure?" which persists until clicked again or blurred. A secondary click runs the deletion.

## Change Type (select all)

- [ ] Bug fix
- [x] Feature
- [ ] Refactor
- [ ] Docs
- [ ] Security hardening
- [ ] Chore/infra

## Scope (select all touched areas)

- [ ] Gateway / orchestration
- [ ] Skills / tool execution
- [ ] Auth / tokens
- [ ] Memory / storage
- [ ] Integrations
- [ ] API / contracts
- [x] UI / DX
- [ ] CI/CD / infra

## Linked Issue/PR

- Closes #39058

## User-visible / Behavior Changes

Users deleting sessions from the dashboard will no longer see a native browser prompt. Instead, the button turns into a confirmation "Are you sure?" state inline.

## Security Impact (required)

None.

## Repro + Verification

### Environment

- OS: Any
- Runtime/container: Web Browser
- Relevant config: N/A

### Steps

1. Start OpenClaw daemon and Web UI
2. Navigate to Dashboard -> Sessions
3. Click "Delete" on any session
4. Click "Are you sure?" to confirm or click outside to cancel

### Expected

- The button changes state inline and successfully deletes the session upon confirmation.

### Actual

- The browser native `window.confirm` alert was blocking the user.

## Evidence

- [x] Tests updated

## Human Verification (required)

- Verified scenarios: Checked how the button click acts via tests.
- What you did **not** verify: N/A

## Compatibility / Migration

- Backward compatible? `Yes`
- Config/env changes? `No`
- Migration needed? `No`

## Failure Recovery (if this breaks)

- How to disable/revert this change quickly: Revert commit
- Known bad symptoms reviewers should watch for: Unable to delete sessions.
