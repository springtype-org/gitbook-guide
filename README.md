---
description: 'Short introduction on SpringType, it''s motivation and how to quickly dive-in.'
---

# Introduction

## What is SpringType?

SpringType is a next-generation frontend framework to build web user interfaces for PWA's, websites, apps and alike. It promotes the use of TypeScript and modern browser API's.

Primary design goals of SpringType are: Simplicity, Minimalism, Performance and Developer Experience. But for now, we stop talking and try to impress you with a real project.

### Peek a boo

We've created a neat CodeSandbox example based on the starter-scratch project template:

{% embed url="https://codesandbox.io/s/github/springtype-org/st-starter-web?autoresize=1&fontsize=12&hidenavigation=1" caption="\"Hello, world!\" in SpringType" %}

{% hint style="info" %}
Please note that you need to login on [codesandbox.io](https://codesandbox.io) to edit the example code.
{% endhint %}

### Dive-in locally

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
That was easy! Let's make some changes to `src/index.tsx` and watch how hot module replacement will auto-magically reload the entry web component.
{% endhint %}

