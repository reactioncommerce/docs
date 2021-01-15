<!-- # Testing Requirements -->

## The basics

When you are changing code in one of the Mailchimp Open Commerce repositories, you should test your code before submitting a pull request for review. Testing Open Commerce breaks down into three broad categories:

- Unit tests
- Integration tests
- Acceptance testing (AT)

All unit and integration tests for Open Commerce should use the [Jest framework](https://jestjs.io). When submitting a pull request, [CircleCI](https://circleci.com) automatically runs all unit and integration tests and will not allow merging until they all pass.

> **Note**: If you make changes in a file that does not have unit tests, you must create a unit test file and add all necessary tests to achieve full coverage of that file. This includes testing all existing code as well as your new changes. Often it is easiest to begin your development by writing all of the missing tests before changing any code.

Because Open Commerce is composed of microservices, most of which integrate with others, there is not a sharp divide between unit and integration tests. We suggest thinking of the terms "unit" and "integration" more as ends of a spectrum, where the "unit" end mocks everything and tests in complete isolation while the "integration" end mocks nothing and is essentially like running the Open Commerce app itself.

In practice, in the Open Commerce codebase, the primary distinction is this:

- Integration tests are all written in the `/tests` folder and can make use of a fake, in-memory version of MongoDB.
- Unit tests have no mock database and are written in files throughout the codebase, with names similar to the name of the file they test.

For example, if you write a GraphQL `shop` resolver and a `tags` resolver, each of those must have a unit test that determines whether the shop or tags list, respectively, is correctly returned. But assuming that a shop has a `tags` field, your `shop` unit test will be mocking or otherwise ignoring the output of the `tags` resolver. To ensure that the two resolvers work together to produce the correct `shop.tags` field, you need an integration test.

## Writing Jest unit tests

_intro sentence tk_

### Filenames and code style

Jest unit test files must be named after the component or utility function that they are testing and end in `.test.js`. The test file should be in the same folder as the file it is testing.

In general, follow the same code style and ESLint rules as for the main codebase. In addition, adhere to the following guidelines:

- Do not add extra `describe` blocks. Jest tests are automatically file-scoped, so a file that performs a single test does not need a `describe` block in it. You _may_ add multiple `describe` blocks to group related tests within one file, but you _should not_ have a file with only a single `describe` block in it.
- Always use `test()` instead of `it()` to define test functions.
- Do not import `describe`, `test`, `jest`, `jasmine`, or `expect`. They are automatic globals in all test files.
- Use arrow functions for all `describe` and `test` functions.
- Use Jest's [built-in `expect` function](https://jestjs.io/docs/en/expect.html) for assertions.

Often you need to [test asynchronous code](https://jestjs.io/docs/en/asynchronous.html) — functions that either return a Promise or take a callback argument. Open Commerce code should prefer Promises over callbacks, but when you need to use a callback due to the API of other packages, you can. Where callbacks are involved, make sure to add a `done` argument to your test function and call `done` when all testing is complete.

### Mocking data

To write unit tests for modules that depend on other modules, you need to supply mock data. There are several ways to mock data for Open Commerce.

- Use [Jest's mocking capabilities](https://jestjs.io/docs/en/mock-functions.html).
- Use the [rewire-exports](https://www.npmjs.com/package/babel-plugin-rewire-exports) Babel plugin, which can temporarily replace anything that another file exports. This is useful for mocking functions that are imported by the function that you're testing.
- Use the built-in `Factory` test utility, which uses [@reactioncommerce/data-factory](https://github.com/reactioncommerce/data-factory) to attach all core schemas to the `Factory` object.

The following examples use the `Factory` utility.

To mock one object, use the `makeOne()` method with the name of the collection, such as:

```js
import { Factory } from '/imports/test-utils/helpers/factory';
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

To mock multiple instances of one type of object, use the `makeMany` method with an integer argument:

```js
import { Factory } from '/imports/test-utils/helpers/factory';
const mockTags = Factory.Tag.makeMany(2);
```

`makeMany` will output an array of objects:

```js
[
  {
    _id: 'e02993ea96d7',
    name: 'mockName',
    slug: 'mockSlug',
    type: 'mockType',
    metafields: ['item'],
    position: '3ff4e0634ecc',
    relatedTagIds: ['mockRelatedTagIds.$'],
    isDeleted: false,
    isTopLevel: true,
    isVisible: true,
    groups: ['mockGroups.$'],
    shopId: 'a05276973251',
    createdAt: '1970-01-02T02:28:37.000Z',
    updatedAt: '2018-06-04T19:16:58.117Z',
    heroMediaUrl: 'mockHeroMediaUrl',
  },
  {
    _id: 'bdc84075a8eb',
    name: 'mockName',
    slug: 'mockSlug',
    type: 'mockType',
    metafields: ['item'],
    position: '5034c879b7c2',
    relatedTagIds: ['mockRelatedTagIds.$'],
    isDeleted: false,
    isTopLevel: true,
    isVisible: true,
    groups: ['mockGroups.$'],
    shopId: '28d65013adc8',
    createdAt: '1970-01-02T02:28:37.000Z',
    updatedAt: '2018-06-04T19:16:58.117Z',
    heroMediaUrl: 'mockHeroMediaUrl',
  },
];
```

Sometimes mock data needs to have a specific value for a property. For example, you might want all mocked objects in a test to have the same `shopId`. To do this you can provide a properties object as an argument to ether the `makeOne` or `makeMany` factory methods to overwrite the mocked value.

```js
import { Factory } from '/imports/test-utils/helpers/factory';
const mockTag = Factory.Tag.makeOne({ shopId: '1234' });
```

<!-- omitted output of this for now -->

Additionally, `makeMany` supports custom values that are not constant. For example, you can create a series of mock objects that have sequential `_id` values by using an arrow function. The below code will output two mocked tags with `_id` values of `"100"` and `"101"`.

```js
import { Factory } from '/imports/test-utils/helpers/factory';
const mockTags = Factory.Tag.makeMany(2, {
  shopId: '1234',
  _id: (index) => (index + 100).toString(),
});
```

Finally, indexes can be passed from one `makeMany` method into another `makeMany` method. In this example, passing `30` into `makeMany` will then pass `30` into `makeMockProductWithSpecificId`.

```js
function makeMockProductWithSpecificId(index) {
  const productId = (index + 100).toString();

  return Factory.CatalogProduct.makeOne({
    _id: productId,
    isDeleted: false,
    isVisible: true,
    tagIds: [mockTagWithFeatured._id],
    shopId: internalShopId,
  });
}

const mockCatalogItemsWithFeaturedProducts = Factory.Catalog.makeMany(30, {
  product: makeMockProductWithSpecificId,
  shopId: internalShopId,
});
```

### React component tests

To render components in Jest unit tests, use either [`react-test-renderer`](https://reactjs.org/docs/test-renderer.html) or [`enzyme`](http://airbnb.io/enzyme/index.html#enzyme). `react-test-renderer` is primarily for snapshots, while `enzyme` is for an interactive component that allows simulated clicks, receiving new props, etc. The Jest documentation provides [a good explanation of both tools](https://facebook.github.io/jest/docs/en/tutorial-react.html).

When testing React components, test both presentation and expected behavior. Assertions made include:

- What a component should output (render), given a set of inputs, states, or props.
- How a component behaves, given a user action. It might make a state update or call a prop function passed to it by a parent.

Enzyme has the capability to shallow render components. When a component is shallow rendered, its children are not rendered and it is not rendered to the actual DOM, only a virtual representation of the DOM. The virtual DOM representation will contain references to unrendered child components.

Shallow rendering allows testing components in isolation, without worrying about their children. And shallow rendering is fast because there isn't much interaction with the actual DOM.

It's common for components to have functionality based around user interaction that fires a callback function or manipulates the component's state. Below are examples of testing these interactions using shallow rendering to simulate the interaction event and test for the component's reaction.

**Simple interaction**

```js
import React from 'react';
import { shallow } from 'enzyme';
import Button from './Button';

const mockHandleClick = jest.fn();

test('Button component fires callback function onClick', () => {
  const component = shallow(<Button onClick={mockHandleClick}>Button</Button>);
  component.find('button').simulate('click');
  expect(mockHandleClick).toHaveBeenCalled();
});
```

**Interaction with state change**

```js
import React from 'react';
import { shallow } from 'enzyme';
import Accordion from './Accordion';

test('Accordion component open & closes onClick', () => {
  const component = shallow(<Accordion>Content</Accordion>);
  component.find('div.header').simulate('click');
  expect(component.state().isExpanded).toBe(true);
  component.find('div.header').simulate('click');
  expect(component.state().isExpanded).toBe(false);
});
```

### Testing form components

All new Open Commerce form and input components are based on the [composable form spec](http://composableforms.com/) and use the [composable form test](https://github.com/DairyStateDesigns/composable-form-tests#readme) package for user interaction.

**Simple input**

```js
import React from 'react';
import { mount } from 'enzyme';
import { testInput } from 'composable-form-tests';
import Input from './Input';

testInput({
  component: Input,
  defaultValue: null,
  exampleValueOne: 'ONE',
  exampleValueTwo: 'TWO',
  mount,
  simulateChanging(wrapper, value) {
    // Refer to Enzyme documentation
    wrapper.find('input').simulate('change', { target: { value } });
  },
  simulateChanged(wrapper, value) {
    // Refer to Enzyme documentation
    wrapper.find('input').simulate('blur', { target: { value } });
  },
  simulateSubmit(wrapper) {
    // Refer to Enzyme documentation
    wrapper.find('input').simulate('keypress', { which: 13 });
  },
});
```

## Writing Jest integration tests

Jest integration test files always end in `.test.js` and should be located in the `/tests` folder. Integration tests can write some initial test data into an in-memory MongoDB store, allowing them to test database queries without mocking them.

> **Note**: The MongoDB collections are simulated in-memory collections, implemented using [`mongodb-memory-server`](https://github.com/nodkz/mongodb-memory-server).

### GraphQL integration tests

Integration tests can send actual GraphQL requests to a temporary test server that behaves the same as the real server. This makes them useful for testing things like:

- Queries involving multiple resolvers
- Response pagination
- Whether mutations properly affect the MongoDB collections
- The effect of complex permission rules on query results

> **Note**: The `query`, `mutation`, and `subscription` properties of the test server are wrappers around the methods of the same name provided by the [graphql.js](https://github.com/f/graphql.js) package.

When creating GraphQL tests, the folder and file structure in `/tests` should match the plugin folder structure as much as possible.

Prior to running the tests in each file, initialize a server, in-memory database, and any collection data you need. Then stop the server when done.

```js
import TestApp from '/imports/test-utils/helpers/TestApp';

let testApp;
beforeAll(async () => {
  testApp = new TestApp();
  await testApp.start();
});

afterAll(() => testApp.stop());

test('something', async () => {
  // (1) use testApp.collections to write to MongoDB collections to set up initial data state
  // (2) execute a GraphQL query or mutation using testApp.query()() or testApp.mutation()()
  // (3) verify the response is as expected and/or verify that the collection data has been changed
  // (4) optionally reset the database if there is a chance it will conflict with the next test in this file
});
```

Below are some examples of common tasks to include in GraphQL integration tests. In these examples, `mockAccount` may either be an account document that you've already inserted or one that will be inserted for you. Either way, it must have an `_id` property, which will be used to set the `user` and `account` properties of the GraphQL context for all test queries.

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
await testApp.createUserAndAccount(mockAdminAccount, ['owner']);
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

For unit tests, the test file can be anywhere in the Open Commerce codebase, but ideally they should be in the same folder and with the same base filename as the code being tested. For integration tests, the test file should always be located in the `/tests` folder.

To run tests, use `npm` and specify the type of test being run. Either:

```sh
npm run test:unit
```

or:

```sh
npm run test:integration
```

To run tests and have them rerun as you make changes to test files, add `:watch`, like:

```sh
npm run test:unit:watch
```

or:

```sh
npm run test:integration:watch
```

> **Note**: To use watch mode on macOS, you must install `watchman`. This can be done via the [Homebrew package manager](https://brew.sh) by running `brew install watchman`.

Jest has a built-in caching feature. This can sometimes cause you to see bad cached results, even if the test has since been fixed. To force a test to ignore the cache, add the `--no-cache` flag:

```sh
npm run test:unit -- --no-cache
```

You can also use Docker Compose to run a local development container and run tests within it. This gives a more accurate picture of how production code running in a container will behave. To run test with Docker Compose:

```sh
docker-compose run --rm reaction npm run test:unit
```

or:

```sh
docker-compose run --rm reaction npm run test:integration
```

These tests can also be run in watch mode by suffixing `:watch`.
