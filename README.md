# Dev Notes

This will be my collection of dev related notes which I can refer to quickly as needed.

If you come across these notes, I hope they help! Feel free to leave feedback or bug notes if you experience any.

## Sections

### CSS

- [Selectors Explained (tool)](https://hugogiraudel.github.io/selectors-explained/)
- [CSS Modules](https://glenmaddern.com/articles/css-modules)

### JavaScript

- [Exception Handling in JavaScript](https://github.com/farhanjiwani/dev-notes/blob/master/javascript/Exception_Handling.md)
- [Linting](https://github.com/farhanjiwani/dev-notes/blob/master/javascript/Linting.md)

#### React

- [Create React App From Scratch](https://github.com/farhanjiwani/dev-notes/blob/master/react/React-From_Scratch.md) &mdash; Create your react app without relying on a pre-packaged toolchain.
- [State Management with Hooks](https://github.com/farhanjiwani/dev-notes/blob/master/react/React-State_Management_with_Hooks.md) &mdash; `useReducer` and dispatchers

#### Performance

- use HTTP/2
- split vendor packages into their own bundle
  - [split react components](https://reacttraining.com/react-router/web/guides/code-splitting)
  - [webpack code splitting and dynamic imports](https://webpack.js.org/guides/code-splitting/#prefetchingpreloading-modules)
- [Performance Tips for Background Video](https://github.com/farhanjiwani/dev-notes/blob/master/html/React-From_Scratch.md)

### Tools

- npm
  - [Verdaccio](https://github.com/farhanjiwani/dev-notes/blob/master/tools/npm-Verdaccio.md) &mdash; A lightweight private npm proxy registry.
- [Webpack](https://github.com/farhanjiwani/dev-notes/blob/master/tools/Webpack.md) &mdash; An in-depth guide aimed for beginners.

## Articles

### React Articles

#### CSS-in-JS

- [The unseen performance costs of modern CSS-in-JS libraries in React apps](https://calendar.perfplanet.com/2019/the-unseen-performance-costs-of-css-in-js-in-react-apps/)

## Quick Reference

This section for notes that don't need their own files just yet.

### React Tips

#### Hooks

- [Thinking in React Hooks](https://wattenberger.com/blog/react-hooks)

#### Indeterminate Form Fields

> To add the indeterminate property to the checkbox, we need to take advantage of the `ref` attribute:
>
> ```javascript
> const { value, checked, indeterminate } = this.props
>
> return render(
>   <input
>     type="checkbox"
>     value={value}
>     checked={checked}
>     ref={el => el && (el.indeterminate = indeterminate)}
>   />
> );
> ```
>
> Since the `ref` is run on each render, the indeterminate property is updated appropriately, and thus the checkbox appears as expected.
>
> <cite>David Walsh &mdash; [React Indeterminate](https://davidwalsh.name/react-indeterminate)</cite>

### npm

```npmrc
# .npmrc
# Force custom registry for specifically scoped packages during `npm install`
@alfatrion:registry=http://localhost:4873/
```
