# Solid Element
[![Build Status](https://img.shields.io/travis/com/ryansolid/solid.svg?style=flat)](https://travis-ci.com/ryansolid/solid)
[![NPM Version](https://img.shields.io/npm/v/solid-element.svg?style=flat)](https://www.npmjs.com/package/solid-element)
![](https://img.shields.io/librariesio/release/npm/solid-element)
![](https://img.shields.io/npm/dt/solid-element.svg?style=flat)
[![Gitter](https://img.shields.io/gitter/room/solidjs-community/community)](https://gitter.im/solidjs-community/community)

This library extends [Solid](https://github.com/ryansolid/solid) by adding Custom Web Components and extensions to manage modular behaviors and composition. It uses [Component Register](https://github.com/ryansolid/component-register) to create Web Components and its composed mixin pattern to construct modular re-usable behaviors. This allows your code to available as simple HTML elements for library interopt and to leverage Shadow DOM style isolation. Solid already supports binding to Web Components so this fills the gap allowing full modular applications to be built out of nested Web Components. Component Register makes use of the V1 Standards and on top of being compatible with the common webcomponent.js polyfills, has a solution for Polyfilling Shadow DOM CSS using the ShadyCSS Parser from Polymer in a generic framework agnostic way (unlike the ShadyCSS package).

## Custom Elements

The simplest way to create a Web Component is to use the customElement method. The first argument is the Custom element tag, the 2nd is optional is the props, and the 3rd is the Solid template function. Solid template is provided State wrapped props as the first argument, and the underlying element as the 2nd.
```jsx
import { customElement } from 'solid-element';

customElement('my-component', {someProp: 'one', otherProp: 'two'}, (props, { element }) => {
  // ... Solid code
})
```
Props get assigned as element properties and hyphenated attributes. This exposes the component that can be used in HTML/JSX as:
```html
<my-component some-prop="some value" other-prop="some value"></my-component>
```

This is all you need to get started with Solid Element.

## Examples

[Web Component Todos](https://wc-todo.firebaseapp.com/) Simple Todos Comparison

## Hot Module Replacement (new)

Solid Element exposes Component Register's Hot Module Replacement solution for Webpack and Parcel. It does not preserve state, swapping Components that are changed and their descendants. This approach is simple but predictable. It works by indicating the component to be Hot Replaced with the `hot` method in your file.

```js
import { customElement, hot } from 'solid-element';

hot(module, 'my-component');
```
This is a new feature that is actively seeking feedback. Read more: [Component Register](https://github.com/ryansolid/component-register#hot-module-replacement-new)

There is also a webpack loader that handles adding this automatically. Check out [Component Register Loader](https://github.com/ryansolid/component-register-loader)

## withSolid

Under the hood the customElement method is using Component Register's mixins to create our Custom Element. So this library also provides the way to do so directly if you wish to mixin your own functionality. It all starts by using the register HOC which upgrades your class or method to a WebComponent. It is always the start of the chain.

```jsx
import { register } from 'component-register';

/*
register(tag, defaultProps)
*/
register('my-component', {someProp: 'one', otherProp: 'two'})((props, options) =>
  // ....
)
```

Component Register exposes a convenient compose method (a reduce right) that makes it easier compose multiple mixins. From there we can use withSolid mixin to basically produce the Component method above. However, now you are able to add more HOC mixins in the middle to add additional behavior in your components.

```jsx
import { register, compose } from 'component-register';
import { withSolid } from 'solid-element';

/*
withSolid
*/
compose(
  register('my-component'),
  withSolid
)((props, options) =>
  // ....
)
```