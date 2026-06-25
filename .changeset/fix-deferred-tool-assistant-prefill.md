---
'ai': patch
---

fix(ai): do not continue the tool loop on a pending deferred provider-tool result when there is no client output to send back

When a provider-executed tool with `supportsDeferredResults` (e.g. Anthropic `code_execution`) was pending but a step produced no client tool output, `streamText`/`generateText` would still schedule another model call. The next request was built from response messages whose last message was the assistant turn, which providers like Anthropic reject with `assistant message prefill not supported` / `the conversation must end with a user message`. A pending deferred result now only drives a continuation when there is client tool output (or a denied approval) to send back, which is the only thing that prompts the provider to deliver the deferred result.
