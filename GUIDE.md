# Event Bus Library

A lightweight, type-safe event bus for TypeScript applications. The EventBus module provides a simple pub-sub pattern implementation with support for one-time listeners and comprehensive subscription management.

## Overview

The EventBus module (`eventBus.ts`) exports the following APIs:

- **EventBus** class - The main event bus manager
- **EventHandler** type - Handler function signature for events
- **EventBusOptions** interface - Configuration options for EventBus instances

## Installation & Setup

```typescript
import { EventBus, EventBusOptions, EventHandler } from './eventBus';

// Create an instance with custom options
const bus = new EventBus({
  maxListeners: 100,
  debug: true
});
```

## EventBusOptions Configuration

The `EventBusOptions` interface allows you to customize the EventBus behavior:

```typescript
interface EventBusOptions {
  maxListeners: number;  // Maximum number of listeners per event (default: 100)
  debug: boolean;        // Enable debug logging (default: false)
}
```

### Options Details

- **maxListeners**: Sets the limit for how many handlers can subscribe to a single event. Attempting to exceed this limit will throw an error.
- **debug**: When enabled, logs subscription and event activities to the console.

## API Methods

### subscribe(event: string, handler: EventHandler): () => void

Register a handler for an event. Returns an unsubscribe function for easy cleanup.

```typescript
const unsubscribe = bus.subscribe('user:login', (payload) => {
  console.log('User logged in:', payload);
});

// Later, unsubscribe using the returned function
unsubscribe();
```

### once(event: string, handler: EventHandler): void

Register a one-time handler that automatically removes itself after the first invocation.

```typescript
bus.once('app:initialized', (data) => {
  console.log('App initialization complete:', data);
  // Handler will be removed after this runs
});
```

### publish(event: string, payload?: any): number

Emit an event with an optional payload. Returns the number of handlers invoked.

```typescript
const handlersInvoked = bus.publish('user:login', { userId: 123, name: 'Alice' });
console.log(`Notified ${handlersInvoked} subscribers`);
```

### unsubscribe(event: string, handler: EventHandler): boolean

Manually remove a previously registered handler. Returns true if removal succeeded.

```typescript
const handler = (payload) => console.log(payload);
bus.subscribe('event:name', handler);
const removed = bus.unsubscribe('event:name', handler);
console.log(removed); // true
```

### clear(event?: string): void

Remove all handlers for a specific event, or clear all events if no event is specified.

```typescript
// Clear specific event
bus.clear('user:login');

// Clear all events
bus.clear();
```

### getSubscriberCount(event: string): number

Return the number of handlers registered for an event.

```typescript
const count = bus.getSubscriberCount('user:login');
console.log(`${count} subscribers listening to 'user:login'`);
```

### listEvents(): string[]

Return a list of all event names that have at least one handler.

```typescript
const activeEvents = bus.listEvents();
console.log('Active events:', activeEvents);
// Output: ['user:login', 'user:logout', 'data:updated']
```

## Complete Usage Example

```typescript
import { EventBus } from './eventBus';

// Initialize EventBus
const eventBus = new EventBus({ debug: false, maxListeners: 100 });

// Subscribe to multiple events
const unsubLogin = eventBus.subscribe('user:login', (user) => {
  console.log(`User ${user.name} logged in`);
});

eventBus.subscribe('user:logout', (user) => {
  console.log(`User ${user.name} logged out`);
});

// One-time listener
eventBus.once('app:ready', () => {
  console.log('App is ready for users!');
});

// Publish events
eventBus.publish('app:ready');
eventBus.publish('user:login', { name: 'Alice', id: 1 });
eventBus.publish('user:login', { name: 'Bob', id: 2 });

// Check active subscriptions
console.log('Active events:', eventBus.listEvents());
console.log('Login listeners:', eventBus.getSubscriberCount('user:login'));

// Unsubscribe when done
unsubLogin();

// Clear all handlers
eventBus.clear();
```

## Source Code

The implementation is defined in `eventBus.ts`. All exported APIs are fully documented with JSDoc comments within the source code.
