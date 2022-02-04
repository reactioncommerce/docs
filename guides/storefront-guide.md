## At a glance

Mailchimp Open Commerce (MOC) is a headless commerce platform, which means that its focus is on providing a top-notch server API rather than on UI. But this doesn't mean you're completely on your own.

For your storefront — your public-facing website or app on which consumers browse your catalog, manage their account, and purchase items — MOC assumes that you will build your own UI to meet your needs. For those who want to get going fast, MOC plans to provide example storefronts that you can use as starting points. These are UI projects that we expect you to fork and modify. Currently, there is one such project, the [example storefront](https://github.com/reactioncommerce/example-storefront).

However, if you already have a storefront UI or if the example projects are not to your liking, then you can connect any UI of your choosing to the MOC API. If this describes your organization, then this guide is for you. It will walk you through everything that is necessary to build or adapt a storefront to use MOC for its data.

## What you’ll need

In general, you'll want to do the following tasks in roughly this order:

1. Add and configure Apollo Client
2. Build a product listing page
3. Build a product detail page
4. Build navigation menus
5. Add a way to add an item to a cart
6. Build a cart page
7. Implement cart modification
8. Build a checkout page
9. Build an order view page
10. Add ability to log in
11. Build an account management page

This guide will walk you through how to complete these tasks in a general, framework-agnostic way. While we don't care which UI framework you use, which component libraries you use, or how you manage app state, we do have one recommendation that will save you time: 

- Use [Apollo Client](https://www.apollographql.com/docs/react/) to interact with the MOC GraphQL API when possible.

## Add and configure Apollo Client
To add Apollo Client to your UI app, read the [excellent Apollo docs](https://www.apollographql.com/docs/react/essentials/get-started.html). For local development, the MOC GraphQL endpoint `uri` is `http://localhost:3000/graphql`, but we recommend storing that value in app config where it can be set differently per environment.

For your test query, try this:

```js
import gql from "graphql-tag";

const testQuery = gql`{
  primaryShop {
    _id
    name
  }
}`;

client
  .query({ query: testQuery })
  .then(result => console.log(result));
```

> NOTE: If it doesn't work in your storefront UI code, try it directly in the GraphQL Playground at http://localhost:3000/graphql. If it works there, then check over your Apollo Client setup code again and check for any errors.

Assuming your test query works, you're ready to start building your storefront UI. You will eventually need to configure authentication, but most of a storefront UI can be built without authenticating, so we'll do that later.

## Build a product listing page
Product listing pages can vary among storefronts, but we'll focus on the one thing they all do: query the catalog for products with pagination.

Using the Apollo Client reactive querying mechanism for your UI framework (e.g., a React `Query` component), start with the following GraphQL query to get the data for a product list:

```graphql
query catalogItemsQuery($shopId: ID!, $first: ConnectionLimitInt, $last:  ConnectionLimitInt, $before: ConnectionCursor, $after: ConnectionCursor, $sortBy: CatalogItemSortByField, $sortByPriceCurrencyCode: String, $sortOrder: SortOrder) {
  catalogItems(shopIds: [$shopId], first: $first, last: $last, before: $before, after: $after, sortBy: $sortBy, sortByPriceCurrencyCode: $sortByPriceCurrencyCode, sortOrder: $sortOrder) {
    totalCount
    pageInfo {
      endCursor
      startCursor
      hasNextPage
      hasPreviousPage
    }
    edges {
      cursor
      node {
        _id
        ... on CatalogItemProduct {
          product {
            _id
            title
            slug
            description
            vendor
            pricing {
              compareAtPrice {
                displayAmount
              }
              displayPrice
            }
            primaryImage {
              URLs {
                small
              }
            }
          }
        }
      }
    }
  }
}
```

Use the variables below to start. As you add more features, such as sort and limit selectors and filters, you can dynamically generate the variables object based on UI state.

```
const variables = {
  shopId: "shop_id_here",
  sortBy: "created_at",
  sortOrder: "desc"
};
```

> NOTE: We assume that you know your external shop ID. You should store it in your UI app config, or for a multi-shop UI, get it from the URL. If you are stuck, try the `{ primaryShopId }` query in GraphQL Playground to get the primary shop ID, which should work for initial development purposes.

> NOTE: While we use the word "page" here for simplicity, we recommend designing your product list component so that it works well when there are multiple lists on a page. Often times a home page or a custom feature page will want to display multiple product lists, potentially each with their own pagination, on a single page.

## Add pagination

If you have read [Using the GraphQL API](../developers-guide/core/using-a-graphql-client.md) and the linked resources, you should be familiar with how pagination works in general. Here's a specific example of how to paginate the `catalogItemsQuery` for a product list page.

First, you're going to want a place in application state where your list paging and sorting is saved. For a web app, that's almost always going to be in the URL. That way, visiting a shared URL will show the same page of the list. We'll leave the state details up to you, but assuming you have a reactive way of obtaining URL state variables, a React query would look something like this:

```js
render() {
  const { shopId } = appConfig;

  const variables = {
    shopId,
    ...getPaginationVariablesFromUrl()
  };

  return (
    <Query errorPolicy="all" query={catalogItemsQuery} variables={variables}>
      {({ data, fetchMore, loading }) => {
        const { catalogItems } = data || {};

        const refetchCatalogItems = () => {
          fetchMore({
            variables: getPaginationVariablesFromUrl(),
            updateQuery: (previousResult, { fetchMoreResult }) => {
              const { [queryName]: items } = fetchMoreResult;

              // Return with additional results
              if (items.edges.length) {
                return fetchMoreResult;
              }

              // Send the previous result if the new result contains no additional data
              return previousResult;
            }
          });
        };

        return (
          <YourCustomListComponent
            catalogItems={catalogItems}
            isLoadingCatalogItems={loading}
            refetchCatalogItems={refetchCatalogItems}
          />
        );
      }}
    </Query>
  );
}
```

`getPaginationVariablesFromUrl` is a function that you would write, which will return `{ first, last, after, before, sortBy, sortOrder }` from some reactive state.

`YourCustomListComponent` would have either previous/next buttons or infinite scrolling, as well as "sort by" and "sort order" select lists, that would properly change the reactive state (e.g., update the URL query string) and then call `refetchCatalogItems`.

## Build a product detail page
A product detail page generally needs only one query for its data. For a more complex storefront, you may have additional queries to other systems for related data that appears on the page.

Here's a typical query to start with:

```graphql
query catalogItemProductQuery($slugOrId: String!) {
  catalogItemProduct(slugOrId: $slugOrId) {
    product {
      _id
      productId
      title
      slug
      description
      vendor
      isLowQuantity
      isSoldOut
      isBackorder
      pricing {
        displayPrice
      }
      media {
        URLs {
          thumbnail
          small
          medium
          large
          original
        }
      }
      tags {
        nodes {
          name
        }
      }
      variants {
        _id
        variantId
        title
        optionTitle
        pricing {
          compareAtPrice {
            displayAmount
          }
          displayPrice
        }
        canBackorder
        inventoryAvailableToSell
        isBackorder
        isSoldOut
        isLowQuantity
        options {
          _id
          variantId
          title
          pricing {
            compareAtPrice {
              displayAmount
            }
            displayPrice
          }
          optionTitle
          canBackorder
          inventoryAvailableToSell
          isBackorder
          isSoldOut
          isLowQuantity
          media {
            URLs {
              thumbnail
              small
              medium
              large
              original
            }
          }
        }
        media {
          URLs {
            thumbnail
            small
            medium
            large
            original
          }
        }
      }
    }
  }
}
```

The `slugOrId` variable will typically be a product slug from the URL. So for example you product list component would have a link somewhere on each list item, which would go to a `/product/:productSlug` route, where you would perform the above query passing the `productSlug` from the URL as the `slugOrId` variable.

From there, it is simply a matter of arranging the data on the page to match your storefront design. You will eventually want an "Add to Cart" button somewhere on the page, but we'll add that later.

## Build navigation menus
If you are building your storefront in the recommended order, at this point you have a product listing page and a product detail page. It's probably about time to add some navigation UI, a home page, and perhaps some additional pages.

You're free to add any additional pages you want using whatever method your router prescribes. A home page may be a specific view of a product list, multiple product lists, static content, or whatever your storefront design spec requires.

After you have created several types of pages, you're ready to add links to them in a navigation component. For a very simple storefront with few navigation links, you may want to design this as a static component. This may be easier in the short term, but remember that it will require a code change and redeployment every time navigation changes are needed.

Most storefronts require more complex and dynamic navigation menus. For this purpose, the MOC admin allows those with proper permissions to build navigation menus and then publish them to one or more storefronts.

On the storefront UI side, you only need to query for the navigation menu that you want when initially loading the UI. Then use that data to dynamically build whatever menu design you need.

You can get the default navigation tree for a shop when you query the shop, which you'll likely want to do on initial UI load anyway, in order to get other shop details for display.

```graphql
fragment NavigationItemFields on NavigationItemData {
  contentForLanguage
  classNames
  url
  isUrlRelative
  shouldOpenInNewWindow
}

query shop($id: ID!, $language: String! = "en") {
  shop(id: $id) {
    defaultNavigationTree(language: $language) {
      items {
        navigationItem {
          data {
            ...NavigationItemFields
          }
        }
        items {
          navigationItem {
            data {
              ...NavigationItemFields
            }
          }
          items {
            navigationItem {
              data {
                ...NavigationItemFields
              }
            }
          }
        }
      }
    }
    description
    name
  }
}
```

As you can see, the response is a tree with up to three levels. Your component should support rendering all three levels of navigation unless you have made a business decision that you will only have a certain number of levels.

Each navigation item has the following information:
- `contentForLanguage`: Display this as the content the shopper sees, e.g., the name of the page the navigation item links to. It will be in whatever language you requested with the `language` variable to your query.
- `classNames`: Optionally set the `className` property of the navigation item element to this. Your organization may choose not to implement this. It is available as a convenience if you need it.
- `url`: Use this as the navigation item link URL
- `isUrlRelative`: This will be `true` or `false`. Use this to build your navigation item component's click handling logic. Relative URLs may need to be handled internally by your router while absolute URLs could be handled in the normal browser way.
- `shouldOpenInNewWindow`: This will be `true` or `false`. Use this to build your navigation item component's click handling logic. If this is `true`, typically you would add `target="_blank"` attribute or do the equivalent in code.

## Add a way to add an item to a cart

In MOC, there are anonymous carts and account carts. Since we haven't added a way to log in to the storefront yet, we're going to work only with anonymous carts in this section.

In your UI, on product list items, the product detail page, or both, you will add a button, which usually says something like "Add to Cart". This button must invoke logic that decides which GraphQL mutation to use:
- If you already have an anonymous cart ID and token in your application state, call the `addCartItems` mutation.
- If you do not have an anonymous cart ID and token in your application state, call the `createCart` mutation.

Defining these mutations looks something like this:

```graphql
mutation createCartMutation($input: CreateCartInput!) {
  createCart(input: $input) {
    cart {
     ...CartFragment
    }
    incorrectPriceFailures {
      ...IncorrectPriceFailuresFragment
    }
    minOrderQuantityFailures {
      ...MinOrderQuantityFailuresFragment
    }
    token
  }
}

mutation addCartItemsMutation($input: AddCartItemsInput!) {
  addCartItems(input: $input) {
    cart {
      ...CartFragment
    }
    incorrectPriceFailures {
      ...IncorrectPriceFailuresFragment
    }
    minOrderQuantityFailures {
      ...MinOrderQuantityFailuresFragment
    }
  }
}

fragment CartFragment on Cart {
  _id
  email
  items {
    ...CartItemConnectionFragment
  }
  totalItemQuantity
}

fragment CartItemConnectionFragment on CartItemConnection {
  pageInfo {
    hasNextPage
    endCursor
  }
  edges {
    node {
      _id
      productConfiguration {
        productId
        productVariantId
      }
      attributes {
        label
        value
      }
      createdAt
      inventoryAvailableToSell
      isBackorder
      isLowQuantity
      isSoldOut
      imageURLs {
        thumbnail
      }
      price {
        displayAmount
      }
      priceWhenAdded {
        displayAmount
      }
      quantity
      subtotal {
        displayAmount
      }
      title
      productVendor
      variantTitle
      optionTitle
    }
  }
}

fragment IncorrectPriceFailuresFragment on IncorrectPriceFailureDetails {
  currentPrice {
    displayAmount
  }
  providedPrice {
    displayAmount
  }
}

fragment MinOrderQuantityFailuresFragment on MinOrderQuantityFailureDetails {
  minOrderQuantity
  quantity
}
```

If you call the `createCart` mutation and get a success response, store the `cart._id` and `token` from the response in persistent app state tied to the browser or device, for example, `localStorage` or a cookie. Then the next time "Add to Cart" is clicked, your logic will see the cart ID and token and choose to call the `addCartItems` mutation.

Regardless of whether you are creating a new cart with items or adding items to an existing cart, you must check for `incorrectPriceFailures` and `minOrderQuantityFailures` in the response. The server may have added only some items to the cart but been unable to add other items. If any items could not be added because you tried to add them at an incorrect price, there will be data in `incorrectPriceFailures` that you can use to show a message to the shopper. This can happen if a price changed after the page was loaded. If any items could not be added because you tried to add fewer than the minimum purchase quantity to the cart, there will be data in `minOrderQuantityFailures` that you can use to show a message to the shopper. They can then increment the quantity and attempt to add it again.

## Build a cart page

After a shopper has clicked "Add to Cart", they are going to want to now see their cart. This could be a sidebar component, a modal, or a full page. Either way, the data loading is similar. You will do one query for cart data, with pagination on the `cart.items` connection.

```graphql
query anonymousCartByCartIdQuery($cartId: ID!, $token: String!, $itemsAfterCursor: ConnectionCursor) {
  cart: anonymousCartByCartId(cartId: $cartId, token: $token) {
    items(first: 20, after: $itemsAfterCursor) {
      ...CartItemConnectionFragment
    }
  }
}
```

Typically we recommend an infinite scrolling style pagination for cart items. When the user is scrolling and nears the bottom of the list, refetch this query with `itemsAfterCursor` variable set to the `cursor` from the last item's edge.

## Implement cart modification

Now that a shopper can view their cart, they'll likely want to change it. They already have the ability to add additional items to it, but at some point they'll want to change the quantity for an item or remove it entirely. We'll implement those actions in this task. There are additional cart mutations that will happen during a checkout flow, which we'll implement later.

## Change the quantity for a cart item

The quantity change mutation is usually invoked by clicking a quantity increment or decrement button or entering or clicking a specific quantity. Regardless of what you do for the UI, all you need to do is figure out what new quantity the shopper wants, and then pass it to the `updateCartItemsQuantity` mutation.

```graphql
mutation updateCartItemsQuantityMutation($input: UpdateCartItemsQuantityInput!) {
  updateCartItemsQuantity(input: $input) {
    cart {
      ...CartFragment
    }
  }
}
```

Where the `input` variable looks like this:

```js
{
  cartId, // from application state
  items: [
    { cartItemId, quantity }
  ],
  token // from application state
}
```

`cartItemId` is the `item._id` and `quantity` is the new desired quantity, which must be an integer of 0 or greater. A quantity of `0` removes the item, but we recommend calling the `removeCartItems` mutation instead.

## Remove a cart item

Removing a cart item is usually done by clicking a "Remove" button on the cart item UI. This should invoke the `removeCartItems` mutation.

```graphql
mutation removeCartItemsMutation($input: RemoveCartItemsInput!) {
  removeCartItems(input: $input) {
    cart {
      ...CartFragment
    }
  }
}
```

Where the `input` variable looks like this:

```js
{
  cartId, // from application state
  cartItemIds: [],
  token // from application state
}
```

`cartItemIds` is an array of IDs from `item._id`.

## Build a checkout page

It's time to support checking out a cart. You have a lot of freedom in how you design your checkout flow. We'll present one suggested flow here, but it will not work for everyone. Keep in mind the following guideline:

> NOTE: At the end of a checkout flow, your goal is to place a valid order using the `placeOrder` GraphQL mutation. This can be done without ever even having a cart! The cart exists as an in-progress or potential order and nothing more. After you create the order, you delete the related cart.

So as you collect information during checkout, you must decide where to store it until the order is placed. Some information can be stored on the cart by mutating it. Some you may want to store in `localStorage` or a cookie. Other sensitive information you may want to store only in memory and have the shopper re-enter it if they refresh or navigate away. Some related data you may even store directly in custom or third-party systems using their own APIs. As long as you can gather all the necessary information when it's time to call `placeOrder` in your UI client code, MOC does not care where it comes from.

In the following sections, we'll assume that you have an anonymous cart with some items in it and the user has clicked a "Checkout" button somewhere in the UI. You navigate to a checkout page, on which you will implement all of these checkout steps.

Generally speaking, a checkout flow consists of a flow controller (some code that decides what step you are on and which steps are complete and incomplete) and several checkout step components. This could happen across more than one page if you prefer, and for many payment methods, part of the flow consists of being redirected to an external checkout page and then coming back and picking up where you left off.

> NOTE: Ensure that you've enabled anonymous checkout in the shop settings on your server. Even if you don't plan to enable anonymous checkout, it's usually easiest to start by building anonymous checkout. We'll later add support for logging in, at which point you'd just need to force the user to log in anytime during the checkout flow, prior to placing the order. After you have all of that working, you can disable anonymous checkout in the shop settings on your server.

Please refer to the [documentation on building a checkout path](../docs/building-a-checkout-path.md) for more detailed information.

## Build an order view page

After you successfully place an order at the end of a checkout flow, you'll typically want to display that final order to the shopper as a sort of confirmation page. We recommend building this as a generic "order view" page, which can serve as a confirmation/thank you page as well as a page to link to from order emails, where the shopper can view the current order, shipment tracking information, order status, and more.

If you're following along creating your storefront UI in the recommended order, then you haven't added the ability to log in yet. Just as with carts, when you create an anonymous order (any order that is placed without authentication in the request header) you get back a token that you'll need to access that order again in the future. It isn't always necessary to store that token anywhere, but you can keep it in temporary or persistent application state if you need to.

For our purposes, we'll assume that upon successfully creating an order, you will redirect to your order view page with both the order `referenceId` and the `token` in the URL. When loading the page, you'll use the `orderByReferenceId` GraphQL query to get the order data.

```graphql
query orderByReferenceId($id: ID!, $shopId: ID!, $token: String) {
  order: orderByReferenceId(id: $id, shopId: $shopId, token: $token) {
    _id
    account {
      _id
    }
    email
    fulfillmentGroups {
      _id
      data {
        ... on ShippingOrderFulfillmentGroupData {
          shippingAddress {
            address1
            address2
            city
            company
            country
            fullName
            isCommercial
            phone
            postal
            region
          }
        }
      }
      items {
        nodes {
          _id
          imageURLs {
            thumbnail
          }
          isTaxable
          optionTitle
          parcel {
            containers
            distanceUnit
            height
            length
            massUnit
            weight
            width
          }
          price {
            displayAmount
          }
          productConfiguration {
            productId
            productVariantId
          }
          productSlug
          productType
          productVendor
          productTags {
            nodes {
              name
            }
          }
          quantity
          subtotal {
            displayAmount
          }
          title
          variantTitle
        }
      }
      selectedFulfillmentOption {
        fulfillmentMethod {
          displayName
        }
        handlingPrice {
          displayAmount
        }
        price {
          displayAmount
        }
      }
      summary {
        fulfillmentTotal {
          displayAmount
        }
        itemTotal {
          displayAmount
        }
        surchargeTotal {
          displayAmount
        }
        taxTotal {
          displayAmount
        }
        total {
          displayAmount
        }
      }
      type
    }
    payments {
      _id
      amount {
        displayAmount
      }
      billingAddress {
        address1
        address2
        city
        company
        country
        fullName
        isCommercial
        phone
        postal
        region
      }
      displayName
      method {
        name
      }
    }
    referenceId
    totalItemQuantity
  }
}
```
## Add ability to log in

After you've implemented a storefront that allows anonymous shopping and checkout, you'll most likely want to add the ability to log in. For details, see [Developer Concepts: Users & Authentication](../developers-guide/concepts/users-and-authentification.md).

Actual implementation will vary depending on whether you're creating a single page app, an app with a server, a native app, or something else. For the purposes of this guide, we're just going to assume that you have an authentication flow working.

### Authenticating GraphQL requests

A successful login flow will result in your application being given an access token. You should store this access token either in a cookie or in `localStorage` such that you retain it until it expires or until the user logs out.

You then need to adjust your Apollo Client initialization code to pass the access token as the `Authorization` header with all requests. Refer to [their example code](https://www.apollographql.com/docs/react/recipes/authentication.html#Header).

### Silent reauthentication

If an access token has expired, you'll see a `401 Unauthorized response` for the next GraphQL request after the expiration. Anytime you see such a response, you should first attempt to silently reauthorize the token with the identity provider server. If that fails, you should clear the access token from wherever you store it and display the UI as if they are not logged in. You may also want to show a temporary message to explain that their session has expired and they'll need to log in again.

With Apollo Client, you can use Apollo Link and code similar to the following to watch for 401 errors and attempt silent reauthentication.

```js
import { onError } from "apollo-link-error";

const STATUS_UNAUTHORIZED = 401;

const errorLink = onError(({ graphQLErrors, networkError }) => {
  if (networkError) {
    const errorCode = networkError.response && networkError.response.status;
    if (errorCode === STATUS_UNAUTHORIZED) {
      // If a 401 Unauthorized error occurred, redirect to /signin on the IDP host.
      // This will re-authenticate the user without showing a login page and a new token is issued.
      window.location = IDP_SIGN_IN_URL;
      return;
    }
    console.error("Unable to access the GraphQL API. Is it running and accessible from the Storefront UI server?");
  }
});
```

This code will work in a browser. If your UI has a server handling URL requests, the idea is similar but you'll do something like a `302` redirect with the `Location` set to the `IDP_SIGN_IN_URL`.

### Cart reconciliation

When a user logs in and every time your app loads, you must attempt to load either an anonymous cart or an account cart. The logic looks something like this:

1. If we are logged in, use the `accountCartByAccountId` GraphQL query to get the account cart.
2. Check for an anonymous cart ID and token in your persistent app state. If you find a cart there and you are not logged in, use the `anonymousCartByCartId` GraphQL query to get it.

You may find that you now have both an anonymous cart and an account cart for the current shop. This will not do. When this happens, a client is expected to reconcile the carts as soon as possible using the `reconcileCarts` mutation. Reconciliation should be quick and nearly unnoticeable to the user, but depending on your needs, you may choose to prompt the shopper to decide how to reconcile instead of guessing what they want.

`reconcileCarts` has 3 available modes: `merge`, `keepAnonymousCart`, and `keepAccountCart`.
- `merge` is the default mode, where the anonymous cart is combined with the account cart, items are deduplicated, and quantities are incremented to match the combined quantity of the items in the carts.
- `keepAnonymousCart` will keep only the items and the checkout information in the anonymous cart.
- `keepAccountCart` will keep only the items and the checkout information in the account cart.

After the server has reconciled the carts as instructed, regardless of which mode you choose, it will have deleted the anonymous cart. Only a single account cart remains. At this time you should delete the anonymous cart ID and token from persistent application state, thus “forgetting” it. If you then log out, you will have no cart for that shop until you add another item or log back in.

```graphql
mutation reconcileCartsMutation($input: ReconcileCartsInput!) {
  reconcileCarts(input: $input) {
    cart {
      ...CartFragment
    }
  }
}
```

With `input` variable similar to this:

```js
// All values come from application state or config.
// There is no need to provide the account cart ID because an account
// may only have one cart and the server will find it based on who is
// authenticated.
{
  anonymousCartId,
  anonymousCartToken,
  shopId
}
```

To avoid confusing your user, we recommend hiding all cart data and showing loading state until you've finished the full get-and-reconcile logic on app startup or login.
## Build an account management page

After you've added the ability to log in, most storefronts also need an account management page, or multiple pages. Here your shoppers with accounts can update their profile information, their stored addresses, and their stored payment details, as well as view, track, and cancel their orders.

Here is a list of GraphQL queries you'll likely need:
- `viewer`
- `ordersByAccountId`

And mutations:
- `addAccountAddressBookEntry`
- `updateAccountAddressBookEntry`
- `removeAccountAddressBookEntry`

## More resources 

[Vue Storefront](https://www.vuestorefront.io/)
[Vercel Commerce](https://github.com/vercel/commerce)