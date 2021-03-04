# Hello, world!

Code snippet:

```json title=foo.json
{"foo": "bar"}
```

```js title=foo-bar.js
const foo = 3;

const bar = (props) => {
  if (foo === 4) {
    return null;
  }
}

console.log('hii');
```

```graphql title=query.gql
# Query
{
  order {
    shop {
      _id
    }
  }
}
```

```graphql title=query.gql
# Response
{
  "data": {
    "order": {
      "shop": {
        "_id": "abcd"
      }
    }
  }
}
```

```graphql title=query.gql
{
  order {
    shop {
      _id
    }
  }
}

# Response
{
  "data": {
    "order": {
      "shop": {
        "_id": "abcd"
      }
    }
  }
}
```

```graphql title=query.gql
# Query
{
  order {
    shop {
      _id
    }
  }
}

# Response
{
  "data": {
    "order": {
      "shop": {
        "_id": "abcd"
      }
    }
  }
}
```

```graphql title=query.gql
# Query
{
  order {
    shop {
      _id
    }
  }
}

{
  "data": {
    "order": {
      "shop": {
        "_id": "abcd"
      }
    }
  }
}
```

## note section

Blockquote note:

> **Note**: hello there

Blockquote multi line note:

> **Note**: This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.