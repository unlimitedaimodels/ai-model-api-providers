# UnlimitedAI pricing and limits: what unlimited actually means

People search for unlimited AI because normal API billing is hard to predict. This page explains the practical questions before you buy: price, model access, parallel work, request rate, queues, and what unlimited does not mean.

For the live offer and terms, use the official page: https://unlimitedai.sh/

## The short answer

UnlimitedAI is sold as flat-price access instead of a token meter. The current offer describes a Max plan and an Ultra add-on. Exact prices, model names, and availability can change, so confirm them at checkout.

Current plan notes published by UnlimitedAI:

- Max: $100 for the plan period, 8 parallel sessions, and 30 requests per minute per key.
- Requests above the per-key rate are queued rather than silently dropped.
- The plan states no token quota, subject to the current terms and abuse protections.
- Ultra add-on: $300, everything in Max plus 16 parallel sessions and additional frontier models.

These are product limits, not a promise that one key can generate unlimited traffic with no latency. Your client, network, model availability, and queue depth still affect the result.

## What 8 parallel sessions means

Parallel sessions are concurrent jobs. If eight tasks are already running, a ninth task must wait or be handled by your client queue. This matters for:

- coding agents running tests and edits at the same time
- batch document work
- multiple developers sharing one workspace
- long-running tool calls

Parallel capacity is different from total messages. A low-volume user may never reach eight sessions, while an agent team can reach it quickly.

## What 30 requests per minute means

The rate is measured per key. Thirty requests per minute is not the same as thirty conversations. One agent turn can create several requests when it uses tools, retries, or streaming. Add client-side backoff so bursts do not become a wall of 429 responses.

A safe retry pattern:

1. Retry only transient 429 and 5xx responses.
2. Wait 1, 2, 4, then 8 seconds.
3. Add small random jitter.
4. Stop after a bounded number of attempts.
5. Record the request ID and status code without recording the API key.

## Who should choose Max

Max is the practical starting point for:

- one developer using Claude Code, Codex, Cursor, or Aider
- a small team that needs several model families
- people who want predictable monthly spend
- projects that switch between reasoning, coding, and long-context work

The value is not that every model is equally good at every task. The value is having multiple model families available through one workflow and one billing relationship.

## Who should choose Ultra

Ultra is for workloads that actually need more concurrency or the additional frontier model set:

- several agents running simultaneously
- a team with parallel code review and test jobs
- long-running automation where queue time is expensive
- experiments that compare multiple frontier models at once

If your workload rarely uses more than one or two active requests, the extra concurrency may not improve your experience.

## Questions to ask before purchase

- Is the model ID I need enabled on the selected plan?
- Does my client use Anthropic compatible or OpenAI compatible authentication?
- What happens to requests during a queue spike?
- Are there fair-use, abuse, or automation restrictions?
- Can I rotate or revoke keys?
- Which regions, payment methods, and support channels are available?
- What is the refund or cancellation policy?

A serious provider should answer these questions in its current terms. If a claim is not written there, treat it as unconfirmed.

## Bottom line

UnlimitedAI makes sense when flat-price access, several model families, and agent workflows matter more than a per-token invoice. It is not a magic bypass for rate limits. Pick the plan based on concurrency, test one real workflow, and keep a fallback provider for production.

See the practical setup guide here:

- https://github.com/unlimitedaimodels/ai-model-api-providers/blob/main/guides/unlimitedai-api-setup.md
- https://unlimitedai.sh/setup-api
