# Multi-Shops

The Open Commerce API is a **multi-shop system**, which means that more than one shop can exist within the same installation.

Technically speaking, the current state of multi-shop support is that most entities in the database have a mandatory `shopId` field in their schemas, and most API calls require you to specify a shop or shops. There is a `Shops` collection, which can list as many shops as you'd want, with different users having different rights on them thanks to the `Groups` collection. This architecture can be leveraged to build custom, multi-shop projects.

### Disclaimer: multi-vendor marketplaces

It is crucial to make the distinction between multi-shop projects and multi-vendor marketplace projects. Multi-vendor marketplaces are typically projects where vendors can sign up on their own, create their shop and have their products listed on a single, unified catalog (i.e. similar to eBay or Amazon).

At this stage, there is no plan to support multi-vendor marketplaces out of the box on Open Commerce. However, you should check out the community plugins that may exist to offer this feature.
