# agent-trace-viewer

Visualize agent tool-call traces as a flame graph. Drop in JSON from an Anthropic or OpenAI trace and see what your agent actually did.

**Live demo:** https://0xelitesystem.github.io/agent-trace-viewer/

Browser-only, single HTML file.

## What it does

Paste a trace, get:

- **Headline metrics**: total duration, tool call count, tokens in/out, estimated cost
- **Timeline flame graph**: each step rendered to scale by duration, color-coded by type
- **Step detail**: full content of each block (text, tool input/output, thinking, errors)

## Trace formats supported

- **Anthropic Messages API**: response objects with `content` blocks (`thinking`, `text`, `tool_use`, `tool_result`)
- **OpenAI Chat Completions**: `choices[].message.tool_calls` with optional `tool_results` array
- **Generic schema**: `{ steps: [{ type, name, duration_ms, tokens_in, tokens_out, content }] }`

Per-step `duration_ms` is required for flame graph rendering. If the SDK you use doesn't include it, wrap your calls to record timing before dumping the trace.

## Why this exists

Agentic systems fail in ways that are hard to see from final output alone:

- Tool loops (calling the same tool 8 times)
- Long thinking blocks that produce nothing useful
- Tools that succeed but return data the agent ignores
- Latency dominated by a single slow tool rather than the model

The big observability platforms (LangSmith, Honeycomb, Phoenix) handle this well but require setup. A drop-in static viewer is useful when:

- You're debugging a one-off trace that won't repeat
- You can't share production data with a third-party service
- You want to share a trace with a teammate without onboarding them

## What's not in the trace

This is a viewer, not an evaluator. It will not tell you whether the agent's answer was correct, whether a tool returned hallucinated data, or whether the system prompt held up under adversarial input. For those questions, see [rag-evaluation-rubrics](https://github.com/0xelitesystem/rag-evaluation-rubrics) and [agent-failure-modes](https://github.com/0xelitesystem/agent-failure-modes).

## Cost estimates

Default rates are Anthropic Sonnet 4.6 ($3/M input, $15/M output). If your trace includes a different model with different costs, the headline cost is a rough comparison only. For accurate pricing across providers, see [llm-pricing-data](https://github.com/0xelitesystem/llm-pricing-data).

## Privacy

The trace JSON you paste never leaves your browser. No requests are made. The HTML file has no external scripts, fonts, or analytics. You can verify by viewing source or running it offline.

## Build

No build. Open `index.html`, or deploy via GitHub Pages.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT.

## Related

- [agent-failure-modes](https://github.com/0xelitesystem/agent-failure-modes): catalog of production agent failures
- [agentic-workflow-patterns](https://github.com/0xelitesystem/agentic-workflow-patterns): design patterns
- [llm-pricing-data](https://github.com/0xelitesystem/llm-pricing-data): current pricing across providers
- [prompt-cost-calculator](https://github.com/0xelitesystem/prompt-cost-calculator): pre-flight cost estimation
