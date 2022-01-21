# The `context` object

Apollo Server passes a `context` object to every GraphQL resolver. By convention, plugins pass this down as the first argument to all internal query, mutation, and util functions, too. We refer to this as the app context, and many different things live on it. It is the primary way that functions and other information are shared among plugins.

There are two types of properties on the app context: instance properties and request properties. Most properties are instance properties; they are set when you initialize a `ReactionAPICore` instance or when you register a plugin, and they do not change while the API is running. A few other properties are added to the context at the start of every request that comes in, and these are the request properties.

### Instance properties:

- `app`: The `ReactionAPICore` instance that owns this context. (Note that `api.context.app` is a circular reference.)
- `appEvents`: A simple event mechanism with `on` and `emit` properties. This allows event-based communication among plugins. Unlike Node's built-in `EventEmitter`, `appEvents.on` functions may return Promises and `appEvents.emit` may be awaited in situations where you want to be sure all event listeners have handled the event before continuing.
- `appVersion`: A string set to whatever the `version` option is when creating the `ReactionAPICore` instance
- `auth`: Contains all keys registered by the plugins using the `auth` key in call to `registerPlugin`
- `collections`: An object with references to [MongoDB collections](http://mongodb.github.io/node-mongodb-native/3.5/api/Collection.html), built by any plugins that register collections. Allows direct access to collections from various plugins even if only one plugin "owns" that collection.
- `getInternalContext`: A function that takes no arguments and returns a context with all authentication and authorization mocked to allow anything. **Useful when you need to pass down context to another query or mutation but circumvent its normal authorization checks.**
- `getFunctionsOfType`: A function that takes one argument, a type string, and returns an array of all functions that were registered with that type. This is a simple way that plugins can share functions with other plugins for specific purposes
- `mutations` and `queries`: These objects have references to all functions that plugins have registered in their own `mutations` and `queries` objects, when calling `registerPlugin`. If multiple plugins have mutations or queries with the same name, the last plugin to be registered will win (unlike `getFunctionsOfType` where multiple functions may be registered).
- `pubSub`: An object of `PubSub` for subscriptions. Read more about [subscriptions here](https://www.apollographql.com/docs/apollo-server/data/subscriptions/).
- `rootUrl`: The root URL string, from `rootUrl` instance option or `ROOT_URL` environment variable
- `getAbsoluteUrl`: A function that is bound to `rootUrl`, for easy conversion of a relative URL to one that is absolute and begins with `rootUrl`.

There may be additional instance properties if any plugins have added them using the `contextAdditions` option for `registerPlugin`. Refer to documentation for individual plugins.

### Request properties:

- `user`: The User document for the user who made the request. This is set to the Express `request.user` property, which any plugin can set by registering request middleware.
- `userId`: If `user` is set, this will be the `user._id`, for convenience.
- `account`: If `context.auth.accountByUserId` is a function and `context.user` is set, `accountByUserId` will be called for the request, and the return value will be available as `context.account`. The `user` is a generic authentication document for OAuth, whereas `account` is a Reaction-specific concept where additional information about each user is tracked.
- `accountId`: If `account` is set, this will be the `account._id`, for convenience.
- `userPermissions`: If `context.auth.permissionsByUserId` is a function, it will be called with `(context, userId)` arguments, and the return value is available as `context.userPermissions`.
- `userHasPermission`: A function that you can call to determine whether a user has needed permissions. It calls any functions registered as type `getHasPermissionFunctionForUser`, passing in `context`, and then calls the function returned by each. Returns `true` only if every registered function returns `true`.
- `validatePermissions`: Calls `userHasPermission` and throws an Access Denied error if it returns `false`.
- `requestHeaders`: An object with all HTTP headers for the current request on it, except with `Authorization` and `Cookie` headers removed for security.

You can always access the app context on `api.context` if you have a reference to the `api` instance, but this will never have any request properties set because there is no request happening.

### Add keys to `context`

There are two ways to add to context.

##### Adding objects for sharing:

A plugin can add properties to context using the contextAdditions option when calling registerPlugin. They are added before any preStartup or startup functions are run. This method is used mostly to share objects between plugins.

```graphql
app.registerPlugin({
  contextAdditions: {
    something: "wicked"
  }
});
```

```
// in startup fn or anywhere you have context
console.log(context.something); // "wicked"
```

##### Adding objects in every request:

Sometimes we need specific things to be added to every request. An example of this would be the authentication plugin which adds use based on the `Authorization` header on every request. A plugin can add an array of functions to `functionByType.graphQLContext`. All of these would be `awaited` and the return value will be added to the context on **every** request. For example:

```
app.registerPlugin({
    functionsByType: {
      graphQLContext: [(session) => { awesome: true }]
    },
});
```
