## At a glance

There are two main routes to getting started with Mailchimp Open Commerce: installing the platform on your local computer or on a server. This guide will focus on the former, which will allow you to explore the main features of Open Commerce.

In this guide, we’ll set up a full local instance of the Open Commerce platform, including core plugins provided by Mailchimp. We’ll walk through cloning and starting the Open Commerce development platform, registering an account, and creating your first shop. 

## What you’ll need

  * [Docker](https://www.docker.com/)

  * [Docker Compose](https://docs.docker.com/compose/)

  * Windows users: [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and [Docker for WSL](https://docs.docker.com/docker-for-windows/wsl/)

## Clone and start the platform

The simplest way to get an instance up and running on your local machine is by using the [development platform](https://github.com/reactioncommerce/reaction-development-platform), a set of `make` scripts that coordinates all of the different pieces of the Open Commerce software system. 

To set up the Open Commerce development platform, navigate to a Unix or WSL directory and run:

```bash
git clone https://github.com/reactioncommerce/reaction-development-platform.git
cd reaction-development-platform
make
```    
    
> **Note**: On Windows, you may need to replace `make` with `sudo make`. Then run `chown -R` on the `reaction-development-platform` directory to set your user as the owner of all the files it contains.

Behind the scenes, the `make` process clones all of the relevant Open Commerce software repositories, sets up each environment, and pulls, builds, and starts each Docker container. 

When `make` completes, five services will be running on `localhost`:

  * Open Commerce API (port 3000), including the [core plugins](/developer/open-commerce/docs/fundamentals/#plugins). This service also contains the GraphQL playground at `localhost:3000/graphql`.

  * [Example Storefront](https://github.com/reactioncommerce/example-storefront) (port 4000), which is built with [Next.js](https://nextjs.org/).

  * [Admin dashboard](https://github.com/reactioncommerce/reaction-admin) (port 4080), used to manage shop settings, accounts, products, and orders.

  * [Identity](https://github.com/reactioncommerce/reaction-identity) (port 4100), which handles Open Commerce customer and shop manager accounts.

  * [Hydra OAuth2 Server](https://github.com/reactioncommerce/reaction-hydra) (port 4444), which can connect to external user accounts.

> **Note**: You can manage individual pieces of the system by changing directories into a subproject and running Docker compose commands (e.g., `cd reaction-admin && docker-compose logs -f`).

## Access the dashboard, playground, and storefront

After the `make` command finishes provisioning the system, you can create a shop manager account for your local instance. 

First, visit the Open Commerce login page at `localhost:4080`. Click **Register** and enter your email address and create a password, which will grant you admin privileges.

Next, log in with your new account to access the shop creation form. Enter a name for your shop and click the **Create Shop** button. You should now see the Open Commerce admin dashboard, from which you can [create products](/developer/open-commerce/docs/creating-organizing-products/), [add tags](/developer/open-commerce/docs/tags-navigation/), and [manage your orders](/developer/open-commerce/docs/fulfilling-orders/).

Once you've created a shop, you can visit the GraphQL playground at `localhost:3000/graphql`, where you can run test GraphQL queries and mutations. In the **Docs** tab of the playground, you can view the complete Open Commerce GraphQL API reference. 

You can also view your Open Commerce storefront at `localhost:4000`.

## Next steps

Now that you’re up and running, you can start managing your store by creating products or building a custom storefront on top of the GraphQL API. The local instance also provides everything you need to code your own plugins for use with Open Commerce; for more information, see the [Build an API Plugin](/developer/open-commerce/guides/build-api-plugin/) guide.

When you’re done exploring Open Commerce, navigate to your `reaction-development-platform` directory and run `make stop`. This ensures that Docker properly saves all your data for the next time you want to run the system. To restart the system, simply run `make` again in the same directory.
