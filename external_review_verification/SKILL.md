# External Review Verification

Public pilot adaptation: personal and environment-specific details removed; substance unchanged.

## Verify each reviewer finding against its source

- Open the exact document version the reviewer cites and read the cited section before you act on any finding.

- Quote the cited text beside each finding and mark the finding supported only when the quote matches the claim.

- Reject a finding that quotes a section that does not exist, cites a file that is absent, or states a number with no source in the document.

- Record a rejected finding with its quoted claim and the reason it failed, and do not apply it or carry it into the next review round.

- Treat a reviewer verdict that contradicts its own earlier verdict as unverified until you read both passages and state which one the document supports.

- Check a reviewer claim about code by reading the code itself, as code inspection resolved one finding that fourteen review rounds had left open.

## Stop a review loop that does not converge

- Count conditional rounds for one document and stop iterating when the same class of finding returns for a third round with no new substance.

- Escalate to the user at that point with the remaining findings, the reason each one cannot be closed by another round, and the option to adopt the document with the finding recorded as a known limitation.

- Prefer a structural fix or a simpler design over another review round when a finding keeps regenerating in a new form.

- Do not send another round only to confirm a change that no finding challenged, and do not treat a conditional adoption as a clean adoption.

## Verify a fix where it will actually run

- Test a fix under the conditions where it will run, including an update install when the user already has an earlier version installed.

- Report a fix as fixed only after a live check in that production condition, and label a sandbox or unit test result as tested in sandbox when no live check exists.

- Reproduce the original failure first when you can, then show the same steps passing after the fix.

- Keep the live check steps, the version, and the result in the record beside the fix.

## Available connectors

- Use the connectors and skills available in your environment to find related tools.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up, or build a custom connector.

## Alternatives for better results

- Ask a second reviewer only about the specific disagreement, with the quoted passages attached, instead of restarting a full review.

- Simplify the design or drop the contested rule when a review keeps returning the same finding, and record the trade off in the adoption note.

- Freeze the document with known limitations and a named recheck point when further rounds add cost without new evidence.
