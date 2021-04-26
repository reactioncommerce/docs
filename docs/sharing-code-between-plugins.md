## The basics

Most of Open Commerce’s features are implemented via plugins, so it’s common for one plugin to use functions provided by another. There are three ways for plugins to share functions:

* [Plugin handlers](#plugin-handlers) are the most flexible way to share code or any other information. They can provide dependency and sorting information, track which plugin registered a function, or include additional metadata.
* [Functions by type](#functions-by-type) allows sharing multiple related functions. This approach does not provide the additional information and tracking that plugin handlers can.
* [Queries and mutations](#queries-and-mutations) can register a single function with a particular name.

You will find all three approaches throughout the Open Commerce codebase, but due to their flexibility, plugin handlers are the recommended way to write new code.

In this documentation, we’ll describe all three approaches with example code. For more information on creating plugins, see the [Build an API Plugin](tk) guide.

## Plugin handlers

When you start up Open Commerce, `registerPluginHandler` is called multiple times—once for every plugin that supplies it. The handler is passed `registerPlugin` options from every plugin, providing information about the presence of other plugins and their configurations. 

The handler is also expected to examine the options passed in from each plugin and save any required data. Any plugin can extend the `registerPlugin` options by documenting data it expects to receive from each subsequent one by using `registerPluginHandler`. 

For example, a tax service might expect a `pluginTaxService`, and ask any custom tax widget to provide `pluginTaxService` as part of its registration: 

```js
export function registerPluginHandlerForTaxes({ name: pluginName, taxServices: pluginTaxServices }) {
  if (Array.isArray(pluginTaxServices)) {
    for (const pluginTaxService of pluginTaxServices) {
      taxServices[pluginTaxService.name] = { ...pluginTaxService, pluginName };
    }
  }
}
```

> **Note**: A `registerPluginHandler` function may not return a Promise. If you need to  process the data asynchronously, save it temporarily. Then create and register a `startup`-type function that imports the temporary data, does the async tasks, and returns a Promise. Startup functions are called immediately after `registerPluginHandler` functions in the Open Commerce startup process.

To use a handler in your plugin, include a file named `registration.js` that defines and exports not only `registerPluginHandler` but also any related data required by other files in the plugin. For example, the `reaction-catalog` plugin's `registration.js` file looks like this:

```js title=registration.js
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
      customPublishedProductVariantFields.push(...publishedProductVariantFields);
    }
  }
}
```

The `registerPluginHandler` function looks for the `catalog` key and pushes its data into exported arrays, so any files in `reaction-catalog` that import `customPublishedProductFields` or `customPublishedProductVariantFields` will have the full list of fields as defined by all registered plugins.



To view all fields registered by a particular plugin, look for the `name` key of the object. You can then access and use any data fields that you need.

The `registerPluginHandler` function itself is registered using `functionsByType`:

```js
import { registerPluginHandler } from "./registration";

await app.registerPlugin({
  label: "Catalog",
  name: "reaction-catalog",
  functionsByType: {
    registerPluginHandler: [registerPluginHandler]
  }
});
```


## Functions by type

To provide a function to another plugin for a specific purpose, pass it to `registerPlugin` in the `functionsByType` list. The plugin that calls the functions must document the arguments it will provide and the expected return value and/or side effects. 

For example, this plugin registers functions of type "funky":

```js
import funkyFn from "./funkyFn";

await app.registerPlugin({
  label: "Cart",
  name: "reaction-cart",
  functionsByType: {
    funky: [funkyFn]
  }
});
```

Another plugin can then loop through and call all "funky" functions that were registered by this plugin (or any other):

```js
for (const funkyFn of context.getFunctionsOfType("funky")) {
  // eslint-disable-next-line no-await-in-loop
  await funkyFn(...args);
}
```

## Queries and mutations

If you need to register just one function, you can use `context.queries` or `context.mutations` instead of `functionsByType`. When you use this approach, the plugin only expects to find a function with a particular name. For example, for a function named `expireCarts` on `context.mutations`, call `context.mutations.expireCarts`. 

>**Note**: We don’t recommend adding functions this way, because it’s the most limited approach. Additionally, if more than one plugin registers a query or mutation function, the one listed last in `plugins.json` will "win" and override prior registrations, and no error or warning will be thrown.

Just as with `functionsByType`, the plugin that calls the function must document the arguments it will provide and the expected return value and/or side effects. You should also add your query or mutation function to your GraphQL schema or create a GraphQL resolver for it, unless you intend it to be called only by _other_ plugins.
