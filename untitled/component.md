---
description: 'Basic building blocks of any web app: Composition over inheritance.'
---

# Component

Let's imagine a simple web app called _My personal blog_. Such a web page obviously would have a page called "Blog". In SpringType, you'd implement such a page as component named `BlogPage` with the intention to render the blog posts you've written:

```jsx
import { st } from "springtype/core";
import { component } from "springtype/web/component";
import { ILifecycle } from "springtype/web/component/interface";

@component
export class BlogPage extends st.component implements ILifecycle {
    ...
}

```

{% hint style="info" %}
This is the most **verbose** way of writing a component. There are more concise ways and there is also a functional API to write such a component in only 1 line of code.
{% endhint %}

As you can see, a component in SpringType is named by its TypeScript class name. The [decorator](https://www.typescriptlang.org/docs/handbook/decorators.html) `@component` makes sure that SpringType recognizes it as a component.

Each component you write should only do one thing - like displaying blog posts and rendering them as their children \(so called "sub components"\). For example, a component `BlogPost` would only display its contents - title, date, author, content etc. but comments would be rendered and managed in other components.

To make things easier, safer and better readable, SpringType makes use of the TSX syntax - typed JSX, which means that you can use TypeScript class names as tags to tell SpringType how the DOM tree should look like in the end:

```jsx
...

@component
export class BlogPage extends st.component implements ILifecycle {
    render() {
        return <div>
            My latest blog posts:
            <BlogPost id="1" />
            <BlogPost id="2" />
        </div>
    }
}
```

As you can see, we're using the _composition pattern_ a lot: Components are wrapped into each other and such form a tree structure. Eventually, each component is transformed into an HTML element in the DOM tree and displayed in the browser.

## The Anatomy of a Component

As you could already grasp, components extend a class referred to as `st.component` . This is just a base class that implements the SpringType VDOM lifecycle. The lifecycle makes sure, the `render()` method is called to produce the DOM tree to render. 

{% hint style="info" %}
If you want your IDE to support you with API auto-complete further more, you can implement the interface `ILifecycle` for further guidance, but this is optional.
{% endhint %}

SpringType Components are powerful. They come with several traits of functionality:

* [x] **Attributes** - to provide parameters from the outside: e.g. the `@attr` `id` in  `<BlogPost id="1" />` tells `BlogPost` which post to display.
* [x] **Children** - to provide child components from the outside: `<BlogLayout><MyHeader />Some content</BlogLayout>` allows `BlogContent` to render and even transform the children directly.
* [x] **Slots** - a virtual slot implementation allows to give children from the outside direction to be rendered in specific places you define.
* [x] **Refs** - allow to reference to the real DOM node rendered by the VDOM safe and easily using `@ref`
* [x] **States** - you decorate class fields with `@state` to activate change detection on them - and your component magically re-renders for changes by reference and deep tree changes \(using `Proxy`\)
* [x] **Context** - Shared memory for real-time, high-speed inter-class data transfers using `@context` 
* [x] **Context States** - Shared memory with change detection and automatic re-rendering using `@contextState`
* [x] **Store** - Reactive integration with Redux stores and automatic re-rendering using `@store` and redux selectors

As you can see, there are many features that will simplify your life as a developer. Once you know them all, you'll be able to write whole, state-of-the-art web apps in a few lines of code. 

Unlike other frontend frameworks that break compatibility, SpringType also has built-in support for integrating with third-party JavaScript libraries like `jQuery`, `Bootstrap` etc. Our lazy VDOM algorithm can be disabled for certain subtrees and extending `st.staticComponent` allows you to one-time render components safely.

Here is another simple example of a component used with an attribute and a child element:

{% tabs %}
{% tab title="Usage" %}
```markup
<MyTag someAttribute="attributeValueGivenFromTheOutside">childText</MyTag>
```
{% endtab %}

{% tab title="my-tag.tsx" %}
```jsx
import { st } from "springtype/core";
import { component } from "springtype/web/component";
import { ILifecycle } from "springtype/web/component/interface";
import { tsx } from "springtype/web/vdom";

@component
export class MyTag extends st.component implements ILifecycle {

    @attr
    someAttribute: string = 'initialAttributeValue'
    
    render() {
        return this.renderChildren();          
    }
}

```
{% endtab %}

{% tab title="DOM in Browser" %}
```markup
<mytag>childText</mytag>
```
{% endtab %}
{% endtabs %}

As you can see, the **name** of the component class becomes **default DOM tag name in lowercase**. This behavior can be changed by adding and `tag` property to the component decorator `@component({ tag: 'my-tag' })`. 

You might also have noticed that the attribute wasn't rendered to the DOM. This is a default behavior because in most cases these are used internally and would just bloat the resulting DOM tree. However, you can tell `@attr` that you'd like to see them rendered as well using: `@attr(AttrType.DOM_TRANSPARENT)`

{% tabs %}
{% tab title="Usage" %}
```markup
<MyTag some="attributeValueGivenFromTheOutside">childText</MyTag>
```
{% endtab %}

{% tab title="my-tag.tsx" %}
```jsx
import { st } from "springtype/core";
import { component } from "springtype/web/component";
import { ILifecycle, AttrType } from "springtype/web/component/interface";
import { tsx } from "springtype/web/vdom";

@component({
    tag: 'my-tag'
})
export class MyTag extends st.component implements ILifecycle {

    @attr(AttrType.DOM_TRANSPARENT)
    some: string = 'initialAttributeValue'
    
    render() {
        return this.renderChildren();          
    }
}

```
{% endtab %}

{% tab title="DOM in Browser" %}
```markup
<my-tag some="attributeValueGivenFromTheOutside">childText</my-tag>
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Because the DOM API doesn't support uppercase letters, **attribute names and tag names** should always be named lowercase. In case of multi-word terms, use the dash \(`-`\) symbol to separate them.
{% endhint %}

## Lifecycle

## Attributes

## States

## DOM References

## Context

## Context State

## TSX

## Store



