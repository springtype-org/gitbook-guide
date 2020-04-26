---
description: "This is a quick introduction to SpringType: \"Hello, world!\" \U0001F603"
---

# Welcome to SpringType!

SpringType is a micro-framework for Website and PWA development. It promotes the use of TypeScript and modern browser APIs. SpringType's unique approach allows for **less complexity**, **fewer code**, **better performance** and a stellar **developer experience** [🚀](https://emojipedia.org/rocket/).

### Getting Started

We recommend, to take like 30 minutes to run through our guide. You can follow the guide with hands-on experience by running just one single CLI command:

```bash
> npx st-create -c project -t guide -n SpringTypeSandBox
```

{% hint style="warning" %}
SpringType just requires [N](https://nodejs.org)[ode.js](https://nodejs.org) to be installed first. We're proud to say that SpringType has 0 dependencies \(but quite a lot `devDependencies`because we're using Webpack, Babel & many plugins to optimize developer experience\). 
{% endhint %}

### SpringType in 30 minutes 

Designing SpringType was quite a journey. It took us more than a year to constantly re-invent the API's until we've came to the current version where the API is small, concise, simple and efficient. Simply put:

> A beautiful new take on modern web development that brings back the fun and simplicity of the old days - while retaining modern web development standards.

{% tabs %}
{% tab title="TypeScript" %}
**Optional Type Safety**

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

**TSX**

TypeScript allows us to use the TSX domain-specific language. TSX is typed JSX and JSX is an XML-syntax to describe a virtual DOM \(HTML-like\) structure which is being transformed into JSON by the TypeScript compiler. 

SpringType therefore takes the transformed JSON structure, optimizes it and renders out a valid native HTML document at high speed. But unlike other frameworks, SpringType has no complex VDOM algorithm and no magic DOM abstraction that could stand between you and your code.

```typescript
import { st } from "springtype";
import { tsx } from "springtype/web";

st.render(<p>SpringType: Simplicity is key! :-)</p>, document.body);
```

**Static imports**

The best practice for using external `npm` modules is to import them with standard TypeScript import syntax:

```typescript
import { MatInput } from "st-materialize";

st.render(<MatInput label="Hello, world!" />, document.body);
```

However, these imports are static and thus result in larger code sizes compared to dynamic imports which can be used to split code in smaller chunks.

**Dynamic imports and code splitting**

Based on the dynamic import feature of TypeScript, static code dependencies can replaced with a method that loads code on demand:

```typescript
const loadModuleB = async() => {
  return import('../module-b');
}

run(async() {
  const moduleB = await loadModuleB();
});
```

Using this language feature, `st-start` can optimize production builds automatically for smaller, differentiated on-demand code delivery. The code is only loaded once the method has been called at runtime.

{% hint style="info" %}
SpringTypes DOM Router has built-in support for dynamic imports and code splitting.
{% endhint %}

**Async/await and "fat arrow functions"**

Typically asynchronous call flows have been quite cumbersome. In SpringType, you can easily mix synchronous and asynchronous code using async/await. This technique is often combines with the use of fat arrow functions:

```typescript
const loadUsers = async() => fetch('/users');

run(async() => {

  const users = await loadUsers();
  st.render(users.map(user => <p>user.name</p>), document.body);
});
```

This is especially helpful with classes where fat arrow functions make sure that the this-scope is pointing to the instance all the time:

```typescript
class Sample {
  loadUsers = async() => fetch('/users');
}

run(async() => {
  await new Sample().loadUsers();
});
```

**Reactive functions**

On top of the TypeScript standard library features, we've implemented functions to provide you with the tools to design software using reactive call flows.

_Run_

Runs a function synchronously or asynchronously and returns:

```typescript
run(async() => {
  ...
});
```

_Debouncing_

Debouncing functions allows to make sure it is called only once in  `n` milliseconds, no matter how often it is called:

```typescript
const sendRequestDebounced = debounce(async(...params) => {
    ...
}, /*ms*/ 500);

sendRequestDebounced(...);
```

_Delaying_

Delaying functions allows to let all calls be delayed by `n` milliseconds:

```typescript
const sendRequestDelayed = delay(async(...) => {
    ...
}, /*ms*/ 500);

sendRequestDelayed(...);
```

_Immediate_

Immediate functions are called on next VM tick. This is helpful if JavaScript code must wait until synchronous DOM operations has finished:

```typescript
const sendRequestImmediate = immediate(async(...params) => {
    ...
});

sendRequestImmediate(...);
```

_AnimationFrame_

Animation frame functions are called after the browsers rendering has been refreshed. This allows to implement smooth animations that are calculated using JavaScript:

```typescript
const calcAnimation = animationFrame(async(...params) => {
    ...
});

calcAnimation(...);
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
    // dispatching "itemClick" triggers registered
    // onItemClick event listeners
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

{% tab title="Routes" %}
Routing in SpringType allows you to do hash routing \(`#/your/page` URL's\) as well as non-hash routing. For the latter, you need to set-up your web server to redirect any URL path to deliver index.html in order to make it work without page refreshes.

**Routes and route lists**

The concept of `<RouteList>` is, that SpringType matches the URL pattern navigated to with a list of `<Route>` components implemented in your application. This feels very much like React's DOM router:

{% code title="src/index.tsx" %}
```typescript
<RouteList>
  <Route path={["", "*"]}>
    <SplashscreenPage />
  </Route>
</RouteList>
```
{% endcode %}

This configuration would always show the `<SplashscreenPage />` component, because for either the `/` and every other path \(wildcard `*`\) it would display this one and hide any other `<Route />` provided in the `<RouteList />`.

**Pages**

There is no special meaning in the naming "Page". A page is just a standard component. The only difference is its purpose to display a page layout:

{% code title="src/page/splashscreen.tsx" %}
```typescript
@component
export class SplashscreenPage extends st.component {

    render() {
        return <p>SplashscreenPage</p>
    }
}
```
{% endcode %}

**Setting a custom loading component**

Once a project becomes a bit more sophisticated, we might like to show a custom loading component, e.g. some placeholder animation. This is easily done using the template/slot mechanism demonstrated before:

{% code title="src/index.tsx" %}
```typescript
<Route path={["", "*"]}>
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <MyCustomLoadingComponent />
  </template>
  <SplashscreenPage />
</Route>
```
{% endcode %}

**Redirects**

Now that we've set up a default route to go to and some animation before, we might like to send the user directly to another page, once the `<SplashscreenPage />` has been shown:

{% code title="src/page/splashscreen.tsx" %}
```typescript
@component
export class SplashscreenPage extends st.component {

  render() {
    return <p>SplashscreenPage</p>
  }
    
  onAfterRender() {
    // redirecting to another route
    st.route = {
      path: LoginPage.PATH,
      params: { ... }
    };
  }
}
```
{% endcode %}

To redirect, we just need to set a new path and optionally parameters to provide.

```typescript
@component({
    tpl
})
export class LoginPage extends st.component {

  // it is a good practice to assign the path statically
  // This would be valid for: /login, /#/login, /#login
  static readonly PATH = "login";
  
  ...
}
```

**Parameters**

SpringType supports path parameters. To configure parameters to be mapped automatically, path parameters should be set like this:

```typescript
@component({
    tpl
})
export class LoginPage extends st.component {

  // it is a good practice to assign the path statically
  // This would be valid for: /login, /#/login, /#login
  static readonly PATH = "login/:username";
  
  ...
}
```

Doing so, and assuming that the path navigated to looks like this: `/login/johndoe`, you could access the mapped parameters like this:

```typescript
@component({
    tpl
})
export class LoginPage extends st.component {

  // it is a good practice to assign the path statically
  // This would be valid for: /login, /#/login, /#login
  static readonly PATH = "login/:username";
  
  onRouteEnter() {
    st.info('username:', st.route.params.username);
  }
}
```

{% hint style="info" %}
For your convenience, query parameters are mapped automatically with higher priority. e.g. `/login/johndoe?username=max` would result in `st.route.params.username` to be `max`.
{% endhint %}

**Links**

The `<Link />` component allows you to display links to page components. Those links automatically get marked with the CSS class active, when the matching path has been navigated to:

```typescript
<Link path={"home"} tag="button" class="home-button">Home</Link>
```

**Nested Routes**

You can and should use nested routes. Nesting means to implement a `<RouteList>` in a page component that was already routed to.

**Guards**

In many projects we need to protect some pages from being visited when the user is not yet logged in or doesn't have specific permissions to do so. It is always wise to check those permissions in the backend. However, for user experience reasons, we'd also want to provide such a feature in the frontend.

In SpringType, we do so using guard functions. This feels much like Angular. Guard functions are async functions and promise a TSX component or a path to be returned. This allows you to trigger services, await their response - and while the custom loading component shows its animation, we check for intent validation:

{% code title="index.tsx" %}
```typescript
<Route path={[DashboardPage.PATH]} 
       guard={this.loginGuard.isLoggedIn}>
       
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <DashboardLoadingAnimation />
  </template>
  
  <DashboardPage />
  
</Route>
```
{% endcode %}

A guard might be implemented as a Service:

```typescript
@service
export class LoginGuard extends st.service {

  @inject(AuthService)
  authService: AuthService;
    
  isLoggedIn = async (match: IRouteMatch): 
    Promise<IRouterGuardResponse> => {
    
    if (!await this.authService.isLoggedIn()) {
      return LoginPage.PATH;
    } else {
      return true;
    }
  }
}
```

**Code Splitting**

If your project becomes even more sophisticated, you might like to offload bandwidth and keep code from being loading as long as pages are not yet visited by the user. To do so, you can use TypeScripts dynamic import syntax. The bundler with automatically split code at this point:

{% code title="index.tsx" %}
```typescript
<Route path={"dashboard"} 
       guard={this.loginGuard.isLoggedIn}>
       
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <DashboardLoadingAnimation />
  </template>
  
  {() => import("./page/dashboard")}
  
</Route>
```
{% endcode %}

**Cache Groups**

When SpringType routes between paths, it caches component instances. This allows for highly efficient switches, because we're only flipping CSS display values. You can control this behavior by setting cache groups. 

Once a path outside the current cache group is visited, the router component cache will be invalidated. Assigning each `<Route />` its own cache group, literally disables caching:

```bash
<Route cacheGroup="login" path={[ResetPasswordPage.PATH]}>
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <MatLoadingIndicator />
  </template>
  <ResetPasswordPage />
</Route>
```
{% endtab %}

{% tab title="i18n" %}
To make your developer life way easier, we've implemented an internationalization \(`st.i18n`\) API that builds on JSON files and a string interpolation implementation with  formatting functions \(`st.format`\).

**Enabling i18n**

Internationalization is a core feature of SpringType but it's not enabled by default. 
{% endtab %}
{% endtabs %}













{% hint style="warning" %}
Please note that SpringType is still in **beta phase.** We're looking forward to release the **first stable 1.0.0 GA in Spring 2020** [🌱](https://emojipedia.org/seedling/)🚀😎. As of now, bugs may happen more often, API's may change at will and CLIs may break on platforms at times.

[Please help us becoming better and report use-cases, suggestions and bugs.](https://github.com/springtype-org/springtype/issues)

Get in touch with us on  [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="info" %}
 **Questions?** Ask us: [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

