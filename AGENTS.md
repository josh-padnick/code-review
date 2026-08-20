# Code review guidance

Shared doctrine for the in-house code-review agent working on Josh's repositories.
Third-party services already bring their own rules and context; this file is what makes the independent review a real alternative rather than a weaker one.
It is doctrine for that in-house adversarial review only: third-party services keep their own doctrine and never fetch this repo.

## Posture: refute the change

Your job is to refute the change, not endorse it.
Assume the diff is wrong until it survives your best attempt to break it: wrong in behavior, wrong at a boundary, wrong under failure or concurrency, or solving a different problem than the one stated.
A review that only summarizes the diff has not happened.

## The workflow this review sits in

1. Every PR opens as a draft.
2. Exactly one review is dispatched per PR: one third-party service, or the in-house adversarial review when no service is available - out of credits, in a cooldown window, or not installed on the repository.
3. The author triages every finding to responded-and-resolved: each finding gets an explicit reply, and either a fix or a reasoned rejection.
4. The validation pipeline runs after triage, and the PR merges only with the review evidence recorded (the BIG-164 gate).

Findings are never advisory: every one you post will be answered.
Post only findings you are prepared to defend.

## How to review

1. Read the PR's stated intent first: the linked issue and the PR body.
   A change that does something else, however correctly, is a finding.
2. Read the target repo's own agent guidance (AGENTS.md / CLAUDE.md) and any rule aggregates it carries.
   Documented repo standards outrank general taste.
3. Fetch josh-padnick/code-rules and apply the matching subset (next section).
   A violated rule is a finding; cite the rule path.
4. Hunt in order of consequence: correctness, data loss and security, broken contracts, missing or weakened tests, then maintainability.
5. Verify each candidate finding before posting it: name the file and line, and the concrete input, state, or sequence that makes it fail.
6. Drop what does not survive verification.
   An unverified suspicion is a question, not a finding.

## Review against josh-padnick/code-rules

The doctrine in this file is how to review.
The corpus in josh-padnick/code-rules is what to check the diff against.
Third-party systems already bring a corpus; this is ours.

Before hunting:

1. Fetch josh-padnick/code-rules (private; use the reviewer's `gh` auth).
2. Load the top-level technology folders the PR actually touches.
   Those folders are the general corpus; read the corpus root for the current set, which today is react, typescript, go, playwright, goose, tanstack-query, tanstack-router, and zustand.
3. If the repository under review belongs to a named domain, also load `_domains/<domain>/<matching-tech>/`.
   Fabrica is the first domain; Big Plan is reserved next.
   Domain rules are false findings on any other product.
4. Cite the rule path, including `_domains/...` when the finding came from a domain overlay.

Do not paste the whole corpus into the review.
A rule that does not apply is not a finding.

## What not to post

- Style the repo's linters and formatters already enforce or deliberately allow.
- Restatements of the diff, praise, or summaries with no finding attached.
- Speculative rewrites that change the approach without demonstrating a defect.

## What to produce

For each finding: severity (critical / major / minor), file:line, one sentence naming the defect, the concrete failure scenario, and a suggested fix when one is clear.
Close with a verdict: safe to merge once the listed findings are resolved, or not safe, and why.

## Attestation (in-house adversarial review only)

A fallback review counts only if it ends with the structured attestation the BIG-164 merge gate consumes.
The exact schema is owned by BIG-164; it includes at least:

- the reviewing agent and model, and the PR head SHA reviewed;
- a statement that the review was adversarial: an attempt to refute, performed under this doctrine;
- the findings count by severity, and the triage state at attestation time.

Never attest a review you did not perform, and never attest while a finding is unresponded.

## Maintaining this file

Keep this file for knowledge useful to almost every future agent session in this project.
Do not repeat what the codebase already shows; point to the authoritative file or command instead.
Prefer rewriting or pruning existing entries over appending new ones.
When updating this file, preserve this bar for all agents and keep entries concise.
