## The basics

The system is made up of various services and web servers, each of which is distributed as a Docker image, or can be customized and then built into your own Docker image. Some environment variables are required because they tell a container service how to connect to another service or a database, others are optional with sensible defaults, but allow you to customize the system to meet your needs.

This article aims to be a full listing of the required and optional environment variables used by all components of the Mailchimp Open Commerce system.

## How to modify

You can set variables in the `.env` file within each plugin/project, which is used by Docker Compose when starting the system and loading plugins. Each Open Commerce repo contains a `.env.example` file that lists the environment variables and default values for that project. You can run the bin/setup script within the project to automatically copy the contents of `.env.example` to `.env` (although it will not overwrite existing values). Your `.env` files will contain personalized and potentially sensitive information, so you should never commit them to a public GitHub repo.

In some cases, projects may look for additional environment variables that are not listed in `.env.example` because they are optional and have default values set in the code. You can find these optional variables by searching for `cleanEnv` within a Node project.

**Note** For the quick start guide, the default values should be good enough.

Below is the list of all environment variable categorized by the services

### API

| Variable Name                                    | Description                                                                                                                                                                          | Default                            |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------- |
| `GRAPHQL_INTROSPECTION_ENABLED`                  | Allow introspection of the GraphQL API.                                                                                                                                              | `true`                             |
| `GRAPHQL_PLAYGROUND_ENABLED`                     | Serve the GraphQL Playground UI from `/graphql`.                                                                                                                                     | `true`                             |
| `MONGO_URL`                                      | The MongoDB connection string URL, including the name of the database you want to use.                                                                                               | mongodb://localhost:27017/reaction |
| `MONGO_USE_UNIFIED_TOPOLOGY`                     | MongoDB’s [`useUnifiedTopology` option](https://mongodb.github.io/node-mongodb-native/3.3/reference/unified-topology/)                                                               | `true`                             |
| `PORT`                                           | Port to serve the API on.                                                                                                                                                            | `3000`                             |
| `REACTION_LOG_LEVEL`                             | The log level, which controls how much is printed in the logs. [See available options](https://github.com/reactioncommerce/logger#log-levels)                                        | `DEBUG`                            |
| `REACTION_APOLLO_FEDERATION_ENABLED`             | Enables [Apollo Federation](https://www.apollographql.com/docs/federation/) support                                                                                                  | `false`                            |
| `REACTION_ADMIN_PUBLIC_ACCOUNT_REGISTRATION_URL` | The registration URL to be used in new account invitation emails                                                                                                                     | `http://localhost:4080`.           |
| `REACTION_GRAPHQL_SUBSCRIPTIONS_ENABLED`         | Enable [GraphQL subscriptions](https://www.apollographql.com/docs/apollo-server/data/subscriptions/) over WebSockets. Docs:                                                          | `true`                             |
| `REACTION_SHOULD_INIT_REPLICA_SET`               | Whether to auto-initialize a MongoDB replica set on startup if one isn’t found                                                                                                       | `true`                             |
| `REACTION_WORKERS_ENABLED`                       | Set to `false` to disable background job workers. Be careful because at least one instance must be working background jobs or features such as emailing and file uploads won't work. | `true`                             |
| `VERBOSE_JOBS`                                   | Set `true` to enable logs of jobs                                                                                                                                                    | `false`                            |
| `ROOT_URL`                                       | The root URL (including protocol) for the API                                                                                                                                        | http://localhost:3000              |
| `STRIPE_API_KEY`                                 | The Stripe secret key from your Stripe account dashboard. Required if you want Stripe payments to work.                                                                              | YOUR_PRIVATE_STRIPE_API_KEY        |
| `REACTION_ADMIN_PUBLIC_ACCOUNT_REGISTRATION_URL` | The URL for the admin login/signup page                                                                                                                                              | http://localhost:4080              |
| `MAIL_URL`                                       | An SMTP mail url, e.g. `smtp://user:pass@example.com:465`, that is used to send all transactional emails from the `email-smtp` plugin.                                               | No default                         |
| `EMAIL_DEBUG`                                    | Enable email plugin logging.                                                                                                                                                         | `false`                            |
| `REACTION_SHOULD_ENCODE_IDS`                     | Set to `false` to disable encoding of ids.                                                                                                                                           | `true`                             |
| `STORE_URL`                                      | The URL where storefront is available                                                                                                                                                | http://localhost:4000              |

### Example Storefront

| Variable Name           | Description                                                                                                  | Default             |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------- |
| `CANONICAL_URL`         | The canonical root public URL for your storefront                                                            |                     |
| `BUILD_GRAPHQL_URL`     | Is used to create a build from outside of CI/CD                                                              |                     |
| `EXTERNAL_GRAPHQL_URL`  | The public GraphQL API URL. Used by Next.js on client.                                                       |                     |
| `INTERNAL_GRAPHQL_URL`  | Is used on the frontend server to reach GraphQL for the build process                                        |                     |
| `NODE_ENV`              | Standard Node environment designation                                                                        |                     |
| `PORT`                  | Port on which to run the storefront server in the container                                                  | 4000                |
| `SESSION_MAX_AGE_MS`    | The maximum age in milliseconds for the storefront session cookie, which is used for authentication          | 86400000 (24 hours) |
| `SESSION_SECRET`        | A unique key for [cookie session](https://www.npmjs.com/package/cookie-session#secret) verification          |                     |
| `STRIPE_PUBLIC_API_KEY` | This is the public/client key from your Stripe dashboard.                                                    |                     |
| `SITEMAP_MAX_AGE`       | The `max-age` value(in seconds) for the `Cache-Control` header included when serving the `sitemap.xml` file. | 43200 (12 hours)    |
| `IS_BUILDING_NEXTJS`    | If `true` uses `BUILD_GRAPHQL_URL` else uses `INTERNAL_GRAPHQL_URL` to serve the API                         | `false`             |

### Admin

| Variable Name                 | Description                                                                                                                                                                                                                                         | Default |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `PUBLIC_GRAPHQL_API_URL_HTTP` | A Reaction API URL that is accessible from browsers and accepts GraphQL POST requests over HTTP                                                                                                                                                     |         |
| `PUBLIC_GRAPHQL_API_URL_WS`   | A Reaction API URL that is accessible from browsers and accepts GraphQL WebSocket connections. Usually the same as `PUBLIC_GRAPHQL_API_URL_HTTP` but replace `http` with `ws`. If this is set, GraphQL subscriptions will be enabled on the client. | ""      |
| `PUBLIC_FILES_BASE_URL`       | A full public URL that has /assets/files and /assets/uploads endpoints for uploading and downloading files. Typically this is the API service root URL.                                                                                             |         |
| `PUBLIC_I18N_BASE_URL`        | Required. A full public URL that has /locales/namespaces.json and /locales/resources.json endpoints for loading translations. Typically this is the API service root URL.                                                                           |         |
| `PUBLIC_STOREFRONT_HOME_URL`  | Required. The URL for your storefront home page. This is only used as fallback if a URL isn’t set for the shop from the admin UI.                                                                                                                   |         |
| `ROOT_URL`                    | The canonical root public URL for the Admin server                                                                                                                                                                                                  |         |
| `PORT`                        | The port on which to run the Node server in the container. Recommend `4080`.                                                                                                                                                                        |         |
| `MONGO_ALLOW_DISK_USE`        | Allows MongoDB to use [temporary files on disk](https://docs.mongodb.com/manual/reference/method/cursor.allowDiskUse/)                                                                                                                              | `false` |
