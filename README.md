---
description: 'A quick introduction to SpringType. Let''s say "Hello, world!"'
---

# Getting Started

## What is SpringType?

SpringType means simply powerful Website, PWA and App development. It promotes the use of TypeScript and modern browser API's. Novel algorithms allow for less complexity, less code, higher performance and a better developer experience.

### Peek a boo

We've created a neat CodeSandbox example based on the `st-starter-web` project template:

{% embed url="https://codesandbox.io/s/github/springtype-org/st-starter-web?autoresize=1&fontsize=12&hidenavigation=1" caption="\"Hello, world!\" in SpringType" %}

{% hint style="warning" %}
**Cannot edit in read-only editor?**  
Please note that you need to login to [codesandbox.io](https://codesandbox.io) to edit the example code.
{% endhint %}

### The numbers

As you can see, we've just built a website using modern core web technologies like Web Components and VDOM in **&lt; 10 KiB**. It renders in less than **&lt; 0.1ms** and \(re\)builds in **&lt; 1ms.**

### Checkout locally

To try SpringType right now on your machine, open a terminal and run:

```text
$ git checkout https://github.com/springtype-org/st-starter-web .
```

Switch to the project directory and install the few dependencies:

```text
$ cd st-starter-web
$ yarn
```

Finally you start the dev server it comes with just by running:

```text
$ yarn start
```

And that's basically it. SpringType will start on `http://localhost:4444` and show "Hello, world!".

{% hint style="info" %}
**That was easy, wasn't it?!** Let's make some changes to `src/index.tsx` and watch how hot module replacement will auto-magically reload the entry web component.
{% endhint %}

