# Exact-head validation

| Client | Tested commit | Focused backend tests | Focused frontend tests |
|---|---|---:|---:|
| MikeOSS | `daf018daa93010103c5f28fdadb2d11f91284797` | 207 passed | 273 passed |
| MikeGC | `f38facad21cec440de1cc2b9a00fc1815db0569b` | 199 passed | 238 passed |

All changed files were compared by SHA-256 with the final check inputs. Independent code reviews returned PROCEED_B; real local acceptance evidence review returned PASS. These are local results, not upstream CI results. The tests are already committed in the ordinary Vitest suites; no separate registry or database migration is needed.

From the candidate repository root (after installing its locked development dependencies):

```sh
npm test --prefix backend -- src/lib/mcp/sourceLinks.test.ts src/lib/mcp/servers.test.ts src/modules/chat/engine/__tests__/toolDispatcherSpotlight.test.ts src/__tests__/integration/chat.routes.test.ts src/__tests__/architecture.test.ts src/__tests__/dependencyCycles.test.ts --maxWorkers=1 --no-file-parallelism
npm test --prefix frontend -- src/app/components/assistant/message/MarkdownContent.test.tsx src/app/components/assistant/message/EventBlocks.test.tsx src/app/components/assistant/AssistantMessage.test.tsx src/app/lib/mcpSourceLinks.test.ts src/app/lib/assistantTurnStream.test.ts src/app/hooks/useAssistantChat.sse.test.ts src/app/components/assistant/ChatView.actions.test.tsx --maxWorkers=1 --no-file-parallelism
./backend/node_modules/.bin/tsc --noEmit -p backend/tsconfig.json
./backend/node_modules/.bin/tsc --noEmit -p backend/tsconfig.test.json
./backend/node_modules/.bin/tsc --noEmit -p packages/contracts/tsconfig.json
npm run typecheck --prefix frontend -- --incremental false
```

Changed-file frontend ESLint also passed. Backend and frontend images were then built independently for each client using its repository Dockerfiles and exact Git archives, with Sentry upload disabled. Source files inside the four actual running images were hashed against their commits. The browser loaded the new Source-link marker.

Real acceptance used fresh loopback-only Docker Compose projects, normal signup/Mailpit confirmation/onboarding, the real browser composer, existing local `qwen-frank:latest` through Ollama, and a real anonymous read-only Counterpedia MCP connector. Nine tools were discovered; this scenario invoked two (`counterpedia.sources` and `counterpedia.get_source_presence`). No model responses, MCP frames, witnesses or database chat rows were injected or mocked.

The SEC prompt deliberately supplied the known bad `comp18200.htm` URL and asked the model to echo it after a real source-list call. The model did echo it; the rendered answer/refreshed chat refused to create a Source link. The court scenario rendered the exact Source-bound original from a genuine SourcePresence result, and real link clicks returned HTTP 200 PDF. Reopen/reload checks used actual chat-ID GET responses with unchanged saved-message digests. PDF bodies were not retained and no fresh PDF-byte digest or native viewer-display claim is made.

The earlier MikeGC court answer without a tool event remains an explicit failure in `gc/client-report.json`. It is excluded from the successful court run. A later fresh explicitly guided chat passed. Model prose still used a relative locator; the accepted clickable original comes from the structured Source links section. This does not establish autonomous first-response or legal-research accuracy.

The isolated public MCP candidate stayed at `1023602a5091a3a9598f87773ec80757b2c368e5`. These are client contributions, with no deployment, upstream release or private-app protection change.
