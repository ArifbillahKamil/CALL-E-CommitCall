# Scheduling Composition

Read this reference only when the user explicitly requests a recurring accountability check-in.

CommitCall owns the adaptive conversation and structured result. The installed `call-reminder` skill supplies scheduler selection, timezone, late-run, duplicate-job, and cancellation conventions. The host scheduler owns recurrence. CALL-E executes exactly one call per scheduled invocation.

## Setup

1. Require explicit `cadence`, `localTime`, IANA `timezone`, self-owned E.164 number, `region`, `language`, `goal`, and `resultOutputPath`.
2. Read the installed `call-reminder` skill and its client-adapter and safety references.
3. Choose a persistent, visible scheduler that can access the authenticated CALL-E MCP route at runtime.
4. Search scheduler state for an existing CommitCall job with the same dedupe identity. Do not create a duplicate.
5. Render a self-contained scheduled prompt containing the fixed CommitCall workflow and private execution inputs. Do not expose the full number in the setup summary.
6. Preview the schedule, masked destination, goal, result target, late-run policy, and cancellation path.
7. Create or update the schedule only after explicit setup approval and only when the scheduler can return authoritative state.
8. Report the scheduler, job identifier, cadence, timezone, next run if known, masked destination, default 30-minute late-run cutoff, result path, and exact cancellation/update action.

Do not use a CLI bootstrap path. A scheduled run must use the authenticated CALL-E MCP route specified in `SKILL.md`. If that route or its auth is unavailable inside the scheduled environment, do not create the schedule.

## Scheduled Runtime

Each scheduled invocation must:

- skip when more than the configured late-run window late;
- validate all fixed fields and the self-call constraint;
- check the durable dedupe log for that intended occurrence;
- run the CommitCall runtime gate;
- place at most one one-off call;
- reconcile the provider result before durable output; and
- never replay a skipped, timed-out, or ambiguous occurrence automatically.

The prior approved schedule authorizes only its exact configured runtime calls. It does not authorize changes to the phone number, goal, cadence, region, language, timezone, or output target.

## Update and Cancellation

Use only the selected scheduler's documented update/delete mechanism. Verify the resulting scheduler state before claiming success. If no programmatic cancellation is available, provide the exact manual path and state that limitation.

Deleting or disabling the schedule prevents future invocations but does not cancel a provider call already accepted. Reconcile an in-flight provider run separately.
