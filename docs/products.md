## Overview 
Products is one of the most used entities in Mailchimp Open Commerce. It is basically what you offer in the store to sell, it can be tangible or intangible, it can also represent a physical product or service.

Priducts can have 2 levels of variants, for example, a t-shirt can be sold by size and color.

### Types

The type `Product` is  the source of truth of Shop Administrators, it is what gets created/updated/archived in the database. When a `Product` gets published it becomes a `CatalogProduct`, which is what should be displayed to shoppers who browse that catalog.

Another keys related to product are the following:
- `metafields`: Metadata you want to save along with the product. Type `MetaField`
-  `media`: Image information. Type `ImageInfo`.
-  `shop`: the related Shop for this product: Type `Shop`
-  `socialMetadata`: metadata for social networks. Type `SocialMetadata`.
-  `supportedFulfillmentTypes`: the types of fulfillment the product supports. Type `FulfillmentType`.
-  `Tags`: Tags relalted to the product. Type `TagsConnection`.
-  `variants`: All variants of the product. Type `ProductVariant`
-  `pricing`: Pricing for the product. Type `ProductPricingInfo`

### Creating products

in order to create products we need to call the mutation `createProduct` , you can create a base product with no variants. the parameter `shouldCreateFirstVariant`  in the `CreateProductInput` field can help create a first placeholder variant with the product creation.

### Creating Variants

Take a look at the `createProductVariant` mutation. by defining a `CreateProductVariantInput`, which is comprised of a productID and a shopID.

### Create, Clone & Archive

Most of the create mutation also have a clone and an archive  mutation, to allow for full CRUD-based operations. for example, `cloneProducts` and `CloneProductVariants`


#### mutation CreateProduct fields

```
product:ProductInput

shopId:ID!

shouldCreateFirstVariant:Boolean = true
```

#### type ProductInput fields 
```graphql 

_id:String

description:String

facebookMsg:String

googleplusMsg:String

isDeleted:Boolean

isVisible:Boolean

metaDescription:String

metafields:[MetafieldInput]

originCountry:String

pageTitle:String

pinterestMsg:String

productType:String

shouldAppearInSitemap:Boolean

slug:String

supportedFulfillmentTypes:[FulfillmentType]

tagIds:[ID]

title:String

twitterMsg:String

vendor:String

```

### publishing a product

the mutation `publishProductsToCatalog` will do the trick provided an array of productIDs, if correctly formed it will return an Array of `CatalogItemProducts`, similar to `ItemProduct` but now as part of the published catalog.

#### mutation publichProductsToCatalog

```graphql

publishProductsToCatalog(
productIds: [ID]!
): [CatalogItemProduct]
Publish products to the Catalog collection by product ID

Represents a catalog item that displays a product

type CatalogItemProduct
implements CatalogItem
implements Node {
_id: ID!
createdAt: DateTime!
product: CatalogProduct
shop: Shop!
updatedAt: DateTime!
}
productIds: [ID]!
```