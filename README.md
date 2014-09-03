react-native-listener
=====================

> A utility component to allow easy access to browser native events.

## Why?

React's uses [event delegation](https://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#under-the-hood-autobinding-and-event-delegation)
with a single event listener on `document`. While this is great if your entire application is inside React,
it's not so great if your React component is inserted into a page containing other event listeners. If an
event happens inside your React component, _**your component will be the last to hear of the event**_.
[Here is a JsFiddle to demonstrate](http://jsfiddle.net/erikras/bL5br9nb/).

If your problem is that you need to stop events leaking out of your React component to the rest of the page,
`<NativeListener>` is the solution.

## Installation

In your project dir:

```shell
npm install --save react-native-listener
```

## Usage

In your JSX file, simply wrap the element (only one!) you want to listen to with `<NativeListener>` and
put your event listener properties (e.g. `onClick`, `onKeyDown`) on `<NativeListener>` instead of on your element.

So, instead of this...

```jsx
/** @jsx React.DOM */
var MyComponent = React.createClass({
  onButtonClick: function(event) {
    // do something (event is React's [SyntheticEvent](https://facebook.github.io/react/docs/events.html) wrapper)
  },
  render: function() {
    return (
      <div>
        <button onClick={this.onButtonClick}>Click Me!</button>
      </div>
      );
  }
});
```
...do this...

```jsx
/** @jsx React.DOM */
var NativeListener = require('react-native-listener');
var MyComponent = React.createClass({
  onButtonClick: function(event) {
    // do something (event is native browser event)
  },
  render: function() {
    return (
      <div>
        <NativeListener onClick={this.onButtonClick}>
          <button>Click Me!</button>
        </NativeListener>
      </div>
      );
  }
});
```

**IMPORTANT: The event passed to your function is the native browser event, _NOT
React's [SyntheticEvent](https://facebook.github.io/react/docs/events.html) wrapper)_!!**

## Advanced Usage

By default, the `onClick`, `onKeyDown` event listeners fire on _bubble_. If you understand [the
difference between _bubble_ and _capture_](http://www.quirksmode.org/js/events_order.html) and
you really need to listen on capture, you can simply append `Capture` to your event property.
e.g. `onClickCapture`, `onKeyDownCapture`.

---

Module submitted by [Erik Rasmussen](http://erikras.com/) [@erikras](https://twitter.com/erikras)