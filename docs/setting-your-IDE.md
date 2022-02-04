## The basics

If you are interested in being able to quickly debug and develop Mailchimp Open Commerce, consider installing these useful tools to help you.

- You should use [nvm](https://github.com/nvm-sh/nvm) to install and manage NodeJS.
- Visual Studio Code is a preferred text editor but you can use Sublime Text, Emacs, Vim or any other editor of your choice.

## Browser Extensions

### Chrome

- [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
- [Apollo Client Developer Tools](https://chrome.google.com/webstore/detail/apollo-client-developer-t/jdkknkkbebbapilgoeccciglkfbmbnfm)
- [MobX Developer Tools](https://chrome.google.com/webstore/detail/mobx-developer-tools/pfgnfdagidkfgccljigdamigbcnndkod)
- [Altair GraphQL Client](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja)
- [Meteor DevTools](https://chrome.google.com/webstore/detail/meteor-devtools/ippapidnnboiophakmmhkdlchoccbgje)
- [z-context](https://chrome.google.com/webstore/detail/z-context/jigamimbjojkdgnlldajknogfgncplbh)

### Firefox

- [Altair GraphQL Client](https://addons.mozilla.org/en-US/firefox/addon/altair-graphql-client/)
- [React Developer Tools](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)

## Code Editor Extensions

### [Visual Studio Code](https://code.visualstudio.com/)

- [Auto Import](https://marketplace.visualstudio.com/items?itemName=steoates.autoimport)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
- [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- [Document This](https://marketplace.visualstudio.com/items?itemName=joelday.docthis)
- [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [GraphQL](https://marketplace.visualstudio.com/items?itemName=kumar-harsh.graphql-for-vscode)
- [JavaScript (ES6) Code Snippets](https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets)
- [Jest Snippets Standard Style](https://marketplace.visualstudio.com/items?itemName=shtian.jest-snippets-standard)
- [npm](https://marketplace.visualstudio.com/items?itemName=eg2.vscode-npm-script)
- [npm Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Prettify Selected JSON](https://marketplace.visualstudio.com/items?itemName=vthiery.prettify-selected-json)
- [Promise Snippets](https://marketplace.visualstudio.com/items?itemName=progre.promise-snippets)
- [ReactJS Code Snippets](https://marketplace.visualstudio.com/items?itemName=xabikos.ReactSnippets)
- [Remark](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-remark)
- [Simple React Snippets](https://marketplace.visualstudio.com/items?itemName=burkeholland.simple-react-snippets)
- [Commitizen Support](https://marketplace.visualstudio.com/items?itemName=KnisterPeter.vscode-commitizen)
- [styled-components](https://marketplace.visualstudio.com/items?itemName=jpoissonnier.vscode-styled-components)
- [Parinfer](https://marketplace.visualstudio.com/items?itemName=shaunlebron.vscode-parinfer)
- [cljfmt](https://marketplace.visualstudio.com/items?itemName=pedrorgirardi.vscode-cljfmt)

### Emacs

 - [js2-mode](https://melpa.org/#/js2-mode)
 - [rjsx-mode](https://melpa.org/#/rjsx-mode)
 - [prettier-js](https://melpa.org/#/prettier-js)
 - [flycheck](https://melpa.org/#/flycheck)
 - [json-mode](https://melpa.org/#/json-mode)
 - [graphql-mode](https://melpa.org/#/graphql-mode)
 - [markdown-mode](https://melpa.org/#/markdown-mode)
 - [dockerfile-mode](https://melpa.org/#/dockerfile-mode)
 - [yaml-mode](https://melpa.org/#/yaml-mode)
 - [editorconfig](https://melpa.org/#/editorconfig)

### Vim

- [ale](https://github.com/w0rp/ale)
- [vim-instant-markdown](https://github.com/suan/vim-instant-markdown)
- [vim-javascript](https://github.com/pangloss/vim-javascript)

## MongoDB IDEs

- [Studio 3T or Robo 3T](https://robomongo.org/)

## Standalone GraphQL Clients

- [GraphQL Playground](https://github.com/prismagraphql/graphql-playground)
- [Altair GraphQL Client](https://altair.sirmuel.design/)
- [GraphiQL Standalone App](https://github.com/skevy/graphiql-app)

## Troubleshooting
due to misconfiguration, missing dependencies, operating system differences, and software bugs. Here are some tips for diagnosing and fixing these issues.

### Docker Issues

These are potential issues you might encounter when running Reaction within a local Docker environment using Docker Compose and the docker-compose.yml file.

### Memory errors or errors about "Meteor rawLogs"

Make sure that you are allowing Docker sufficient memory to run. In your Docker preferences, we suggest adjusting the `Memory` setting to allow at least `3.0Gb`, and the `Swap` setting to allow at least `1.5Gb`. If you are running many containers, make these as high as possible as long as it doesn't negatively affect the performance of your computer.

