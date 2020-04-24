---
description: "This is a quick introduction to SpringType: \"Hello, world!\" \U0001F603"
---

# Welcome to SpringType!

SpringType is a full-stack web framework for Website and PWA development. It promotes the use of TypeScript and modern browser APIs. SpringType's unique approach allows for **less complexity**, **fewer code**, **better performance** and a stellar **developer experience** [🚀](https://emojipedia.org/rocket/).

### Mile Zero

We recommend, to take like 30 minutes to run through our guide. You can follow the guide with hands-on experience by running a single CLI command:

```bash
> npx st-create -p guide
```

{% hint style="warning" %}
SpringType just requires [N](https://nodejs.org)[ode.js](https://nodejs.org) to be installed first. We're proud to say that SpringType has 0 dependencies \(but quite a lot `devDependencies`because we're using Webpack, Babel & many plugins to optimize developer experience\). 
{% endhint %}

### SpringType in 30 minutes 

Designing SpringType was quite a journey. It took us more than a year to constantly re-invent the API's until we've came to the current version where the API is small, concise, simple and efficient. Simply put:

> A beautiful new take on modern web development that brings back the fun and simplicity of the old days - while retaining modern web development standards.

{% tabs %}
{% tab title="TypeScript" %}
SpringType is a typed superset of JavaScript. You'll get auto-completable API's and compile-time type safety without any runtime overhead:

```typescript
interface IGreeting {
    text: string;
    mode: EGreetingMode;
}

enum EGreetingMode {
    PUSH_NOTIFICATION,
    MODAL
}

const sendGreeting = async(greeting: IGreeting) => { ... }
```

{% hint style="info" %}
**Use a popular IDE**

We recommend  to work with an IDE like [VS Code](https://code.visualstudio.com/) or [IntelliJ WebStorm](https://www.jetbrains.com/webstorm/) to name two popular ones.
{% endhint %}

TypeScript allows us to use the TSX domain-specific language. TSX is typed JSX and JSX is an XML-syntax to describe a virtual DOM \(HTML\) structure which is being transformed into JSON by the TypeScript compiler. 

SpringType therefore takes this JSON structure, optimizes it and renders out a valid native HTML document at high speed. But there is no complex VDOM algorithm and no magic DOM abstraction in SpringType to wrap your head around:

```typescript
import { st } from "springtype";
import { tsx } from "springtype/web";

st.render(<p>SpringType: Simplicity is key! :-)</p>, document.body);
```
{% endtab %}

{% tab title="Components" %}
SpringType's TSX supports **standard HTML markup**. It feels like React, but with SpringType you can easily copy & paste HTML code.

You can render with one simple function call: 

```typescript
st.render(<p>Hello, SpringType! </p>, document.body);
```

But it's way cooler to define components, so you can re-use code like this. So we do that like everybody does today in React, Angular or Vue. Just, without much abstraction  and with native DOM API access. **SpringType gives you back control and promotes simplicity.**

**Functional API**

In SpringType you can choose what you think is the best of both worlds. Functional API's look like this:

```typescript
// simplified functional component, just renders what is given
const Container = component(
  render((container: Component) => {
    return <div>{container.renderChildren()}</div>;
  })
);
st.render(<Container>Hello! :-)</Container>, document.body);
```

Or if you need some local state context: 

```typescript
const Clock = component((clock: Component) => {
  
  let now = Date.now();
  
  const updateUnixTime = () => {
    now = Date.now();
    st.renderPartial(clock.timeDisplay, now);
  });
  
  return () => (
    <fragment>
      <button onClick={updateUnixTime}>Show time</button>
      <p ref={{ timeDisplay: clock }}>{now}</p>
    </fragment>
  );
});
st.render(<Clock />, document.body);
```

**Object-oriented API**

Attributes are like properties in React. But you can type them easily and they can be set transparent to the DOM, so they would appear in the DOM as HTML attributes. The `render()` method returns `tsx`:

{% code title="error-message.tsx" %}
```typescript

interface IErrorMessageAttrs {
  message: string;
}

@component
export class ErrorMessage extends st.component<IErrorMessageAttrs> {

  @attr
  message: string;
  
  render() {
      return <p>{ this.message }</p>;
  }
}
```
{% endcode %}

**How do you use this component?** 

Either inside another component's `tsx` \(default\), or portal-like, somewhere in an existing DOM structure:

```typescript
st.render(<ErrorMessage message="Happy error :)" />, document.body);
```

**Refs and native DOM access**

TSX is internally transformed into HTML elements. While rendering, we know which element goes where. So if you want to get hold of some DOM element, you can just ask the renderer to create a reference for you:

```typescript
@component
export class ErrorMessage extends st.component<IErrorMessageAttrs> {

    ...
    
    @ref
    messageElementRef: HTMLElement;
    
    render() {
      return <p ref={{ messageElementRef: this }}>
          { this.message }
      </p>;
    }
    
    onAfterRender() {
        this.messageElementRef.classList.add('black');
    }
}
st.render(<ErrorMessage message="Happy error :)" />, document.body);
```

This is like fishing in the DOM, and allows you to access DOM elements directly. There is **no complex DOM API abstraction** to wrap your head around.

**CSS, CSS modules, Sass and Less**

You can easily import CSS, CSS as CSS modules \(auto-generates class names, scoped to your component\), Sass \(SCSS\) and Less files. They are auto-compiled and integrated, inlined and optimized with zero configuration:

{% code title="error-message.tsx" %}
```typescript
// simple import (global, without CSS modules)
import "error-message.scss";

// with CSS module support
import * as styles from "error-message.scss";

...

render() {
    return <p class={styles.black}></p>
}

...

// or without CSS modules

...

render() {
    return <p class="black"></p>
}

...
```
{% endcode %}

{% code title="error-message.scss" %}
```css
.black {
    color: #fff;
    background-color: #000;
}
```
{% endcode %}

**Asset management**

For sure you'll have some images and other assets to be used in your project. This is super easy to handle in SpringType. Just create some folder where you save your assets and use the builtin `require()` function like magic:

```typescript
<img src={ require('../../assets/some-image.png') } />
```

**Outer attribute overrides**

Did you ever struggle to apply DOM attributes like `class`, `style`, `tabindex` and `id` and to a component from outside, while using it? If the component gives you no chance to set it, this might be troublesome. In SpringType, there is no hassle:

```typescript
st.render(<ErrorMessage message="What do you know :)" 
    id="foo1" 
    tabIndex={1} 
    class={['a', 'b']} 
    style={{
        color: '#cc0000'
    }} 
/>, document.body);
```

For sure this works for any component and virtual DOM element too. `style` is also auto-completed and `class` allows for `string` and `array`, as you can see.

**Slots**

Imagine that you want a `<Container />` component to render **outer children** at a specific place. It's straight-forward, just like the HTML standard for web components:

{% code title="some-component.tsx" %}
```typescript
...

render() {
  return <p>
      <Container>
          <template name="footer">
              <div>Placed into footer section</div>
          </template>
      </Container>
  </p>
}
...
```
{% endcode %}

{% code title="container.tsx" %}
```typescript
@component
export class Container extends st.component {

    ...
    
    render() {
      return <div class="container">
          <div class="header">
              <slot name="header" />
          </div>
          <div class="footer">
              <slot name="footer" />
          </div>
      </div>;
    }
}
```
{% endcode %}

Pretty self-explanatory, isn't it?

**Fragments, Interpolations, Comments**

Sometimes, you need to directly return an array of virtual elements to be rendered. Therefore, you'd use `<>` and `</>` in React, but `<fragment>` in SpringType, because we find this a bit more self-explanatory. For sure, TSX supports comments and interpolations too:

{% code title="some-component.tsx" %}
```typescript
...

render() {
  return <fragment>
      {/* Some random values */}
      <p>A {Math.random()}</p>
      <p>B {Math.random()}</p>
  </fragment>
}
...
```
{% endcode %}

**External templates**

SpringType supports external template files. This allows to keep components free of TSX syntax, evicting the need of `render()` method. This feels more like the Angular way and helps to keep code more clean when templates grow bigger:

{% code title="login-page.ts" %}
```typescript
import * as tpl from "./login-page.tpl";

@component({
    tpl
})
export class LoginPage extends st.component {

    onLoginClick = (click: MouseEvent) => {
        console.log('Login clicked...');
    }
}
```
{% endcode %}

External templates have a scope-binding, so it works like within the `render()` method:

{% code title="login-page.tpl.tsx" %}
```typescript
export default (component: LoginPage) => (
    <fragment>
        <h1>Login</h1>
        <div class="container">
            ...
            <button onClick={component.onLoginClick}>
                Login!
            </button>
        </div>
    </fragment>
);
```
{% endcode %}

**Updating rendered DOM elements**

We had a fancy VDOM algorithm, but we deprecated it due to complexity and  that this wasn't the best idea we've had. It was a complexity hell of it's own kind and led to many hidden performance gotchas as the developer doesn't know anymore when the UI re-renders. Having only 1 more LoC in your component to re-render partials isn't a big deal and even enhances readability and simplicity. In SpringType you can easily understand where and when the components UI update:

```typescript
interface IErrorMessageAttrs {
  message: string;
}

@component
export class ErrorMessage extends st.component<IErrorMessageAttrs> {

  @attr
  // local state
  message: string;
  
  @ref
  messageDisplayRef: HTMLElement;
  
  setMessage(message: string) {
      // update local state
      this.message = message;
      // trigger re-render
      this.renderPartial(this.message, this.messageDisplayRef);
  }
  
  render() {
      return <p ref={{ messageDisplayRef: this }}>
          { this.message }
      </p>;
  }
}
```

**Safe native DOM access and shortcuts**

SpringType doesn't have a VDOM and so you don't need to be worried about one-source-of-truth limitations. SpringType can deal with external DOM structure changes. This allows for:

* Compatibility with external JavaScript libraries to be used in and with your Components
* Direct DOM access and mutation of e.g. `@ref` references DOM elements

The UX designer asks you to integrate a jQuery plugin? Or to use some CSS framework?There is no argument against it anymore. Just go ahead - you don't need a specific wrapper library anymore. **Springtype is out-of-the-box compatible with any legacy and any future JS and CSS library by design.**
{% endtab %}

{% tab title="State" %}
Architecture is important to keep the complexity low. With complexity comes bugs and issues in any form you can think of. Our \#1 priority is simplicity. In our view there is a big misunderstanding that complex requirements have to end up in complex code. This isn't true, if you choose the right architecture.

**State**

When we think of data in an application, we should think of states. States may mutate and change. Every variable is a state, and when it's an object, it's a compound state. When states change, UI's have to react. But we think, it's up to the developer, how an application reacts to state change and how far changes escalate.   
  
In SpringType knows states in three contexts:  ****

* Local Context: No change detection. It's just a variable, or a class property.
* Active Context: Deep change detection on objects.
* Shared Active Context: Deep change detection on objects, shared between classes.

**Active Context**

If you need to react to changes in an object, you can just decorate a class property with `@context('myContext')` / functionally call `st.context('myContext', { ... })`. An active context has change detection. You can declare many context states but their scope is bound to one component:

{% code title="demo-active-context.tsx" %}
```typescript
@component
export class ActiveContextDemo extends st.component {

  @context('myContext')
  myContext: st.context('myContext', {  foo: 'bar' });
  
  @ref
  valueRef: HTMLElement;
  
  render() {
      return <p ref={{ valueRef: this }}>
          { this.myContext.foo }
      </p>;
  }
  
  onContextChange(change: IContextChange) {
      switch(change.name) {
          case 'myContext':
              st.info('change.path', change.path);
              st.info('change.value', change.value);
              st.info('change.prevValue', change.prevValue);
              
              // update element explicitly
              st.renderPartial(this.myContext.foo, this.valueRef);
              break;
      }
  }
}
st.render(<ActiveContextDemo />, document.body);
```
{% endcode %}

**Shared Active Context**

If you want to share state between components, but don't need an immutable state store, you can go with SpringTypes shared memory implementation. Just use `sharedContext` instead of context:

{% code title="demo-shared-active-context.tsx" %}
```typescript
@component
export class ComponentA extends st.component {

  @sharedContext('mySharedContext')
  mySharedContext: st.context('mySharedContext', {  foo: 'bar' });
  
  ...
}


@component
export class ComponentB extends st.component {

  @sharedContext('mySharedContext')
  mySharedContext: st.context('mySharedContext', {  foo: 'bar' });
  
 ...
}

st.render(<fragment>
    <ComponentA />
    <ComponentB />
</fragment>, document.body);

```
{% endcode %}

Sharing a state object between `n` components allows you to interactively connect components and let their UI's react to changes happening on either side. You can think of this like a multi-redux-store without immutability and direct access. It's both efficient and simple - but should be handled with care.

**Redux Store Integration**

If you need an immutable state store, Redux is the gold standard. It has one of the largest communities and comes with very good debugging tools and many high-quality middleware plugins. Therefore, we've loosely integrated Redux with typed support:

{% code title="store-counter.tsx" %}
```typescript

@component
export class StoreCounter extends st.component implements ILifecycle {

  @store('count')
  count!: number;
  
  @ref
  counterRef: HTMLElement;

  increment = () => {
    st.getStore().dispatch(actions.increment);
  };

  decrement = () => {
    st.getStore().dispatch(actions.decrement);
  };

  render() {
    return (
      <div>
        <h2>Store test</h2>

        <button onClick={this.increment}>Increment</button>
        <button onClick={this.decrement}>Decrement</button>

        <p ref={{ counterRef: this }}>{this.renderCounter()}</p>
      </div>
    );
  }
  
  renderCounter() {
    return <fragment>Count: {this.count}</fragment>
  }

  onStoreChange(name: string, value: any) {
    console.log('Store change', '@store("count") prop name', name, 'value', value, ' === ', this.count);
    this.renderPartial(this.renderCounter(), this.counterRef);
  }
}

st.setStore(appStore);

st.render(<StoreCounter />);
```
{% endcode %}

The store definition is just a standard Redux store implementation:

{% code title="counter-store.ts" %}
```typescript
export interface AppAction extends Action {
  type: string;
  meta?: any;
  data?: any;
}

// initial redux store state
const initialState: AppState = { count: 0 };

// app redux store actions
export const actions = {
  increment: { type: "INCREMENT" },
  decrement: { type: "DECREMENT" },
};

// example count reducer
const countReducer = (state: AppState = initialState, action: AppAction) => {
  switch (action.type) {
    case actions.increment.type:
      return {
        count: state.count + 1,
      };

    case actions.decrement.type:
      return {
        count: state.count - 1,
      };

    default:
      return state;
  }
};

// enable chrome developer tools for redux
const composeEnhancers = (window as any).__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

// create redux store with initial state and reducer (use compose for many reducers)
export const appStore: Store<AppState, AppAction> = createStore(countReducer, initialState, composeEnhancers(
  applyMiddleware()
));

```
{% endcode %}

{% hint style="info" %}
We recommend to use a state store like Redux in cases of complex business requirements. Immutable state stores can drastically decrease developer productivity and runtime performance \(especially, memory wise\) but increase an applications stability and resilience by providing better ways to test and debug your application state mutations.
{% endhint %}
{% endtab %}

{% tab title="Routing" %}
TODO
{% endtab %}

{% tab title="DI" %}
Unlike React, we've implemented some standard features, e.g. for Inversion of Control. But unlike Angular, we don't need complex module configurations:

**Dependency Injection**

Dependency injection has a functional API and an object-oriented API. Lets start with how we make a class injectable:

{% code title="crypto-service.ts" %}
```typescript
@injectable
export class CryptoService {

    hash(token: string, length: number = 64) {
        const hash = sodium.crypto_generichash(length, sodium.from_string(token));
        return sodium.to_hex(hash);
    }
}
```
{% endcode %}

Now if you'd like to share a single instance of this service across your application and re-use it in many functions and components, you could either use the functional interface:

```typescript
const cryptoService = st.inject(CryptoService);
cryptoService.hash('test');
```

Or let it be injected as a class property on class construction time:

{% code title="auth-service.ts" %}
```typescript
@injectable
export class AuthService {

    @inject(CryptoService)
    cryptoService: CryptoService;
}
```
{% endcode %}

Here we see an injectable that injects an injectable. Be careful with cyclic dependencies though.

{% hint style="info" %}
The `@component` decorator makes sure that you can `@inject` safely. With `@component` on a class, you don't need to add `@injectable` anymore.
{% endhint %}
{% endtab %}

{% tab title="Events" %}
In SpringType, there are two event systems. The native DOM event system and the SpringType event bus. Depending on the use-case,  you decide, which event system to use.

**Custom native DOM events**

With native DOM events, you can bubble events up a component tree. This type of event system is useful when you'd like to communicate with a parent component via DOM events:

{% code title="item.tsx" %}
```typescript
export interface IItemClickDetail {
    selected: boolean;
    item: Item;
}

@component
export class Item extends st.component {
    
    @event
    onItemClick!: IEventListener<IItemClickDetail>;

    onItemClick = (detail: IItemClickDetail) => {
        this.dispatchEvent<IItemClickDetail>("itemClick", {
            bubbles: true,
            cancelable: true,
            composed: true,
            detail: {
                ...detail,
            },
        });
    };
    
    ...
}
```
{% endcode %}

The parent component of `<Item>` would then listen to the custom event `onSelectItemClick`:

{% code title="item-container.tsx" %}
```typescript
@component
export class ItemContainer extends st.component {
    
    items = ['a', 'b', 'c'];
    
    onItemClick(evt: IEvent<IItemClickDetail>) {
        st.info('item', evt.target);
        st.info('evt.detail.selected', evt.detail.selected);
    }
    
    render() {
        return this.items.map(label => 
            <Item onItemClick={this.onItemClick} />
        )
    }
}
```
{% endcode %}

**Event bus**

The event bus allows to broadcast events application-wide. It supports topics with namespaces and object segregation. You can use it in a functional way:

```typescript
const TOPIC_APP_NEW_RANDOM = 'app:random:new';

st.subscribe(TOPIC_APP_NEW_RANDOM, (randomValue: number) => {
    console.log('new random value', randomValue);
});

setInterval(() => {
    st.publish(TOPIC_APP_NEW_RANDOM, { value: Math.random() });
}, 1000);
```

Alternatively, you can use the component event bus API and send/receive any message application-wide:

```typescript
@component
export class ComponentSendsMessage extends st.component {

  sendMessageOnClick = (event: Event) => {
    this.sendMessage('ComponentSendsMessage:onClick', { clicked: true })
  };
  
  onMessage(topic: string, value: any) {

    console.log('onMessage', topic, value, this)

    switch (topic) {
      case 'ComponentSendsMessage:onClick':
        console.log('ComponentSendsMessage:onClick')
        break;
    }
  }
  
  ...
}
```

{% hint style="info" %}
**Please note**

Using the event bus is especially helpful to broadcast messages and weakly interact with components across your application. Messaging works well for situations where it's fine to ignore if components already/still exist. Event bus messaging solves the problem of interacting with components across routes/modules in code-split situations.
{% endhint %}
{% endtab %}

{% tab title="i18n" %}
TODO
{% endtab %}

{% tab title="Validation" %}
TODO
{% endtab %}
{% endtabs %}

### Why we've built SpringType

In recent years, we've seen many frameworks rise and fall. The only thing that seems to be constant, is change. While projects like React, Angular and Vue became large code bases, non-framework approaches like Svelte and Stencil are promoting approaches to inlining functionality but limiting flexibility to deal with efficiency and complexity.

There are many metaphysical things that each of those frameworks have in common, but lets pick out some major vectors:

1. **Complexity**: Expect a steep learning curve to be productive
2. **Abstraction**: With VDOM's you can't safely work with real DOM API's anymore 
3. **Vendor lock-in**: Due to their non-standard API's code is not inter-operable
4. **Size:** Even if we take Svelte or Stencil: Inlining leads to code duplication and we don't gain much when many components inline the same boilerplate code.

With SpringType, we're breaking these limitations, while providing a state-of-the-art API. We allow component-driven development and safe, native DOM API access at the same time. The size of the transpiled and optimized framework is at records low and there is still much more  potential as we're still in beta phase.

In SpringType is an attempt to give you back the power: _You decide what happens and when_.

{% hint style="warning" %}
Please note that SpringType is still in **beta phase.** We're looking forward to release the **first stable 1.0.0 GA in Spring 2020** [🌱](https://emojipedia.org/seedling/)🚀😎. As of now, bugs may happen more often, API's may change at will and CLIs may break on platforms at times.

[Please help us becoming better and report use-cases, suggestions and bugs.](https://github.com/springtype-org/springtype/issues)

Get in touch with us on  [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="info" %}
 **Any questions?** Ask us: [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

