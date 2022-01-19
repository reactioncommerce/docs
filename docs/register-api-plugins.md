### Register API Plugins

The Reaction API server has a plugin system that allows code to be broken into small packages. Plugins can register functions, configuration, and GraphQL schemas and resolvers.

Some API plugins are included in the API codebase, but in general you should create plugins as NPM packages. This does not necessarily mean they need to be published to NPM, but they must be something you can add to `package.json` and NPM will know how to install it.

At a high level, an API plugin package is one where the main export is an async function that accepts a `ReactionAPI` instance and calls `api.registerPlugin` before returning. In a Reaction API project, your plugin package will be imported and used like this:

```js
import registerSomePlugin from "some-plugin";

const api = new ReactionAPI();

async function startAPI() {
  await registerSomePlugin(api);
  await api.start();
}
```

## registerPlugin

A plugin package must export the following function by default:

```js
export default async function register(app) {
  // register your plugin here
  await app.registerPlugin({
    label: "Plugin Example",
    name: "plugin-example",
    version: pkg.version,
  });
}
```

The two keys that every plugin will include are `name` and `label`. `name` must be unique, cannot contain spaces, and identifies your plugin; `label` is the human-readable version of your plugin name, for showing in UIs.

Beyond `name` and `label`, the following standard keys can be included in your `registerPlugin` object:

- `auth`
- `collections`
- `contextAdditions`
- `expressMiddleware`
- `functionsByType`
- `graphQL`
- `i18n`
- `mutations`
- `queries`
- `simpleSchemas`

### auth

Plugins can pass functions in an `auth` object, which are then used to add permission and account information to `context` for each API request.

```js
auth: {
  accountByUserId,
    getHasPermissionFunctionForUser,
    getShopsUserHasPermissionForFunctionForUser;
}
```

- `accountByUserId`: An async function with signature `(context, userId)` that must return an account document for the given `userId`, or `null` if one cannot be found. This will be used to set `context.account` and `context.accountId`.
- `getHasPermissionFunctionForUser`: An async function with signature `(context)`, which must return an async function that checks permissions for `context.user` and returns a boolean. The signature of the returned function must be `(permissions, shopId)`, where `permissions` is an array of strings and `shopId` is an optional permission filter. The function must return `true` only if the context user has ANY of the permissions.
- `getShopsUserHasPermissionForFunctionForUser`: An async function with signature `(context)`, which must return an async function that checks shop access for `context.user` and returns a boolean. The signature of the returned function must be `(permission)`, where `permission` is a permission string. The function must return an array of shop IDs for which the context user has the given permission.

### collections

Use this to add new collections to the `context` object. Here is a [guide](how-tos/add-collections-from-plugin.md) to help.

### contextAdditions

A plugin can add properties to context using the `contextAdditions` option when calling `registerPlugin`. They are added before any `preStartup` or `startup` functions are run.

```js
app.registerPlugin({
  // ...
  contextAdditions: {
    custom_key: "custom_value",
  },
});
```

```js
// in startup fn or anywhere you have context
console.log(context.something); // "custom_value"
```

### expressMiddleware

Plugins can register Express middleware using `registerPlugin`:

```js
expressMiddleware: [
  {
    route: "graphql",
    stage: "authenticate",
    fn: tokenMiddleware,
  },
];
```

For now, only "graphql" route is supported, and the following stages are supported in this order:

- `first`
- `before-authenticate`
- `authenticate`
- `before-response`

The `first` middleware stage can be used for loggers or anything else that needs to be first in the middleware list. `before-response` middleware will have the user available if there is one, and is called before the Apollo GraphQL middleware.

An `authenticate` middleware function should do something like look up the user by the Authorization header, and either set `request.user` or send a 401 response if the token is invalid. It should not require a token. This is what the built-in `account` service now does.

A middleware function is passed `context` and must return the Express middleware handler function, which must call `next()` or send a response.

### functionsByType

The `functionsByType` object is a map of function types to arrays of functions of that type. This pattern can be used by any plugin to allow any other plugin to register certain types of functions for plugin points.

Documentation for individual plugins will tell you how to use this for that plugin, but there are also a few core types that any plugin might want to use:

- createDataLoaders
- preStartup
- registerPluginHandler
- startup
- shutdown

### graphQL

Use the `graphQL` object to register schemas and resolvers that extend the core GraphQL API. It can have the following structure:

```js
{
      typeDefsObj: <array of typedefs>,
      resolvers: <array of resolvers>,
      schemas: <array of schemas>,
}
```

### i18n

Use the `i18n` object to register translations.

### mutations and queries

The `mutations` and `queries` objects are maps of functions that are extended onto `context.mutations` and `context.queries`. These may be functions that are called from GraphQL resolvers of a similar name, or functions that are intended to be called only by other plugin code. Functions that modify data should be registered as `mutations` and all other functions should be registered as `queries`.

### simpleSchemas

The SimpleSchema package is used by many plugins to validate data, often before inserting or updating MongoDB documents. To allow other plugins to extend these schemas, some plugins register them such that they are accessible in a `preStartup` function.

Here's an example of registering two schemas:

```js
simpleSchemas: {
  Cart, CartItem;
}
```

And a different plugin can then extend them in a `preStartup` function:

```js
export default function preStartup(context) {
  context.simpleSchemas.CartItem.extend({
    additionalCartItemField: String,
  });
}
```

(This could be done in a `startup` function, but because startup code sometimes validates against these schemas, it's safer to do it in a `preStartup` function.)
