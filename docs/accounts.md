## Overview

Account is the main document used to authenticate and authorize users in Open Commerce.

### Types

The type `Account` is used to fetch and create accounts.

Other keys related to product are the following:

- `addressBook`: stores the address for the user
- `adminUIShops`: list of shops that this account has UI access to
- `currency`: user's preferred currency
- `emailRecords`: list of emails associated with this user
- `preferences`: JSON storing the user's prefrences
- `primaryEmailAddress`: the primary email address
- `groups`: The authorization groups the user is part of.

### Fetching Account

Returns the account with the provided ID

#### query - account(id: ID!): Account

#### type - Account

```
_id: ID!
addressBook(...): AddressConnection
adminUIShops: [Shop]
bio: String
createdAt: DateTime!
currency: Currency
emailRecords: [EmailRecord]
firstName: String
language: String
lastName: String
metafields: [Metafield]
name: String
note: String
picture: String
preferences: JSONObject
primaryEmailAddress: Email!
updatedAt: DateTime
userId: String!
username: String
groups(...): GroupConnection
```

### Fetching multiple Accounts

Returns accounts optionally filtered by account groups

#### query - accounts(groupIds: [ID], notInAnyGroups: Boolean, after: ConnectionCursor, before: ConnectionCursor, first: ConnectionLimitInt, last: ConnectionLimitInt, offset: Int, sortOrder: SortOrder = asc, sortBy: AccountSortByField = createdAt): AccountConnection!

#### type - AccountConnection

```
edges: [AccountEdge]
nodes: [Account]
pageInfo: PageInfo!
totalCount: Int!
```

### Adding email to Account

Add an email address to an account

#### mutation - addAccountEmailRecord(input: AddAccountEmailRecordInput!): AddAccountEmailRecordPayload

#### type - AddAccountEmailRecordPayload

```
account: Account
clientMutationId: String
```

### Adding an account

Create an account based off a user

#### mutation - createAccount(input: CreateAccountInput!): CreateAccountPayload

#### type - CreateAccountPayload

```
account: Account
clientMutationId: String
```

### Updating an account

Update account fields

#### mutation - updateAccount(input: UpdateAccountInput!): UpdateAccountPayload

#### type - UpdateAccountPayload

```
account: Account!
clientMutationId: String
```
