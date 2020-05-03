---
description: >-
  ...is a fancy word for data. How to handle data and changes (mutations) in
  SpringType?
---

# H. State

{% tabs %}
{% tab title="1. State" %}
When we think of data in an application, we think of states. States may mutate and change. Every variable is a state, and when it's an object, it's a compound state. When states change, UI's have to react. But we think, it's up to the developer, how an application reacts to state change and how far changes escalate.   
  
In SpringType knows states in three contexts:  ****

* Local: No change detection. It's just a variable or a class property.
* Class-local context: Deep change detection on objects with shared memory.
* Shared context: Application-wide states for reactive change notification.
{% endtab %}

{% tab title="2. Reactive Context" %}
**Reactive Context**

A context is a singleton state for one class, shared by all instances. It comes with deep  change detection, but no magic will happen, except you decorate methods with `@onContextChange('contextName')`. Reactive Context is available for:

* Component
* Service

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
st.render(<ContextDemo />);
```
{% endcode %}

You can imagine `@context` as a way to manage singleton state. The idea is, that when you implement one view like a Login Screen, you'll just have a neat, reactive state management, but retain full control, circuit-breakable.
{% endtab %}

{% tab title="3. Shared Context" %}
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
{% endtab %}

{% tab title="4. Functional API" %}
**Functional API**

You can assign, fetch and mutate a context's value functionally too: `const globalAuthState = st.context('globalAuthState')`, assignments cause mutations: `globalAuthState.shouldLogIn = true` triggers `@onContextChange` in class instances. Also, you can assign or reassign values by setting the value parameter e.g.: `st.context('globalAuthState', { shouldLogIn: false })`
{% endtab %}

{% tab title="5. Redux" %}
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
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

