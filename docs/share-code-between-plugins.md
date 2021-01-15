<!-- # Share Code Between Plugins -->

## The basics

Because Mailchimp Open Commerce is composed of microservices, sometimes one API plugin needs to provide a function that will be called by another plugin. There are three ways to provide functions.

- [Plugin handlers](#plugin-handler) are the most flexible way to share code or any other information. It can provide dependency/sorting information and additional information, and it can track which plugin registered a function.
- [Functions by type](#functions-by-type) can share multiple functions by identifying what type of functions they are. It does provide the additional information and tracking that plugin handlers can.
- [Queries and mutations](#queries-mutations) can register a single function with a particular name.

We recommend using `registerPluginHandler` in new code.

## Plugin handlers

A `registerPluginHandler` function is called multiple times when Open Commerce starts, immediately after all plugins are loaded. The handler is passed the `registerPlugin` options provided by each plugin. [tk what does this mean? -- The handler is expected to examine the options and save off anything it needs. This means that any plugin can extend the `registerPlugin` options by documenting something it expects to find there.]

The typical and recommended pattern is to include a file named `registration.js` in the plugin, which defines and exports not only your `registerPluginHandler` but also any related data that other files in your plugin require. For example, here is the `reaction-catalog` plugin's `registration.js` file:

```js
export const customPublishedProductFields = [];
export const customPublishedProductVariantFields = [];

/**
 * @summary Will be called for every plugin
 * @param {Object} options The options object that the plugin passed to registerPlugin
 * @returns {undefined}
 */
export function registerPluginHandler({ catalog }) {
  if (catalog) {
    const { publishedProductFields, publishedProductVariantFields } = catalog;
    if (Array.isArray(publishedProductFields)) {
      customPublishedProductFields.push(...publishedProductFields);
    }
    if (Array.isArray(publishedProductVariantFields)) {
      customPublishedProductVariantFields.push(
        ...publishedProductVariantFields,
      );
    }
  }
}
```

This `registerPluginHandler` function looks for the `catalog` key and pushes data from it into exported arrays. In this way, any files in the `reaction-catalog` plugin that import `customPublishedProductFields` or `customPublishedProductVariantFields` will have the full list of fields, as defined by all registered plugins.

To keep track of which plugins are registering what, look at the `name` key in the object. You can save off and export as much information as you need, in whatever format is best.

> **Note**: A `registerPluginHandler` function may _**not**_ return a Promise. If you need to asynchronously process the data, save it temporarily. Then create and register a `startup` type function, which imports the temporary data, does the `async` tasks, and returns a Promise. Startup functions are called immediately after `registerPluginHandler` functions in the Open Commerce startup process.

The `registerPluginHandler` function itself is registered using `functionsByType`:

```js
import { registerPluginHandler } from './registration';

await app.registerPlugin({
  label: 'Catalog',
  name: 'reaction-catalog',
  functionsByType: {
    registerPluginHandler: [registerPluginHandler],
  },
});
```

## Functions by type

To provide a function to another plugin for a specific purpose, pass it to `registerPlugin` in the `functionsByType` list. In this example, a plugin registers functions of type "funky".

```js
import funkyFn from './funkyFn';

await app.registerPlugin({
  label: 'Cart',
  name: 'reaction-cart',
  functionsByType: {
    funky: [funkyFn],
  },
});
```

Another plugin can then loop through and call all "funky" functions that were registered by this plugin (or any other plugins):

```js
for (const funkyFn of context.getFunctionsOfType('funky')) {
  // eslint-disable-next-line no-await-in-loop
  await funkyFn(...args);
}
```

> **Note**: The plugin that calls the functions must document what arguments it will provide and what return value and/or side effects it expects.

## Queries and mutations

To register just one function, `context.queries` or `context.mutations` can be used instead of `functionsByType`. In this case, the plugin simply documents that it expects to find a function with a particular name.

For example, for a function named "expireCarts" on `context.mutations`, call `context.mutations.expireCarts`. Just as with `functionsByType`, the plugin that calls the function must document what arguments it will provide and what return value and/or side effects it expects. If your query or mutation function is intended to be called only by other plugins, you do not need to add it to your GraphQL schema or create a GraphQL resolver for it.

> **Note**: If more than one plugin registers a query or mutation function, the last one registered will "win" and override prior registrations. There is no error or warning thrown in this case. Being able to override these functions in a custom plugin is a feature of Mailchimp Open Commerce.
