# CommitCall Examples

All examples use fictional data. `+15550101234` is reserved for documentation and must never be dialed.

## Complete One-Off Request

```text
Call me once on my own number +15550101234 to check whether I finished the thesis proposal. Use region US, language English, timezone America/New_York, and save the result to ./results/commit-call.csv.
```

Normalized preview:

```yaml
mode: dry-run
recipient: +1******1234
recipient_basis: requesting user's own number
goal: finish the thesis proposal
region: US
language: English
timezone: America/New_York
result_output: ./results/commit-call.csv
dedupe: no matching checkinId found
provider_readiness: blocked until route, authentication, and compatible tools pass
call_side_effect: not authorized
```

The preview must not place or plan a call. When provider onboarding is blocked, it must not offer an execution control that implies a call can run.

## Missing Input

```text
Give me an accountability call about my proposal.
```

Ask only for the missing explicit fields: the user's own E.164 number, region, language, IANA timezone, and result-output path. Do not infer them.

## Adaptive Outcomes

Clear completion:

```json
{
  "completed": true
}
```

Incomplete goal with follow-up:

```json
{
  "completed": false,
  "reason": "I needed more source material",
  "next_checkin_time": "tomorrow at 9 AM"
}
```

No answer or unclear evidence:

```json
{
  "callDisposition": "no_answer",
  "structuredResult": null
}
```

## Dialogue Quality Examples

The provider plan must preserve short, complete turns. It must not expand the user's goal into a checklist or ask several questions at once.

Clear completion:

```text
BOT: Hi, this is CommitCall, an AI check-in you requested. Did you finish this goal: "finish the thesis proposal"? Please say yes or no.
USER: Yes.
BOT: Great. I've recorded it as complete. Goodbye.
```

Incomplete goal:

```text
BOT: Hi, this is CommitCall, an AI check-in you requested. Did you finish this goal: "finish the thesis proposal"? Please say yes or no.
USER: No.
BOT: What is the main reason it is not finished?
USER: I still need two sources.
BOT: When will you work on it next?
USER: Tomorrow at 9 AM.
BOT: Thanks. I've recorded that. Goodbye.
```

Wait and resume:

```text
USER: Wait a moment.
BOT: Okay, I'll wait.
USER: I'm ready.
BOT: Did you finish the goal? Please answer yes or no.
```

The bot must remain silent between the wait acknowledgement and the recipient's next utterance. It must not say filler such as "Take your time" or repeatedly ask whether the recipient is ready.

## Recurring Request

```text
Set up a daily check-in to my own number +15550101234 at 8:30 PM America/New_York about finishing the thesis proposal. Use region US and language English, and save results to ./results/commit-call.csv.
```

Compose with `call-reminder` only after reading `references/scheduling.md`. Preview the schedule and call contract, check for duplicates, and obtain setup approval before creating it. A successful summary must include the scheduler identity and exact cancellation/update instructions.
