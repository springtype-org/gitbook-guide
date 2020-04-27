---
description: A collection of common errors that might happen during development.
---

# Common Errors

## Error: listen EADDRINUSE: address already in use 127.0.0.1:4444

There is another port of `st-start` running on your system. Because of that, the port `4444` is already in use. You can either stop the other process or create a file `st.config.ts` with the following code:

{% code title="st.config.ts" %}
```typescript
import { setConfig } from "st-start";

setConfig({
    port: 4445 /* or any other free port you desire */
});
```
{% endcode %}

## 



{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

