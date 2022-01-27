# Plugins Reference

All of Mailchimp Open Commerce's features are provided by plugins, either first-party or third-party. This structure gives you complete control over what plugins and features are active in your installation. First-party Open Commerce services are distributed as Docker images, or can be customized and then built into your own Docker image.

## Available Plugins

A wide range of first-party plugins covers all the basic platform features, including the [API core](https://github.com/reactioncommerce/api-core), handling backend services, shop configuration, and the shopper experience. You can view a list of the plugins active in your instance in the admin dashboard under **Settings > System Information**.

The plugins included with the development platform are:

| Plugin                                                                                                         | Description                                                                                                               |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| [`api-plugin-accounts`](https://github.com/reactioncommerce/api-plugin-accounts)                               | Manages internal accounts for shop operators and shoppers.                                                                |
| [`api-plugin-address-validation`](https://github.com/reactioncommerce/api-plugin-address-validation)           | Validates shopper addresses during checkout.                                                                              |
| [`api-plugin-address-validation-test`](https://github.com/reactioncommerce/api-plugin-address-validation-test) | A template for creating a new address validation plugin.                                                                  |
| [`api-plugin-authentication`](https://github.com/reactioncommerce/api-plugin-authentication)                   | Connects the Identity service to the Open Commerce API.                                                                   |
| [`api-plugin-authorization-simple`](https://github.com/reactioncommerce/api-plugin-authorization-simple)       | Checks account permissions to allow or deny actions.                                                                      |
| [`api-plugin-carts`](https://github.com/reactioncommerce/api-plugin-carts)                                     | Handles shopping cart objects.                                                                                            |
| [`api-plugin-catalogs`](https://github.com/reactioncommerce/api-plugin-catalogs)                               | Handles product catalogs.                                                                                                 |
| [`api-plugin-discounts`](https://github.com/reactioncommerce/api-plugin-discounts)                             | Handles discounting products.                                                                                             |
| [`api-plugin-discounts-codes`](https://github.com/reactioncommerce/api-plugin-discounts-codes)                 | Enables discount codes that shoppers can enter at checkout.                                                               |
| [`api-plugin-email`](https://github.com/reactioncommerce/api-plugin-email)                                     | Sends transactional emails.                                                                                               |
| [`api-plugin-email-smtp`](https://github.com/reactioncommerce/api-plugin-email-smtp)                           | Manages the SMTP connection for outgoing transactional emails.                                                            |
| [`api-plugin-email-templates`](https://github.com/reactioncommerce/api-plugin-email-templates)                 | Provides transactional email templates that shop operators can customize.                                                 |
| [`api-plugin-files`](https://github.com/reactioncommerce/api-plugin-files)                                     | Manages file uploads for a shop.                                                                                          |
| [`api-plugin-i18n`](https://github.com/reactioncommerce/api-plugin-i18n)                                       | Handles internationalization options for a shop.                                                                          |
| [`api-plugin-inventory`](https://github.com/reactioncommerce/api-plugin-inventory)                             | Tracks inventory counts for product variants.                                                                             |
| [`api-plugin-inventory-simple`](https://github.com/reactioncommerce/api-plugin-inventory-simple)               | Determines product availability status based on inventory counts.                                                         |
| [`api-plugin-job-queue`](https://github.com/reactioncommerce/api-plugin-job-queue)                             | Manages job queuing and execution.                                                                                        |
| [`api-plugin-navigation`](https://github.com/reactioncommerce/api-plugin-navigation)                           | Allows for the creation of hierarchical navigation in a shop.                                                             |
| [`api-plugin-notifications`](https://github.com/reactioncommerce/api-plugin-notifications)                     | Sends messages about order statuses.                                                                                      |
| [`api-plugin-orders`](https://github.com/reactioncommerce/api-plugin-orders)                                   | Handles orders, including fulfillment groups.                                                                             |
| [`api-plugin-payments`](https://github.com/reactioncommerce/api-plugin-payments)                               | Manages all payment methods for a shop.                                                                                   |
| [`api-plugin-payments-example`](https://github.com/reactioncommerce/api-plugin-payments-example)               | A template for creating a new payment handler.                                                                            |
| [`api-plugin-payments-stripe`](https://github.com/reactioncommerce/api-plugin-payments-stripe)                 | Processes payments with Stripe.                                                                                           |
| [`api-plugin-pricing-simple`](https://github.com/reactioncommerce/api-plugin-pricing-simple)                   | Enables setting prices on product variants.                                                                               |
| [`api-plugin-products`](https://github.com/reactioncommerce/api-plugin-products)                               | Creates products and manages their information.                                                                           |
| [`api-plugin-settings`](https://github.com/reactioncommerce/api-plugin-settings)                               | Manages shop settings like those available in the admin dashboard.                                                        |
| [`api-plugin-shipments`](https://github.com/reactioncommerce/api-plugin-shipments)                             | Handles shipment types, including allowing or denying shipment methods for products or orders that meet certain criteria. |
| [`api-plugin-shipments-flat-rate`](https://github.com/reactioncommerce/api-plugin-shipments-flat-rate)         | Handles shipment types that always cost the same amount.                                                                  |
| [`api-plugin-shops`](https://github.com/reactioncommerce/api-plugin-shops)                                     | Allows creation and management of shops within an Open Commerce instance.                                                 |
| [`api-plugin-simple-schema`](https://github.com/reactioncommerce/api-plugin-simple-schema)                     | Implements [SimpleSchema](https://github.com/longshotlabs/simpl-schema) to validate data for MongoDB documents.           |
| [`api-plugin-sitemap-generator`](https://github.com/reactioncommerce/api-plugin-sitemap-generator)             | Creates sitemap files for search engines at a specified time interval.                                                    |
| [`api-plugin-surcharges`](https://github.com/reactioncommerce/api-plugin-surcharges)                           | Allows applying surcharges to a cart.                                                                                     |
| [`api-plugin-system-information`](https://github.com/reactioncommerce/api-plugin-system-information)           | Provides information about the running instance of Open Commerce, including registered plugins.                           |
| [`api-plugin-tags`](https://github.com/reactioncommerce/api-plugin-tags)                                       | Handles tag creation and organization.                                                                                    |
| [`api-plugin-taxes`](https://github.com/reactioncommerce/api-plugin-taxes)                                     | Manages tax rates based on location, product type, and more.                                                              |
| [`api-plugin-taxes-flat-rate`](https://github.com/reactioncommerce/api-plugin-taxes-flat-rate)                 | Allows for setting a single tax rate.                                                                                     |
| [`api-plugin-translations`](https://github.com/reactioncommerce/api-plugin-translations)                       | Supplies translations of terms used in the API in over 20 languages.                                                      |

## Register API Plugins

The Reaction API server has a plugin system that allows code to be broken into small packages. Plugins can register functions, configuration, and GraphQL schemas and resolvers.

Some API plugins are included in the API codebase, but in general you should create plugins as NPM packages. This does not necessarily mean they need to be published to NPM, but they must be something you can add to `package.json` and NPM will know how to install it.

At a high level, an API plugin package is one where the main export is an async function that accepts a `ReactionAPI` instance and calls `api.registerPlugin` before returning. In a Reaction API project, your plugin package will be imported and used like this:

```js
import registerYourPlugin from "<your-plugin>";

const api = new ReactionAPI();

async function startAPI() {
  await registerYourPlugin(api);
  await api.start();
}
```

## registerPlugin

A plugin package must export the following function by default:

```js
export default async function register(app) {
  // register your plugin here
  await app.registerPlugin({
    label: "Plugin Example",
    name: "plugin-example",
    version: "<package-version>",
  });
}
```

The two keys that every plugin will include are `name` and `label`. `name` must be unique, cannot contain spaces, and identifies your plugin; `label` is the human-readable version of your plugin name, for showing in UIs.

Beyond `name` and `label`, the following standard keys can be included in your `registerPlugin` object:

- `auth`
- `collections`
- `contextAdditions`
- `expressMiddleware`
- `functionsByType`
- `graphQL`
- `i18n`
- `mutations`
- `queries`
- `simpleSchemas`

### auth

Plugins can pass functions in an `auth` object, which are then used to add permission and account information to `context` for each API request.

```js
auth: {
  accountByUserId,
    getHasPermissionFunctionForUser,
    getShopsUserHasPermissionForFunctionForUser;
}
```

- `accountByUserId`: An async function with signature `(context, userId)` that must return an account document for the given `userId`, or `null` if one cannot be found. This will be used to set `context.account` and `context.accountId`.

- `getHasPermissionFunctionForUser`: An async function with signature `(context)`, which must return an async function that checks permissions for `context.user` and returns a boolean. The signature of the returned function must be `(permissions, shopId)`, where `permissions` is an array of strings and `shopId` is an optional permission filter. The function must return `true` only if the context user has ANY of the permissions.

- `getShopsUserHasPermissionForFunctionForUser`: An async function with signature `(context)`, which must return an async function that checks shop access for `context.user` and returns a boolean. The signature of the returned function must be `(permission)`, where `permission` is a permission string. The function must return an array of shop IDs for which the context user has the given permission.

### collections

Use this to add new collections to the `context` object. Here is a [guide](how-tos/add-collections-from-plugin.md) to help.

### contextAdditions

A plugin can add properties to context using the `contextAdditions` option when calling `registerPlugin`. They are added before any `preStartup` or `startup` functions are run.

```js
app.registerPlugin({
  // ...
  contextAdditions: {
    custom_key: "custom_value",
  },
});
```

```js
// in startup fn or anywhere you have context
console.log(context.something); // "custom_value"
```

_Note:_ Another plugin can override the keys that you added. So it's a good idea to namespace your keys like "myPluginName:myKey".

### expressMiddleware

Plugins can register Express middleware using `registerPlugin`:

```js
expressMiddleware: [
  {
    route: "graphql",
    stage: "authenticate",
    fn: tokenMiddleware,
  },
];
```

For now, only "graphql" route is supported, and the following stages are supported in this order:

- `first`
- `before-authenticate`
- `authenticate`
- `before-response`

The `first` middleware stage can be used for loggers or anything else that needs to be first in the middleware list. `before-response` middleware will have the user available if there is one, and is called before the Apollo GraphQL middleware.

An `authenticate` middleware function should do something like look up the user by the Authorization header, and either set `request.user` or send a 401 response if the token is invalid. It should not require a token. This is what the built-in `account` service now does.

A middleware function is passed `context` and must return the Express middleware handler function, which must call `next()` or send a response.

### functionsByType

The `functionsByType` object is a map of function types to arrays of functions of that type. This pattern can be used by any plugin to allow any other plugin to register certain types of functions for plugin points.

Documentation for individual plugins will tell you how to use this for that plugin, but there are also a few core types that any plugin might want to use:

- createDataLoaders
- preStartup
- registerPluginHandler
- startup
- shutdown

### graphQL

Use the `graphQL` object to register schemas and resolvers that extend the core GraphQL API. It can have the following structure:

```js
{
      typeDefsObj: <array of typedefs>,
      resolvers: <array of resolvers>,
      schemas: <array of schemas>,
}
```

### i18n

Use the `i18n` object to register translations.

### mutations and queries

The `mutations` and `queries` objects are maps of functions that are extended onto `context.mutations` and `context.queries`. These may be functions that are called from GraphQL resolvers of a similar name, or functions that are intended to be called only by other plugin code. Functions that modify data should be registered as `mutations` and all other functions should be registered as `queries`.

### simpleSchemas

The SimpleSchema package is used by many plugins to validate data, often before inserting or updating MongoDB documents. To allow other plugins to extend these schemas, some plugins register them such that they are accessible in a `preStartup` function.

Here's an example of registering two schemas:

```js
simpleSchemas: {
  Cart, CartItem;
}
```

And a different plugin can then extend them in a `preStartup` function:

```js
export default function preStartup(context) {
  context.simpleSchemas.CartItem.extend({
    additionalCartItemField: String,
  });
}
```

(This could be done in a `startup` function, but because startup code sometimes validates against these schemas, it's safer to do it in a `preStartup` function.)

## Developing API Plugins

### Switching Reaction to Development Mode

Now let’s say you want to make some changes to a service and be able to see those changes reflected in the running service. By default, any changes you make do not affect the running service because all services run in standard mode. It’s done this way because it is much faster to start a service in standard mode. To develop a service, you need to switch it to development mode.
Development mode differs from standard mode in the following ways:

- A generic Node.js development Docker image is used for the container rather than the published service image.
- Your locally checked out project files, other than those under `node_modules`, are mirrored into the container by way of a Docker volume mount.
- NPM packages are installed when the container starts and therefore reflect the project’s `package.json` file. (In other words, you can add additional NPM dependencies and then stop and restart the container.)
- In the container, the project runs within nodemon, which will restart the app whenever you change any file in the project.

Switching a project to development mode is easy.

1. Start the whole system in standard mode using `make` command in the Reaction Development Platform directory.
2. Run `make dev-<project folder name>` for one or more services to restart them in development mode. For example, `make dev-reaction` if you want to make API changes.

But what exactly is this doing? Hopefully you don’t need to concern yourself with the details, but for those who want to know or who run into troubles, here’s the breakdown:

- Every project repo has two Docker Compose configuration files: `docker-compose.yml` for standard mode and `docker-compose.dev.yml` for dev mode. The dev mode file is intended to extend the standard file, so it isn’t a complete configuration.
- There is a feature built in to Docker Compose that looks for a `docker-compose.override.yml` file and uses it to extend `docker-compose.yml` whenever you run `docker-compose` commands.
- The `make dev-*` commands stop the service if necessary, symlink `docker-compose.dev.yml` to `docker-compose.override.yml` in that service’s project folder, and then restart the service with the dev override now in effect. Conversely the `make` command and other non-dev-mode `make` subcommands always remove the `docker-compose.override.yml` symlink before running the service, ensuring that it will revert to standard mode.

To put that another way, this command:

```
make dev-reaction
```

is equivalent to this sequence of commands:

```
cd reaction
docker-compose down
ln -sf docker-compose.dev.yml docker-compose.override.yml
docker-compose pull
docker-compose up -d
cd ..
```

## Developing API Plugins

Let’s talk about how to clone the built-in plugin packages. There are nearly 40 API plugins. That’s a lot of repositories to clone, and it can be helpful to clone them all so that you can run searches across the full codebase. Fortunately, there is a quick way to do that, too:

```
make clone-api-plugins
```

When you run this command in the Reaction Development Platform directory, it will clone every built-in plugin into an `api-plugins` subfolder, alongside the service subfolders. You can then modify files in these plugins as necessary and link them into the API project to test them.

So back to the linking scripts. Let’s say you think there’s a bug in the built-in carts plugin. You cloned all the plugins and then changed a file in `api-plugins/api-plugin-carts` directory in an attempt to fix the bug. Now the first thing to do is to put the API in development mode if you haven’t already. After that, linking in your local carts plugin code is as simple as this:

```
cd reaction
bin/package-link @reactioncommerce/api-plugin-carts
```

The already-running API server will automatically restart to pull in your changes.

If your fix wasn’t quite correct and you make more changes to files in the carts plugin, you’ll have to run the link command again:

```
bin/package-link @reactioncommerce/api-plugin-carts
```

If necessary, you can run the link command for other plugins as well. You can even run it for other NPM packages that are not API plugins, but then you’ll need an additional argument that is the relative or absolute path to the package code on your host machine. For example:

```
bin/package-link some-other-package ../../some-other-package
```

(You can also use the code path argument for API plugin linking if you have cloned your API plugins to a non-standard location.)

When you’re done, be sure to unlink before stopping the API service or running `make stop`:

```
bin/package-unlink @reactioncommerce/api-plugin-carts
```

This linking approach works pretty well but has the potential to get the API into a state where it complains about missing dependencies and won’t start. If this happens and restarting the API service does not fix it, you will need to use the `docker volume rm` command to delete the API `node_modules` volume (usually named something like `reaction_api_node_modules`). If that doesn’t work, running `docker-compose down -v` in the `reaction` directory will work, but be careful because that command will also wipe out your local MongoDB database.

### Link multiples packages with package configuration file

Before running the `bin/package-link` script, create a `yalc-packages` file from the example.

```sh
cp yalc-packages.example yalc-packages
```

Then run the link script without any arguments

```sh
./bin/package-link
```

This will link every package in the `yalc-packages` file to your api app. If you don't want every plugin to be linked, edit `yalc-packages` and set the packages you want disabled and unlinked to `false`.

Format: `path=true|false`

```
../api-plugins/api-core=true
../api-plugins/api-plugin-accounts=true

```

(You must have a blank line at the end of the file, otherwise your last plugin will be omitted)

### Update package links on stop/start

If you've stopped, then started your container you may notice your linked packages have been reset. To re-link previously linked packages you can run the package update script.

```
./bin/package-update
```

(This runs `yalc update` under the hood, and only re-links packages in the `yalc.lock` file, and only if those packages are still published inside the docker container.)

Alternatively, if you've configured `yalc-packages`, then you can always run `./bin/package-link` again to re-link and update everything.

## Environment variables

You can set variables in each service's `.env` file, which is used by Docker Compose when starting the system and loading plugins. Each Open Commerce repo contains a `.env.example` file that lists the environment variables and default values for that project. You can run the `bin/setup` script within the project to automatically copy the contents of `.env.example` to `.env` (although it will not overwrite existing values). Your `.env` files will contain personalized and potentially sensitive information, so you should never commit them to a public GitHub repo.

In some cases, projects may look for additional environment variables that are not listed in `.env.example` because they are optional and have default values set in the code. You can find these optional variables by searching for `cleanEnv` within a Node project.

The following tables detail the environment variables used by the API core, storefront, and admin services.

### API

| Variable Name                                    | Description                                                                                                                                                                                        | Default                              |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `GRAPHQL_INTROSPECTION_ENABLED`                  | Allow introspection of the GraphQL API.                                                                                                                                                            | `true`                               |
| `GRAPHQL_PLAYGROUND_ENABLED`                     | Serve the GraphQL Playground at `/graphql`.                                                                                                                                                        | `true`                               |
| `MONGO_URL`                                      | The MongoDB connection string URL, including the name of the database you want to use.                                                                                                             | `mongodb://localhost:27017/reaction` |
| `MONGO_USE_UNIFIED_TOPOLOGY`                     | MongoDB’s [`useUnifiedTopology`](https://mongodb.github.io/node-mongodb-native/3.3/reference/unified-topology/) option.                                                                            | `true`                               |
| `PORT`                                           | Port to serve the API on.                                                                                                                                                                          | `3000`                               |
| `REACTION_LOG_LEVEL`                             | Control how much is [printed in the logs](https://github.com/reactioncommerce/logger#log-levels). Possible values, from most to least verbose: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`. | `DEBUG`                              |
| `REACTION_APOLLO_FEDERATION_ENABLED`             | Enable [Apollo Federation](https://www.apollographql.com/docs/federation/) support.                                                                                                                | `false`                              |
| `REACTION_ADMIN_PUBLIC_ACCOUNT_REGISTRATION_URL` | The registration URL to be used in new account invitation emails.                                                                                                                                  | `http://localhost:4080`              |
| `REACTION_GRAPHQL_SUBSCRIPTIONS_ENABLED`         | Enable [GraphQL subscriptions](https://www.apollographql.com/docs/apollo-server/data/subscriptions/) over WebSockets.                                                                              | `true`                               |
| `REACTION_SHOULD_INIT_REPLICA_SET`               | Automatically initialize a MongoDB replica set on startup if one isn’t found.                                                                                                                      | `true`                               |
| `REACTION_WORKERS_ENABLED`                       | Enable background job workers. If set to `false`, features that require background jobs, such as emailing and file uploads, won't work.                                                            | `true`                               |
| `VERBOSE_JOBS`                                   | Enable logging of jobs to the console.                                                                                                                                                             | `false`                              |
| `ROOT_URL`                                       | The root URL (including protocol) for the API.                                                                                                                                                     | `http://localhost:3000`              |
| `STRIPE_API_KEY`                                 | The secret key from your Stripe account dashboard. Required for using Stripe payments.                                                                                                             | `YOUR_PRIVATE_STRIPE_API_KEY`        |
| `MAIL_URL`                                       | An SMTP URL (e.g. `smtp://user:pass@example.com:465`) that is used to send all transactional emails from the `email-smtp` plugin.                                                                  | none                                 |
| `EMAIL_DEBUG`                                    | Enable email plugin logging.                                                                                                                                                                       | `false`                              |
| `REACTION_SHOULD_ENCODE_IDS`                     | Transform internal IDs to opaque IDs using the `encodeOpaqueId` function.                                                                                                                          | `true`                               |
| `STORE_URL`                                      | The URL for the storefront.                                                                                                                                                                        | `http://localhost:4000`              |

### Example Storefront

| Variable Name           | Description                                                                                                 | Default               |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- | --------------------- |
| `CANONICAL_URL`         | The canonical root public URL for your storefront.                                                          | none                  |
| `BUILD_GRAPHQL_URL`     | A URL for creating a build outside of CI/CD.                                                                | none                  |
| `EXTERNAL_GRAPHQL_URL`  | The public GraphQL API URL used by Next.js.                                                                 | none                  |
| `INTERNAL_GRAPHQL_URL`  | The URL used by the frontend server to reach GraphQL for the build process.                                 | none                  |
| `NODE_ENV`              | Node environment designation, usually `production` or `development`.                                        | none                  |
| `PORT`                  | Port to run the storefront server on.                                                                       | `4000`                |
| `SESSION_MAX_AGE_MS`    | The maximum age in milliseconds for the storefront session cookie, which is used for authentication.        | `86400000` (24 hours) |
| `SESSION_SECRET`        | A unique key for [cookie session](https://www.npmjs.com/package/cookie-session#secret) verification.        | none                  |
| `STRIPE_PUBLIC_API_KEY` | The public/client key from your Stripe dashboard.                                                           | none                  |
| `SITEMAP_MAX_AGE`       | The `max-age` value in seconds for the `Cache-Control` header included when serving the `sitemap.xml` file. | `43200` (12 hours)    |
| `IS_BUILDING_NEXTJS`    | If `true`, serve the API from `BUILD_GRAPHQL_URL`; if `false`, serve the API from `INTERNAL_GRAPHQL_URL`.   | `false`               |

### Admin

| Variable Name                 | Description                                                                                                                                                                                                                                          | Default |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `PUBLIC_GRAPHQL_API_URL_HTTP` | An API URL that is accessible from browsers and accepts GraphQL POST requests over HTTP.                                                                                                                                                             | none    |
| `PUBLIC_GRAPHQL_API_URL_WS`   | An API URL that is accessible from browsers and accepts GraphQL WebSocket connections. Enables GraphQL subscriptions on the client. The location is usually the same as `PUBLIC_GRAPHQL_API_URL_HTTP` but using the `ws` protocol instead of `http`. | none    |
| `PUBLIC_FILES_BASE_URL`       | A full public URL that has `/assets/uploads` and `/assets/files` endpoints for uploading and downloading files. Usually the same as the API service root URL.                                                                                        | none    |
| `PUBLIC_I18N_BASE_URL`        | Required. A full public URL that has `/locales/namespaces.json` and `/locales/resources.json` endpoints for loading translations. Usually the same as the API service root URL.                                                                      | none    |
| `PUBLIC_STOREFRONT_HOME_URL`  | Required. The URL for the storefront home page. This is only used as fallback if a storefront URL isn’t set within the admin dashboard.                                                                                                              | none    |
| `ROOT_URL`                    | The canonical root public URL for the admin server.                                                                                                                                                                                                  | none    |
| `PORT`                        | The port to run the Node server on. We recommend using `4080`.                                                                                                                                                                                       | none    |
| `MONGO_ALLOW_DISK_USE`        | Allow MongoDB to use [temporary files on disk](https://docs.mongodb.com/manual/reference/method/cursor.allowDiskUse/).                                                                                                                               | `false` |
