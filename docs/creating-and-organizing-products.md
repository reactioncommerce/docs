<!-- # Creating and Organizing Products -->
## The basics

*Preliminary draft content as of 02/26/2021*

Every Mailchimp Open Commerce store is built around the products it is selling. This documentation outlines the structure of products, how to set up new products from scratch, and how to manage existing products within your store.

### Product structure

A *product* in Open Commerce represents a single type of item that you want to list in your store’s catalog. Depending on what you are selling—and how you want to organize and manage your inventory—every unit of a given product might be the same, or there might be *variants* of the product. 

> **Note**: Every product must have at least one variant, because some information is set at the product level and some is set at the variant level.

For example, you could have a particular design of t-shirt (the product) that has multiple sizes (its variants). In that case, you likely would not want to set up separate products for small, medium, and large shirts, since that would add repetition in your catalog pages. In another example, a bookseller could carry a particular book title in hardcover, softcover, and e-book editions. In this case, different businesses might make different choices: you might want all three as variants of a single product, you might group the physical books in one product and the e-book in another, or you might set up all three as separate products.

Open Commerce provides yet another level of organizational flexibility in *second-level variants* or *options*. Expanding our t-shirt example, a particular design (product) could be printed on two different colors of shirt (variants), each of which comes in three sizes (options). To order one of these shirts—or any product—a customer has to choose a *terminal variant*. In other words, a choice has to be made at each variant level to uniquely identify an item in your inventory. (Ordering just a red shirt without specifying a size, or just a large shirt without specifying a color wouldn’t make sense in a bricks-and-mortar store any more than it would in an Open Commerce store!)

### Product status

You can create products, variants, and options in the Open Commerce admin dashboard without making them live on your site. For products to be added to your catalog, first you need to publish the products and then make them visible to customers. If you no longer want to offer a product for sale, you can hide it from customers or archive it to remove it from your store permanently. You can change the price of individual product variants at any time, or you can affect the price of multiple products at once by using the [Discounts](#discounting-products) feature.
## Creating a product


To create a product, log in to the Open Commerce admin dashboard and follow these steps:

1. Click **Products** in the left sidebar.

2. Click **Create product**.

3. Enter the product information, such as title, permalink, subtitle, vendor, description, and origin country, in the **Details** card.

3. To add images, click **Drag image or click to upload** in the **Media Gallery** card. Once uploaded, you can use the Order fields to set the order that the images appear in.

4. Customize the product's social sharing messages under the **Social** card.

5. Add product tags by clicking on the **+** button in the **Tags** card. Select tags from the dropdown menu in the dialog, and then click **Add tags to products**. (If you need to create a new tag, see [tk Tags doc] for further information.)

6. Make sure to click **Save changes** in each card where you have entered new information.

7. Make your product visible by clicking the **⋯** button next to its name and then clicking **Make Visible**.

This creates the product, but it can't be published yet. To be publishable, it will need to have at least one variant.

## Configuring a variant

Variants allow you to create different versions of the same base product. Variants are perfect for products that come in multiple colors, sizes, shapes, etc. Every product comes filled with one default, required variant. 

To create more variants, click the **⋯** button next to the product's name, click **Create variant**, and fill out the following fields:

| Field | Description |
| ----- | ----------- |
| Attribute label | Required. The attribute label describes the category of variant, for example, "Color" or "Size". |
| Short title | Describes this particular variant within the category set by the attribute label. For example, a variant with attribute label "Color" might have short title "Red". This is usually shown in a drop-down list or button on the product detail page in your shop. |
| Title | The full title is usually shown on carts, checkout pages, order summary pages, and invoices. It should fully describe the configured variant. For example, if this is an option with short title "Large", its parent variant has short title "Red", and the product title is "Fancy T-Shirt", then a good title would be "Fancy T-Shirt – Red – Large". |
| Origin country | Optional. |
| Width, Length, Height, Weight | Optional. Units for these measurements can be changed under **Settings > Shop Localization**. |
| Price | Required. Enter a number without currency symbols. |
| Compare at price | Optional. The suggested retail price of your product. |
| Manage inventory | Optional. If you'd like to track this item in your inventory, check this box. <ul><li>**Quantity** – Optional. Enter your current inventory quantity.</li><li>**Warn at** – Optional. Allows for "Limited Supply" notifications to be displayed on product pages when quantity is lower than this number.</li><li>**Allow backorder** – Optional. Allows customers to backorder the product.</li></ul>|
| Taxable | Optional. Check this box to automatically add tax to this item when purchased. When checked, you can optionally add a product by product tax code (e.g., `1250L`) or leave the Tax Code field blank to use the code set in **Settings > Taxes > Default tax code for products**. |


Once you’ve added the variant’s information, make the variant visible by clicking the **⋯** button next to the variant’s name and then clicking "Make Visible."

> **Note**: You cannot publish a new product unless the product itself and all of its variants are made visible.

## Adding options to a variant

Options provide a second layer of customization within each variant. For instance, in addition to carrying shirts in multiple colors, you may also want to carry multiple size options for each color. To create an option:

1. Hover over the variant you'd like to create an option for. Click the **⋯** button to the right of its name and then click **Create variant**.

2. Add the option's information as you would when [creating a top-level variant](#configuring-a-variant). 

3. To make the option visible, hover over the variant name, click the **⋯** button, and then click **Make Visible**. 


## Publishing, hiding, and showing

Configuring products and variants do not make them immediately visible to customers. Products can be marked as visible or hidden, which determines whether customers can see them on your shop. When you click **Publish**, all your saved changes will become visible to customers. 

Products published to the Catalog cannot be unpublished, though they may be set as **Not Visible** to hide them from customers. To change change the visibility of a product, variant, or option, click the **⋯** button next to the product title in the left sidebar and select **Make Hidden** or **Make Visible**.


Visibility settings are available for the top-level product, each variant and each option. Hiding the product will hide the entire product—including all of its variants—from the storefront. Hiding a variant will hide that variant and all of its options, while hiding an individual option will affect only that option.

> **Note**: In some cases, logged-in administrators may be able to see hidden products on the storefront—even though they are hidden from customers.
## Duplicating products

Duplicating a product, variant, or option will create a new copy of that item, including its children. It can be a useful tool for speeding up creation of similar products, variants, or options.

To duplicate a product, variant, or option, click the **⋯** button next to the product title and select **Duplicate**. Click the **Publish** button at the top right of the window to make your changes appear on the shop.

## Archiving products

Archiving products is immediate upon confirmation, and is not reversible from the admin dashboard. It may be a better to make the product hidden with the **Make Hidden** option instead. Variants and options can also be archived.

To remove an entire product, variant, or option from your shop, click on the **⋯** button next to the product, variant, or option title, and select **Archive**.

## Discounting products

Open Commerce has support for discount codes, which can be entered during checkout and apply to the subtotal for that particular order. Discount codes can be enabled from the **Discounts** section of the sidebar. To create a new discount code, click **Add Discount** and enter the following:

| Field | Description |
| ----- | ----------- |
| Discount Code | a case-sensitive string, which customers must enter to receive the discount |
| Discount | a string or number, depending on the calculation method |
| Calculation Method | determines how the discount value applies <ul><li>Credit: A credit is applied to the order subtotal, up to the discount value.</li><li>Discount: The discount value is applied as a percentage of the order subtotal.</li><li>Sale: Item pricing is overridden with a fixed sale price. [tk applies to all items? how does this work?]</li><li>Shipping: The discount value should be a string that matches the name of a shipping method. The calculated shipping rate will be applied as a discount.</li></ul>|
| Account Limit | number of times a single customer can use the code |
| Total Limit | number of times all customers combined can use the code |

Discounts are validated when a customer enters a code during checkout, and they are applied as payment methods on the cart. The discount code usage is tracked once the order has been placed.

## Bulk actions and filtering

You can perform bulk actions on multiple products at once. Either select the products manually by clicking the checkbox next to them, or use the filtering feature. To filter by name, type in the field above the product list. 

You can also filter by product ID:

1. Create a CSV file with a comma-separated list of product IDs you'd like to filter. 
2. Click the **Actions** menu and choose **Filter by file**.
3. Click **Import** and choose the CSV file you'd like to use.
4. Click **Filter products**.

Once you've filtered the products, you can use the **Actions** menu to perform bulk actions on them, including adding or removing tags, publishing, hiding or showing, duplicating, or archiving them.
