# D. Components

{% tabs %}
{% tab title="1. Rendering" %}
**What is a Component?**

SpringType's TSX supports **HTML and SVG markup**. It feels like React, but with SpringType you can easily copy & paste HTML code, because we support all standards.

You can render TSX markup with one simple function call: 

```typescript
st.render(<p>Hello, SpringType! </p>, document.body);
```

But it's way cooler to define _Components_: Using them, you can re-use code you've written once, make markup rendering dynamic, conditionally and customizable by _Attributes_ and handle user interaction with _Events_. Data can be assigned with local States and _Context States_. 

We know this pattern from React, Angular and Vue. Just, in SpringType, we do that without much abstraction and give you native DOM API access. 

**Native DOM access and DOM API shortcuts**

SpringType doesn't have a VDOM and so you don't need to worry about one-source-of-truth limitations like in React, Angular and other popular frameworks. 

SpringType can also deal with external native DOM changes. 

This allows for:

* Compatibility with external JavaScript libraries to be used in and with your Components
* Direct DOM access and mutation of e.g. `@ref` references DOM elements

The UX designer asks you to integrate a jQuery plugin? Or to use some CSS framework?There is no argument against it anymore. Just go ahead - you don't need a specific wrapper library anymore. 

{% hint style="info" %}
**Springtype is by design compatible with every JS and CSS library.**
{% endhint %}
{% endtab %}

{% tab title="2. API" %}
**Object-oriented Component API**

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

**External templates**

SpringType supports external template files. Such a template is just a replacement for a `render` function. It makes it possible to keep components free from TSX syntax, rendering the `render()` method pointless :-\) 

This feels much like Angular and helps to keep the code more clean when templates grow:

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

External templates have a class instance scope-binding. You can access the component instance just like within the `render()` method:

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

**Functional Component API**

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

{% hint style="warning" %}
**A note on HoC \(Higher order Components\)**

With the Functional Component API, higher order functions and higher order components can be implements. This concept is similar to the idea of inheritance in object-oriented programming.

Higher order functions are created when you write a function to return a function that does even more \(is enhanced with more logic\). This can easily lead to vast layers of complexity. 

We highly recommend not to use this pattern extensively. SpringType promotes Composition over Inheritance and Composition over Higher Ordering. 

**It's a key success factor for any software project to keep cyclomatic complexity as low as possible.**
{% endhint %}
{% endtab %}

{% tab title="3. Attrs" %}
**Attributes**

A SpringType component can set and receive attributes. This feels much like React properties, but in fact, it's just passing properties to a class and, if desired, passing it down to the DOM element directly:

```typescript
interface IErrorMessageAttrs {
  message: string;
}

@component
export class ErrorMessage extends st.component<IErrorMessageAttrs> {

  @attr
  message: string;
  
  render() {
      return <p>{ this.message }</p>
  }
}
```

In case you'd want the attribute to be transparent and "shine through" to the DOM element, you'd just set one additional flag:

```typescript
  @attr(AttrType.DOM_TRANSPARENT)
  message: string;
```

**Outer native DOM attributes**

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
{% endtab %}

{% tab title="4. Children" %}
**Composition over Inheritance and Higher Ordering**

In SpringType we strongly promote the Composition over Inheritance pattern. Many years of experience show the complexity, OOP inheritance can bring. The same is true for the modern hype of writing higher order functions. This drives cyclomatic complexity. If you can, do compositing instead of inheriting or enhancing. 

One way of doing so, is writing small components that do one job well. Later, you can composite them:

```text
<MyPanel>
  <MyButton>Click me!</MyButton>
</MyPanel>
```

Writing such a MyPanel component, is pretty easy:

{% code title="my-panel.tsx" %}
```typescript
@component
export class MyPanel extends st.component {

    render() {
      return this.renderChildren();
    }
}
```
{% endcode %}

However, what do you do, when there is a specific TSX markup and you'd like to render outer children at specific places?

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
{% endtab %}

{% tab title="5. Updates " %}
**Updating rendered DOM elements**

We had a fancy VDOM algorithm, but we deprecated it due to it's complexity. Our current approach means to have only one more line of code but this massively enhances readability and simplicity. 

In SpringType you can easily understand where, when and why the UI updates:

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

**Refs and native DOM access**

TSX is internally transformed into HTML elements. While rendering, we know exactly, which element belongs what component instance. So if you'd like to get hold of some native DOM element reference, you can do so by asking the renderer to remind a reference for you via `@ref` and `ref= {{ ... }}` 

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
      // native DOM element reference
      this.messageElementRef.classList.add('black');
    }
}
st.render(<ErrorMessage message="Happy error :)" />, document.body);
```

This is like fishing in the DOM tree and allows you to access DOM elements once after they have been \(re-\)rendered.
{% endtab %}

{% tab title="6. CSS" %}
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
{% endtab %}

{% tab title="7. Assets" %}
**Using assets**

For sure you'll want to use some images and other assets in your project. This is super easy to handle in SpringType. Just create some folder where you want to save your assets and provide the built-in `require()` function with a relative path:

```typescript
<img src={ require('../../assets/some-image.png') } />
```

**Automatic asset optimization**

We strongly recommend to always import assets or use the require\(\) function.  Doing so will inform the SpringType build system about the existence of this asset and include it in the PWA manifest cache file as well as copying it over and optimizing it for production builds.
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}

