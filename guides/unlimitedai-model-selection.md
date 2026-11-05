# Which UnlimitedAI model should you use for coding, research, or long context?

A model list is only useful when it helps you choose a model for a real job. This guide maps common tasks to a sensible starting point, then shows how to switch models without rewriting your app.

Check the live model IDs and availability at https://unlimitedai.sh/ and https://unlimitedai.sh/setup-api

## Start with the task

### Coding and repository changes

Use a strong coding or reasoning model when the task includes:

- understanding an unfamiliar repository
- changing several files safely
- running tests and fixing failures
- reviewing a pull request
- planning a migration

Start with a model labeled for coding or reasoning in the current UnlimitedAI list. If latency matters more than deep planning, try the current fast model for the first pass and move the final review to a stronger model.

### Long documents and large codebases

Use a model that exposes the larger context window when you need to:

- compare several documents
- review a large repository
- summarize logs without deleting important details
- trace a bug across many files

A large context window does not remove the need for good retrieval. Send the smallest relevant set of files first, then add more context when the answer needs it.

### Fast classification and routine edits

Use a fast model for:

- tagging tickets
- extracting fields into JSON
- rewriting short text
- generating test cases
- routing support messages

Fast models are usually the best way to control queue time for high-volume work. Reserve the strongest model for exceptions.

### Reasoning and security review

Use a reasoning model for:

- threat modeling
- authorization review
- complex debugging
- edge-case analysis
- comparing competing implementation plans

Require the model to show assumptions, evidence, and a test plan. Never treat an AI answer as proof that a security issue exists.

### Multilingual work

Use a model that performs well on the languages in your product data. Test with your own examples instead of trusting a generic leaderboard. Measure accuracy, tone, and preservation of names or structured fields.

## A practical two-model workflow

Many teams do not need one model for every request.

1. Use a fast model to classify the task and gather the relevant files.
2. Use a stronger model for the final change or decision.
3. Run tests or a human review.
4. Use the fast model again for a concise summary.

This workflow reduces cost and keeps long agent runs easier to debug.

## How to change models safely

Keep the provider settings fixed and change only the model field. For an OpenAI compatible client:

    Base URL: https://api.unlimitedai.sh/v1
    API key: your UnlimitedAI key
    Model: current model ID from the official list

For Claude Code, use the Anthropic compatible endpoint documented here:

    https://api.unlimitedai.sh

Do not copy a model name from an old screenshot. Model aliases change. A valid key with an outdated model ID commonly produces an invalid model error.

## What to benchmark

Run the same small test set on two or three models:

- one real bug fix
- one structured JSON extraction
- one long-context question
- one code review
- one multilingual example if relevant

Record:

- correctness
- number of retries
- latency
- context handling
- tool-call reliability
- human review time

A model that looks impressive in a demo may be slower or less reliable in your actual workflow.

## The honest answer

There is no universally best model. The best choice depends on task difficulty, context size, latency, tool use, and review requirements. UnlimitedAI is useful when you need several model families in one endpoint and want to test them in the same workflow.

For setup examples, see:

- https://github.com/unlimitedaimodels/ai-model-api-providers/blob/main/guides/unlimitedai-api-setup.md
- https://github.com/unlimitedaimodels/ai-model-api-providers/blob/main/guides/unlimitedai-pricing-and-limits.md
