---
description: A few words on the structure of the framework.
---

# Bits and Pieces

### [️](https://emojipedia.org/gear/)Core [⚙](https://emojipedia.org/gear/)

The frameworks core package wraps up implementations for basic architectural needs:

* [Globals](https://github.com/springtype-org/springtype/tree/master-v2/src/core/st) `st`
* [Language Support](https://github.com/springtype-org/springtype/tree/master-v2/src/core/lang) `lang`
* [Logging](https://github.com/springtype-org/springtype/tree/master-v2/src/core/log) `log`
* [Change Detection](https://github.com/springtype-org/springtype/tree/master-v2/src/core/cd) `cd`
* [Dependency Injection](https://github.com/springtype-org/springtype/tree/master-v2/src/core/di) `di`
* [State Management](https://github.com/springtype-org/springtype/tree/master-v2/src/core/state) `state`
* [Global Context Management](https://github.com/springtype-org/springtype/tree/master-v2/src/core/context) `context`
* [Internationalization](https://github.com/springtype-org/springtype/tree/master-v2/src/core/i18n) `i18n`

These API's are available in all execution contexts, say Server \(Node.js\) and Web \(browsers\).

### Web [🌐](https://emojipedia.org/globe-with-meridians/)

This package contains all implementations necessary for modern web development:

* [Web Components / Custom Elements](https://github.com/springtype-org/springtype/tree/master-v2/src/web/customelement) `customelement`
* [VDOM](https://github.com/springtype-org/springtype/tree/master-v2/src/web/vdom) `vdom`
* [Client-side DOM Router](https://github.com/springtype-org/springtype/tree/master-v2/src/web/router) `router`
* [Typed StyleSheets \(TSS\)](https://github.com/springtype-org/springtype/tree/master-v2/src/web/tss) `tss`

### Server [🎛](https://emojipedia.org/control-knobs/)

To complete the feature-set and allow developers to use the same APIs full-stack, SpringType comes with a powerful, server-implementation agnostic Node.js server middleware:

* T.B.A

### Bundle [📦](https://emojipedia.org/package/)

Finally, to transpile and bundle all assets of a frontend or backend application, the bundle-package uses FuseBox to create high performance production builds and have a blazing fast developer experience with hot module replacement and in-memory code caching.

