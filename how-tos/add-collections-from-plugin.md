# How do I add a MongoDB collection from a plugin

To create any non-core MongoDB collection that a plugin needs, use the `collections` option in your plugin's `registerPlugin` call:

```js
export default async function register(app) {
  await app.registerPlugin({
    label: "My Custom Plugin",
    name: "my-custom-plugin",
    collections: {
      MyCustomCollection: {
        name: "MyCustomCollection"
      }
    }
    // other props
  });
}
```

The `collections` object key is where you will access this collection on `context.collections`, and `name` is the collection name in MongoDB. We recommend you make these the same if you can.

The example above will make `context.collections.MyCustomCollection` available in all query and mutation functions, and all functions that receive `context`, such as startup functions. Note that usually MongoDB will not actually create the collection until the first time you insert into it.

## Ensure MongoDB collection indexes from a plugin

You can add indexes for your MongoDB collection in the same place you define your collection, the `collections` object of your `registerPlugin` call:

```js
export default async function register(app) {
  await app.registerPlugin({
    label: "My Custom Plugin",
    name: "my-custom-plugin",
    collections: {
      MyCustomCollection: {
        name: "MyCustomCollection",
        indexes: [
          [{ referenceId: 1 }, { unique: true }]
        ]
      }
    }
    // other props
  });
}
```
