---
description: "A quick introduction to SpringType. Let's say: \"Hello, world!\" \U0001F603"
---

# Getting Started

## What is SpringType? ****[🚀](https://emojipedia.org/rocket/)

SpringType is a web framework for Website, PWA and App development. It promotes the use of TypeScript and modern browser APIs. Novel algorithms allow for **less complexity**, **fewer code**, **higher performance** and an improved **developer experience**.

While designing SpringType, we've re-invented almost all wheels. It comes with a **lazy VDOM**, uses **native Web Components**, **Dependency Injection**, **Change Detection**, **Shared Contexts** and **TypeScript StyleSheets \(TSS\)** to fix all the headaches in modern web development.

### Peek a boo [🤓](https://emojipedia.org/nerd-face/)

We've created a neat CodeSandbox example based on the `st-starter-web` project template:

{% embed url="https://codesandbox.io/s/github/springtype-org/st-starter-web?autoresize=1&fontsize=12&hidenavigation=1" caption="\"Hello, world!\" in SpringType" %}

{% hint style="warning" %}
**Cannot edit in read-only editor?**  
Please note that you need to login to [codesandbox.io](https://codesandbox.io) to edit the example code.
{% endhint %}

### The numbers [📊](https://emojipedia.org/bar-chart/)

As you can see, we've just built a website using modern core web technologies like Web Components and VDOM in **&lt; 10 KiB**. It renders in less than **&lt; 0.1ms** and \(re\)builds in **&lt; 1ms.**

### Checkout locally [💻](https://emojipedia.org/personal-computer/)

To try SpringType right now on your machine, open a terminal and run:

```text
$ git checkout https://github.com/springtype-org/st-starter-web .
```

Switch to the project directory and install the few dependencies:

```text
$ cd st-starter-web
$ yarn
```

Finally, start the dev server by running:

```text
$ yarn start
```

And that's it. SpringType will start on `http://localhost:4444` 

{% hint style="info" %}
**That was easy, wasn't it?!** Let's make some changes to `src/index.tsx` and watch how HMR will auto-magically reload the entry web component [👌](https://emojipedia.org/ok-hand-sign/).
{% endhint %}

