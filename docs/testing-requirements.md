## The basics

When changing code in an Open Commerce repository, you should test your code before submitting a pull request for review. Open Commerce testing breaks down into three broad categories:

- Unit tests
- Integration tests
- Acceptance tests

All unit and integration tests should use the [Jest framework](https://jestjs.io). When submitting a pull request, [CircleCI](https://circleci.com) automatically runs unit and integration tests and will not allow merging until they all pass. Acceptance tests verify app functionality from a user’s perspective and can be done with a script or manually.

In this documentation, we’ll cover how to create and run unit and integration tests so your code can be accepted into the Open Commerce codebase.

### Unit vs integration tests

Because many Open Commerce services integrate with others, there’s no sharp divide between unit and integration tests. Think of the terms "unit" and "integration" as two ends of a spectrum, where the "unit" end mocks everything and tests in complete isolation, while the "integration" end mocks nothing and is essentially like running the Open Commerce app itself.

In practice, the primary distinction is: 

- Unit tests have no mock database and are written in files throughout the codebase, with names similar to the name of the file they test.
- Integration tests are all written in the `/tests` folder and can make use of a fake, in-memory version of MongoDB.

For example, say you’ve written GraphQL resolvers for `shop` and `tags`, and you want the two resolvers to work together to produce a `shop.tags` field. You’ll need to write unit tests for each of them to determine whether they’re returning the correct information—the shop or tags list, respectively. But each individual unit test will ignore or mock the output of the other, so you’ll need an integration test to ensure that the output of `shop.tags` is correct.


## Writing Jest unit tests

Every component and utility function in Open Commerce must have a corresponding file containing unit tests written using the Jest testing framework. If you aren’t familiar with Jest, you should check out [the Jest documentation](https://jestjs.io/docs/getting-started) before writing your tests.

Some older files may not have existing unit tests. In order to update them, you must create a unit test file and add all necessary tests to achieve full coverage of that file—including testing all existing code as well as your new changes. It’s often easiest to begin your development by writing all of the missing tests before changing any code.

### Filenames and code style

Jest unit test files must be named after the component or utility function that they are testing and end in `.test.js`. The test file can be anywhere in the Open Commerce codebase, but ideally it should be in the same folder and have the same base filename as the code being tested. 

When writing your test:

- Do not add extra `describe` blocks. Jest tests are automatically file-scoped, so a file that performs a single test does not need a `describe` block in it. You may add multiple `describe` blocks to group related tests within one file, but you should not have a file with only a single `describe` block in it.
- Always use `test()` instead of `it()` to define test functions.
- Do not import `describe`, `test`, `jest`, `jasmine`, or `expect`. They are automatic globals in all test files.
- Use arrow functions for all `describe` and `test` functions.
- Use Jest's [built-in `expect` function](https://jestjs.io/docs/en/expect.html) for assertions.

You might need to [test asynchronous code](https://jestjs.io/docs/en/asynchronous.html): functions that either return a Promise or take a callback argument. You should use Promises unless you need to use a callback, such as when the API of another package requires it. When using callbacks, make sure to add a `done` argument to your test function and call `done` when all testing is complete.

### Mocking data

To write unit tests for modules that depend on other modules, you need to supply mock data. 

There are several ways to mock data in Open Commerce:

- Use [Jest's mocking capabilities](https://jestjs.io/docs/en/mock-functions.html). 
- Use the [rewire-exports](https://www.npmjs.com/package/babel-plugin-rewire-exports) Babel plugin, which can temporarily replace anything that another file exports. This is useful for mocking functions that are imported by the function that you're testing.
- Use the built-in [`Factory` test utility](https://github.com/reactioncommerce/data-factory), which uses `@reactioncommerce/data-factory` to attach all core schemas to the `Factory` object. You need to add the schema to the `Factory` prior to use, or it will evaluate to an empty object.

> **Note**: `Factory` should primarily be used for backend-specific testing, such as integration testing at the API server level or unit testing at the plugin level.

To set up the initial schema for `Factory`, use `SimpleSchema` and `createFactoryForSchema`, like this:

``` js
import SimpleSchema from 'simpl-schema';
import { createFactoryForSchema } from "@reactioncommerce/data-factory";

const Example = new SimpleSchema({
  strProp: String,
  boolProp: Boolean,
});
createFactoryForSchema("Example", Example);
```

To mock a single object, use the `makeOne()` method with the name of the collection, such as:

``` js
import { Factory } from "/imports/test-utils/helpers/factory";
const mockTag = Factory.Tag.makeOne();
```

The output of `Factory` will be something like:

```js
{
  _id: "e02993ea96d7",
  name: "mockName",
  slug: "mockSlug",
  type: "mockType",
  metafields: ["item"],
  position: 3ff4e0634ecc,
  relatedTagIds: ["mockRelatedTagIds.$"],
  isDeleted: false,
  isTopLevel: true,
  isVisible: true,
  groups: ["mockGroups.$"],
  shopId: "a05276973251",
  createdAt: "1970-01-02T02:28:37.000Z",
  updatedAt: "2020-03-01T19:16:58.117Z",
  heroMediaUrl: "mockHeroMediaUrl"
}
```

To mock multiple instances of a single type of object, use the `makeMany` method with an integer argument:

``` js
import { Factory } from "/imports/test-utils/helpers/factory";
const mockTags = Factory.Tag.makeMany(2);
```

`makeMany` will output an array of objects:

```js
[
 {
   _id: "e02993ea96d7",
   name: "mockName",
   slug: "mockSlug",
   type: "mockType",
   metafields: ["item"],
   position: "3ff4e0634ecc",
   relatedTagIds: ["mockRelatedTagIds.$"],
   isDeleted: false,
   isTopLevel: true,
   isVisible: true,
   groups: ["mockGroups.$"],
   shopId: "a05276973251",
   createdAt: "1970-01-02T02:28:37.000Z",
   updatedAt: "2018-06-04T19:16:58.117Z",
   heroMediaUrl: "mockHeroMediaUrl"
 },
 {
   _id: "bdc84075a8eb",
   name: "mockName",
   slug: "mockSlug",
   type: "mockType",
   metafields: ["item"],
   position: "5034c879b7c2",
   relatedTagIds: ["mockRelatedTagIds.$"],
   isDeleted: false,
   isTopLevel: true,
   isVisible: true,
   groups: ["mockGroups.$"],
   shopId: "28d65013adc8",
   createdAt: "1970-01-02T02:28:37.000Z",
   updatedAt: "2018-06-04T19:16:58.117Z",
   heroMediaUrl: "mockHeroMediaUrl"
 }
]
```

You might need to specify a value for a property within your mock data, like making all mocked objects have the same `shopId`. Provide a properties object as an argument to either `makeOne` or `makeMany`:

``` js
import { Factory } from "/imports/test-utils/helpers/factory";
const mockTag = Factory.Tag.makeOne({ shopId: "1234" });
```

`makeMany` also supports custom values that are not constant, e.g., a series of mock objects that have sequential `_id` values. To use an arrow function to output two mocked tags with `_id` values of `"100"` and `"101"`:

``` js
import { Factory } from "/imports/test-utils/helpers/factory";
const mockTags = Factory.Tag.makeMany(2, { shopId: "1234", _id: (index) => (index + 100).toString() });
```

Finally, indexes can be passed from a `makeMany` method into another method. Here, passing `30` into `makeMany` will also pass `30` into `makeMockProductWithSpecificId`:

```js
function makeMockProductWithSpecificId(index) {
  const productId = (index + 100).toString();

  return Factory.CatalogProduct.makeOne({
    _id: productId,
    isDeleted: false,
    isVisible: true,
    tagIds: [mockTagWithFeatured._id],
    shopId: internalShopId
  });
}

const mockCatalogItemsWithFeaturedProducts = Factory.Catalog.makeMany(30, {
  product: makeMockProductWithSpecificId,
  shopId: internalShopId
});
```


### React component tests

Many Open Commerce user interface components are written in React. To write Jest unit tests for these components, you need to use a tool that renders them for testing purposes. We recommend the [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/), which allows writing tests that closely resemble how interface components are used, rather than their inner workings.

## Writing Jest integration tests

Jest integration test files always end in `.test.js` and should be located in the `/tests` folder. Integration tests can write some initial test data to an in-memory MongoDB store, allowing them to test database queries without mocking them. The MongoDB collections are simulated in-memory collections, implemented using [`mongodb-memory-server`](https://github.com/nodkz/mongodb-memory-server).

### GraphQL integration tests

Integration tests can send actual GraphQL requests to a temporary test server that behaves like the real server. This makes them useful for testing things such as:

- Queries involving multiple resolvers
- Response pagination
- Whether mutations properly affect the MongoDB collections
- The effect of complex permission rules on query results

> **Note**: The `query`, `mutation`, and `subscription` properties of the test server are wrappers around the methods of the same name provided by the [graphql.js](https://github.com/f/graphql.js) package.

When creating GraphQL tests, the folder and file structure in `/tests` should match the plugin folder structure as much as possible.

Prior to running the tests in each file, initialize a server, in-memory database, and any collection data you need; then stop the server, like so:

```js
import { importPluginsJSONFile, ReactionTestAPICore } from "@reactioncommerce/api-core";

let testApp
beforeAll(async () => {
  // init the reaction test API server
  testApp = new ReactionTestAPICore();
  // create a list of plugins
  const plugins = await importPluginsJSONFile("../../../../../plugins.json", (pluginList) => {
    // Remove the `files` plugin when testing. Avoids lots of errors.
    delete pluginList.files;
    return pluginList;
  });
  
  // register plugins and start test server
  await testApp.reactionNodeApp.registerPlugins(plugins);
  await testApp.start();
})

// stoping the test server will drop all test data
afterAll(() => testApp.stop());

test("primaryShop query returns a shop", async () => {
  // (1) use testApp.collections to write to MongoDB collections to set up initial data state
  await testApp.collections.Shops.insertOne({ shopType: "primary", name: "Test Shop", createdAt: new Date(), _id: "123456" });
  // (2) execute a GraphQL query or mutation using testApp.query()() or testApp.mutation()()
  const results = await testApp.query(`query primaryShop { name _id }`);
  // (3) verify the response is as expected and/or verify that the collection data has been changed
  expect(results.name).toBe("Test Shop");
  // (4) optionally reset the database if there is a chance it will conflict with the next test in this file
  await testApp.collections.Shops.remove({ _id: "123456" });
});
```

The following code snippets are examples of common tasks to include in GraphQL integration tests. `mockAccount` may either be an account document that you’ve already inserted or one that the test server will insert for you. Either way, it must have an `_id` property, which will be used to set the `user` and `account` properties of the GraphQL context for all test queries.

**Insert a primary shop**

```js
const shopId = await testApp.insertPrimaryShop();
```

**Create a mock user**

```js
mockAccount = Factory.Accounts.makeOne({
  // ...any specific properties you need on the account
});
await testApp.createUserAndAccount(mockAccount);
```

**Create a mock admin user**

```js
mockAdminAccount = Factory.Accounts.makeOne({
  // ...any specific properties you need on the account
});
await testApp.createUserAndAccount(mockAdminAccount, ["owner"]);
```

**Set and clear the mock user**

```js
beforeAll(async () => {
  await testApp.setLoggedInUser(mockAccount);
});

afterAll(async () => {
  await testApp.clearLoggedInUser();
});
```

## Running Jest tests

When running tests, first make sure they are located in the correct directory (for unit tests, the same directory as the code being tested; for integration tests, the `/tests` folder). 

To run tests, use `npm` and specify the type of test being run—either `npm run test:unit` or `npm run test:integration`. Most repos will also accept `npm run test` for running unit tests. To  have tests rerun as you make changes to test files, add `:watch`—either `npm run test:unit:watch` or `npm run test:integration:watch`.

> **Note**: To use watch mode on macOS, you must install `watchman`. This can be done via the [Homebrew package manager](https://brew.sh) by running `brew install watchman`.

Jest has a built-in caching feature, which can sometimes give you bad cached results, even if the test has since been fixed. To force a test to ignore the cache, add the `--no-cache` flag.

You can also use Docker Compose to run tests within a local development container. This gives a more accurate picture of how production code running in a container will behave. Run `docker-compose run --rm reaction npm run test:unit` or `docker-compose run --rm reaction npm run test:integration` These tests can also be run in watch mode by suffixing `:watch`.

