---
description: "This is a quick introduction to SpringType: \"Hello, world!\" \U0001F603"
---

# Welcome to SpringType!

SpringType is a full-stack web framework for Website and PWA development. It promotes the use of TypeScript and modern browser APIs. SpringType's unique approach allows for **less complexity**, **fewer code**, **better performance** and a stellar **developer experience** [🚀](https://emojipedia.org/rocket/).

### Getting Started

We recommend, to take like 30 minutes to run through our guide. You can follow the guide with hands-on experience by running just one single CLI command:

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
**Better use a popular IDE**

We recommend  to work with an IDE like [VS Code](https://code.visualstudio.com/) or [IntelliJ WebStorm](https://www.jetbrains.com/webstorm/) to name two popular ones. This makes sure you'll have less struggle finding imports, get auto-completable API's and see issues ahead of build time.
{% endhint %}

TypeScript allows us to use the TSX domain-specific language. TSX is typed JSX and JSX is an XML-syntax to describe a virtual DOM \(HTML-like\) structure which is being transformed into JSON by the TypeScript compiler. 

SpringType therefore takes the transformed JSON structure, optimizes it and renders out a valid native HTML document at high speed. But unlike other frameworks, SpringType has no complex VDOM algorithm and no magic DOM abstraction that could stand between you and your code.

```typescript
import { st } from "springtype";
import { tsx } from "springtype/web";

st.render(<p>SpringType: Simplicity is key! :-)</p>, document.body);
```
{% endtab %}

{% tab title="Components" %}
SpringType's TSX supports **HTML and SVG markup**. It feels like React, but with SpringType you can easily copy & paste HTML code, because we support all standards.

Render with one simple function call: 

```typescript
st.render(<p>Hello, SpringType! </p>, document.body);
```

But you know, it's way cooler to define components, so you can re-use code you've written once. We do that like everybody does it today - just like you know it from React, Angular or Vue. Just, in SpringType, we do that without much abstraction  and with native DOM API access. **SpringType gives you back control and promotes simplicity.**

**Object-oriented API**

A component is a standard TypeScript class and @attr attributes are like properties in React. The `render()` method returns `tsx`:

{% code title="error-message.tsx" %}
```typescript
interface IErrorMessageAttrs {
  message: string;
}

@component
export class ErrorMessage extends st.component<IErrorMessageAttrs> {

  tag = 'my-error-message';

  @attr
  message: string = "Hello, world!";
  
  // this attribute shines throuh in DOM
  @attr(AttributeType.DOM_TRANSPARENT)
  cool: string = "yeah";
  
  render() {
      return <p>{ this.message }</p>;
  }
}
```
{% endcode %}

**How do you use this component?** 

Either inside another component's `tsx` \(default\), or portal-like, somewhere in an existing DOM element:

```typescript
st.render(<ErrorMessage message="Happy error :)" />, document.body);
```

And then, a beautiful, clean DOM will be rendered:

```markup
<body>
  <my-error-message cool="yeah">
    <p>Happy error :)</p>
  </my-error-message>
</body>
```

**Refs and native DOM access**

TSX is internally transformed into HTML elements. While rendering, we know which element goes where. So if you want to get hold of some native DOM element, you can just ask the renderer to remind a reference for you via `@ref` and `ref= {{ ... }}` 

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
    }###
    
    onAfterRender() {
      // native DOM element reference
      this.messageElementRef.classList.add('black');
    }
}
st.render(<ErrorMessage message="Happy error :)" />, document.body);
```

This is like fishing in the DOM and allows you to access DOM elements directly after they have been rendered.

**CSS, CSS modules, Sass and Less**

You can easily import CSS, CSS as CSS modules \(auto-generates class names, scoped to your component\), Sass \(SCSS\) and Less files. They compile as you go and integrate, inline and optimize with zero configuration:

{% code title="error-message.tsx" %}
```typescript
// simply import (global, without CSS modules)
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

For sure you'll want to use some images and other assets in your project. This is super easy to handle in SpringType. Just create some folder where you want to save your assets and provide the builtin `require()` function with a relative path:

```typescript
<img src={ require('../../assets/some-image.png') } />
```

**Outer attribute overrides**

Did you ever want to apply DOM attributes like `class`, `style`, `tabindex` and `id` to a component from outside - right wehre you use it? But the component doesn't give you any API to do so? In SpringType, there is no struggle:

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

For sure this works for any other component, HTML and SVG element as well. `style` is typed and auto-completed; `class` allows for `string` and `array` input.

**Slots**

Imagine that you want a `<Container />` component to render **outer children** at a specific place. This is what we know as only being supported with `<template>/<slot>` in web components. 

With SpringType, you have that in non-shadow DOM:

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

**Fragments, Interpolations, Comments**

Sometimes, you might need to directly return an array of virtual elements to be rendered, because there should be none in between. Therefore, you'd use `<>` and `</>` in React, but `<fragment>` in SpringType. We find this a bit more self-explanatory. For sure, TSX supports comments and interpolations too:

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

SpringType supports external template files. This allows you to keep components free of TSX syntax, rendering the `render()` method pointless :-\) This gives an Angular-like feeling and helps to keep the code more clean as templates grow:

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

External templates have a scope-binding. You can access the component instance just like within the `render()` method:

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

We had a fancy VDOM algorithm, but we deprecated it due to it's complexity. Having only 1 more LoC in your component to re-render partials isn't a big deal and even enhances readability and simplicity of the code. 

In SpringType you can easily understand where and when the UI updates:

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

**Functional Components**

A simple functional component looks like this:

```typescript
// simplified functional component, just renders what is given
const Container = component(
  render((container: Component) => {
    return <div>{container.renderChildren()}</div>;
  })
);
st.render(<Container>Hello! :-)</Container>, document.body);
```

For cases where you'd need closure states, you can return the `render` function after processing. The component instance handed in allows you access to attributes and the components standard lifecycle API:

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

**Native DOM access and shortcuts**

SpringType doesn't have a VDOM and so you don't need to be worried about one-source-of-truth limitations. SpringType can deal with external DOM structure changes. This allows for:

* Compatibility with external JavaScript libraries to be used in and with your Components
* Direct DOM access and mutation of e.g. `@ref` references DOM elements

The UX designer asks you to integrate a jQuery plugin? Or to use some CSS framework?There is no argument against it anymore. Just go ahead - you don't need a specific wrapper library anymore. **Springtype is out-of-the-box compatible with any legacy and any future JS and CSS library by design.**
{% endtab %}

{% tab title="Events" %}
In SpringType, there are two event systems. The native DOM event system and the SpringType event bus. Depending on the use-case,  you decide, which event system to use.

**Standard DOM events**

Binding standard DOM events and dealing with them is super easy. Just take care to use fat arrow functions to make sure that `this` points to the component instance:

```typescript
@component
export class MyButton extends st.component {
    
  onButtonClick = (evt: MouseEvent) => {
    // evt is the native DOM event
    evt.stopPropagation();
        
    st.info(evt.target);
  } 
    
  render() {
    return <button onClick={ this.onButtonClick }>Click</button>
  }
}

st.render(<MyButton />, document.body);
```

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

The event bus allows to broadcast events application-wide. It supports topics with namespaces. 

```typescript
export const TOPIC_SOME_CMP_ON_CLICK = 'SomeComponent:onClick';

@component
export class SomeCmp extends st.component {

  sendMessageOnClick = (event: Event) => {
    st.sendMessage(TOPIC_SOME_CMP_ON_CLICK, { clicked: true })
  };
  
  @onMessage(TOPIC_SOME_CMP_ON_CLICK)
  handleOnClick(details: any) {
    st.info('clicked?', details.clicked);
  }
  
  ...
}
```

**Functional API**

Alternatively, you can use the component event bus API and send/receive any message application-wide, also in a functional way:

```typescript
const TOPIC_APP_NEW_RANDOM = 'app:random:new';

st.onMessage(TOPIC_APP_NEW_RANDOM, (randomValue: number) => {
    st.info('new random value', randomValue);
});

setInterval(() => {
    st.sendMessage(TOPIC_APP_NEW_RANDOM, { value: Math.random() });
}, 1000);
```

{% hint style="info" %}
**Please note**

The event bus is especially helpful to broadcast messages and weakly interact with components across your application. Messaging works well for situations where it is okay to ignore if components exist or not. Event bus messaging solves the problem of interacting with components across routes/modules in code-split situations and prevents memory leaks.
{% endhint %}
{% endtab %}

{% tab title="Services" %}
Services are meant to implement business logic. Services are usually singleton instances of classes that are shared across a whole application. A service e.g. implements network request/response handling, data validation and handles logical decision making. 

To communicate and interact with the rest of the application - namely the components, a service can:

* Be injected \(DI\) in components and other services
* Send and receive messages \(events\)
* Be attached to Redux store fields
* Declare class-local and shared contexts

**Service injection**

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

To use this random service inside components, you just need to inject it:

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

The other features mentioned are described in State and Events.
{% endtab %}

{% tab title="DI" %}
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

**Functional API**

Now if you'd like to share a single instance of this service across your application and re-use it in many functions and components, you could either use the functional interface:

```typescript
const cryptoUtil = st.inject(CryptoUtil);
cryptoUtil.hash('test');
```

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

{% tab title="State" %}
When we think of data in an application, we think of states. States may mutate and change. Every variable is a state, and when it's an object, it's a compound state. When states change, UI's have to react. But we think, it's up to the developer, how an application reacts to state change and how far changes escalate.   
  
In SpringType knows states in three contexts:  ****

* Local: No change detection. It's just a variable or a class property.
* Class-local context: Deep change detection on objects with shared memory.
* Shared context: Application-wide states for reactive change notification.

**Class-local context**

A context is a singleton state for one class, shared by all instances. It comes with deep  change detection, but no magic will happen, except you decorate methods with `@onContextChange('contextName')`. Those methods will be called:

{% code title="src/page/context/context-page.tsx" %}
```typescript
@component
export class ContextPage extends st.component {

  @context
  localAuthState = { shouldLogIn: false, isLoggedIn: false };
  
  @ref
  displayRef: HTMLElement;
  
  render() {
    return <div>
        <p ref={{ displayRef: this }}>
          { this.renderIsLoggedIn() }
        </p>
      <button onClick={this.doLogin}>Login!</button>
    </div>;
  }
  
  doLogin = () => {
    this.localAuthState.shouldLogIn = true;
  }
  
  renderIsLoggedIn() {
    return `Is logged in? ${ this.localAuthState.isLoggedIn }`;
  }
  
  @onContextChange('localAuthState', 'shouldLogIn')
  onShouldLoginStateChange(shouldLogIn: boolean) {
  
    if (shouldLogIn === true) {
      // mock login
      this.localAuthState.isLoggedIn = true;
           
      // update element explicitly
      st.renderPartial(this.renderIsLoggedIn(), this.displayRef);Ï
    }
  }
}
st.render(<ContextDemo />, document.body);
```
{% endcode %}

You can imagine `@context` as a way to manage singleton state. The idea is, that when you implement one view like a Login Screen, you'll just have a neat, reactive state management, but retain full control, circuit-breakable.

**Shared Context**

On top of class-local contexts, you can extend the scope of a context up to being accessible by the whole application. This is accomplished by simply giving the context a unique name, e.g.: `@context('globalAuthState')`. 

The idea behind this is, that you can have topic-specific global states, with change detection and reactive method calls when `@onContextChange` is used. This is very powerful for reactive notifications on data changes across multiple components and services without having a need for any complex stream setup:

{% code title="Shared context based, reactive notification:" %}
```typescript
@component
export class ContextPage extends st.component {

  @context('globalAuthState')
  authState = st.context('globalAuthState');
  
  ...
  
  onLoginClick = () => {
    // mutation triggers doLogin() in AuthService
    this.authState.shouldLogIn = true;
  }
}

@service
export class AuthService extends st.service {

  @context('globalAuthState')
  authState = { shouldLogIn: false, isLoggedIn: false };
  
  @onContextChange('globalAuthState', 'shouldLogIn')
  async doLogin(shouldLogIn: boolean) {
  
    if (shouldLogIn) {
      // TODO: Call server to login
    }
  }
}
```
{% endcode %}

**Functional API**

You can assign, fetch and mutate a context's value functionally too: `const globalAuthState = st.context('globalAuthState')`, assignments cause mutations: `globalAuthState.shouldLogIn = true` triggers `@onContextChange` in class instances. Also, you can assign or reassign values by setting the value parameter e.g.: `st.context('globalAuthState', { shouldLogIn: false })`

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

{% tab title="i18n" %}
TODO
{% endtab %}
{% endtabs %}

### Why we've built SpringType

We've seen many frameworks rise and fall. The only thing that seems to be constant, is change. While projects like React, Angular and Vue became large code bases, non-framework approaches like Svelte and Stencil are promoting methods of inlining functionality to get around the need to deploy a framework.

There are many, those frameworks have in common, but lets pick out some major vectors:

1. **Complexity**: Expect a steep learning curve to be productive
2. **Abstraction**: With VDOM's you can't safely work with real DOM API's at runtime 
3. **Vendor lock-in**: Due to their non-standard API's, code isn't inter-operable
4. **Size:** Even if we take Svelte or Stencil: Inlining leads to code duplication and we don't gain much in size with many components being deployed in larger projects.

With SpringType, we're breaking these limitations, while providing a state-of-the-art API. SpringType is a micro-framework that allows component-driven development and safe, native DOM API access at the same time. The size of the transpiled and optimized framework is at records low and there is still potential as we're in beta phase right now.

SpringType is an attempt to give you back the power: _You decide what happens and when_.

{% hint style="warning" %}
Please note that SpringType is still in **beta phase.** We're looking forward to release the **first stable 1.0.0 GA in Spring 2020** [🌱](https://emojipedia.org/seedling/)🚀😎. As of now, bugs may happen more often, API's may change at will and CLIs may break on platforms at times.

[Please help us becoming better and report use-cases, suggestions and bugs.](https://github.com/springtype-org/springtype/issues)

Get in touch with us on  [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="info" %}
 **Any questions?** Ask us: [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

