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
{% endtab %}

{% tab title="2. Custom native DOM events" %}
**Dispatching custom events**

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
{% endtab %}

{% tab title="3. Reactive Event Bus" %}
**Horizontal communication of components, application-wide** 

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
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

