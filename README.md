---
description: "A quick introduction to SpringType. Let's say: \"Hello, world!\" \U0001F603"
---

# Getting Started

## What is SpringType? ****[🚀](https://emojipedia.org/rocket/)

SpringType is a full-stack web framework for Website, PWA and App development. It promotes the use of TypeScript and modern browser and Node.js APIs. Novel algorithms allow for **less complexity**, **fewer code**, **higher performance** and a stellar **developer experience**.

While designing SpringType, we've re-invented almost all wheels. SpringType uses **Virtual  Components** that render a **clean DOM**,  emit **Custom Events** and allow for **Custom Attributes**.

Our  **Virtual DOM** is HTML and SVG-compatible \(no nasty attribute names\), allows for **non-VDOM subtrees** \(old JS code is compatible!\), implements `<slot>` , `<fragment>` and `<template>` **without web components**. 

Oh, and it **re-hydrates the existing DOM**, so you can also use all your components **with HTML only** and **server-side rendering \(SSR\)** is no headache at all. 

But guys, can it handle **Style Encapsulation**? Yes, using strong conventions. And it supports **PostCSS out-of-the-box** at compile time, so there is **no runtime overhead as well.**

Nice, but can we have support for **DI**, **Change Detection \(by reference and deep\)** and **Shared Memory** as well? Yes, including reactiveness. And on top of that you get an **i18n API,** a **DOM Router**, **scoped** **globalThis** and a great **Logging API** for free.

My oh my, is this another fatty JS framework? Not at all! **Typical app builds are &lt; 10 KiB gzipped** and  **render in &lt; 0.1ms,** even with the worst Google Lighthouse settings**.**

**Uh, and**  `st-start` transpiles TypeScript and JavaScript into **optimized production builds** with **true zero configuration**. It comes with a clever dynamic webpack configuration and seamlessly reloads codes changes in development mode \(**HMR**\).

Finally, if you're as lazy as we are, **don't even read any guide, just scaffold** your apps and **learn with fun and by playing around** using `st-create`: _\(You'll notice there is not much to learn\)_

`$ npx st-create`

* [x] Your next awesome app starts here.
  * [x] It supports many project templates \(PWA, website, game, etc.\) 
  * [x] It also supports your **own custom templates**
* [x] You can also **generate new components** for your app
  * [x] With templates for standard UI's like: Login form etc.
  * [x] In all flavors, like **Ionic**, **Google** **Material Design** and even **Bootstrap**

Did we mention that SpringType comes with official third-party integrations?

* [x] Ionic 4 \(official\) for PWA's
* [x] Google Material Design \(official\) for Websites
* [x] Babylon 3D for Games
* [x] OpenLayers for interactive Map Applications

This gives you a **jump-start that never has been seen before**. Have fun! 

### Checkout a simple demo manually [💻](https://emojipedia.org/personal-computer/)

To try SpringType without the CLI, just open a terminal and run:

```text
git checkout https://github.com/springtype-org/st-starter-web .
```

Switch to the project directory and install the few dependencies:

```text
cd st-starter-web
yarn
```

Finally, start the dev server for HMR by running:

```text
yarn start
```

And that's it. SpringType will start on: `http://localhost:4444` 

Or start a production build by running:

```text
yarn start:prod
```

Find your production build in: `./dist/index.html`

{% hint style="info" %}
**That was easy, wasn't it?!** Let's make some changes to `src/index.tsx` and watch how HMR will auto-magically reload [👌](https://emojipedia.org/ok-hand-sign/).
{% endhint %}

