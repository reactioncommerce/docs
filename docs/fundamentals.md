## The basics

Mailchimp Open Commerce is an API-first, modular commerce stack. The API implements all features in GraphQL and is fully [extensible via plugins](/open-commerce/guides/build-api-plugin/).

Open Commerce is open-source-licensed, freely available software. You can explore the platform’s [repos on GitHub](https://github.com/reactioncommerce), or follow the [Quick Start guide](/open-commerce/guides/quick-start/) to install the development platform on your computer. If you’re interested in contributing to Open Commerce, be sure to read and follow the [code of conduct](https://github.com/reactioncommerce/reaction-docs/blob/trunk/public-docs/code-of-conduct.md/) and check out the [community resources](#community).

### Using Open Commerce

Whether you’re a business owner running a shop or a developer setting up a custom implementation, it’s easy to get a shop up and running with the Open Commerce development platform and build from there.

Every shop is built around a catalog of products. You can [set up your products](/open-commerce/docs/creating-organizing-products/) in the admin dashboard  and then organize them with [tags](/open-commerce/docs/tags-navigation/) to help shoppers navigate your catalog. Once shoppers have found what they want and make a purchase, you’ll [fulfill their orders](/open-commerce/docs/fulfilling-orders/) by accepting payments and delivering their items.

Developers can customize any part of the administrator or shopper experience by [writing plugins](/open-commerce/guides/build-api-plugin/). Because Open Commerce is fully modular, you’ll often want to [share code between plugins](/open-commerce/docs/sharing-code-between-plugins/) so features are available across the entire platform. If you’ve created a new plugin or modified an existing one and you want to share it with other developers, you can contribute to the [Open Commerce community](#community).

### Open Commerce vs Reaction Commerce

Open Commerce was formerly known as Reaction Commerce. You may see references to Reaction throughout the guides and docs, especially in code—all Open Commerce repos are under the [`reactioncommerce` GitHub organization](https://github.com/reactioncommerce). 

As we continue the renaming process, we’ll announce any future breaking changes in the [release notes](/release-notes/?filter=open-commerce).

## Development platform

The Open Commerce development platform is the simplest way to set up the API and its supporting services on your local machine. The development platform is containerized and uses public Docker images by default, though you can also choose to run the platform in development mode, which uses local images that can be updated on the fly as you make edits to a running plugin. 

The development platform has several `make` scripts that automatically pull the latest version of Open Commerce and start all of the required services using Docker Compose. The development platform runs on `localhost`; see the [Quick Start guide](/open-commerce/guides/quick-start/#clone-and-start-the-platform) for a list of ports where you can access its various services. For details on specific `make` commands for controlling and customizing the development platform, see the [development platform project documentation](https://github.com/reactioncommerce/reaction-development-platform#project-commands) on GitHub.

## Plugins

All of Open Commerce’s features, including the [API core](https://github.com/reactioncommerce/api-core), are provided by first-party plugins—the [main project](https://github.com/reactioncommerce/reaction) is a shell that that imports all of the required NPM packages. This structure gives you complete control over what plugins and features are active in your installation. For more information on customizing these plugins or creating your own, check out the [Build an API Plugin guide](/open-commerce/guides/build-api-plugin/).

A wide range of first-party plugins covers all the basic platform features, including handling backend services, shop configuration, and the shopper experience. You can view a list of the plugins active in your instance in the admin dashboard under **Settings > System Information**.

|Plugin|Description|
|------|-----------|
|[`api-plugin-accounts`](https://github.com/reactioncommerce/api-plugin-accounts)|Manages internal accounts for shop operators and shoppers.|
|[`api-plugin-address-validation`](https://github.com/reactioncommerce/api-plugin-address-validation)|Validates shopper addresses during checkout.|
|[`api-plugin-address-validation-test`](https://github.com/reactioncommerce/api-plugin-address-validation-test)|A template for creating a new address validation plugin.|
|[`api-plugin-authentication`](https://github.com/reactioncommerce/api-plugin-authentication)|Connects the Identity service to the Open Commerce API.|
|[`api-plugin-authorization-simple`](https://github.com/reactioncommerce/api-plugin-authorization-simple)|Checks account permissions to allow or deny actions.|
|[`api-plugin-carts`](https://github.com/reactioncommerce/api-plugin-carts)|Handles shopping cart objects.|
|[`api-plugin-catalogs`](https://github.com/reactioncommerce/api-plugin-catalogs)|Handles product catalogs.|
|[`api-plugin-discounts`](https://github.com/reactioncommerce/api-plugin-discounts)|Handles discounting products.|
|[`api-plugin-discounts-codes`](https://github.com/reactioncommerce/api-plugin-discounts-codes)|Enables discount codes that shoppers can enter at checkout.|
|[`api-plugin-email`](https://github.com/reactioncommerce/api-plugin-email)|Sends transactional emails.|
|[`api-plugin-email-smtp`](https://github.com/reactioncommerce/api-plugin-email-smtp)|Manages the SMTP connection for outgoing transactional emails.|
|[`api-plugin-email-templates`](https://github.com/reactioncommerce/api-plugin-email-templates)|Provides transactional email templates that shop operators can customize.|
|[`api-plugin-files`](https://github.com/reactioncommerce/api-plugin-files)|Manages file uploads for a shop.|
|[`api-plugin-i18n`](https://github.com/reactioncommerce/api-plugin-i18n)|Handles internationalization options for a shop.|
|[`api-plugin-inventory`](https://github.com/reactioncommerce/api-plugin-inventory)|Tracks inventory counts for product variants.|
|[`api-plugin-inventory-simple`](https://github.com/reactioncommerce/api-plugin-inventory-simple)|Determines product availability status based on inventory counts.|
|[`api-plugin-job-queue`](https://github.com/reactioncommerce/api-plugin-job-queue)|Manages job queuing and execution.|
|[`api-plugin-navigation`](https://github.com/reactioncommerce/api-plugin-navigation)|Allows for the creation of hierarchical navigation in a shop.|
|[`api-plugin-notifications`](https://github.com/reactioncommerce/api-plugin-notifications)|Sends messages about order statuses.|
|[`api-plugin-orders`](https://github.com/reactioncommerce/api-plugin-orders)|Handles orders, including fulfillment groups.|
|[`api-plugin-payments`](https://github.com/reactioncommerce/api-plugin-payments)|Manages all payment methods for a shop.|
|[`api-plugin-payments-example`](https://github.com/reactioncommerce/api-plugin-payments-example)|A template for creating a new payment handler.|
|[`api-plugin-payments-stripe`](https://github.com/reactioncommerce/api-plugin-payments-stripe)|Processes payments with Stripe.|
|[`api-plugin-pricing-simple`](https://github.com/reactioncommerce/api-plugin-pricing-simple)|Enables setting prices on product variants.|
|[`api-plugin-products`](https://github.com/reactioncommerce/api-plugin-products)|Creates products and manages their information.|
|[`api-plugin-settings`](https://github.com/reactioncommerce/api-plugin-settings)|Manages shop settings like those available in the admin dashboard.|
|[`api-plugin-shipments`](https://github.com/reactioncommerce/api-plugin-shipments)|Handles shipment types, including allowing or denying shipment methods for products or orders that meet certain criteria.|
|[`api-plugin-shipments-flat-rate`](https://github.com/reactioncommerce/api-plugin-shipments-flat-rate)|Handles shipment types that always cost the same amount.|
|[`api-plugin-shops`](https://github.com/reactioncommerce/api-plugin-shops)|Allows creation and management of shops within an Open Commerce instance.|
|[`api-plugin-simple-schema`](https://github.com/reactioncommerce/api-plugin-simple-schema)|Implements [SimpleSchema](https://github.com/longshotlabs/simpl-schema) to validate data for MongoDB documents.|
|[`api-plugin-sitemap-generator`](https://github.com/reactioncommerce/api-plugin-sitemap-generator)|Creates sitemap files for search engines at a specified time interval.|
|[`api-plugin-surcharges`](https://github.com/reactioncommerce/api-plugin-surcharges)|Allows applying surcharges to a cart.|
|[`api-plugin-system-information`](https://github.com/reactioncommerce/api-plugin-system-information)|Provides information about the running instance of Open Commerce, including registered plugins.|
|[`api-plugin-tags`](https://github.com/reactioncommerce/api-plugin-tags)|Handles tag creation and organization.|
|[`api-plugin-taxes`](https://github.com/reactioncommerce/api-plugin-taxes)|Manages tax rates based on location, product type, and more.|
|[`api-plugin-taxes-flat-rate`](https://github.com/reactioncommerce/api-plugin-taxes-flat-rate)|Allows for setting a single tax rate.|
|[`api-plugin-translations`](https://github.com/reactioncommerce/api-plugin-translations)|Supplies translations of terms used in the API in over 20 languages.|

## Environment variables

Much of the platform-wide configuration of Open Commerce is set via environment variables—either to pass information from one service to another or simply to customize the platform to meet your needs.

You can set variables in the `.env` file within each plugin, which is used by Docker Compose when starting the system and loading plugins. Each Open Commerce repo contains a `.env.example` file that lists the environment variables and default values for that project. You can run the `bin/setup` script within the project to automatically copy the contents of `.env.example` to `.env` (although it will not overwrite existing values). Your `.env` files will contain personalized and potentially sensitive information, so you should never commit them to a public GitHub repo.

>**Note**: When running the development platform, its `make` scripts automatically set up the required `.env` files for you.

In some cases, projects may look for additional environment variables that are not listed in `.env.example` because they are optional and have default values set in the code. You can find these optional variables by searching for `cleanEnv` within a Node project.

## Community

Open Commerce has a global team of contributors to its [projects on GitHub](https://github.com/reactioncommerce). New contributors are always welcome, and they should read and abide by the [code of conduct](https://github.com/reactioncommerce/reaction-docs/blob/trunk/public-docs/code-of-conduct.md/).

If you have questions about implementing Open Commerce, customizing code for your own purposes, or contributing to a project, check out the [developer forum](https://forums.reactioncommerce.com) and [Gitter chat](https://gitter.im/reactioncommerce/reaction/). For bug reports and feature requests, you can create a new issue directly in the appropriate repo.

For more information on getting your code ready to commit, check out the [ESLint configuration](https://github.com/reactioncommerce/reaction-eslint-config) project and the [Testing Requirements](/open-commerce/docs/testing-requirements/) doc.
