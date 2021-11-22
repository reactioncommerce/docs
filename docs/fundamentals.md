## The basics

Mailchimp Open Commerce is an API-first, modular commerce stack. The API implements all features in GraphQL and is fully [extensible via plugins](/open-commerce/guides/build-api-plugin/).

Open Commerce is open-source-licensed, freely available software. You can explore the platform’s [repos on GitHub](https://github.com/reactioncommerce), or follow the [Quick Start guide](/open-commerce/guides/quick-start/) to install the development platform on your computer. If you’re interested in contributing to Open Commerce, be sure to read and follow the [code of conduct](https://github.com/reactioncommerce/reaction-docs/blob/trunk/public-docs/code-of-conduct.md/) and check out the [community resources](#community).

Like the code, the documentation is also [open source](https://github.com/reactioncommerce/docs), and previous versions of the Reaction Commerce docs are [archived on GitHub](https://github.com/reactioncommerce/reaction-docs).

### Using Open Commerce

Whether you’re a business owner running a shop or a developer setting up a custom implementation, it’s easy to get a shop up and running with the Open Commerce development platform and build from there.

Every shop is built around a catalog of products. You can [set up your products](/open-commerce/docs/creating-organizing-products/) in the admin dashboard and then [organize them with tags](/open-commerce/docs/tags-navigation/) to help shoppers navigate your catalog. Once shoppers have found what they want and make a purchase, you’ll [fulfill their orders](/open-commerce/docs/fulfilling-orders/) by accepting payments and delivering their items.

Developers can customize any part of the administrator or shopper experience by manipulating existing [plugins and environment variables](/open-commerce/docs/plugins-environment-variables/) or by [writing their own plugins](/open-commerce/guides/build-api-plugin/). Because Open Commerce is fully modular, you’ll often want to [share code between plugins](/open-commerce/docs/sharing-code-between-plugins/) so features are available across the entire platform. If you’ve created a new plugin or modified an existing one and you want to share it with other developers, you can contribute to the [Open Commerce community](#community). When committing to existing Open Commerce repos, you should follow the [testing requirements](/open-commerce/docs/testing-requirements/).

### Open Commerce vs Reaction Commerce

Open Commerce was formerly known as Reaction Commerce. You may see references to Reaction throughout the guides and docs, especially in code—all Open Commerce repos are under the [`reactioncommerce`](https://github.com/reactioncommerce) GitHub organization.

As we continue the renaming process, we’ll announce any future breaking changes in the [release notes](/release-notes/?filter=open-commerce).

### Local GraphQL playground

One of the development platform services is a local instance of the [GraphQL playground](/open-commerce/playground/). When using the playground, some queries—such as `ping`—don't require authentication, but many others do. Running them without authenticating will return an error or incorrect results. 

For example, if you try to retrieve the viewer’s email address without authenticating, you will receive a `null` result:

```graphql
# Query
{
  viewer {
    emailRecords {
      address
    }
  }
}

# Response
{
  "data": {
    "viewer": null
  }
}
```

To authenticate, you need an access token based on the SHA256 hash of your account password. To get that hash, open a terminal window and run `echo -n "YOUR_PASSWORD" | shasum -a 256`. Then pass the hash in the `authenticate` mutation to generate the access token:

 ```graphql
    mutation {
      authenticate(serviceName: "password", params: { user: { email: "YOUR_EMAIL_ADDRESS"}, password: "YOUR_HASHED_PASSWORD" }) {
        tokens {
          accessToken
        }
      }
    }
 ```

Click **HTTP HEADERS** at the bottom of the playground and add the access token as the value of `"Authorization"` in the header JSON object. Now you should be able to perform queries and mutations that require authentication.

## Community

Open Commerce has a global team of contributors to its [projects on GitHub](https://github.com/reactioncommerce). New contributors are always welcome, and they should read and abide by the [code of conduct](https://github.com/reactioncommerce/reaction-docs/blob/trunk/public-docs/code-of-conduct.md/).

If you have questions about implementing Open Commerce, customizing code for your own purposes, or contributing to a project, check out the [Discord server](https://discord.gg/Bwm63tBcQY). For bug reports and feature requests, you can create a new issue directly in the appropriate repo.

For more information on getting your code ready to commit, check out the [ESLint configuration](https://github.com/reactioncommerce/reaction-eslint-config) project and the [Testing Requirements](/open-commerce/docs/testing-requirements/) doc.
