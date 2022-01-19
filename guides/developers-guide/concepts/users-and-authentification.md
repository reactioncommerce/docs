# Authentication

## How to Get an Access Token for Development

For development purposes, you'll want to try out GraphQL requests in GraphQL Playground or another GraphQL client. The normal way of getting OAuth access tokens is by going through an OAuth flow from a browser or native client, but this is a lot of extra work when you just want to test some requests on your local computer.

You can get the access token on GraphQL Playground by running the below mutation
```
mutation authenticate($serviceName: String!, $params: AuthenticateParamsInput!) {
  authenticate(serviceName: $serviceName, params: $params) {
    tokens {
      accessToken
    }
  }
}
```
Make sure you have a running instance of Open Commerce to view GraphQL playground on http://localhost:3000/graphql

You will need the below query variables as input for the mutation. Paste this in the *QUERY VARIABLES* tab in the bottom left of the playground.
```
{
  "serviceName": "password",
  "params": {
    "user": {
      "email": "<user-email>"
    },
    "password": "<hashed-password>"
  }
}
```

The password should be a SHA-256 hashed password. You can use this [online tool for hashing](https://emn178.github.io/online-tools/sha256.html).

Once you run this mutation you can run other queries and mutation from the playground using the access token. Copy the `accessToken` from the result and paste it in the *HTTP HEADRES* tab in the playground.
```
{
    "Authorization": "<accessToken from above>"
}
```

## Express Middleware

The `api-plugin-authentication` provides authentication middleware for the Reaction API. Utils included in this plugin help connect the Account-js library and the Reaction API via an Auth Token.

### How it works

This plugin registers the schema and resolvers that are required by account-js.

Whenever a header with name `Authorization` is present in the request, this plugin uses account-js to get the user for that token. Then the user is attached to the `req` object.

Also on each request, `accountsGraphQL.context` is called so that it can pass the `user` to the "internal context" of account-js.

### Supported actions

1. Signup
2. Login
3. Change Password
4. Forgot Password - A email is sent with a reset token url of the form `https://<storefront-url>/?resetToken=<token>`

### Environment variables for api-plugin-authentication
- STORE_URL
- TOKEN_SECRET