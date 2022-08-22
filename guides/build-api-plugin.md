## At a glance

Mailchimp Open Commerce has a plugin system that allows you to extend or replace any of the features offered by the core platform. You can add plugins written by the Open Commerce community, or you can create your own. The plugins that you register will determine the exact functionality of your API.

You can use plugins to:

  * Add new types of information to products

  * Accept payments or ship orders with your preferred provider

  * Validate customers’ addresses when they check out

For the purposes of this guide, we manage the Open Commerce store for Leader of the Pack, an outdoor supplier that sells backpacks, among other things. We currently list fairly standard product dimensions—length, width, height—but recently, our customers have been asking just how much stuff they can fit in our backpacks. To answer their questions, we’ve decided to add a `volume` attribute to those products in our store.

In this guide, we’ll create a new plugin from the template provided by Open Commerce and register it with the platform running in development mode. Then we’ll set up the volume field and add it to the schemas for our products. Finally, we’ll extend the GraphQL API so our new field is available across our store.

## What you’ll need

  * An [Open Commerce installation](/developer/open-commerce/guides/quick-start/)

  * [Docker](https://www.docker.com/)

  * [npm](https://www.npmjs.com/get-npm)

  * A [GitHub](https://github.com/) account

## Create your custom plugin

Navigate to where you have created your api project (called `myapiserver` in the getting started doc). From there 
you can execute `reaction create-plugin api <mypluginname>` where "mypluginname" is the name of the plugin you want 
to create. This will create new plugin from the plugin template and place it in the `custom-packages` directory. The local instance of the plugin has automatically been added to `plugins.json` so that you can work on it locally

Next, we’ll edit the `package.json` and `src/index.js` files to replace the template defaults with information about our volume attribute plugin; the information in these files will be used when linking, installing, and registering the plugin. We’ll name our plugin Product Volume, with an npm name of `@leaderpack/api-plugin-products-volume`.

> **Note**: To keep things organized, it’s a good idea to name plugins after their “parent” plugins. In our case, we’re customizing aspects of `api-plugin-products`, so we named our plugin `api-plugin-products-volume`.

## Add functions

So we know that our new plugin is running, but we haven’t programmed it to do anything yet. Now we can start to add functionality—and since Open Commerce is in development mode, our plugin will reload every time we modify it.

Open Commerce offers several ways to [extend its functionality and API](/developer/open-commerce/docs/sharing-code-between-plugins/). Our plugin will use two approaches: `functionsByType`, which overrides or extends existing functions, and `graphQL`, which adds queries and mutations to the GraphQL API. 

Since our goal is to add a `volume` attribute to product variants, we’ll need to extend the existing product and catalog SimpleSchema to include this new field. First, we’ll add a startup function to extend the two SimpleSchemas, allowing them to be validated and saved to MongoDB:

```node title="startup.js"
function myStartup(context) {
  context.simpleSchemas.ProductVariant.extend({
	volume: {
	  type: Number,
	  min: 0,
	  optional: true
	}
  });

  context.simpleSchemas.CatalogProductVariant.extend({
	volume: {
	  type: Number,
	  min: 0,
	  optional: true
	}
  })
}
```

We’ll also extend the function that publishes products to the catalog, adding extra steps to `publishProductToCatalog`. The new `myPublishProductToCatalog` function parses our products, gets the new `volume` attribute, and adds it to the corresponding catalog variant in preparation for publishing it to the catalog:

```node title="index.js"
function myPublishProductToCatalog(catalogProduct, { context, product, shop, variants }) {
 catalogProduct.variants && catalogProduct.variants.map((catalogVariant) => {
   const productVariant = variants.find((variant) => variant._id === catalogVariant.variantId);
   catalogVariant.volume = productVariant.volume || null;
 });
}
```

Then we’ll instruct the `catalogs` plugin to looks for the `myPublishProductToCatalog` function by type, making it the key to communicating data from the new field to the catalog:

```node title="index.js"
export default async function register(app) {
  await app.registerPlugin({
	label: "Product Dimensions",
	name: "products-dimensions",
	version: pkg.version,
	functionsByType: {
	  startup: [myStartup]
	  publishProductToCatalog: [myPublishProductToCatalog]
	},
  });
}
```

## Extend the GraphQL API

Our custom plugin is linked, registered, and has a startup function, so now it’s time to extend the GraphQL API. We’ll do that by creating a `schema.graphql` file in the plugin’s `src/` directory and adding type extensions:

```graphql title="schema.graphql"
extend type ProductVariant {
  volume: Float
}

extend input ProductVariantInput {
  volume: Float
}

extend type CatalogProductOrVariant {
  volume: Float
}
```

Back in our plugin’s `src/index.js` file, we can import the new schema using `importAsString` and add it to the `app.registerPlugin` options:

```node title="src/index.js"

import importAsString from “@reactioncommerce/api-utils/importAsString.js”;

const mySchema = importAsString(“./schema.graphql”);

export default async function register(app) {
  await app.registerPlugin({
	label: "Product Dimensions",
	name: "products-dimensions",
	version: pkg.version,
	functionsByType: {
	  startup: [myStartup]
	  publishProductToCatalog: [myPublishProductToCatalog]
	},
	graphQL: {
	  schemas: [mySchema]
	}
  });
}
```

That’s it! We can see the plugin in action by using a GraphQL mutation to update a product variant with a `volume` attribute:

```graphql title="Mutation"
mutation {
  updateProductVariant(input: {
	variantId: "{encoded-variant-id}",
	shopId: "{encoded-shop-id}",
	variant: {
	  volume: 200
	}
  }) {
	variant {
	  volume
	}
  }
}

```

```graphql title="Response"
{
  "data": {
	"updateProductVariant": {
	  "variant": {
		"volume": 200
	  }
	}
  }
}

```

And once that variant is published, the new `volume` field will be available on the catalog product, thanks to the `publishProductToCatalog` function:

```graphql title="Query"
query {
  catalogItemProduct(
	shopId: "{encoded-shop-id}",
	slugOrId: "{encoded-slug-or-id}") {
	product {
	  variants {
		volume
	  }
	}
  }
}
 
```


```graphql title="Response"
{
  "data": {
	"catalogItemProduct": {
	  "product": {
		"variants": [
		  {
			"volume": 200
		  }
		]
	  }
	}
  }
}

```

We’ve done everything we need to add volume data to our backpacks and query that information across our store. That’s useful to Leader of the Pack’s store administrators, but we also need to get that information to customers. This will require some design work on our product pages, which are dependent on the storefront plugin that we’re using. Generally, this will involve making sure that our product query includes the new `volume` field and that the information is passed to the `render()` function in the file that describes our product detail page. For more information, see the [Creating and Organizing Products doc](/developer/open-commerce/docs/creating-organizing-products/).

## More resources

  * [Sharing Code Between Plugins](/developer/open-commerce/docs/sharing-code-between-plugins/)

  * [GraphQL Playground](/developer/open-commerce/playground/)

  * [Open Commerce Fundamentals](/developer/open-commerce/docs/fundamentals/)

