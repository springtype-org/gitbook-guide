# Component

A component in _SpringType_ represent an HtmlElement tag, that use TypeScript [decorators](https://www.typescriptlang.org/docs/handbook/decorators.html). An HtmlElement include a namespace, tagname, attributes and _optional_ nested child elements. 

_SpringType_ components are **classes** that _**extend**_ from `st.component` and are decorated with`@component`.

{% tabs %}
{% tab title="TSX in other component" %}
```markup
<Tag attribute="attributeValue">children</tag>
```
{% endtab %}

{% tab title="component" %}
```typescript
import { st } from "springtype/core";
import { component } from "springtype/web/component";
import { tsx } from "springtype/web/vdom";

@component()
export class Tag extends st.component {

    @attr
    attribute: string = 'attributeValue'
    
    render() {
        return this.renderChildren();          
    }
}

```
{% endtab %}
{% endtabs %}



The **name** of a component class is the **default tagname**. It can be changed by adding and _tag_ property to the component decorator `@component({tag: 'test'})`.  The tagname of every component can be overridden by the special attribute tag.

{% hint style="warning" %}
**CSS / SASS / SCSS selectors** have to be **always lowercase**, because the DOM API doesn't support uppercase letters.
{% endhint %}

{% tabs %}
{% tab title="TSX" %}
```markup
<Tag tag="TEsT" attribute="attributeValue">children</tag>
```
{% endtab %}

{% tab title="Browser \(Rendered\)" %}
```markup
<test attribute="attributeValue">children</test>
```
{% endtab %}
{% endtabs %}



## Lifecycle

## DOM Attribute

## State

## DOM Reference

## Context

## TypeScriptXML TSX / JavaScriptXML JSX



