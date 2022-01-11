# How to create and run migrations

## Prerequisites
Must be running node 14.17 or later

## Adding a migration to an API plugin package
To add a migration to an API plugin package, there are four main steps:

1. Add the migration code in the plugin package, in a `migrations` folder alongside the `src` folder.
2. Add `export { default as migrations } from "./migrations/index.js";` in your plugin entry point file.
3. Add the latest version of the `@reactioncommerce/db-version-check` NPM package as a dependency.
4. Add and register a `preStartup` function in the plugin source. In it, call `doesDatabaseVersionMatch` to prevent API startup if the data isn't compatible with the code.

These steps are explained in more detail [here](https://github.com/reactioncommerce/migrator#how-to-publish-a-package-with-migrations), and you can look at the [simple-authorization](https://github.com/reactioncommerce/plugin-simple-authorization) plugin code for an example to follow.

IMPORTANT: If the plugin you added a migration to is one that is built in to the stock Reaction API releases, then at the same time you bump the plugin package version in https://github.com/reactioncommerce/reaction `trunk` branch, you must also update the data version in the trunk branch of `migrator.config.js` in this repo.

## Running migrations locally for development

1. Fork/clone this repo.
2. Check out the [tag](https://github.com/reactioncommerce/api-migrations/tags) that corresponds to your version of Reaction Platform. (Only 3.0.0 and higher are supported.)
3. `npm install`
4. Then to see a report of necessary migrations for your local MongoDB database and optionally run them:

```sh
MONGO_URL=mongodb://localhost:27017/reaction npx migrator migrate
```

Use a different `MONGO_URL` to run them on a different database.

Refer to [https://github.com/reactioncommerce/migrator](https://github.com/reactioncommerce/migrator) docs for other commands. Prefix them with `npx`.
5. Try to start your API service. If there are database version errors thrown on startup, then change the versions or add/remove tracks in `migrator.config.js` as necessary based on whatever those errors are asking for. Then repeat the previous step. (If you've added new tracks, you'll need to `npm install` the latest version of those packages first.) Keep doing this until the API service starts.

## Migrating Deployment Databases

Option 1: You can follow the above "Local Development Usage" instructions but specify a remote database if you want. Migrations will run on your computer, which may not be very fast or reliable.

Option 2: You can set up a CI task for this repo:

1. Create different configuration files for each deployed environment. For example, `migrator.config-staging.js` for the "staging" environment.
2. Add the necessary `MONGO_URL`s to your CI environment/secrets.
3. When config file changes are merged to the main branch, run `npx migrator migrate <env> -y` as a CI task with `MONGO_URL` set to the correct database for that environment. Do this for each Reaction environment (database) you have.
    - Ensure that your CI Docker image uses at least the version of Node that's in the `.nvmrc` file.
    
