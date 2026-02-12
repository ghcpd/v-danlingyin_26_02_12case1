# Event Bus Library

A lightweight event bus for TypeScript applications.

## Overview

`EventBus` is a tiny, dependency-free publish/subscribe helper implemented in TypeScript. It's suitable for in-memory eventing inside a process (components, UI, services) and exposes a small, well-documented API for subscribing, publishing and managing listeners.

Source: `eventBus.ts`

---

## Installation

This repository is a source library — drop `eventBus.ts` into your project or import it directly from the package when published.

---

## Quick example

```ts
import { EventBus } from "./eventBus";

const bus = new EventBus({ debug: true });

function onUserCreated(payload: { id: string; name: string }) {
  console.log("user created:", payload);
}

// subscribe returns an unsubscribe function
const off = bus.subscribe("user.created", onUserCreated);

// publish notifies all listeners for the event
bus.publish("user.created", { id: "u1", name: "Alice" });

// remove the listener
off();

// one-time listener
bus.once("user.created", (p) => console.log("once:", p));

// inspect subscriptions
console.log(bus.getSubscriberCount("user.created"));
console.log(bus.listEvents());

// clear all listeners
bus.clear();
```

---

## Public API (summary)

- `EventBus` (class) — main event bus instance.
- `EventBusOptions` (interface) — configuration accepted by the constructor.
- `EventHandler<T>` (type) — handler signature for events (`(payload: T) => void`).

### EventBus constructor

new EventBus(options?: Partial<EventBusOptions>)

Options:
- `maxListeners` (number, default 100) — maximum listeners allowed per event; adding beyond this throws.
- `debug` (boolean, default false) — when true, the bus logs subscription events to the console.

### Instance methods

- `subscribe(event: string, handler)` → `() => void`
  - Register a persistent handler. Returns an unsubscribe function.
- `once(event: string, handler)` → `void`
  - Register a one-time handler that is removed after it fires.
- `unsubscribe(event: string, handler)` → `boolean`
  - Remove a specific handler for an event; returns whether removal occurred.
- `publish(event: string, payload)` → `number`
  - Emit an event; returns the number of handlers invoked.
- `clear(event?: string)` → `void`
  - Remove all handlers for `event`, or all events if no argument provided.
- `getSubscriberCount(event: string)` → `number`
  - Return how many listeners are registered for `event`.
- `listEvents()` → `string[]`
  - Return all event names that currently have listeners.

---

## Usage notes & best practices

- This EventBus is in-memory and synchronous. Do not use it for cross-process or persistent messaging.
- Prefer `once` for single-shot handlers (e.g., initialization events).
- Reuse a single `EventBus` instance where possible; avoid creating many instances.
- Tune `maxListeners` to avoid accidental listener leaks in long-running components.

---

## Troubleshooting

- If you hit a `Max listeners` error, audit subscriptions and either remove unused handlers or increase `maxListeners` in the `EventBusOptions`.
- For debugging, enable `debug: true` to get basic subscription logs.

---

## Contributing

Contributions should include unit tests for behavior changes. Keep the API small and synchronous to preserve compatibility.

---

## License

MIT

