---
description: "This is a quick introduction. We're just saying: \"Hello, world!\" here \U0001F603"
---

# Welcome to SpringType!

SpringType is a full-stack web framework for Website and PWA development. It promotes the use of TypeScript and modern browser APIs. SpringType's unique approach allows for **less complexity**, **fewer code**, **higher performance** and a stellar **developer experience**.

### Why we've built SpringType

In recent years, we've seen many frameworks rise and fall. The only thing that seems to be constant, is the change. While projects like React, Angular and Vue became large code bases, non-framework approaches like Svelte and Stencil are promoting approaches to inlining functionality but limiting flexibility.

There are three things that each of those frameworks have in common:

1. **Complexity**: Expect a steep learning curve to be productive
2. **Abstraction**: With VDOM's you can't safely work with real DOM API's anymore 
3. **Vendor lock-in**: Due to their non-standard API's code is not inter-operable

With SpringType, we're breaking these limitation, while providing a state-of-the-art but more low-level API, allowing for component-driven development and safe, real DOM API access at the same time. 

In SpringType, we give you back the power: You decide what happens and when.

{% hint style="warning" %}
Please note that SpringType is currently in **beta phase.** We're looking forward to release the **first stable 1.0.0 GA in Spring 2020** [🌱](https://emojipedia.org/seedling/)🚀😎. As of now, bugs may happen more often, API's may change at will and CLIs may break on platforms at times as we're still working to increase test coverage and automated CI pipelines.

[Please help us becoming better and report use-cases, suggestions and bugs.](https://github.com/springtype-org/springtype/issues)

Get in touch with us on  [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

### Framework Design Principles and Goals 

Designing SpringType was quite a journey. It took us more than a year to constantly re-invent the API's until we've came to what we have now and we think, is a beautiful approach to modern web development. Here are some highlights:

#### TypeScript first

**SpringType is typed**, so you get **auto-completable API's** and **compile-time type safety without any runtime overhead**. 

#### Components with .tsx

It almost feels like React, but you'll notice that we use **standard HTML markup**. So you can easily copy & paste HTML code, without any hassle. However, there are SpringType **Virtual  Components.**

**Web Component features without shadow DOM**

We like standards. But we've missed `<slot>` , `<fragment>` and `<template>`in non-shadow DOM scopes. With SpringType you can use standard HTML5 web component tags and they work as expected.

**Component references**

No need to pass or select components. You can just reference them with `@ref`. 

**Debugging like back in the day - if you like**

Every DOM element, that has a relation to a component references the component via `$stComponent`. Inspecting component state from within a DOM inspector has never been easier.

**Simple component lifecycle**

Feels and looks like React, but without a complex lifecycle. In SpringType there is not much to learn. Most of the time you'll just attach a `.tsx` or implement the  `render()` method. 

**Safe native-DOM API access**

You can use all DOM API's safely because SpringType doesn't do magic in the background. You control the framework, not vice versa. Your UX designer asks you to use a jQuery plugin? There is no VDOM that overrides dynamic DOM changes - so go ahead if you like! 

**Local state, change detection and data-binding**

We've that based on the standard Proxy API. You can add `@state` to any class property to activate it and handle state transitions in `onStateChange()`. 

**Shared state**

You can share states between classes. When it mutates, it mutates everywhere. This is a simplified, non-global approach to application state management.

**Event bus**

Instead of bubbling events through component and DOM trees, you can use our blazing-fast, memory-efficient publish/subscribe event bus for application-wide evening.

**CSS, CSS modules, Sass, PostCSS**

Just as you know it from React.

**Assets**

Import images and other files just as you know it from React.

**i18n**

Don't worry about complex i18n issues anymore. This is our simplified approach on i18next's API's.

**Scaffolding**

You're faster with creating that file in your IDE. 

**Dependency hell**

SpringType has 0 runtime dependencies.

**DOM API shortcuts**

SpringType promotes the use of standard DOM API's, but some are cumbersome. So, we've introduced simple API's like `st.hide()` and `st.show()` - but there's no magic.

**Bundling**

Yes, we're using webpack and babel under the hood. Our bundler is called `st-start` because it starts your project at runtime. Oh, and this is a reimplementation of `create-react-app`, basically. We have all their features, so it feels like React again.

**No VDOM?**

We had one, but we threw that out. Now we're free of magic and complexity. We're even writing less code and the performance increased in real-world apps. Why? Many automatic re-renders that happened based on state changes doesn't happen anymore. Now we have to actively trigger DOM mutations, but we only do that when it's visually needed. 

{% hint style="info" %}
 **Does it support `$feature`?** Ask us here: [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

