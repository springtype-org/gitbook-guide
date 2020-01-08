# Component

A component in _SpringType_ represent an HtmlElement tag, that use TypeScript [decorators](https://www.typescriptlang.org/docs/handbook/decorators.html).

Simple Example component:

{% tabs %}
{% tab title="html" %}
```markup
<tag attribute="attributeValue">children</tag>
```
{% endtab %}

{% tab title="component" %}
```typescript
import { st} from "springtype/core";
import { component } from "springtype/web/component";
import { tsx } from "springtype/web/vdom";

@component({tag: 'tag'})
export class HomePage extends st.component {


    @attr
    attribute: string = 'attribute'
    
    render() {
        return this.renderChildren();          
    }
}

```
{% endtab %}
{% endtabs %}

## Lifecycle

## DOM Attribute

## State

## DOM Reference

## Context

## TSX



