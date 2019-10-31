---
description: "A quick introduction to SpringType. Let's say: \"Hello, world!\" \U0001F603"
---

# Getting Started

## What is SpringType? ****[🚀](https://emojipedia.org/rocket/)

SpringType is a full-stack web framework for Website, PWA and App development. It promotes the use of TypeScript and modern browser and Node.js APIs. Novel algorithms allow for **less complexity**, **fewer code**, **higher performance** and a stellar **developer experience**.

{% hint style="warning" %}
Please note that SpringType is currently in **beta phase.** We're looking forward to release the **first stable 1.0.0 GA in Spring 2020** [🌱](https://emojipedia.org/seedling/)🚀😎. As of now, bugs may happen more often, API's may change at will and CLIs may break on platforms at times as we're still working to increase test coverage and automated CI pipelines.

Also, some features already promoted here, are not yet fully implemented:

* [ ] SpringType Server \(Node.js, high-performance micro services\) 
* [ ] Full SSR support \(based on SpringType Server\) and VDOM re-hydration
* [ ] Reactive Programming / Streaming \(RxJS alternative impl.\)
* [ ] Redux-compatible immutable state machine \(Redux alternative impl.\)
* [ ] High-performance Bundler/Compiler \(Webpack & Babel alternative\)
* [ ] Browser Developer Extensions for Google Chrome & Mozilla Firefox
{% endhint %}

While designing SpringType, we've re-invented almost every wheel. **SpringType is Typed**, so you get **auto-completable API**'s and **compile-time type safety without any runtime overhead**. It uses **Virtual  Components** that are rendered as a **clean DOM**, emit **Custom DOM Events** and allow for **Custom DOM Attributes**. It is all DOM standards based, so there is **no magic abstraction**.

Our **Virtual DOM** is HTML and SVG-compatible \(no nasty attribute names\), allows for **non-VDOM subtrees** \(your old JS code is compatible\) and implements `<slot>` , `<fragment>` && `<template>` **without the need for web components**. 

Oh, and it **re-hydrates the existing DOM**, so you can also use all your components **with HTML only** and **server-side rendering \(SSR\)** is no issue at all. 

But can it handle **Style Encapsulation**? Yes, using auto-prefixing CSS selectors and strong conventions. And it supports **PostCSS out-of-the-box** at compile time, so there is **no runtime overhead as well.**

Nice, but can we have support for **DI**, **Change Detection \(by reference and deep\)** and a **Context API** too? Sure, it is reactive as well. And on top of that you get an **i18n API,** a **DOM Router**, **scoped** **globalThis** and a great **Logging API** for free.

My oh my, this sounds like another fatty JS framework? Not at all! **Typical app builds are &lt; 10 KiB gzipped** and **render in &lt; 0.1ms** \(even with the worst Google Lighthouse settings\)**.**

**Uh, and**  `st-start` transpiles TypeScript and JavaScript into **optimized production builds** with **true zero configuration**. It comes with a clever dynamic webpack configuration and seamlessly reloads codes changes in development mode \(**HMR**\).

{% hint style="info" %}
 **Does it support `$feature`?** Ask us here: [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

