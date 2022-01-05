# How do I: Emit and Listen for AppEvents

## Background:

AppEvents is a simple interface that allows you to build a more event-drive approach. This allows one part of the app to announce that something important has happened, and other parts of the app
to "react" without the emitter needing to know anything about the listeners. We call sending events "emitting" and listeners "handlers". A simple example of where RC uses appEvents is that when
an order is created an event is emitted for "orderPlaced". Currently there is a listener that listens for this event and sends an email. But what if you wanted to also send a text and a Slack message?
Using a listener in your plugin you could listen for this same event and do whatever action you like without need to modify the original code, providing a convenient way of keeping these uncoupled.

## Get the appEvent object from the context

In any function that receives context (which should be most of them) you should be able to extract the appEvents by doing:

```js
const { appEvents } = context;
```

## Emit an app event

Emit app events in API code using `appEvents.emit`. The `emit` function takes at least two parameters: the name of the event as a string and the `payload` of functions as an object.

### Function parameters and options

- *Event name*: The first argument is the event name, as a string. There is currently no limit to what event name you can emit, but generally try to follow established patterns for naming. See events table below.
- *Payload*: The second argument, the `payload`, should always be an object, and not an entity object. Rather than passing `order` in directly, pass it in an object: `{ order }`, so that more fields can be added or removed from the payload more easily.
- *Option arguments*: The last argument is an array of arguments to pass to each function from `payload`.
- *Using `await`*: Using the method with `await` will not resolve until all registered handler methods have resolved.

## Emit an app event

```js
context.appEvents.emit("eventName", payload, options);
```

## Listen for an app event

See ["Run plugin code on app startup"](run-function-on-startup.md) and attach event listeners in startup code.

```js title=startup.js
export default function startup(context) {
    const { appEvents } = context;

    appEvents.on("eventName", (payload, options) => { // if you pass in context here, all your listeners will have the context available to them
      // Handle the event
    });
}
```

## Avoid infinite loops

It's possible to get stuck in an infinite loop of emitting and listening for the same event. To avoid this, pass `emittedBy` key with a string value in the third options parameter on the `emit`, and check for it on the `on` function:

```js
const EMITTED_BY_NAME = "myPluginHandler";

appEvents.on("afterCartUpdate", async ({ cart }, { emittedBy } = {}) => {
  if (emittedBy === EMITTED_BY_NAME) return; // short circuit infinite loops

  appEvents.emit("afterCartUpdate", { cart: updatedCart, updatedBy: userId }, { emittedBy: EMITTED_BY_NAME });
});
```

## Events

Developers are encouraged to add AppEvent emitters to their code whenever they create a serious event, allowing custom plugin authors to use this for extension

