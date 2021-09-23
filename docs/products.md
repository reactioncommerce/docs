## Overview 
Products is one of the most used entities in Mailchimp Open Commerce. It is basically what you offer in the store to sell, it can be tangible or intangible, it can also represent a physical product or service.

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

