# D. Components

{% tabs %}
{% tab title="1. Rendering" %}
**What is a Component?**

SpringType's TSX supports **HTML and SVG markup**. It feels like React, but with SpringType you can easily copy & paste HTML code, because we support all standards.

You can render TSX markup with one simple function call: 

```typescript
import { st } from "springtype/core";
import { tsx } from "springtype/web/vdom";

st.render(<p>Hello, SpringType! </p>);
```

But it's way cooler to define _**Components**_: Using them, you can re-use code you've written once, make markup rendering dynamic, conditionally and customizable by _Attributes_ and handle user interaction with _Events_. Data can be assigned with local States and _Context States_. 

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

{% code title="src/component/error-message-demo.tsx" %}
```typescript
import { st } from "springtype/core";
import { tsx } from "springtype/web/vdom";
import { AttrType } from "springtype/web/component/interface";
import { component, attr } from "springtype/web/component";

// this interface allows for an auto-completable 
// API when using the <ErrorMessage /> component 
// in a modern IDE like VS Code or IntelliJ IDEA / WebStorm
export interface IErrorMessageAttrs {
  message: string;
  shinesthrough: string;
}

@component
export default class ErrorMessage extends st.component<IErrorMessageAttrs> {

  // sets a specific DOM tag name
  tag = 'my-error-message';

  // this attribute it not transparent; it doesn't 
  // shine through to the DOM, thus it's not being
  // rendered in the resulting HTML representation
  @attr
  message: string = "Hello, world!"; // default value
  
  // this attribute shines throuh in DOM
  @attr(AttrType.DOM_TRANSPARENT)
  shinesthrough: string = "someValueShinesThrough"; // default value
  
  render() {
      return <p>{ this.message }</p>;
  }
}

// st.render(<ErrorMessage message="foo" shinesthrough="yes" />);
//
// this would result in a DOM-rendered component like this:
//
// <my-error-message shinesthrough="yes">
// . <p>foo</p>
// </my-error-message>
```
{% endcode %}

**How do you use this component?** 

Either inside another component's `tsx` \(default\), or portal-like, somewhere in an existing DOM element:

{% code title="src/index.tsx" %}
```typescript
st.render(<ErrorMessage message="Happy error :)" />);
```
{% endcode %}

And then, a beautiful, clean DOM will be rendered:

{% code title="src/index.html at runtime \(in browser\): http://localhost:4444" %}
```markup
<body>
  ...
  <my-error-message shinesthrough="someValueShinesThrough">
    <p>Happy error :)</p>
  </my-error-message>
</body>
```
{% endcode %}

**External templates**

SpringType supports external template files. Such a template is just a replacement for a `render` function. It makes it possible to keep components free from TSX syntax, rendering the `render()` method pointless :-\) 

This feels much like Angular and helps to keep the code more clean when templates grow:

{% code title="src/page/login/login-page.ts" %}
```typescript
import { st } from "springtype/core";
import { component, attr } from "springtype/web/component";

// external TSX template import
import tpl from "./login-page.tpl";

@component({
    // passes the template function reference to the component  
    tpl
})
export default class LoginPage extends st.component {

    onLoginClick = (evt: MouseEvent) => {
        console.log('Login clicked...');
    }
}
```
{% endcode %}

External templates have a class instance scope-binding. You can access the component instance just like within the `render()` method:

{% code title="src/page/login/login-page.tpl.tsx" %}
```typescript
// class import to reference the component scope
import LoginPage from "./login-page";

export default (component: LoginPage) => (
    <fragment>
        <h1>Login</h1>
        <div class="container">

            Username:<br />
            <input type="text" name="username" />
            
            <br /><br />
            
            Password:<br />
            <input type="password" name="password" />
            
            <br /><br />

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

{% code title="src/component/functional-container-demo.tsx" %}
```typescript
import { tsx } from "springtype/web/vdom";
import { st } from "springtype/core";
import { component, Component, render } from "springtype/web/component";

// simplified functional component, just renders what is given
export const Container = component(
  render((container: Component) => {
    return <div>{container.renderChildren()}</div>;
  })
);

/*
st.render(
  <Container>
    <strong>Rendered inside container</strong>
  </Container>
);

would be rendered as:

<fnc-1>
  <div><strong>Rendered inside container</strong></div>
</fnc-1>
*/
```
{% endcode %}

For cases where you'd need closure states, you can return the `render` function after processing. The component instance handed in allows you access to attributes and the components standard lifecycle API:

{% code title="src/component/functional-clock.tsx" %}
```typescript
import { tsx } from "springtype/web/vdom";
import { st } from "springtype/core";
import { component, Component } from "springtype/web/component";

const getTime = () => new Date().toString();

export const Clock = component((clock: Component & {
    timeDisplay: HTMLElement
}) => {

    // closure-local scope state
    let now: string = getTime();

    // mouse event handler
    const updateUnixTime = (evt: MouseEvent) => {

        // update closure-local state
        now = getTime();

        // update display value
        clock.renderPartial(now, clock.timeDisplay!);
    };

    // update every second
    setInterval(updateUnixTime, 1000);

    // initially render current state
    return () => (
        <fragment>
            <button onClick={updateUnixTime}>Update time</button>
            <p ref={{ timeDisplay: clock }}>{now}</p>
        </fragment>
    );
}, 'clock' /* DOM tag name to use*/);
```
{% endcode %}

Rendering via `st.render(<Clock />)`  would result in a fully functional component, updating the time display every second. The HTML structure will e.g. look like this:

```markup
<clock>
  <button>Update time</button>
  <p>Sat May 02 2020 17:31:43 GMT+0200 (Mitteleuropäische Sommerzeit)</p>
</clock>
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

As already seen in section API, SpringType component can set and receive attributes. This feels much like React properties, but in fact, it's just passing properties to a class and, if desired, passing it down to the DOM element directly. using an additional flag:

{% code title="src/component/error-message-demo.tsx" %}
```typescript
  @attr(AttrType.DOM_TRANSPARENT)
  shinesthough: string = "defaultValue";
```
{% endcode %}

{% hint style="warning" %}
Attributes that are of type `AttrType.DOM_TRANSPARENT` should be named lowercase. The DOM can't handle `camelCase` attribute names, so using such a naming would lead to inconsistent set/get operations.
{% endhint %}

**Outer native DOM attributes**

Did you ever want to apply DOM attributes like `class`, `style`, `tabindex` and `id` on a component from outside? In SpringType, it can be done like this:

{% code title="src/index.tsx" %}
```typescript
st.render(<ErrorMessage message="What do you know :)" 
    id="foo1" 
    tabIndex={1} 
    class={['a', 'b']} 
    style={{
        color: '#cc0000'
    }} 
/>);
```
{% endcode %}

For sure this works for any other component, HTML and SVG element as well. `style` is typed and auto-completed with the standard `CSSDeclaration` interface; `class` allows for `string` and `array` values.
{% endtab %}

{% tab title="4. Children" %}
**Composition over Inheritance and Higher Ordering**

In SpringType we strongly promote the Composition over Inheritance pattern. Many years of experience show the complexity, OOP inheritance can bring. The same is true for the modern hype of writing higher order functions. This drives cyclomatic complexity. If you can, do compositing instead of inheriting or enhancing. 

One way of doing so, is writing small components that do one job well. Later, you can composite them:

```markup
<SimplePanel>
  <ErrorMessage message="Mayday, mayday!" />
</SimplePanel>
```

Writing such a `<SimplePanel />` component is pretty easy:

{% code title="src/component/simple-panel.tsx" %}
```typescript
import { component } from "springtype/web/component";
import { st } from "springtype/core";

@component
export default class SimplePanel extends st.component {

    render() {
      return this.renderChildren();
    }
}
```
{% endcode %}

Rendering the above TSX would result in the following HTML structure:

```markup
<simplepanel>
  <my-error-message shinesthrough="someValueShinesThrough">
    <p>Mayday, mayday!</p>
  </my-error-message>
</simplepanel>
```

However, what do we do, when there is a specific TSX markup and you'd like to render outer children at specific places? This is where Slots come into play.

**Slots**

Imagine that you want a `<Container />` component to render **outer children** at a specific place. This is what we know as only being supported with `<template>/<slot>` in web components. 

With SpringType, you have that in non-shadow DOM:

{% code title="src/index.tsx" %}
```typescript
st.render(
  <p>
    <ComplexPanel>
      <template slot={ComplexPanel.HEADER}>
        <div>MyHeader</div>
      </template>
      
      <template slot={ComplexPanel.FOOTER}>
        <div>MyFooter</div>
      </template>
          
      <span>MyContent</span>
    </ComplexPanel>
  </p>
);
```
{% endcode %}

{% code title="src/component/complex-panel.tsx" %}
```typescript
import { component } from "springtype/web/component";
import { st } from "springtype/core";
import { tsx } from "springtype/web/vdom";

@component
export class ComplexPanel extends st.component {

    static readonly HEADER = "header";    
    static readonly FOOTER = "footer";
    
    render() {
      return <div class="container">
          <div class="header">
              {this.renderSlot(ComplexPanel.HEADER, <p>Default header content</p>)}
          </div>
          {this.renderChildren(<p>Default content</p>)}
          <div class="footer">
              {this.renderSlot(ComplexPanel.FOOTER, <p>Default footer content</p>)}
          </div>
      </div>;
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="5. Updating UI " %}
**Updating existing DOM elements**

We had a fancy VDOM algorithm that would diff the whole DOM and only apply atomic change-sets, but we deprecated it due to its complexity. Our current implementation requires you to specify what must be applied where and when. This is one more line of code but massively improves readability and simplicity. 

In SpringType you can easily understand where, when and why the UI updates:

{% code title="src/component/error-message-demo.tsx" %}
```typescript
import { st } from "springtype/core";
import { tsx } from "springtype/web/vdom";
import { AttrType } from "springtype/web/component/interface";
import { component, attr } from "springtype/web/component";
import { ref } from "springtype/core/ref";

// this interface allows for an auto-completable 
// API when using the <ErrorMessage /> component 
// in a modern IDE like VS Code or IntelliJ IDEA / WebStorm
export interface IErrorMessageAttrs {
    message: string;
    shinesThrough: string;
}

@component
export class ErrorMessage extends st.component<IErrorMessageAttrs> {

    // sets a specific DOM tag name
    tag = 'my-error-message';

    // this attribute it not transparent; it doesn't 
    // shine through to the DOM, thus it's not being
    // rendered in the resulting HTML representation
    @attr
    message: string = "Hello, world!";

    // this attribute shines throuh in DOM
    @attr(AttrType.DOM_TRANSPARENT)
    shinesThrough: string = "someValueShinesThrough";

    @ref
    messageDisplayRef: HTMLParagraphElement;

    setMessage(message: string) {
        
        // update local state
        this.message = message;

        // trigger UI update
        this.renderPartial(this.message, this.messageDisplayRef);
    }

    render() {
        return <p ref={{ messageDisplayRef: this }}>{this.message}</p>;
    }
}
```
{% endcode %}

The method `this.renderPartial` is used to replace children of an already rendered DOM element by a new TSX template. Alternatively, you can update attributes using the native DOM API and `@ref` DOM element references.

**Refs and native DOM access**

TSX is internally transformed into HTML elements. While rendering, we know exactly, which element belongs to what component instance. So if you'd like to get hold of some native DOM element reference, you can do so by asking the renderer to remind a reference for you via `@ref` and `ref= {{ ... }}` 

```typescript
...
@component
export class ErrorMessage extends st.component<IErrorMessageAttrs> {

    ...
    
    @ref
    messageElementRef: HTMLElement;
    
    onAfterRender() {
      // native DOM element reference; updating the classList:
      this.messageElementRef.classList.add('black');
    }
}
```

This allows you to apply UI updates at highest speed, without any abstraction layer in between your code and the DOM while retaining the modern component architecture pattern.
{% endtab %}

{% tab title="6. CSS" %}
**CSS, CSS modules, Sass and Less**

You can easily import CSS, CSS as CSS modules \(auto-generates class names, scoped to your component\), Sass \(SCSS\) and Less files. They compile as you go and integrate, inline and optimize with zero configuration:

{% code title="error-message.tsx" %}
```typescript
// simply import (global, without CSS modules)
import "error-message.scss";

// with CSS module support
import styles from "error-message.scss";

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

