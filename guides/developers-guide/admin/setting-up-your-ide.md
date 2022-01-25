# Setting up your IDE
## Overview

If you are interested in being able to quickly debug and develop Reaction, consider installing these useful tools to help you. Most of the core team uses [Visual Studio](https://visualstudio.microsoft.com/) but options like Sublime Text, Atom, Emacs are perfectly fine, just watch for similar code extensions like the ones below.

- You should use [nvm](https://github.com/creationix/nvm) to install and manage NodeJS.
- [Kadira](https://github.com/kadira-open/kadira-server) can help you optimize performance of a Meteor app.

You can debug Meteor server code using the `--inspect` flag in Chrome DevTools and within integrated developer environments like [Visual Studio Code (VS Code)](https://code.visualstudio.com/), a free code editor, and [WebStorm by JetBrains](https://www.jetbrains.com/webstorm/).

Here are the steps to get started using Reaction in `inspect` mode in any editor:

## Debugging 

Run `reaction-admin` with the `--inspect` flag, by running

```sh
npm run inspect
```

Once this process has started, Node opens a WebSocket to listen for a debugger on port 9229 by default. Once you've successfully attached a debugger, you'll see the Debugger attached message in your Terminal:

```sh
Debugger listening on ws://127.0.0.1:9229/...
```

After that, the application will run, You'll see the typical Open Commerce application logging:

```sh
=> Started your app.
=> App running at: http://localhost:3000/
```

Now, you're ready to debug!

### Inspecting with Chrome DevTools

1. Open Google Chrome and visit `chrome://inspect`.
2. Click **Open dedicated DevTools for Node**.
3. There are two main ways to set up breakpoints: in the DevTools or in the code.

- To set up a breakpoint in DevTools, open up the **Source** tab and navigate to a file you would like to debug in the left-side bar. Click on the line number where you'd like the code to stop executing. You can set up as many breakpoints as you'd like.
- To set up a breakpoint in your code, add the keyword `debugger` right before you'd like the application to pause execution.

Remember: Since you are currently debugging the Reaction server, you'll only have access to code that runs on the server - not the client.

4. Now open `http://localhost:3000` as you normally would and the code should stop executing at your first breakpoint.

5. At this breakpoint, you can access the Console by hitting <kbd>esc</kbd> and opening the _Drawer_.

Now you can watch variables, check the call stack and investigate scope to better debug while you develop.

To learn more about Chrome DevTools, check out Google's [Tools for Web Developers](https://developers.google.com/web/tools/chrome-devtools/javascript/).

### Inspecting in VS Code

_Note:_ You can only have one debugger connected at a time, so if you've already connected to DevTools, then you'll need to disconnect before you can connect with VS Code.

Setting up [VS Code](https://code.visualstudio.com/) and connecting it to the Node debugger is only slightly more complicated than using DevTools. But once it's set up, it can easily become a part of your regular workflow.

1. In the root of your project directory, add a `.vscode/launch.json` file.

2. Set up your file:

**.vscode/launch.json**

```js
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Reaction Server",
            "cwd": "${workspaceRoot}/",
            "runtimeExecutable": "${workspaceRoot}/.meteor/local/dev_bundle/bin/npm",
            "restart": true,
            "timeout": 30000,
            "stopOnEntry": false,
            "sourceMaps": true,
            "protocol": "inspector",
            "port": 9229
        }
    ]
}
```

This borrows heavily from a Meteor forum post on [Meteor 1.6 server debugging with VS Code](https://forums.meteor.com/t/meteor-1-6-server-debugging-with-vs-code/39821).

**Tip:** Port 9229 is the default Node inspector port, but if you switch to another one, eg. `--inspect=5000`, then you'll need to adjust the port in your `launch.json` file.

3. Open the debug panel and click the **Play** icon

Now you can debug without even leaving your code editor.

To learn more about debugging JavaScript with VS Code, check out [VS Code's debugging guide](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

### Inspecting in Webstorm

1. Create a new [Run/Debug configuration](https://www.jetbrains.com/help/webstorm/run-debug-configuration-javascript-debug.html) based on the Meteor default.

Use the following settings:

- Meteor executable: `/usr/local/bin/meteor`
- Program arguments: `--settings settings/dev.settings.json --raw-logs`
- Working directory: `/YourMachine/code/reaction`
- Environmental variables: `REACTION_EMAIL=youremail@gmail.com;REACTION_AUTH=...`


2. Select your breakpoints by clicking along the left-hand side line numbers.
3. Click on the **Debug** icon to start you Reaction app in debugger mode.
4. Use the **Step In**, **Step Out**, **Steop Over** buttons to navigate through the code.

For more on debugging with Webstorm, check out the [Jetbrains guide](https://www.jetbrains.com/help/webstorm/debugging-javascript-in-chrome.html).

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

