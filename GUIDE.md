# Event Bus Library

A lightweight event bus for TypeScript applications.

## Overview

`eventBus.ts` provides a minimal, dependency-free EventBus implementation for TypeScript projects. It exports:

- `EventBus` (class) — the main event bus used to publish and subscribe to named events.
- `EventBusOptions` (interface) — construction options (e.g. `maxListeners`, `debug`).
- `EventHandler<T>` (type) — handler signature used by subscribers.

This document describes public APIs, options, and usage examples.

## Installation

Simply include `eventBus.ts` in your TypeScript project and import the exported symbols.

## API Reference (high level)

- subscribe(event: string, handler): Register a handler and receive an unsubscribe function.
- once(event: string, handler): Register a one-time handler that is removed after the first invocation.
- unsubscribe(event: string, handler): Remove a previously registered handler.
- publish(event: string, payload): Emit an event and invoke all handlers for that event.
- clear(event?): Remove all handlers for the provided event, or all handlers if no event is provided.
- getSubscriberCount(event): Returns the number of handlers registered for an event.
- listEvents(): Returns an array of event names that currently have handlers.

See `eventBus.ts` for full TypeScript signatures.

## Options (`EventBusOptions`)

- `maxListeners` (number): Maximum listeners allowed per event (default: 100). If the limit is reached subscribing will throw an error.
- `debug` (boolean): When set to `true` the bus logs basic lifecycle messages to the console.

## Usage Examples

Basic usage:

```ts
import { EventBus } from "./eventBus"; // or the appropriate path to eventBus.ts

const bus = new EventBus({ debug: true, maxListeners: 10 });

const unsubscribe = bus.subscribe("user:login", (payload) => {
  console.log("User logged in:", payload);
});

bus.publish("user:login", { id: 1, name: "Alice" });

// remove subscription
unsubscribe();
```

One-time listeners:

```ts
bus.once("data:ready", (data) => {
  console.log("Data received once:", data);
});

bus.publish("data:ready", { items: [1,2,3] });
// handler will be removed automatically after first publish
```

Inspecting and management:

```ts
console.log(bus.getSubscriberCount("user:login")); // number
console.log(bus.listEvents()); // ["user:login", ...]

bus.clear("user:login"); // remove all listeners for that event
bus.clear(); // remove all listeners for all events
```

## Notes & Best Practices

- Prefer descriptive event names (e.g., `user:created`, `order:paid`).
- Be mindful of `maxListeners` when registering many handlers for the same event.
- Use `once` for handlers that should run only a single time.
- The `debug` option is convenient for development but should typically be disabled in production.

## Source

Primary implementation: `eventBus.ts` (see the repository root).

## Contributing

If you add new public APIs to `eventBus.ts`, update this guide and `api_reference.json` to keep the documentation in sync.
