# Manually constructing html for hydrate

It's possible to manually generate the HTML necessary for `ReactDOM.hydrate`,
so as to enable serverside rendering of initial page load.
This is useful in situations where you don't have access to a native
`ReactDOMServer` module or library, for example C/C++ or Go, or if you won't or
can't use nodejs on the backend.

This is of course not very optimal and _will_ mean having to duplicate atleast
the static rendering of the html on both frontend and backend.

Note: This is not the officially supported way of doing this and
might break in future updates to the react core rendering.

### How it works

All that is really necessary is to replicate the basic structure of a rendered
react view, without newlines and indentation. Also to "mark" variable positions
where data were/is inserted by the javascript you have to use `<!-- -->`
before and after the value in the rendered html.

It's not strictuly necessary to follow the above rules since the `ReactDOM`
library will overwrite the differences, but it will also shout in the console.

### Example

```html
<div id="root"><div>Hello, <!-- -->World<!-- -->!</div></div>
```

```javascript
class Hello extends React.Component {
  render() {
    return (
      <div>
        Hello, {this.props.name}!
      </div>
    );
  }
}

ReactDOM.hydrate(
  <Hello name="World" />,
  document.getElementById('root')
);
```
