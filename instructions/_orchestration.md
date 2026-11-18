# Orchestration

## Spawning a subagent

Use native subagents when available and permitted. Run independent tasks concurrently when supported, within the environment's concurrency limits; otherwise wait for each task in turn. Do not require a specific agent CLI, external spawning script, or background mode.

When delegation is unavailable or not permitted, execute the same work yourself, one item at a time, using its instruction and inputs. Tell the user once that execution is sequential and verification is performed by the same agent that writes the documents. Do not claim independent review.

This fallback applies to every command's subagent requirements, including `exec`. Preserve dependency order, input restrictions, validation, and status updates. In sequential mode, finish and verify each assigned document before switching to the coordinator role to update its index.

Give each subagent its command instruction path, installed skill directory, project root, resolved configuration paths, assigned ID and inputs, and permitted output path. Do not assume subagents inherit the parent's skill context.

## Subagent coordination

- The parent exclusively updates shared indexes.
- Each subagent writes only its assigned document and reports its ID and success or failure.
- As each subagent completes, the parent verifies its document and immediately updates that record to `done` on success or leaves it `pending` on failure.
- Do not wait for other subagents before updating a completed record.
- Serialize all shared index writes through the parent.
