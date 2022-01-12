# Products, Variants, and Options

## Products
When we use the term **“product”** in Open Commerce, we are referring to a single type of thing that is available in a shop catalog, for example, a pair of Best Brand shoes, model B. A product cannot be directly ordered, though. Each product must have at least one **“variant”** added to it.

## Creating a product

To create a product, login to your dashboard and follow these steps:

1. Click on the **Products** navigation item in the left hand navigation bar.

2. Click on **Create Product** to create a product.

3. You can change the product title permalink, subtitle, vendor, description and origin country and more from the **Details** card.

4. To add images, click the **Drag image or click to upload** button. Once uploaded you'll be able to set the order of the media with the **Order** fields.

5. You can customize your product's social sharing messaging under the **Social** card.

6. Add product tags by clicking on the plus button and selecting your tags from the dropdown in the modal.

7. Ensure that you've saved all of your changes.

8. Make your product visible by clicking the icon next to its name in the left sidebar, and then clicking "Make Visible."

But we're not finished yet. For our product to be publishable, it will need to have at least one variant.

## Variants

A variant defines a certain product configuration, a set of configurable attributes that makes up a discrete, orderable item. For example, the “Pair of Best Brand Shoes, Model B” product might have three variants, “Size 9”, “Size 10”, and “Size 11”.

## Configuring your first Variant

Every product comes filled with one default, required variant. To create more:
1. Click the icon located next to the product's name in the left sidebar, and click "Add variant."

2. Fill out the following fields in the edit **Variant** panel:

- **Attribute label** - Required. The attribute label describes the category of variant, for example, "Color" or "Size". In most cases this will be the same for all variants at the same level.
- **Short title** - Required. The short title is the value for the attribute label. For example, a variant with attribute label "Color" might have short title "Red". This is usually shown in a select list or button set on a product detail page in your storefront.
- **Title** - Required. The full title is usually shown on cart, checkout, and order summaries and on invoices. It should fully describe the configured variant. For example, if this is an option with short title "Large", its parent variant has short title "Red", and the product title is "Fancy T-Shirt", then a good title might be "Fancy T-Shirt - Red - Large".
- **Origin country** - Optional. A drop-down menu of countries.
- **Price** - Required. The price that you are actually selling your product.
- **Compare at price** - Optional. The suggested retail price of your product.
- **Width, Length, Height, Weight** - Optional. The size and weight of your product
- **Taxable** - Optional. Check this box to automatically add tax to this item when purchased. Add Tax Code and Tax Description for more options.
- **Inventory tracking** - Optional. Check this box if you'd like to track this item in your inventory.
- **Warn at** - Optional. Allows for `Limited Supply` notifications to be displayed on when quantity is lower than this number.
- **Allow Backorder** - Optional. Allows customers to backorder the product.

3. Make your variant visible by clicking "Make Visible" in the dropdown next to their name in the left sidebar.

4. Click the **Publish** button on the top right to publish your product and all of its variants to your catalog.

## Options

If necessary, a variant can have further variants, which we call **“options”** or **“second level variants”**. For example, the “Size 9” variant for “Pair of Best Brand Shoes, Model B”, may come in two different colors, black and red. So the “Size 9” variant would have two variants of its own, “Black” and “Red”.

It’s important to note that only a terminal variant may be added to a cart for checkout. So if a variant has options, a shopper will need to select one of the options, but if a variant has no options, that variant itself may be added to a cart to place an order.

## Adding more product Variant Options

1. Click on the button to the right of the variant you'd like to create an option for. Then, click "Add variant."

2. Add the option's **Attribute Lanel**, **Label**, **Short Label**, **Quantity** and **Price**. **Label** is displayed in cart, checkout and orders. **Short Label** is displayed on the Product Detail Page. 

3. Make the option visible from the left sidear, then click the **Publish** button on the top right to make your changes show up on the storefront.

## Adding product images

To add an image gallery to your product, drag and drop files from your computer the main Product page. You may also upload images using the **Drag image or click to upload** button under each Variant and Option.

Once you have images added to your gallery, use the **Order** field to set their position.

![](/assets/operator-ui-product-media.png "Product media")

## Discounting products

Open Commerce supports discount codes and rates. Read more about discounting products in the [Dashboard's Payment section](payments-discounts.md).

## Hiding and showing products from your storefront

Making products **Visible** or **Not Visible** will affect how they're seen on your storefront to customers. Visible products are visible to all customers, while products marked as **Not Visible** products will be hidden from customers. In some cases, and administrator signed into the storefront may still be able to see products marked as **Not Visible**.

These options are available for the top-level **Product**, each **Variant** and each **Option**. Hiding the product will remove the entire product from the storefront. Hiding a variant will hide that **Variant** and all **Options**, while hiding an individual **Option** will see that **Option** hidden.

To change change the visibility of a product, variant, or option:

1. Click on the button next to the product title in the left sidebar.

2. Select **Make Hidden** or **Make Visible** and confirm.

3. Click the **Publish** button on the top right to make your changes appear on the storefront.

## Duplicating products, variant and options

Duplicating a product, variant or option will create a new copy of that item with it's children. It can be a useful tool speed up creation of similar products, variants, or options.

To duplicate a product, variant, or option:

1. Click on the button next to the product title in the left sidebar.

2. Select **Duplicate** and confirm.

3. Click the **Publish** button on the top right to make your changes appear on the storefront.

## Archiving and restoring a product

Archiving **Products** is immediate upon confirmation, and is not reversible from `reaction-admin`. It may be a better to make the **Product** hidden with the **Make Hidden** option instead. Archiving may also apply to **Variants** and **Options**.

To remove an entire **Product**, **Variant**, or **Option** from your inventory:

1. Click on the button next to the product, variant or option title in the left sidebar.

2. Select **Archive** and confirm.

3. Click the **Publish** button on the top right to make your changes appear on the storefront.

To restore an archived **Product**, **Variant**, or **Option**:

1. Products cannot be restored through `reaction-admin`.