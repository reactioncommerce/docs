## At a Glance
In Open Commerce parlance, fulfillment generally means how the product gets into the hands of the consumer. This could be via shipping, download, or other.
This guide will walk you through creating your own fulfillment plugin that will then be available in checkout.

In general, to add a fulfillment method you must do the following:
- Create a new plugin
- Create a `getFulfillmentMethodsWithQuotes` function in your plugin
- Register that function via your new plugin


## What you need
- [Understanding Plugins]((/developer/open-commerce/guides/build-api-plugin/)

Reaction ships with one such plugin, `shipping-rates`, which you can look at for inspiration. You will want to remove that plugin if adding your own, unless you're combining multiple fulfillment method sources

## Create the Function

Your plugin must define a `getFulfillmentMethodsWithQuotes` function. This function must somehow query for a list of fulfillment methods that should be available based on the information passed in, and get quotes for those methods. It can be an `async` function.

The signature of this function is `(context, commonOrder, previousQueryResults = [])`.

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


First register the function:
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

Once registered, your method should be available in the admin, and once enabled there should be available to consumers.
