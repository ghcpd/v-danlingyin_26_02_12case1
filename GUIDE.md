# Event Bus Library

A lightweight event bus for TypeScript applications.

## Features

- Type-safe event handling
- Support for one-time listeners
- Configurable maximum listeners per event
- Debug logging option
- Easy subscription management with unsubscribe functions

## Installation

This library is designed to be used as a module in your TypeScript project. Simply import the necessary exports from `eventBus.ts`.

## Getting Started

### Basic Usage

```typescript
import { EventBus } from './eventBus';

// Create an event bus instance
const eventBus = new EventBus();

// Subscribe to an event
const unsubscribe = eventBus.subscribe('userLoggedIn', (user) => {
  console.log('User logged in:', user);
});

// Publish an event
eventBus.publish('userLoggedIn', { id: 1, name: 'John' });

// Unsubscribe when done
unsubscribe();
```

### Configuration Options

You can configure the EventBus with options:

```typescript
const eventBus = new EventBus({
  maxListeners: 50,  // Maximum listeners per event (default: 100)
  debug: true        // Enable debug logging (default: false)
});
```

### One-Time Listeners

Use `once` for handlers that should only fire once:

```typescript
eventBus.once('appReady', () => {
  console.log('App is ready!');
});
```

## API Reference

### EventBusOptions

Interface for configuring the EventBus.

- `maxListeners`: number - Maximum number of listeners allowed per event (default: 100)
- `debug`: boolean - Enable debug logging (default: false)

### EventHandler<T>

Type alias for event handler functions.

```typescript
type EventHandler<T = any> = (payload: T) => void;
```

### EventBus

The main event bus class.

#### Constructor

```typescript
new EventBus(options?: Partial<EventBusOptions>)
```

Creates a new EventBus instance with optional configuration.

#### Methods

- `subscribe<T>(event: string, handler: EventHandler<T>): () => void`  
  Registers a handler for an event. Returns an unsubscribe function.

- `once<T>(event: string, handler: EventHandler<T>): void`  
  Registers a one-time handler that is automatically removed after firing.

- `unsubscribe<T>(event: string, handler: EventHandler<T>): boolean`  
  Removes a previously registered handler. Returns true if removed.

- `publish<T>(event: string, payload: T): number`  
  Emits an event with a payload, invoking all registered handlers. Returns the number of handlers called.

- `clear(event?: string): void`  
  Removes all handlers for a given event, or all events if none specified.

- `getSubscriberCount(event: string): number`  
  Returns the number of handlers registered for an event.

- `listEvents(): string[]`  
  Returns a list of all event names that have at least one handler.

## Examples

### Managing Subscriptions

```typescript
const eventBus = new EventBus();

const handler1 = () => console.log('Handler 1');
const handler2 = () => console.log('Handler 2');

eventBus.subscribe('test', handler1);
eventBus.subscribe('test', handler2);

console.log(eventBus.getSubscriberCount('test')); // 2

eventBus.publish('test'); // Calls both handlers

eventBus.unsubscribe('test', handler1);
console.log(eventBus.getSubscriberCount('test')); // 1
```

### Clearing Events

```typescript
eventBus.clear('test'); // Clear specific event
eventBus.clear(); // Clear all events
```

## Contributing

This is a simple library. For contributions, please ensure type safety and add tests for new features.
