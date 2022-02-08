## At a glance

If you need to migrate your API to a newer version or your plugin needs to add new elements to the core schema, this guide will help you in the process. 

## What you’ll need

You must be running node 14.17 or later

## Adding a migration to an API plugin package
You can look at the [simple-authorization](https://github.com/reactioncommerce/plugin-simple-authorization) plugin code for an example to follow.
To add a migration to an API plugin package, follow the below steps:

- In the plugin package create a `migrations` folder alongside the `src` folder.
- Create `migrationsNamespace.js` and add the line `export const migrationsNamespace = "<package-name>";`.
- Create `<n>.js` which will house the `up()` and `down()`. The `up` and `down` functions should do whatever they need to do to move data from your N-1 or N+1 schema to your N schema. Both types of functions receive a migration context, which has a connection to the MongoDB database and a progress function for reporting progress. The keys of the migration object are the database version numbers. These must be a single number (2) or two numbers separated by a dash (2-1) if you need to branch off your main migration path to support previous major releases. Only one branch level is allowed.
- Create `index.js` which exports the namespace and migration script.
- Add `export { default as migrations } from "./migrations/index.js";` in your plugin entry point file i.e, `<api-plugin-package>/index.js`.
- Add the latest version of the `@reactioncommerce/db-version-check` NPM package as a dependency using `npm install @reactioncommerce/db-version-check@latest`.
- Add and register a `preStartup` function in `src/index.js`. In it, call `doesDatabaseVersionMatch` to prevent API startup if the data isn't compatible with the code. The preStartup.js is responsible for identifying version mismatch. It throws an error if a migration is required and prevents api from running.

## Add the migration in api-migrations
- Add the new track to `api-migrations/migrator.config.js` following the existing syntax. The app looks at this file to pick the desired version and checks against the migrations table in the DB to identify the current version.
- Install the plugin package using `npm install @reactioncommerce/<api-plugin-package>`

These steps are explained in more detail [here](https://github.com/reactioncommerce/migrator#how-to-publish-a-package-with-migrations), and you can look at the [simple-authorization](https://github.com/reactioncommerce/plugin-simple-authorization) plugin code for an example to follow.

> IMPORTANT: If the plugin you added a migration to is one that is built in to the stock Reaction API releases, then at the same time you bump the plugin package version in https://github.com/reactioncommerce/reaction `trunk` branch, you must also update the data version in the trunk branch of `migrator.config.js` in this repo.

## Running migrations locally for development

- Fork/clone this repo.
- Check out the [tag](https://github.com/reactioncommerce/api-migrations/tags) that corresponds to your version of Reaction Platform. (Only 3.0.0 and higher are supported.)
- `npm install`
- Then to see a report of necessary migrations for your local MongoDB database and optionally run them:

```sh
MONGO_URL=mongodb://localhost:27017/reaction npx migrator migrate
```

Use a different `MONGO_URL` to run them on a different database.

Refer to [https://github.com/reactioncommerce/migrator](https://github.com/reactioncommerce/migrator) docs for other commands. Prefix them with `npx`.
- Try to start your API service. If there are database version errors thrown on startup, then change the versions or add/remove tracks in `migrator.config.js` as necessary based on whatever those errors are asking for. Then repeat the previous step. (If you've added new tracks, you'll need to `npm install` the latest version of those packages first.) Keep doing this until the API service starts.

## Migrating Deployment Databases

Option 1: You can follow the above "Local Development Usage" instructions but specify a remote database if you want. Migrations will run on your computer, which may not be very fast or reliable.

Option 2: You can set up a CI task for this repo:

- Create different configuration files for each deployed environment. For example, `migrator.config-staging.js` for the "staging" environment.
- Add the necessary `MONGO_URL`s to your CI environment/secrets.
- When config file changes are merged to the main branch, run `npx migrator migrate <env> -y` as a CI task with `MONGO_URL` set to the correct database for that environment. Do this for each Reaction environment (database) you have.
- Ensure that your CI Docker image uses at least the version of Node that's in the `.nvmrc` file.
