# Create an Inventory Data Provider Plugin

Before you create a custom inventory data provider plugin, make sure that you need one.
- If your inventory system outputs periodic files but doesn't allow you to read inventory programmatically in real time, you may want to use the "Simple Inventory" plugin and only update the values. Refer to option 2 [here](./core-plugins-simple-inventory#how-to-sync-inventory-quantities-from-an-external-system).
- If you use a well-known third-party inventory system, there may be a community plugin already available for it. If not, consider making yours available as open source so that the community can help you maintain it.

Once you're sure you need to create a custom inventory plugin, use the standard steps to create the plugin boilerplate. Beyond that, an inventory plugin needs to do the following:
- Register an `inventoryForProductConfigurations` type function that returns current inventory in stock and other related data for a list of product configurations.
- Optionally provide a way of editing the inventory in stock in the operator UI, by registering client components
- Optionally track order status changes to keep track of "reserved" inventory, which is inventory that is still technically in stock but should not be considered available to sell.

## Register an `inventoryForProductConfigurations` function

An `inventoryForProductConfigurations` function must look up inventory information for the received `productConfiguration` from your external inventory system. Each item in the return array must have the same `productConfiguration` object and an `inventoryInfo` object field with all of the fields shown in the example code below.

If your plugin is unsure what the inventory is for this product configuration, it must return `null` for the `inventoryInfo` field. It's possible to have multiple plugins providing inventory info, and some other plugin might know what the inventory is. Returning `null` tells the core "inventory" plugin to ask the next inventory data provider or use default values if all inventory data plugins return `null`.

```js
/**
* @summary Returns an object with inventory information for one or more
*   product configurations.
* @param {Object} context App context
* @param {Object} input Additional input arguments
* @param {Object[]} input.productConfigurations An array of ProductConfiguration objects
* @return {Promise} Array of responses
*/
export default async function inventoryForProductConfigurations(context, input) {
 const { collections } = context;
 const { SimpleInventory } = collections;
 const { productConfigurations } = input;

 return productConfigurations.map((productConfiguration) => {
   // Look up inventory information for `productConfiguration` from external system.

   return {
     inventoryInfo: {
       canBackorder,
       inventoryAvailableToSell,
       inventoryInStock,
       inventoryReserved,
       isLowQuantity
     },
     productConfiguration
   };
 });
}
```

Pass your function in the `functionsByType` list:

```js
import inventoryForProductConfigurations from "./inventoryForProductConfigurations";

/**
* @summary Import and call this function to add this plugin to your API.
* @param {ReactionAPI} app The Reaction API apistance
* @return {undefined}
*/
export default async function register(app) {
 await app.registerPlugin({
   label: "My Inventory Data Plugin",
   name: "my-inventory-data-plugin",
   functionsByType: {
     inventoryForProductConfigurations: [inventoryForProductConfigurations]
   }
 });
}
```

> You may be aware that the core inventory plugin provides two queries: `inventoryForProductConfigurations` (multiple) and `inventoryForProductConfiguration` (single). The single version of the function is a convenience wrapper that calls the multiple versions, an inventory data plugin needs to provide only this one function, which expects multiple product configurations.

## Optionally add UI components

How and whether you should do this step depends a lot on how your plugin is tracking inventory. If you want any information to be viewable or editable in the operator UI, you can extend the GraphQL API and register UI blocks to appear where you need them.

## Track reserved/available inventory

In addition to providing "in stock" inventory values, most inventory data providers also somehow track "reserved" inventory and/or automatically increase or decrease the "in stock" and "available" values as orders are placed, approved, and fulfilled. Refer to the `startup` function in the Simple Inventory plugin for one example of how you can do this. The rest of the Reaction system does not care how you do this, and for a low volume shop you may be able to skip it entirely.

> If you do track reserved inventory, it can be an inexact science. We recommend that you provide operators with a way of manually fixing or updating the reserved quantity, as the Simple Inventory plugin does with the `recalculateReservedSimpleInventory` GraphQL mutation and corresponding UI button.# Inventory

The default Reaction system does not have inventory tracking enabled for any products. All products that you publish to the catalog are available for purchase in any quantity and will never show up as low on stock, sold out, or backordered.

If you do track inventory, Reaction provides a [Simple Inventory plugin](./core-plugins-simple-inventory.md) that allows you to update inventory in stock and other inventory settings in the operator UI. It also keeps track of inventory that was ordered but which you have not yet removed from the in-stock number, to ensure that you do not sell what you don't have available.

If you use a third-party inventory system or want to design your own inventory system, you can [create a custom plugin](./how-to-create-inventory-plugin.md).

Regardless of which plugin tracks your inventory, there are some general ways in which inventory impacts your catalog products, catalog variants, and cart items.

## Catalog Product Variant Inventory Data

The following data is available for every variant in the catalog. If a variant has options, then these values are summed or aggregated from the child options.

### Inventory In Stock

The "in stock" number represents how many units you actually have in stock.

### Reserved Inventory

Most inventory plugins will want to track new orders and consider the ordered quantity of each item to be "reserved" until you pull the order and decrease the "in stock" quantity. If an inventory plugin doesn't track this, then "reserved" inventory will always be zero.

### Inventory Available To Sell

The "available to sell" number represents how many units the system will allow a shopper to order. It is calculated by subtracting "reserved" from "in stock". If an inventory plugin doesn't track "reserved", then the available inventory will always be the same as the in stock inventory.

### Can Backorder

Each variant may or may not have backordering enabled. With backordering enabled, the orders service will allow an order to be placed for a quantity higher than the "available to sell" quantity.

### Sold Out

A variant that has no inventory available to sell is marked sold out. A variant with options is marked sold out only if ALL options have no inventory available to sell.

### Low Quantity

A variant that has low inventory available to sell is marked low quantity. A variant with options is marked low quantity only if ANY option has low inventory available to sell. Exactly what "low" means is left up to the individual plugin you use. The Simple Inventory plugin has you enter a "low inventory warning threshold" number and considers a variant to be low quantity when the available inventory is less than or equal to this threshold.

### Backordered

As a convenience, variants that are sold out but also have "can backorder" enabled are marked "backordered".

## Catalog Product Inventory Data

At the product level of the catalog, the same inventory statuses are available based on aggregating the product variant data.

- **Sold Out**: A product is marked sold out only if ALL variants and options have no inventory available to sell.
- **Low Quantity**: A product is marked low quantity if ANY variant or option has low inventory available to sell.
- **Backordered**: A product is marked backordered if ALL variants and options are backordered.

The catalog GraphQL query also allows filtering and sorting by these inventory statuses.

## Cart Item Inventory Data

Cart items have the three variant statuses and the "available to sell" number for the related catalog variant.