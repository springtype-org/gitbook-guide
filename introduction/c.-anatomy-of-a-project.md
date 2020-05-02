---
description: 'In fact, it''s just 3 to 4 files to really care about.'
---

# C. Anatomy of a project

Once you've generated your first SpringType project, you'll notice a few files and folders: 

![](../.gitbook/assets/anatomy.png)

All source files should be placed in the directory `src` and assets like images and fonts should be copied into the `static` directory. The `node_modules` folder contains JavaScript dependency packages installed via `yarn`/`npm` whereas the `dist` folder contains the latest production build and all its assets. 

{% hint style="info" %}
It's a good practice to Git `.ignore` the directories `node_modules` and `dist`. You should also consider excluding them from IDE code indexing for performance.
{% endhint %}

**Entrypoint: `src/index.html`**

This file contains references to JavaScript and CSS resources bundled by the SpringType build system and DevServer `st-start`. You can also set the project HTML title, meta tags and add additional resources like custom CSS and JavaScript. You don't need to add any references here manually. `st-start` will take care of everything auto-magically.

![src/index.html default content](../.gitbook/assets/index.png)

**Entrypoint: `src/index.tsx`**

This file is the entry point for your TypeScript and TSX code. Usually, this is just a simple SpringType component that renders a `<RouteList>` for DOM routing in multi-page setups. We're also importing the file `assets/theme.scss` here to apply global CSS styles.

![typical src/index.tsx structure](../.gitbook/assets/index_tsx.png)

**Node package config: `package.json`**

This file describes your project in terms of the [Node.js](https://nodejs.org/) / [npm](https://npmjs.com) / [yarn](https://yarnpkg.com/en/) ecosystem. [If that doesn't sound familiar, please read on here](https://nodejs.org/en/knowledge/getting-started/npm/what-is-the-file-package-json/).

A typical `package.json` file of a SpringType project looks like this:

![SpringType projects have a &quot;springtype&quot; dependency](../.gitbook/assets/package-json.png)

As you can see, we have three default **`npm` scripts**: `clean`, `start` and `start:prod`. 

* `clean` wipes the `dist` folder
* `start` runs the DevServer on `http://localhost:4444` by default
* `start:prod` creates a production build and stores it in the `dist` folder

The `devDependencies` are also managed by `st-start`. Using this architecture, you are autoritative for updating package versions, e.g. in security risk situations.

**TypeScript config: `tsconfig.json`**

This file contains the projects TypeScript configuration. There are a few requirements for any SpringType project that must be met:

![tsconfig.json compilerOptions requirements](../.gitbook/assets/tsconfig_requirements.png)

Usually there is no need to touch this file ever again after is has been created once. However, it's a good think to know [how it works](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html).

**Optional: `yarn.lock`**

In SpringType we favor the `yarn` package manager for `Node.js`/`npm` package management. You can out out and use `npm` instead. However, we'd not advice this because `npm` is still known to run into whole error classes that are unknown in yarn. The yarn.lock file makes sure that the same versions of npm packages are installed on every system when the project is checked out across more than one machines. [Read more about this here](https://classic.yarnpkg.com/en/docs/yarn-lock/).

**Optional: `st.config.ts`**

The `st-start` build system is a DevServer and likewise a tool for production builds of your project. Its behavior can be be configured in many ways. To do so, you'd just create a file called `st.config.ts` in the root directory of your project \(next to `package.json`\):

![An example st.config.ts that allows for custom SCSS transformations](../.gitbook/assets/stconfig_.png)

If you're interested to learn more about `st.config.ts`, you can find the documentation of all options available in the [`IBuildConfig` interface](https://github.com/springtype-org/st-start/blob/master/src/interface/ibuild-config.ts).

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

