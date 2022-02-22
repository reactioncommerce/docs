## At a glance

You're working on an implementation for Leader of the Pack, an outdoor equipment retailer. In the GraphQL query guide we described data we added that contains whether we can ship a category of products to a particular zip. But now we need a way to update that information. For this we will create a GraphQL mutation

## What you need 

You need to understand what key you will use to query the data, and what the payload of the data is.

## Identify which plugin owns the mutation

When adding a new mutation, the first step is to decide which plugin should own it. This is usually obvious, but not always. You should think about whether any other plugins or services will need to call your mutation.

## Define the mutation in the schema

- If it doesn't already exist, create `schemas` folder in the plugin, and add an `index.js` file there.
- If it doesn't already exist, create `schema.graphql` in `schemas` in the plugin.
- Import the GraphQL file into `index.js` and default export it in an array:

```js
import importAsString from "@reactioncommerce/api-utils/importAsString.js";

const schema = importAsString("./schema.graphql");

export default [schema];
```

> NOTE: For large plugins, you can split to multiple `.graphql` files and export a multi-item array.

- In the `.graphql` file, add your mutation within `extend type Mutation { }`. Add an `extend type Mutation` section near the top if the file doesn't have it yet.
- Follow [Relay recommendations](https://relay.dev/docs/guided-tour/updating-data/graphql-mutations/) for mutation input arguments, which is to have only one argument named `input` that takes an input type that is the capitalized mutation name plus the suffix "Input", and to return a type that is the capitalized mutation name plus the suffix "Payload".

   Example: `addAccountEmailRecord(input: AddAccountEmailRecordInput!): AddAccountEmailRecordPayload`

- Add the Input and Payload types to the schema. Both must have `clientMutationId: String` field and may have any other fields as necessary. The mutation response payload should include whatever object was mutated.
- Document your mutation, the new types, and all fields in those types using string literals.
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

## Create the plugin mutation file

- If it doesn't already exist, create `mutations` folder in the plugin, and add an `index.js` file there.
- In `mutations`, create a file for the mutation, e.g. `updateRestrictions.js` for the `updateRestrictions` mutation. The file should look something like this:

```js
import Logger from "@reactioncommerce/logger";

/**
 * @method updateRestrictions
 * @summary TODO
 * @param {Object} context - an object containing the per-request state
 * @return {Promise<Object>} TODO
 */
export default async function updateRestrictions(context) {
  Logger.info("updateRestrictions mutation is not yet implemented");
  return null;
}
```

## Add the plugin mutation to the mutations context

In `mutations/index.js` in the plugin, import your mutation and add it to the default export object. Example:

```js
import updateRestrictions from "./updateRestrictions.js"

export default {
  updateRestrictions
};
```

If this is the first mutation for the plugin, you'll also need to pass the full `mutations` object to `registerPlugin` in the plugin's `index.js` file:

```js
import mutations from "./mutations";

export default async function register(app) {
  await app.registerPlugin({
    mutations,
    // other props
  });
}
```

Your plugin mutation function is now available in the GraphQL context as `context.mutations.updateRestrictions`.

> NOTE: The mutations objects from all plugins are merged, so be sure that another plugin does not have a mutation with the same name. The last one registered with that name will win, and plugins are generally registered in alphabetical order by plugin name. Tip: You can use this to your advantage if you want to override the mutation function of a core plugin without modifying core code.

## Add a test file for your mutation

If your mutation is in a file named `updateRestrictions.js`, your Jest tests should be in a file named `updateRestrictions.test.js` in the same folder. Initially you can copy and paste the following test:

```js
import mockContext from "/imports/test-utils/helpers/mockContext";
import updateRestrictions from "./updateRestrictions.js";

test("expect to return a Promise that resolves to null", async () => {
  const result = await updateRestrictions(mockContext);
  expect(result).toEqual(null);
});
```

## Create the GraphQL mutation resolver file

- If it doesn't already exist, create `resolvers` folder in the plugin, and add an `index.js` file there.
- If it doesn't already exist, create `resolvers/Mutation` folder in the plugin, and add an `index.js` file there. "Mutation" must be capitalized.
- In `resolvers/Mutation`, create a file for the mutation resolver, e.g. `updateRestrictions.js` for the `updateRestrictions` mutation. The file should look something like this initially:

```js
/**
 * @name "Mutation.updateRestrictions"
 * @method
 * @memberof MyPlugin/GraphQL
 * @summary resolver for the updateRestrictions GraphQL mutation
 * @param {Object} parentResult - unused
 * @param {Object} args.input - an object of all mutation arguments that were sent by the client
 * @param {String} [args.input.clientMutationId] - An optional string identifying the mutation call
 * @param {Object} context - an object containing the per-request state
 * @return {Promise<Object>} UpdateRestrictionsgPayload
 */
export default async function updateRestrictions(parentResult, { input }, context) {
  const { clientMutationId = null } = input;
  // TODO: decode incoming IDs here
  const renameMe = await context.mutations.updateRestrictions(context);
  return {
    renameMe,
    clientMutationId
  };
}
```

Make adjustments to the resolver function so that it reads and passes along the parameters correctly. The general pattern is:
- Destructure `input`. Include `clientMutationId`.
- Decode any opaque IDs that are in the input object
- Call `context.mutations.<mutationName>` (your new plugin mutation) with the necessary arguments, and `await` a response.
- Return an object that contains the `clientMutationId` and the object returned by the plugin mutation. (This must match the "Payload" type from the schema.)

## Register the resolver

In `resolvers/Mutation/index.js` in the plugin, import your mutation resolver and add it to the default export object. Example:

```js
import updateRestrictions from "./updateRestrictions.js"

export default {
  updateRestrictions
};
```

If this is the first mutation for the plugin, you'll also need to import the `Mutation` object into the `resolvers` object. In `resolvers/index.js` in the plugin, import `Mutation` and add it to the default export object.

```js
import Mutation from "./Mutation"

export default {
  Mutation
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

Calling your mutation with GraphQL should now work.

## Add a test file for your mutation resolver

If your mutation resolver is in a file named `updateRestrictions.js`, your Jest tests should be in a file named `updateRestrictions.test.js` in the same folder. Initially you can copy and paste the following test:

```js
import updateRestrictions from "./updateRestrictions.js";

test("correctly passes through to mutations.updateRestrictions", async () => {
  const fakeResult = { /* TODO */ };

  const mockMutation = jest.fn().mockName("mutations.updateRestrictions");
  mockMutation.mockReturnValueOnce(Promise.resolve(fakeResult));
  const context = {
    mutations: {
      updateRestrictions: mockMutation
    }
  };

  const result = await updateRestrictions(null, {
    input: {
      /* TODO */
      clientMutationId: "clientMutationId"
    }
  }, context);

  expect(result).toEqual({
    renameMe: fakeResult,
    clientMutationId: "clientMutationId"
  });
});
```

This of course should be updated with tests that are appropriate for whatever your mutation resolver does. For example, verify that all ID and schema transformations happen.

##  Finish implementing your mutation

Adjust the mutation function and the mutation resolver function until they work as expected, with tests that prove it. This will likely involve adding additional input fields, ID transformations, permission checks, MongoDB calls, and event emitting.

## Update the JSDoc comments

Write/update jsdoc comments for the plugin mutation function, the mutation resolver, and any util functions. The resolver function must have `@memberof <PluginName>/GraphQL` in the jsdoc, and the `@name` must be the full GraphQL schema path in quotation marks, e.g., "Mutation.updateRestrictions". (The quotation marks are necessary for the output API documentation to be correct due to the periods.)

## More resources

[Build an API plugin guide](https://mailchimp.com/developer/open-commerce/guides/build-api-plugin/)
