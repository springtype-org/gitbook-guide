---
description: Understanding the SpringType dependency injection system.
---

# Dependency Injection

{% tabs %}
{% tab title="1. Injectables" %}
Unlike React, we've implemented some standard features, e.g. for Inversion of Control. But unlike Angular, we don't need complex module configurations.

**Dependency Injection \(DI\)**

Dependency injection has a functional API and an object-oriented API. It is automatically available for Components and Services by default, but can be enabled for arbitrary classes too. Lets start with how we can make any class injectable:

{% code title="crypto-util.ts" %}
```typescript
@injectable
export class CryptoUtil {

    hash(token: string, length: number = 64) {
        const hash = sodium.crypto_generichash(length, sodium.from_string(token));
        return sodium.to_hex(hash);
    }
}
```
{% endcode %}

**Injecting non-injectable classes**

TODO
{% endtab %}

{% tab title="2. Functional API" %}
**Functional API**

Now if you'd like to share a single instance of this service across your application and re-use it in many functions and components, you could either use the functional interface:

```typescript
const cryptoUtil = st.inject(CryptoUtil);
cryptoUtil.hash('test');
```
{% endtab %}

{% tab title="3. Injection hierarchy" %}
**Injection trees**

Injectable can be injected in injectables. Be careful with cyclic dependencies though.

{% code title="auth-service.ts" %}
```typescript
@injectable
export class AuthService {

    @inject(CryptoUtil)
    cryptoUtil: CryptoUtil;
}
```
{% endcode %}
{% endtab %}

{% tab title="4. Custom factory functions" %}
**Factory and custom factory functions**

There are rare cases where you'd want the injection to create a new instance of a class for each injection point:

```typescript
@injectable(InjectionStrategy.FACTORY)
class FooFactory {
	constructor(public name: string) {}
}
```

In rare cases, you might need full control over the way an instance is created:

```typescript
@injectable(
  InjectionStrategy.FACTORY, 
  () => new FooFactoryFunction("factory")
)
class FooFactoryFunction extends FooFactory {}
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

