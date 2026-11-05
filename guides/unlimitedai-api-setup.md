# UnlimitedAI API setup for Claude Code, Codex, Cursor, Cline and Aider

If you searched for an UnlimitedAI API setup guide, this page is the short path from a new key to a working coding tool. UnlimitedAI currently documents one endpoint for multiple frontier models. Always check the official setup page for the current model IDs and plan status: https://unlimitedai.sh/setup-api

## 1. Create a key

Open https://unlimitedai.sh/ and choose a current plan. Copy the API key once and store it in your password manager. Treat the key like a production credential. Do not commit it to GitHub, paste it into a public issue, or put it in a browser extension you do not trust.

## 2. The two base URLs

Use the Anthropic compatible endpoint for Claude Code:

    ANTHROPIC_BASE_URL=https://api.unlimitedai.sh
    ANTHROPIC_AUTH_TOKEN=sk-your-key
    CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1

Use the OpenAI compatible endpoint for tools that ask for an OpenAI base URL:

    OPENAI_API_BASE=https://api.unlimitedai.sh/v1
    OPENAI_API_KEY=sk-your-key

The exact model name is important. Copy a model ID from the official model list instead of guessing.

## 3. Claude Code

macOS or Linux:

    export ANTHROPIC_BASE_URL=https://api.unlimitedai.sh
    export ANTHROPIC_AUTH_TOKEN=sk-your-key
    export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
    claude

Windows PowerShell:

    $env:ANTHROPIC_BASE_URL="https://api.unlimitedai.sh"
    $env:ANTHROPIC_AUTH_TOKEN="sk-your-key"
    $env:CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC="1"
    claude

If Claude Code starts but a model fails, remove the guessed model name and select a model ID shown on the official setup page.

## 4. Codex and other OpenAI compatible clients

Set the base URL to:

    https://api.unlimitedai.sh/v1

Set the API key to your UnlimitedAI key. Keep the provider format set to OpenAI compatible. This works for many clients that expose base URL, API key, and model as separate settings.

A minimal curl check:

    curl https://api.unlimitedai.sh/v1/models \
      -H "Authorization: Bearer sk-your-key"

A successful response confirms that the key and endpoint are reachable. It does not guarantee that every model is enabled on every plan.

## 5. Cursor

In Cursor, open Settings, Models, and add an OpenAI compatible provider. Use:

    Base URL: https://api.unlimitedai.sh/v1
    API key: sk-your-key

Choose a model ID from the current UnlimitedAI model list. Start with a small chat request before enabling long context or agent workflows.

## 6. Cline, Roo Code, Aider, Open WebUI and LibreChat

These tools normally ask for the same three values:

1. Provider: OpenAI compatible
2. Base URL: https://api.unlimitedai.sh/v1
3. API key: your UnlimitedAI key

For Aider, the documented pattern is:

    OPENAI_API_BASE=https://api.unlimitedai.sh/v1
    OPENAI_API_KEY=sk-your-key
    aider --model openai/your-model-id

For Open WebUI or LibreChat, add a custom OpenAI endpoint and use the same base URL. Keep the key on the server side when the app is shared with other users.

## 7. The five errors that waste the most time

### Invalid model

The endpoint is reachable but the model ID is not valid. Copy the ID from the current official list. Do not replace spaces with random punctuation.

### 401 or 403

The key is missing, expired, copied with an extra space, or being sent to the wrong endpoint. Test /v1/models first.

### 404

The client may be appending /chat/completions to a base URL that already contains that path. Use the base URL exactly as the client expects.

### 429

Requests are being throttled or queued. Reduce parallel jobs, add retry with backoff, and check the current plan limits. A queued request is different from a failed request.

### Long context failure

A model may support a large context window while the client has a smaller local limit. Reduce attached files, raise the client context setting, and test with a short prompt first.

## 8. Safe production checklist

- Keep the key in environment variables or a secret manager.
- Rotate a key immediately after accidental exposure.
- Log model name, latency, status code, and request ID, but never log the key or full private prompts.
- Add retry with exponential backoff for transient 429 and 5xx responses.
- Set a client timeout so a stuck request cannot block your worker forever.
- Verify the current plans, model list, and terms at https://unlimitedai.sh/ before shipping.

This guide is maintained as a practical setup reference. Product facts can change, so the official UnlimitedAI pages remain the source of truth.
