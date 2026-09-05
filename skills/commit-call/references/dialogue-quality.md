# CommitCall Dialogue Quality Contract

Read this reference before compiling the outbound goal and again when inspecting the provider plan.

## Conversation State Machine

Use one state at a time. Do not merge questions from different states.

1. **Opening:** disclose that the caller is AI, say the check-in was requested by the recipient, quote the goal once, and ask for a yes-or-no completion answer.
2. **Complete:** after a clear yes, acknowledge completion in one short sentence and end the call.
3. **Incomplete reason:** after a clear no, ask for the single main reason. Wait for the answer.
4. **Next work time:** ask when the recipient will work on the goal next. Wait for the answer, acknowledge it, and end.
5. **Unclear:** repeat the short yes-or-no question once. If the answer remains unclear, end politely and leave `structuredResult` null.
6. **Wait:** when the recipient asks for time, acknowledge once and remain silent. Resume only after the recipient speaks again. Return to the state that was interrupted.

## Voice Delivery Rules

- Deliver a complete sentence before yielding the turn; never emit a partial clause.
- Ask one question per turn and wait for the answer.
- Keep ordinary bot turns at 20 words or fewer, excluding the quoted goal in the opening.
- Prefer periods and short questions. Avoid em dashes, semicolons, parenthetical phrases, and comma-heavy clauses in spoken text.
- Quote the user's goal exactly once. Do not expand it into a list of inferred tasks.
- Avoid compound questions, long enumerations, filler, repeated greetings, and repeated acknowledgements.
- Do not speak during an acknowledged wait state.
- Use plain spoken language without Markdown, headings, field names, or JSON.
- Keep the complete call under 90 seconds unless the recipient explicitly asks to pause.

## Provider Plan Rejection Conditions

Reject the plan and do not execute it if any of these are true:

- the opening omits either the AI disclosure or recipient-request disclosure;
- the goal is paraphrased, expanded into subtasks, or repeated throughout the call;
- a turn asks both the completion question and follow-up questions together;
- the yes branch asks another progress question instead of ending;
- the no branch asks why and when in the same turn;
- the plan instructs the bot to fill silence, repeatedly check readiness, or use conversational filler;
- the plan permits an ambiguous response to become `completed: false`;
- the plan lacks a clear terminal action for yes, no, and unresolved answers.

When a plan is rejected, revise only the provider goal or planning input. Do not place a call until a new inspected plan satisfies this contract and receives the required execution approval.
