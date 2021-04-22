<p align="center">
  <a href="https://mailchimp.com/developer/">
    <img src="https://raw.githubusercontent.com/mailchimp/mailchimp-client-lib-codegen/master/resources/images/mcdev-banner.png" alt="Mailchimp Developer" width="100%" height="auto">
  </a>
</p>

# Mailchimp Open Commerce Docs

The open source documentation repo for [Mailchimp Open Commerce](https://mailchimp.com/developer/open-commerce/) (formerly Reaction Commerce).

## Custom Syntax

All docs should be valid Markdown. In addition to this, we use some specialized syntax to allow for embedding custom components in the documentation at [https://mailchimp.com/developer/open-commerce/docs/](https://mailchimp.com/developer/open-commerce/docs/). These custom syntaxes are documented below.

### Code snippets

If a vanilla code block receives a valid ([GFM-flavored](https://docs.github.com/en/github/writing-on-github/creating-and-highlighting-code-blocks#syntax-highlighting)) language as well as a `title` param, e.g.:

<pre>
```js title=Example
console.log('hello world');
```
</pre>

Will be rendered on the docs site as follows:

![Custom code block](docs/_assets/readme-custom-code-block.jpg)

Code blocks without a language and title, e.g.:

<pre>
```
testing 1 2 3
```
</pre>

Will be rendered as follows:

![Vanilla code block](docs/_assets/readme-vanilla-code-block.jpg)

Note: Currently, the title param cannot contain any spaces, though periods, hyphens, and some other punctuation is allowed. This is a limitation of MDX, specifically in the way the metastring is parsed. See [this issue](https://github.com/mdx-js/mdx/issues/702) in the MDX repo for more context.

### Note blocks

If a blockquote begins with `**Note**: `, e.g.:

<pre>
> **Note**: hello there
</pre>

it will be rendered as follows:

![Custom note block](docs/_assets/readme-custom-note-block.jpg)

Other blockquotes, e.g.:

<pre>
> Hello there
</pre> 

will be rendered normally:

![Vanilla block quote](docs/_assets/readme-vanilla-block-quote.jpg)

## Contributing

Contributions to this repo are welcome. Please submit an issue or pull request if you feel any information is incomplete or inaccurate.
