# How to extend a Simple Schema

## Background

While Reaction uses a (mostly) schemaless database, we do value data validation and do that by using the package [SimpleSchema](https://www.npmjs.com/package/simpl-schema) which provides
sophisticated data type checking not available in something like traditional SQL schemas or something like JSON schema. However, when
adding functionality via plugins it can often be necessary to modify or extend these schemas to store extra data. This doc explains how to do that.

## Determining if the schema you want to extend is extendable

Generally there are two types of schemas public and private. Private schemas are schemas that are used by internal Reaction
processes and may change at any time therefore external plugins are discouraged or sometimes prevented from modifying them.
Public schemas are schemas that available to all plugins and are meant to be extended. You can see if a plugin is meant to be
public by seeing if the schema is added to the context via registration. If it is, it is meant to be public. For example in the built in
`api-plugin-orders` registration file you see where three schemas are made public (which would include a subschemas defined within)

```js file=index.js
    simpleSchemas: {
      Order,
      OrderFulfillmentGroup,
      OrderItem
    }
```

## Adding a field to an existing schema

Probably the most common thing you need to do is add a field or fields to an existing schema. You do this by:

1. Obtaining the existing schema from the context
2. Creating a new schema with the extra fields you need
3. Using the `extend` keyword on the existing schema

For example say you want to add `purchaseOrderNumber` field to the `Order` schema. You would add a function in the startup sections of
your plugin registration file, and then in that file extend the schema. So your registration file might look something like this:

```js title=index.js
import extendSchemas from "./startup/extendSchema.js";

export default async function register(app) {
  await app.registerPlugin({
    label: "My Custom Plugin",
    name: "custom-order-data",
    version: pkg.version,
    functionsByType: {
      preStartup: [extendSchemas]
    }
  });
}
```

Then the function in `extendSchemas.js` would look like this:

```js title=extendSchemas.js
import SimpleSchema from "simpl-schema";

const purchaseOrderSchema = new SimpleSchema({
  purchaseOrderNumber: String
});

export default function extendSchemas(context) {
  const { simpleSchemas: { Order } } = context;
  Order.extend(purchaseOrderSchema);
}
 ```

Since `context` here serves as a giant Singleton that is shared throughout the entire app, your now mutated schema is not available everywhere.
