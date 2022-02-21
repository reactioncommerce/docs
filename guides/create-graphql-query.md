## At a glance

You're working on a new implementation for Leader of the Pack, an outdoor equipment retailer. Let's say that you don't ship Kayaks to every location because of the logistical challenges of shipping something that large. So you need to add a query that will show you if a particular product can be shipped to a particular zip code. Once you have that data in the system you need to have a way to make it available to the client. This is usually done by creating a GraphQL query. In this guide, we’ll walk you through the steps you need to create a query in GraphQL and have it recognized by the system.

## What you need

You should already have an understanding of what fields/data you wish to share with the client and what GraphQL type each field is. Consult the [GraphQL Reference](https://graphql.org/learn/schema/) for more info on GraphQL types.

## Identify which plugin owns the query

The complete Open Commerce GraphQL API is created by stitching together domain-specific APIs from all the API plugins. So when adding a new query, the first step is to decide which plugin should own it. This is usually obvious, but not always. You should think about whether any other plugins or services will need to call your query.
Typically, if you are adding data your new query will exist in your custom plugin.

## Name the query

When choosing a name for the query, there are a few rules to follow:
- In keeping with general GraphQL best practices, do not use verbs such as "list", "get", or "find" at the beginning of your query name. For example, use "cart" instead of "getCart" and "carts" instead of "listCarts".
- Prefix with adjectives as necessary to fully describe what the query returns. For example, "anonymousCart" and "accountCart" queries.
- If there are similar queries that take slightly different parameters, add a suffix to clarify. In most cases, we begin the suffix with "By". For example, "accountCartById" and "accountCartByAccountId".

## Define the query in the schema

- If it doesn't already exist, create `schemas` folder in the plugin, and add an `index.js` file there.
- If it doesn't already exist, create `schema.graphql` in `schemas` in the plugin.
- Import the GraphQL file into `index.js` and default export it in an array:

```js
import importAsString from "@reactioncommerce/api-utils/importAsString.js";

const schema = importAsString("./schema.graphql");

export default [schema];
```

> NOTE: For large plugins, you can split to multiple `.graphql` files and export a multi-item array.

- In the `.graphql` file, add your query within `extend type Query { }`. Add an `extend type Query` section near the top if the file doesn't have it yet.
- If your query returns multiple documents, it should return a Relay-compatible Connection and accept standard connection arguments. This is true of any `fields` on any types you create as well.

   Example: `groups(after: ConnectionCursor, before: ConnectionCursor, first: ConnectionLimitInt, last: ConnectionLimitInt, sortOrder: SortOrder = asc, sortBy: GroupSortByField = createdAt): GroupConnection`

- Document your query, the new types, and all fields in those types using string literals. <!-- TODO: See [Documenting a GraphQL Schema](../guides/developers-guide/core/developing-graphql.md#documenting-a-graphql-schema) -->.
- If not already done, register your schemas in the plugin's `index.js` file:

```js
import schemas from "./schemas";

export default async function register(app) {
  await app.registerPlugin({
    graphQL: {
      schemas
    },
    // other props
  });
}
```

## Create the plugin query file

- If it doesn't already exist, create `queries` folder in the plugin, and add an `index.js` file there.
- In `queries`, create a file for the query, e.g. `isAvailableToShip.js` for the `isAvailableToShip` query. The file should look something like this:

```js
import Logger from "@reactioncommerce/logger";

/**
 * @method isAvailableToShip
 * @summary TODO
 * @param {Object} context - an object containing the per-request state
 * @return {Promise<Object>} TODO
 */
export default async function isAvailableToShip(context) {
  Logger.info("isAvailableToShip query is not yet implemented");
  return null;
}
```

## Add the plugin query to the queries context

In `queries/index.js` in the plugin, import your query and add it to the default export object. Example:

```js
import isAvailableToShip from "./isAvailableToShip.js"

export default {
  isAvailableToShip
};
```

If this is the first query for the plugin, you'll also need to pass the full `queries` object to `registerPlugin` in the plugin's `index.js` file:

```js
import queries from "./queries";

export default async function register(app) {
  await app.registerPlugin({
    queries,
    // other props
  });
}
```

Your plugin query function is now available in the GraphQL context as `context.queries.isAvailableToShip`.

> NOTE: The queries objects from all plugins are merged, so be sure that another plugin does not have a query with the same name. The last one registered with that name will win, and plugins are generally registered in alphabetical order by plugin name. Tip: You can use this to your advantage if you want to override the query function of a core plugin without modifying core code.

## Add a test file for your query

If your query is in a file named `isAvailableToShip.js`, your Jest tests should be in a file named `isAvailableToShip.test.js` in the same folder. Initially you can copy and paste the following test:

```js
import mockContext from "/imports/test-utils/helpers/mockContext";
import isAvailableToShip from "./isAvailableToShip.js";

test("expect to return a Promise that resolves to null", async () => {
  const result = await isAvailableToShip(mockContext);
  expect(result).toEqual(null);
});
```

This of course should be updated with tests that are appropriate for whatever your query does.

## Create the GraphQL query resolver file

> Note: All of these elements are required for your query to work and if missing often will not throw an error. If your query is not showing or not returning any data up you probably need to recheck that all of these pieces are here and have the correct name and format.

- If it doesn't already exist, create `resolvers` folder in the plugin, and add an `index.js` file there.
- If it doesn't already exist, create `resolvers/Query` folder in the plugin, and add an `index.js` file there. "Query" must be capitalized.
- In `resolvers/Query`, create a file for the query resolver, e.g. `isAvailableToShip.js` for the `isAvailableToShip` query. The file should look something like this initially:

```js
/**
 * @name "Query.isAvailableToShip"
 * @method
 * @memberof MyPlugin/GraphQL
 * @summary resolver for the isAvailableToShip GraphQL query
 * @param {Object} parentResult - unused
 * @param {Object} args - an object of all arguments that were sent by the client
 * @param {Object} context - an object containing the per-request state
 * @return {Promise<Object>} TODO
 */
export default async function isAvailableToShip(parentResult, args, context) {
  // TODO: decode incoming IDs here
  return context.queries.isAvailableToShip(context);
}
```

Make adjustments to the resolver function so that it reads and passes along the parameters correctly. The general pattern is:
- Decode any opaque IDs that are in the arguments
- Call `context.queries.<queryName>` (your new plugin query) with the necessary arguments, and `await` a response.
- Return a single document or an array of them using either `getPaginatedResponse` or `xformArrayToConnection` util function.

## Register the resolver

In `resolvers/Query/index.js` in the plugin, import your query resolver and add it to the default export object. Example:

```js
import isAvailableToShip from "./isAvailableToShip.js"

export default {
  isAvailableToShip
};
```

If this is the first query for the plugin, you'll also need to import the `Query` object into the `resolvers` object. In `resolvers/index.js` in the plugin, import `Query` and add it to the default export object.

```js
import Query from "./Query"

export default {
  Query
};
```

If you are returning multiple documents, you'll need to add an additional export here, `getConnectionTypeResolvers`, in order to be able to query `edges->node`:

```js
import { getConnectionTypeResolvers } from "@reactioncommerce/reaction-graphql-utils";
import Query from "./Query"

export default {
  Query,
  ...getConnectionTypeResolvers("QueryName")
};
```

Then pass the full `resolvers` object to `registerPlugin` in the plugin's `index.js` file:

```js
import resolvers from "./resolvers";
import schemas from "./schemas";

export default async function register(app) {
  await app.registerPlugin({
    graphQL: {
      resolvers,
      schemas
    },
    // other props
  });
}
```

Calling your query with GraphQL should now work.

## Add a test file for your query resolver

If your query resolver is in a file named `isAvailableToShip.js`, your Jest tests should be in a file named `isAvailableToShip.test.js` in the same folder. Initially you can copy and paste the following test:

```js
import isAvailableToShip from "./isAvailableToShip.js";

test("calls queries.isAvailableToShip and returns the result", async () => {
  const mockResponse = "MOCK_RESPONSE";
  const mockQuery = jest.fn().mockName("queries.isAvailableToShip").mockReturnValueOnce(Promise.resolve(mockResponse));

  const result = await isAvailableToShip(null, { /* TODO */ }, {
    queries: { isAvailableToShip: mockQuery },
    userId: "123"
  });

  expect(result).toEqual(mockResponse);
  expect(mockQuery).toHaveBeenCalled();
});
```

This of course should be updated with tests that are appropriate for whatever your query resolver does. For example, verify that all ID and schema transformations happen.

## Finish implementing your query

Adjust the query function and the query resolver function until they work as expected, with tests that prove it. This will likely involve adding additional arguments, ID transformations, permission checks, and MongoDB queries.

## Update the JSDoc comments

Write/update jsdoc comments for the plugin query function, the query resolver, and any util functions. The resolver function must have `@memberof <PluginName>/GraphQL` in the jsdoc, and the `@name` must be the full GraphQL schema path in quotation marks, e.g., "Query.isAvailableToShip". (The quotation marks are necessary for the output API documentation to be correct due to the periods.)

## More resources

[Build an API plugin guide](https://mailchimp.com/developer/open-commerce/guides/build-api-plugin/)
