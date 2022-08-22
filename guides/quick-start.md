## At a glance

There are two main routes to getting started with Mailchimp Open Commerce: installing the platform on your local computer or on a server. This guide will focus on the former, which will allow you to explore the main features of Open Commerce.

In this guide, we’ll set up a full local instance of the Open Commerce platform, including core plugins provided by Mailchimp. We’ll walk through cloning and starting the Open Commerce development platform, registering an account, and creating your first shop.

## What you’ll need


- We recommend installing [nvm](https://github.com/nvm-sh/nvm)
- [14.18.1 ≤ Node version < 16](https://nodejs.org/ja/blog/release/v14.18.1/)
- [Git](https://git-scm.com/)
- [Yarn](https://yarnpkg.com/cli/install) (for the storefront)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

- Windows users: [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and [Docker for WSL](https://docs.docker.com/docker-for-windows/wsl/)

In addition you need to have your system setup for [SSH authentication with GitHub](`https://docs.github.com/en/authentication/connecting-to-github-with-ssh`)

## Install the CLI

`npm install -g reaction-cli`

## Install the API Server

- Create a directory for your entire project using `mkdir myproject` and then `cd` into it
- Create an API server by running `reaction create-project api <myapiserver>`, _you can substitute any directory 
  name for `<myapiserver>`_
- Change directory into your newly created server directory and run `npm install`
- Once this is complete run `reaction develop`. This will start the Open Commerce server in development mode.
- Congrats you have installed the Mailchimp Open Commerce API server. You can now proceed to install the Storefront 
  and Admin applications. You can view the GraphQL playground locally `http://localhost:3000/graphql`

## Install the Storefront

- Open a new terminal window
- Change to the root of the project directory you created above
- Execute `reaction create-project storefront <mystorefront>` (like above you can name this directory whatever you like)
- Change directory into the newly created storefront by doing `cd <mystorefront>` (using whatever name you have it 
  above)
- Now run `yarn install` to install the dependencies
- Then run `reaction develoop` to start the storefront in development mode
- Congratulations, you have installed the default storefront for Mailchimp Open Commerce. You can access the 
  storefront from http://localhost:4000

## Install the Reaction Admin
- Open a new terminal window
- Change to the root of the project directory you created above
- Execute `reaction create-project admin <myadmin>` (like above you can name this directory whatever you like)
- Change into your newly created directory by doing `cd <myadmin>`
- Now run `npm install`
- Then run `reaction develop` which will start the Admin in development mode (Note: The admin can take a little 
  while to start up the first time because it's in Meteor)
- Once the server has started you can access it by going to `http://localhost:4080`

## Access the dashboard, playground, and storefront

Now that you have the entire Mailchimp Open Commerce project running locally, you can create a shop manager account for your local instance.

First, visit the Admin interface at `localhost:4080`. On the top right, click the user profile image. Click **Create Account** and enter your email address and create a password, which will grant you admin privileges.

Enter a name for your shop and click the **Create Shop** button. You should now see the Open Commerce admin dashboard, from which you can [create products](/developer/open-commerce/docs/creating-organizing-products/), [add tags](/developer/open-commerce/docs/tags-navigation/), and [manage your orders](/developer/open-commerce/docs/fulfilling-orders/).

Once you've created a shop, you can visit the GraphQL playground at `localhost:3000/graphql`, where you can run test GraphQL queries and mutations. In the **Docs** tab of the playground, you can view the complete Open Commerce GraphQL API reference.

You can also view your Open Commerce storefront at `localhost:4000`.

## Next steps

Now that you’re up and running, you can start managing your store by creating products or building a custom storefront on top of the GraphQL API. The local instance also provides everything you need to code your own plugins for use with Open Commerce; for more information, see the [Build an API Plugin](/developer/open-commerce/guides/build-api-plugin/) guide.

Once you are done working with any of the servers you can stop then by either pressing Ctrl+C or by simply closing 
the terminal window.
