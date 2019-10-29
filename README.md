---
description: "A quick introduction to SpringType. Let's say: \"Hello, world!\" \U0001F603"
---

# Getting Started

## What is SpringType? ****[🚀](https://emojipedia.org/rocket/)

SpringType is a full-stack web framework for Website, PWA and App development. It promotes the use of TypeScript and modern browser and Node.js APIs. Novel algorithms allow for **less complexity**, **fewer code**, **higher performance** and a stellar **developer experience**.

While designing SpringType, we've re-invented almost every wheel. **SpringType is Typed**, so you get **auto-completable API**'s and **compile-time type safety without any runtime overhead**. It uses **Virtual  Components** that are rendered as a **clean DOM**, emit **Custom DOM Events** and allow for **Custom DOM Attributes**. It is all DOM standards based, so there is **no magic abstraction**.

Our **Virtual DOM** is HTML and SVG-compatible \(no nasty attribute names\), allows for **non-VDOM subtrees** \(your old JS code is compatible\) and implements `<slot>` , `<fragment>` && `<template>` **without the need for web components**. 

Oh, and it **re-hydrates the existing DOM**, so you can also use all your components **with HTML only** and **server-side rendering \(SSR\)** is no issue at all. 

But can it handle **Style Encapsulation**? Yes, using auto-prefixing CSS selectors and strong conventions. And it supports **PostCSS out-of-the-box** at compile time, so there is **no runtime overhead as well.**

Nice, but can we have support for **DI**, **Change Detection \(by reference and deep\)** and **Shared Memory** too? Sure, it is reactive as well. And on top of that you get an **i18n API,** a **DOM Router**, **scoped** **globalThis** and a great **Logging API** for free.

My oh my, this sounds like another fatty JS framework? Not at all! **Typical app builds are &lt; 10 KiB gzipped** and **render in &lt; 0.1ms** \(even with the worst Google Lighthouse settings\)**.**

**Uh, and**  `st-start` transpiles TypeScript and JavaScript into **optimized production builds** with **true zero configuration**. It comes with a clever dynamic webpack configuration and seamlessly reloads codes changes in development mode \(**HMR**\).

Finally, if you're as lazy as we are, **don't even read any guide, just scaffold** your apps and **play with the apps and components** `st-create`generates for you:

`$ npx st-create`

 _You'll probably notice that there is not much to learn though :-\)_

* [x] Your next awesome app starts here.
  * [x] It supports many **project templates** \(PWA, website, game, etc.\) 
  * [x] It also supports your **own custom templates**
* [x] You can also **generate new components** for your app
  * [x] With templates for **standard UI's like: Login form** etc.
  * [x] In all flavors, like **Ionic**, **Google** **Material Design** and even **Bootstrap**

Did we mention that SpringType comes with cool third-party framework integrations?

* [x] Ionic 4 \(official\) for PWA's
* [x] Google Material Design \(official\) for Websites
* [x] Babylon 3D for Games
* [x] OpenLayers for interactive Map Applications

This gives you a **jump-start that has never been seen before**. Have fun! 

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

