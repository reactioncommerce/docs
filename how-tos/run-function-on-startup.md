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

