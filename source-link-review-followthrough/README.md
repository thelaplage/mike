# Source-link review follow-through — 2026-11-14

The existing MikeOSS #884 and MikeGC #38 contributions were updated to address code-review findings. This evidence supplements the original real-client acceptance; it does not replace its exact commit pins.

## Fixes

- Compare structured and text JSON independently of object member order, retaining array-order/value/identity conflict refusals.
- Require explicit HTTP(S) authority in backend and persisted-history URL validation; preserve accepted URL bytes.
- Copy the rendered Source identity, exact URL and access qualification into HTML and plain text, including an explicit refusal when no witness exists.
- Include completed delegated research, recursively; ineligible parents can establish only the refusal, never a clickable Source witness.
- Retain MCP result-only stream frames; distinguish separate same-tool calls and suppress replay by SSE cursor sequence.
- Refuse empty and sanitized Markdown hrefs explicitly.

## Local verification

See verification.json for exact heads, trees, reviewed diff hashes, changed-file hashes and log digests. All focused tests and checks used network-isolated disposable containers with existing exact-lockfile dependencies; no private environment files were copied. A shared local resource reservation bounded CPU/memory.

Frontend focused scope: MarkdownContent, EventBlocks, AssistantMessage, mcpSourceLinks, assistantTurnStream, useAssistantChat.sse, ChatView.actions. Commands, from frontend/:

```
./node_modules/.bin/vitest run src/app/components/assistant/message/MarkdownContent.test.tsx src/app/components/assistant/message/EventBlocks.test.tsx src/app/components/assistant/AssistantMessage.test.tsx src/app/lib/mcpSourceLinks.test.ts src/app/lib/assistantTurnStream.test.ts src/app/hooks/useAssistantChat.sse.test.ts src/app/components/assistant/ChatView.actions.test.tsx --maxWorkers=1 --no-file-parallelism
./node_modules/.bin/tsc --noEmit --incremental false
./node_modules/.bin/eslint src/app/components/assistant/AssistantMessage.tsx src/app/components/assistant/AssistantMessage.test.tsx src/app/components/assistant/message/MarkdownContent.tsx src/app/components/assistant/message/MarkdownContent.test.tsx src/app/components/assistant/message/SourceLinksBlock.tsx src/app/components/assistant/message/EventBlocks.tsx src/app/components/assistant/message/EventBlocks.test.tsx src/app/lib/mcpSourceLinks.ts src/app/lib/mcpSourceLinks.test.ts src/app/lib/assistantTurnStream.ts src/app/lib/assistantTurnStream.test.ts
```

Backend focused scope: sourceLinks, MCP servers, toolDispatcherSpotlight, chat.routes integration, architecture and dependency cycles. Commands, from backend/:

```
./node_modules/.bin/vitest run src/lib/mcp/sourceLinks.test.ts src/lib/mcp/servers.test.ts src/modules/chat/engine/__tests__/toolDispatcherSpotlight.test.ts src/__tests__/integration/chat.routes.test.ts src/__tests__/architecture.test.ts src/__tests__/dependencyCycles.test.ts --maxWorkers=1 --no-file-parallelism
./node_modules/.bin/tsc --noEmit -p tsconfig.json
./node_modules/.bin/tsc --noEmit -p tsconfig.test.json
./node_modules/.bin/tsc --noEmit -p ../packages/contracts/tsconfig.json
```

Totals: MikeOSS 215 backend + 295 frontend; MikeGC 207 backend + 260 frontend. Production/test/contracts typechecks, frontend typechecks and changed-file lint passed.

Retained local diagnostic attempts: the initial new clipboard test used a jsdom FileReader against the suite's Node Blob and one SSE fixture had the wrong expected tool name; both test mistakes were corrected. GC typechecking first exceeded a 4 GiB container, then exposed an older declaration without an interrupted property. The guarded property check fixed compatibility and the final 6 GiB run passed. Earlier failed attempts remain in the local work folder; these four logs are final successful runs.

## Preserved limitations

The original real-model, browser, document and saved-chat acceptance remains at daf018d/f38faca. These follow-up heads have focused local automated acceptance and independent diff review, without a repeated real-model session or fresh production Docker build. Do not transfer the earlier full-journey results automatically. The MikeGC no-tool first response remains a recorded failure; explicitly guided research is the supported evidence. No upstream acceptance, merge, release or fresh PDF hash is asserted. Hosted MCP is frozen; no deployment occurred.
