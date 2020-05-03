---
description: >-
  Ways to strongly and loosely inter-wire components of your app without any
  hassle.
---

# E. Events

{% tabs %}
{% tab title="1. Native DOM events" %}
**Parent/Child eventing in Component trees**

Binding standard DOM events and dealing with them is super easy. Just take care to use fat arrow functions to make sure that `this` points to the component instance:

```typescript
import { component } from "springtype/web/component";
import { st } from "springtype/core";
import { tsx } from "springtype/web/vdom";

@component
export class SomeComponent extends st.component {
    
  onButtonClick = (evt: MouseEvent) => {
    // evt is the native DOM event
    evt.stopPropagation();
        
    st.info('User clicked on an HTML button', evt);
  } 
    
  render() {
    return <button onClick={ this.onButtonClick }>Click</button>
  }
}

st.render(<SomeComponent />);
```
{% endtab %}

{% tab title="2. Relaying" %}
**Intercepting and relaying native DOM events** 

From time to time, DOM event interception is necessary - and often combined with relaying. e.g. you listen for a specific event, handle it and dispatch another event providing an aggregated event state. 

The `<ButtonDebounced />` component is a suiting use-case. A `click` `MouseEvent` is catched and whenever it doesn't happen again for a certain time, a `clickDebounced` `MouseEvent` is relayed, resampling the same native DOM event:

{% code title="src/component/button-debounced.tsx" %}
```typescript
import { component, event } from "springtype/web/component";
import { st } from "springtype/core";
import { tsx } from "springtype/web/vdom";
import { debounce } from "springtype/core/lang/debounce";
import { IEventListener, IEvent } from "springtype/web/component/interface";

export interface IButtonDebouncedAttrs {

    /**
     * Debounce time in milliseconds; default: 1000
     */
    debounceTimeMs: number;

    /**
     * Fired on debounced click
     */
    onClickDebounced: IEventListener<void, MouseEvent>;
}

@component
export class ButtonDebounced extends st.component<IButtonDebouncedAttrs> {

    static readonly EVENT_CLICK_DEBOUNCED = 'clickdebounced';

    @event
    onClickDebounced: IEventListener<void, MouseEvent>;

    debounceTimeMs: number = 1000;

    onBeforeConnect() {
        
        // assigning the debounced click handler here, 
        // because the local state this.debounceTimeMs is just 
        // accessible after instance contruction time.
        // debounce() makes sure that the function is called
        // only once in a timeframe (see: Introduction / A. About TypeScript / 5. Reactive)
        this.onButtonClick = debounce((evt: MouseEvent) => {

            st.info('dispatching a debounced click event', evt);
    
            // dispatch/relay the native DOM event received
            this.dispatchEvent(ButtonDebounced.EVENT_CLICK_DEBOUNCED, evt);

        }, this.debounceTimeMs);
    }

    onButtonClick = () => {} // replaced by reference in this.onBeforeConnect

    render() {
        return <button onClick={this.onButtonClick}>{this.renderChildren('Click')}</button>
    }
}
```
{% endcode %}

This component is used in `src/page/service/service-page.tsx`, a page that demonstrates how to use SpringType Services. The button prevents the API from being called too quickly.
{% endtab %}

{% tab title="2. Custom events" %}
**Dispatching custom DOM events**

With native DOM events, you can bubble events up a DOM / HTML tree. This type of event system is useful when you'd like to communicate with a parent component via DOM events. If it's not a native DOM event you'd like to relay, but custom event state that you'd like to propagate, it's necessary to specify a few more details:

{% code title="src/page/todo/component/todo-item.tsx" %}
```typescript
import { component, event, attr } from "springtype/web/component";
import { tsx } from "springtype/web/vdom";
import { st } from "springtype/core";
import { ref } from "springtype/core/ref";
import { IEventListener } from "springtype/web/component/interface";

export interface ITodoItemState {
    id: number;
    text: string;
    done: boolean;
}

export interface ITodoItemEventDetail {
    state: ITodoItemState;
}

export interface ITodoItemAttrs {
    state: ITodoItemState;
    onDoneStateChange: IEventListener<ITodoItemEventDetail>;
}

@component
export class TodoItem extends st.component<ITodoItemAttrs> {

    @ref
    inputRef: HTMLInputElement;

    @attr
    state: ITodoItemState;

    @event
    onDoneStateChange!: IEventListener<ITodoItemEventDetail>;

    onDoneInputChange = (evt: Event) => {

        this.setDone(!!this.state.done);

        // dispatching "doneStateChange" triggers registered
        // onDoneStateChange event listeners
        this.dispatchEvent<ITodoItemEventDetail>("doneChange", {
            bubbles: true,
            cancelable: true,
            composed: true,
            detail: {
                state: this.state,
            },
        });
    };

    setDone(isDone: boolean) {
        this.state.done = isDone;
        this.inputRef.checked = this.state.done;
    }

    render() {
        return <span>
            <input
                onChange={this.onDoneInputChange}
                ref={{ inputRef: this }}
                type="radio"
                checked={this.state.done} />
            {this.state.text}
        </span>;
    }
}
```
{% endcode %}

The parent component of `<TodoItem>` would then listen to the custom event `onDoneStateChange`:

{% code title="src/page/todo/component/todo-list.tsx" %}
```typescript
import { component, event } from "springtype/web/component";
import { st } from "springtype/core";
import { tsx } from "springtype/web/vdom"; 
import { TodoItem, ITodoItemState, ITodoItemEventDetail } from "./todo-item";
import { IEvent } from "springtype/web/component/interface";

@component
export class TodoList extends st.component {
    
    items: Array<ITodoItemState> = [{
      id: 1,
      done: false,
      text: "Max"
    }, {
      id: 2,
      done: true,
      text: "Julia"
    }];
    
    onItemDoneStateChange(evt: IEvent<ITodoItemEventDetail>) {
        st.info('item id', evt.detail.state.id);
        st.info('item name', evt.detail.state.text);
        st.info('item done?', evt.detail.state.done);
    }
    
    render() {
        return <ul>{
          this.items.map((itemState: ITodoItemState) => 
            <li>
              <TodoItem 
                state={itemState} 
                onDoneStateChange={this.onItemDoneStateChange} />
            </li>
          )
        }</ul>
    }
}
```
{% endcode %}

**Challenge: Implement an "Add" button**

You would probably like to use the following SpringType features:

* `<fragment>`
* native DOM event listener
* `this.rerender();`

In case you want to customize the text, an `<input>` for the text would be helpful too.

**Challenge 2: Implement an `<input>` for the item text**

For this, you would probably like to implement an event propagation similar to the `onDoneStateChange` mechanism.

**Challenge 3: Implement an "Delete" button for each item**

Implementing this will probably remind you of the "Add" button implementation.
{% endtab %}

{% tab title="3. Reactive Event Bus" %}
**Horizontal communication of components, application-wide** 

The event bus allows to broadcast events application-wide. It supports topics with namespaces. You can use this for loose connectivity between Components but also between Components and Services:

{% code title="src/service/login.ts" %}
```typescript
import { st } from "springtype/core";
import { service } from "springtype/core/service";
import { onMessage } from "springtype/core/event-bus/decorator/on-message";

export interface ILoginEvent {
  username: string;
  password: string;
}

export const TOPIC_LOGIN = 'LoginService:login';

/**
 * Simple login service communicating via Reactive Event Bus.
 * 
 * Inject this service in components and other services using:
 * 
 * @inject(LoginService)
 * loginService: LoginService;
 */
@service
export class LoginService extends st.service {

  // this service listens for LoginService:login messages
  // once a message like this is sent, this method will be called
  @onMessage(TOPIC_LOGIN)
  onLogin(loginEvent: ILoginEvent) {
    console.log('logging in using credentials....', loginEvent);
    // fetch(...)
  }
}
```
{% endcode %}

Sending such a message is as simple as:

```typescript
st.sendMessage<ILoginEvent>(TOPIC_LOGIN, {
  username: 'test',
  password: 'demo'
});
```

This mechanism is used in `src/page/login-page.ts` to inform the `LoginService`.

**Functional API**

Alternatively, you can use the Event Bus API and send/receive any message application-wide, also in a functional way:

```typescript
const TOPIC_APP_NEW_RANDOM = 'app:random:new';

st.onMessage(TOPIC_APP_NEW_RANDOM, (randomValue: number) => {
    st.info('new random value', randomValue);
});

setInterval(() => {
    st.sendMessage(TOPIC_APP_NEW_RANDOM, { value: Math.random() });
}, 1000 /* ms */);
```

{% hint style="info" %}
**Please note**

The event bus is especially helpful to broadcast messages and weakly interact with components across your application. Messaging works well for situations where it is okay to ignore if components exist or not. Event bus messaging solves the problem of interacting with components across routes/modules in code-split situations and prevents memory leaks.
{% endhint %}
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

