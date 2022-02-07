
# How To: Create a Fulfillment Methods Plugin

## Prerequisite Reading
- [Concepts: Fulfillment](../guides/developers-guide/concepts/fulfillment-groups.md)
- [Understanding Plugins](../guides/developers-guide/core/build-api-plugin.md)

## Overview
In general, to add a fulfillment method you must do the following:
- Create a new plugin
- Create a `getFulfillmentMethodsWithQuotes` function in your plugin
- Create a React component for operators to enter and edit fulfillment method details, unless they'll be managed in an external system
- Register the function and the React components using your plugin's `registerPlugin` call

Reaction ships with one such plugin, `shipping-rates`, which you can look at for inspiration. You will want to remove that plugin if adding your own, unless you're combining multiple fulfillment method sources

## Create the Function

Your plugin must define a `getFulfillmentMethodsWithQuotes` function. This function must somehow query for a list of fulfillment methods that should be available based on the information passed in, and get quotes for those methods. It can be an `async` function.

The signature of this function is `(context, commonOrder, previousQueryResults = [])`. See [Understanding CommonOrder](../guides/developers-guide/concepts/understanding-common-order.md).

You can extract `rates` and `retrialTargets` out of `previousQueryResults` like this:

```js
const [rates = [], retrialTargets = []] = previousQueryResults;
```

You can use `retrialTargets` to track failures and rerun. See the `shipping-rates` plugin for an example.

The function must return either `previousQueryResults` or an array like `[rates, retrialTargets]`, where `rates` and `retrialTargets` are both arrays.

`rates` can be either a list of fulfillment methods with quotes or an array with a single item that is an error object. An error object looks like this:

```js
{
  requestStatus: "error",
  shippingProvider: "abc",
  message: "Something went wrong"
}
```

Otherwise each item in `rates` must match the `ShipmentQuote` schema. They will look something like this:

```js
carrier: "USPS",
method: {
  _id: "abc123",
  name: "internal_name",
  label: "Display name for shoppers in checkout",
  handling: 0,
  rate: 5.99,
  enabled: true
},
rate: 5.99
```

## Registration

Everything needs to be registered to be seen by Reaction core.

### Register the function

Pass your function in the `functionsByType` list:

```js title=index.js
import getFulfillmentMethodsWithQuotes from "./getFulfillmentMethodsWithQuotes";

export default async function register(app) {
  await app.registerPlugin({
    label: "My Fulfillment Plugin",
    name: "my-fulfillment-plugin",
    functionsByType: {
      getFulfillmentMethodsWithQuotes: [getFulfillmentMethodsWithQuotes]
    },
    // other props
  });
}
```
