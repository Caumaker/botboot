# BotBoot concept

## The problem

Teams of agents usually coordinate through chat. Chat is easy to start and hard to trust: instructions are plain text, nobody knows who really sent them, work disappears when a window closes, and there is no clear point where a human says yes or no.

## The answer

BotBoot replaces "agents talking in a chat" with five small, boring pieces.

### 1. Goal

A goal is a record, not a sentence in a conversation:

- `objective`: what must be true at the end
- `done`: how anyone can check it (tests, a file, a URL, a reviewer's approval)
- `scope`: what may be touched, and what may not
- `budget`: maximum cycles, wall time and cost
- `gates`: which steps need a human

### 2. Queue

Items are stored durably. Each item moves through `queued -> claimed -> done | failed | blocked`. A claim is a lease with an expiry, so a crashed executor does not hold work forever. Every transition is appended to the item's history.

Delivery is at least once. Consumers deduplicate by item id or event id. BotBoot does not promise exactly once.

### 3. Gate

A gate is a pause that only a named human can release. Typical gates: publish, deploy to production, delete data, spend money, change permissions, contact third parties. A refused gate ends the cycle and records the reason.

### 4. Adapter

An adapter turns a participant (a model, a CLI agent, a script, a person behind a form) into something that can claim an item and return a result. Adapters hold no state: everything they need comes from the item, and everything they produce goes back to the queue.

### 5. Envelope

Every event is wrapped in an authenticated envelope (see [envelope.md](envelope.md)). Authority comes from the signature and the sender's role, never from what the text claims ("the owner said so").

## Budgets and stopping

An executor stops when any of these happens: the definition of done is met, the budget is exhausted, a gate is refused, or the same item fails a set number of times. Loops have fixed ceilings by design.

## What BotBoot is not

- Not a model or an agent framework. It coordinates agents you already have.
- Not a CI system. It can call one at a gate.
- Not tied to a vendor, a host or a product.
