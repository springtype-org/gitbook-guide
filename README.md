---
description: "A quick introduction to SpringType. Let's say: \"Hello, world!\" \U0001F603"
---

# Getting Started

## What is SpringType? ****[🚀](https://emojipedia.org/rocket/)

SpringType is a full-stack web framework for Website, PWA and App development. It promotes the use of TypeScript and modern browser and Node.js APIs. Novel algorithms allow for **less complexity**, **fewer code**, **higher performance** and an improved **developer experience**.

While designing SpringType, we've re-invented almost all wheels. SpringType uses **native Web Components**, comes with a **lazy VDOM**, **Dependency Injection**, **Change Detection**, **Shared Contexts** and **TypeScript StyleSheets \(TSS\)** to fix all the headaches in web development.

**The novel Bundler/Compiler** `stbc` transpiles TypeScript and JavaScript blazingly fast into optimized production builds and seamlessly reloads codes changes in development mode.

### The numbers [📊](https://emojipedia.org/bar-chart/)

As you can see, we've just built a website using modern core web technologies like Web Components and VDOM in **&lt; 10 KiB**. It renders in less than **&lt; 0.1ms** and \(re\)builds in **&lt; 1ms.**

### Checkout locally [💻](https://emojipedia.org/personal-computer/)

To try SpringType right now on your machine, open a terminal and run:

```text
git checkout https://github.com/springtype-org/st-starter-web .
```

Switch to the project directory and install the few dependencies:

```text
cd st-starter-web
yarn
```

Finally, start the dev server by running:

```text
yarn start
```

And that's it. SpringType will start on `http://localhost:4444` 

{% hint style="info" %}
**That was easy, wasn't it?!** Let's make some changes to `src/index.tsx` and watch how HMR will auto-magically reload the entry web component [👌](https://emojipedia.org/ok-hand-sign/).
{% endhint %}

