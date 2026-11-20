# BotBoot

**BotBoot** puts a team of bots to work: people and AI agents pursuing one explicit goal, with a durable queue, human approval and authenticated events.

The name joins two words. *Bot* comes from robot. *Boot* comes from bootstrap: the strap on a work boot you pull to get it on, and the small loader that starts a whole system. BotBoot is that strap and that loader for a team.

> Status: concept and specification. There is no running service, no hosted instance and no integration shipped yet. Everything below describes the intended design.

## Principles

1. **Explicit goal.** Every cycle starts from a written objective with a definition of done, a scope and a budget (turns, time, cost). Nothing runs "until it feels finished".
2. **Durable queue.** Work items live in persistent storage, not in a chat window or in process memory. A restart loses nothing; every item has a state and a history.
3. **Human approval.** Irreversible or outward-facing steps (publishing, deleting data, spending money, changing permissions) stop at a gate and wait for a named human to approve.
4. **Stateless adapters.** Each agent, model or tool connects through an adapter that keeps no state of its own. State belongs to the queue. Adapters can be swapped, restarted or run in parallel.
5. **Authenticated event envelopes.** Every message between participants travels in a signed envelope that says who sent it, for which goal, and when. Unsigned or unknown senders are ignored, so text alone never carries authority.

## Roles

| Role | Does | Does not |
|---|---|---|
| Owner | Sets goals, approves gates | Babysit every step |
| Coordinator | Breaks the goal into items, reviews results | Write to production |
| Executor | Implements one item at a time inside its budget | Approve its own gates |
| Integrator | The single participant allowed to merge | Implement |
| Reviewer (optional) | Comments on artifacts | Change them directly |

Any role can be filled by a person or by an agent. One participant can hold more than one role, except that an executor never approves its own work.

## Cycle

```
goal -> plan -> item queued -> executor claims (lease) -> result + artifact
     -> review -> gate (human, if required) -> integrate -> next item or stop
```

A cycle ends when the definition of done is met, the budget runs out, or a gate is refused. Each of those endings is recorded.

## Core and optional pieces

The core is independent of any specific product: goal, queue, envelope, adapter interface and gate.

Possible integrations are **proposals, not proven features**:

- Hosting on any small virtual server, for example a VPS such as the one used by Agentwares. This is a generic example, not a requirement.
- Reviewing artifacts in a commentable document tool, for example Viberich. This is optional.
- Waking a coordinator through MCP events or webhooks.
- Git hosting with a single integrator and deploy gates.

## Documents

- [docs/concept.md](docs/concept.md): the concept in more detail
- [docs/envelope.md](docs/envelope.md): draft of the event envelope

## Founder

Created by **Cauê Somensi**.

## License

MIT, see [LICENSE](LICENSE). It covers the original material in this repository.
