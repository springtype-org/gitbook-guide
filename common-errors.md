---
description: A collection of common errors that might happen during development.
---

# Common Errors

### Error: listen `EADDRINUSE`: address already in use `127.0.0.1:4444`

There is another process of `st-start` running on your system. Because of that, the port `4444` is already in use. You can either stop the other process or create a file `st.config.ts` in the project folder next to `package.json` with the following code:

{% code title="st.config.ts" %}
```typescript
import { setConfig } from "st-start";

setConfig({
    port: 4445 /* or any other free port you desire */
});
```
{% endcode %}

### Class `${className}` does not extend from `st.component`

Your component definition probably looks like this:

```typescript
import { component } from "springtype/web/component";

@component
export class ComponentClassName {
  ...
}
```

But it should look like this \(extending from `st.component`\):

```typescript
import { component } from "springtype/web/component";
import { st } from "springtype/core";

@component
export class ComponentClassName extends st.component {
  ...
}
```

### The attribute `${className}.${attrName}` is `DOM_TRANSPARENT` and thus has a bad naming. It should be like: `${attrNameLowerCase}`

This warning happens when there is a code like this:

```typescript
import { st } from "springtype/core";
import { AttrType } from "springtype/web/component/interface";
import { component, attr } from "springtype/web/component";

@component
export class ComponentClassName extends st.component {

  ...
   
  // this attribute shines throuh in DOM
  @attr(AttrType.DOM_TRANSPARENT)
  attrName: string;
```

The `attrName` should be named lowercase, such as: `attrname` because in HTML, attributes are  lowercase. If we'd name our attributes camelCase, we the DOM would automatically name them lowercase and when you try to read the attribute from the DOM, it might result in errors.







{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

