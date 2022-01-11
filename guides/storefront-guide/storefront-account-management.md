#  Storefront UI Development 
## Build an account management page

### Index
This article is part of the [Storefront UI Development Guide](./storefront-intro.md).
- Previous Task: [Add ability to log in](./storefront-login.md)
- Next Task: [Show inventory status badges](./storefront-inventory-status-badges.md)

### Overview
After you've added the ability to log in, most storefronts also need an account management page, or multiple pages. Here your shoppers with accounts can update their profile information, their stored addresses, and their stored payment details, as well as view, track, and cancel their orders.

Here is a list of GraphQL queries you'll likely need:
- `viewer`
- `ordersByAccountId`

And mutations:
- `addAccountAddressBookEntry`
- `updateAccountAddressBookEntry`
- `removeAccountAddressBookEntry`

