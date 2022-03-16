## At a glance

Queries are a way to allow clients to ask a question of the system. Queries can be complex similar to a database 
query, or they can be simple like asking the system a simple yes or no question. This is opposed to mutations which 
are ways via GraphQL to change data. Queries never change data, just return it.

We're working on a new implementation for Leader of the Pack, the outdoor equipment retailer that we just built an API plugin for. The store stocks kayaks, but can't ship them to every location because of the logistical challenges of shipping something that large. This leads to annoyed customers who get excited about buying the kayaks of their dreams but then find that we can't get it to them. We need a way to show if a particular product can be shipped to a particular ZIP code. This is usually accomplished by creating a GraphQL query.

In this guide, we'll walk you through the process of creating a GraphQL query `canShipStandardGround` that will 
query by `productId` and `postalCode` whether a product can be shipped to a particular address.

## What you need

You should have already created a plugin from the [example plugin template](https://github.com/reactioncommerce/api-plugin-example) and that plugin should be installed into your local development environment.
You can also look at the [Leader of the Pack example plugin](https://github.com/reactioncommerce/leader-of-the-pack-example) which contains the finished files for this and other guides.

You should already have an understanding of what fields/data you wish to share with the client and what GraphQL type 
each field is. Consult the [GraphQL Reference](https://graphql.org/learn/schema/) for more info on GraphQL types. 
For our example we will query using `productId` and `postalCode` which are both strings, and return a simple true or 
false, indicating if we can ship this product via a standard ground method.


## Name the query

When choosing a name for the query, there are a few rules to follow:
- In keeping with general GraphQL best practices, do not use verbs such as "list", "get", or "find" at the beginning of your query name. For example, use "cart" instead of "getCart" and "carts" instead of "listCarts".
- Add prefixes with adjectives as necessary to fully describe what the query returns. For example, use "anonymousCart" or "accountCart" to name your queries.
- If there are similar queries that take slightly different parameters, add a suffix to clarify. In most cases, we 
  begin the suffix with "By". For example, "accountCartById" and "accountCartByAccountId". 

For our example we will use `canShipStandardGround`.

## Define the query in the schema

If it doesn't already exist, create a `schemas` folder in the plugin and add an index.js file there. Then check to see 
if there is a schema.graphql file in your schemas directory within the plugin. If there isn't, create that file now. 

> Note: You must have `@reactioncommerce/api-utils` installed as a dependency

Next import the GraphQL file into `index.js` and default export it in an array:

```js
import importAsString from "@reactioncommerce/api-utils/importAsString.js";

const schema = importAsString("./schema.graphql");

export default [schema];
```

> NOTE: For large plugins, you can split to multiple `.graphql` files and export a multi-item array.

In the `.graphql` file, add your query within `extend type Query { }`. Add an `extend type Query` section near the top if the file doesn't have it yet.
If your query returns multiple documents, it should return a Relay-compatible Connection and accept standard connection arguments. This is true of any `fields` on any types you create as well.

   Example: `groups(after: ConnectionCursor, before: ConnectionCursor, first: ConnectionLimitInt, last: ConnectionLimitInt, sortOrder: SortOrder = asc, sortBy: GroupSortByField = createdAt): GroupConnection`

Next, document your query, the new types, and all fields in those types using string literals. <!-- TODO: See 
[Documenting a GraphQL Schema](../guides/developers-guide/core/developing-graphql.md#documenting-a-graphql-schema) -->.

Here is what our example query for LOTP looks like:

```graphql

input CanShipStandardGroundInput {
  "The product we want to ship"
  productId: String!
    
  "The postal code of the address we want to ship to"
  postalCode: String!
}

type CanShipStandardGroupPayload {
  "Can ship standard ground"
  canShipStandardGround: Boolean!
}

extend type Query {
  "Can ship standard ground"
  canShipStandardGround(
    input: CanShipStandardGroundInput
  ): Boolean!
}
```

Notice that we created an `input` type since our query contains multiple fields, but we just return a simple scalar 
value so there is no need to declare another type. All the fields are required so they are suffixed by the ! 
character. Also notice that we document each field with a short description of what is in the field, but we don't need to repeat it's type.

Next we need to register the schema within the plugin.

If not already done, register your schemas in the plugin's `index.js` file:

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

> NOTE: Be careful to make it `graphQL` and not `graphQl` as this will be a difficult bug to find

## Create the plugin query file

- If it doesn't already exist, create `queries` folder in the plugin, and add an `index.js` file there.
- In `queries`, create a file for the query, e.g. `canShipStandardGround.js` for the `canShipStandardGround` query. The file should look something like this:

```js
/**
 * @method canShipStandardGround
 * @summary Query the shipping service and return if we can ship a product via standard ground to the zipcode
 * @param {Object} context - an object containing the per-request state
 * @return {Promise<Boolean>} - If we can ship this product standard ground to this zipcode
 */
export default async function canShipStandardGround(context) {
    // Put your logic here. For this example we just always return false
    return false;
}
```

## Add the plugin query to the queries context

In `queries/index.js` in the plugin, import your query and add it to the default export object. Example:

```js
import canShipStandardGround from "./canShipStandardGround.js";

export default {
    canShipStandardGround
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

Your plugin query function is now available in the GraphQL context as `context.queries.canShipStandardGround`.

> NOTE: The queries objects from all plugins are merged, so be sure that another plugin does not have a query with the same name. The last one registered with that name will win, and plugins are generally registered in alphabetical order by plugin name. Tip: You can use this to your advantage if you want to override the query function of a core plugin without modifying core code.

## Add a test file for your query

If your query is in a file named `canShipStandardGround.js`, your Jest tests should be in a file named 
`canShipStandardGround.test.js` in 
the same folder. Initially you can copy and paste the following test:

```js
import mockContext from "/imports/test-utils/helpers/mockContext";
import canShipStandardGround from "./canShipStandardGround.js";

test("expect to return a Promise that resolves to null", async () => {
  const result = await canShipStandardGround(mockContext);
  expect(result).toEqual(false);
});
```

## Create the GraphQL query resolver file

> Note: All of these elements are required for your query to work and if missing often will not throw an error. If your query is not showing or not returning any data up you probably need to recheck that all of these pieces are here and have the correct name and format.

- If it doesn't already exist, create `resolvers` folder in the plugin, and add an `index.js` file there.
- If it doesn't already exist, create `resolvers/Query` folder in the plugin, and add an `index.js` file there. "Query" must be capitalized.
- In `resolvers/Query`, create a file for the query resolver, e.g. `canShipStandardGround.js` for the `canShipStandardGround` query. The file should look something like this initially:

```js
/**
 * @summary resolver for the canShipStandardGround GraphQL query
 * @param {Object} parentResult - unused
 * @param {Object} args - an object of all arguments that were sent by the client
 * @param {Object} context - an object containing the per-request state
 * @return {Promise<Object>} Results of the canShipStandardGround query
 */
export default async function canShipStandardGround(parentResult, args, context) {
    // TODO: decode incoming IDs here
    return context.queries.canShipStandardGround(context);
}
```

Make adjustments to the resolver function so that it reads and passes along the parameters correctly. The general pattern is:
- Decode any opaque IDs that are in the arguments
- Call `context.queries.<queryName>` (your new plugin query) with the necessary arguments, and `await` a response.
- Return a single document or an array of them using either `getPaginatedResponse` or `xformArrayToConnection` util function.

## Register the resolver

In `resolvers/Query/index.js` in the plugin, import your query resolver and add it to the default export object. Example:

```js
import canShipStandardGround from "./canShipStandardGround.js";

export default {
    canShipStandardGround
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

If your query resolver is in a file named `canShipStandardGround.js`, your Jest tests should be in a file named `canShipStandardGround.test.js` in the same folder. Initially you can copy and paste the following test:

```js
import canShipStandardGround from "./canShipStandardGround.js";

test("calls queries.canShipStandardGround and returns the result", async () => {
  const mockResponse = false;
  const mockQuery = jest.fn().mockName("queries.canShipStandardGround").mockReturnValueOnce(Promise.resolve(mockResponse));

  const result = await canShipStandardGround(null, { /* TODO */ }, {
    queries: { canShipStandardGround: mockQuery },
    userId: "123"
  });

  expect(result).toEqual(mockResponse);
  expect(mockQuery).toHaveBeenCalled();
});
```

This of course should be updated with tests that are appropriate for whatever your query resolver does. For example, verify that all ID and schema transformations happen.

## Finish implementing your query

Adjust the query function and the query resolver function until they work as expected, with tests that prove it. This will likely involve adding additional arguments, ID transformations, permission checks, and MongoDB queries.

## More resources

[Build an API plugin guide](https://mailchimp.com/developer/open-commerce/guides/build-api-plugin/)
