# How to Run plugin code on app startup
Copy the following into a startup.js file in the plugin folder:

```js
/**
 * @summary Called on startup
 * @param {Object} context Startup context. This is the normal app context but without
 *   any information about the current request because there is no current request.
 * @returns {undefined}
 */
export default function startup(context) {
  // Plugin startup code here
}
```

Then import and register the startup function in the plugin's `index.js` file:

```js
export default async function register(app) {
  await app.registerPlugin({
    label: "Shipping",
    name: "reaction-shipping",
    functionsByType: {
      startup: [startup]
    }
    // other props
  });
}
```

If you want to run certain code before startup functions run (for example to check if something exists before the full startup begins)
you can use `preStartup` the same way you use `startup`. If you want to run something **after** all startup functions have completed you
can use 
