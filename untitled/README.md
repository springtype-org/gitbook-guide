---
description: SpringType's powerful but concise API.
---

# In Depth

### [️](https://emojipedia.org/gear/)Core [⚙](https://emojipedia.org/gear/)

The frameworks core package wraps up implementations for basic architectural needs:

* [Globals](https://github.com/springtype-org/springtype/tree/master-v2/src/core/st) `st` Implements the `st` and `$st` global framework API.
* [Language Support](https://github.com/springtype-org/springtype/tree/master-v2/src/core/lang) `lang` Helpful shortcut functions like `isPrimitive()`.
* [Logging](https://github.com/springtype-org/springtype/tree/master-v2/src/core/log) `log` Log using `st.info(...)`, `st.warn(...)` and `st.error(...)`.
* [Change Detection](https://github.com/springtype-org/springtype/tree/master-v2/src/core/cd) `cd` Proxy based object change detector, exposed as `st.onChange(...)`. 
* [Dependency Injection](https://github.com/springtype-org/springtype/tree/master-v2/src/core/di) `di` Class-property based DI, exposes: `@inject(...)` and `@injectable(...)`. 
* [Global Context Management](https://github.com/springtype-org/springtype/tree/master-v2/src/core/context) `context` Change Detection based global contexts, exposes `@context(...)`, `initContext(...)` and `getContext(...)`. 
* [Internationalization](https://github.com/springtype-org/springtype/tree/master-v2/src/core/i18n) `i18n` Allows for easy JSON-file based translations. Exposes: `@translation(...)`, `@formatter(...)`, `st.t(...)`. 

These API's are available in all execution contexts, say Server \(Node.js\) and Web \(browsers\).

{% hint style="info" %}
**Are you missing something?** Propose an API here: [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

### Web [🌐](https://emojipedia.org/globe-with-meridians/)

This package contains all implementations necessary for modern web development:

* [Component](https://github.com/springtype-org/springtype/tree/master-v2/src/web/component) `component`
* [Virtual DOM renderer](https://github.com/springtype-org/springtype/tree/master-v2/src/web/vdom)   `vdom`
* [Client-side, DOM Router](https://github.com/springtype-org/springtype/tree/master-v2/src/web/router) `router`

### Bundler [📦](https://emojipedia.org/package/)

Finally, to transpile and bundle everything that belongs to a SpringType project, `st-start` was written. It comes with a customizable API, a CLI, live reload DevServer and creates high performance production builds.

