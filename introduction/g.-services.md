---
description: Implementing re-usable and reactive business logic
---

# G. Services

{% tabs %}
{% tab title="1. Services" %}
**Implementing business logic**

Services are meant to implement business logic. Services are usually singleton instances of classes that are shared across a whole application. A service e.g. implements network request/response handling, data validation and handles logical decision making. 

To communicate and interact with the rest of the application - namely the components, a service can:

* Be injected \(DI\) in components and other services
* Send and receive messages \(events\)
* Be attached to Redux store fields
* Declare class-local and shared contexts

**Service implementation**

Writing a service in SpringType is simple:

{% code title="src/demo/service/random-service.ts" %}
```typescript
@service
export class RandomService extends st.service {

  getNewRandom(): number {
    return Math.random();
  }
}
```
{% endcode %}

**Using a Service with dependency injection**

To use a Service like this inside of components or other Services, you just need to inject it like this:

```typescript
@component
export class RandomComponent extends st.component {

  @inject(RandomService)
  randomService: RandomService;

  render() {
    return <p>{ this.randomService.getNewRandom() }</p>
  }
}

st.render(<RandomComponent />, document.body);
```
{% endtab %}

{% tab title="2. Reactive Event Bus" %}
TODO
{% endtab %}

{% tab title="3. Reactive Context States" %}
TODO
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

