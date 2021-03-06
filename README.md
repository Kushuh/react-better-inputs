# React Better Inputs

<h6 align="center">About</h6>
<p align="center">
    <a href="https://github.com/Kushuh/react-better-inputs">
        <img src="https://img.shields.io/npm/v/react-better-inputs" alt="version"/>
    </a>
    <a href="https://github.com/Kushuh/react-better-inputs/blob/master/LICENSE">
        <img src="https://img.shields.io/npm/l/react-better-inputs" alt="license"/>
    </a>
    <a href="https://github.com/Kushuh/react-better-inputs/issues">
        <img src="https://img.shields.io/github/issues-raw/kushuh/react-better-inputs" alt="open issues"/>
    </a>
    <a href="https://github.com/Kushuh/react-better-inputs">
        <img src="https://img.shields.io/github/last-commit/Kushuh/react-better-inputs" alt="activity"/>
    </a>
    <a href="https://github.com/Kushuh/react-better-inputs/graphs/contributors">
        <img src="https://img.shields.io/github/contributors/Kushuh/react-better-inputs" alt="contributors"/>
    </a>
</p>

<h6 align="center">More</h6>
<p align="center">
    <a href="https://github.com/Kushuh/react-better-inputs">
        <img src="https://img.shields.io/github/repo-size/kushuh/react-better-inputs" alt="repo size"/>
    </a>
    <a href="https://github.com/facebook/react">
        <img src="https://img.shields.io/github/package-json/dependency-version/Kushuh/react-better-inputs/react" alt="react version"/>
    </a>
    <a href="https://github.com/facebook/react/tree/master/packages/react-dom">
        <img src="https://img.shields.io/github/package-json/dependency-version/Kushuh/react-better-inputs/react-dom" alt="react version"/>
    </a>
    <a href="https://github.com/Kushuh/kushuh-react-utils">
        <img src="https://img.shields.io/github/package-json/dependency-version/Kushuh/react-better-inputs/kushuh-react-utils" alt="react version"/>
    </a>
    <a href="https://github.com/Kushuh/react-better-containers">
        <img src="https://img.shields.io/github/package-json/dependency-version/Kushuh/react-better-inputs/react-better-containers" alt="react version"/>
    </a>
</p>

## About

> React Better Inputs is an enhanced implementation of DOM inputs for React.

## Navigation

+ **[Components](#components)**
    + [Input](#input)
    + [ContentEditable](#contenteditable)
+ **[Copyright](#copyright)**

## Components

### Input

> Alternative implementation of \<input/> component.

```jsx
// Uncontrolled input.
<Input value={data.email}/>
```
ℹ️ Every \<input/> prop is compatible with this component.

ℹ️ Below is a list of parameters that either are specific to this component,
or that have a different implementation from the base component.

| Parameter | Type | Required | default | Description |
| :--- | :--- | :--- | :--- | :--- |
| onChange | Function | true | - | Change Handler. **(1)** |
| tag | string | - | 'input'<br/>'textarea' (with autosize = true) | A custom HTML tag to enforce (example 'textarea'). |
| autosize | boolean | - | false | Let textarea inputs sync their height automatically with the content inside. |
| preventPostComputing | boolean | - | false | Prevent post computing on update (such as reseting caret position). |
| keepOverflow | boolean | - | false | If content overflows maximum value, it will not be automatically cropped. |

 **(1)** onChange implementation is slightly different from the one in the DOM.
First, it runs asynchronously, for better sync with your modifications in the DOM.
Thus, postComputing operation (such as ensuring correct caret position, which can jump due to React implementation) will only occur once the value successfully updated.

Note it is your duty to ensure that any async operation remains as short as possible, to avoid some lags on user side.
Async implementation is only useful to enforce synchronization for short operation that can take a bit longer to fulfill. 
Avoid operations longer than 50ms (such as network calls).

onChange can be a classic sync function.

Other point of this implementation is that it passes some different arguments to your `onChange()` handler.
Instead of receiving an event, you receive the following arguments:

| Argument | Type | Description |
| :--- | :--- | :--- |
| value | string or number | Equivalent to evt.target.value . |
| options | object | Some useful information. |
| options.event | Event | The full evt object. |
| options.overflow | boolean | Custom flag to indicate value was updated above the maximum limit, before being capped. |

Note any custom onChange function should return a string, with an updated value.

```javascript
const onChange = async (value, options) => {
  // Do some stuff.
  
  return newValue;
};
```

### ContentEditable

Advanced contenteditable input component. React has poor contenteditable handling (in regard to the input implementation).
This component allows contenteditable to behave almost like a regular input, 
while handling complex child tree beneath (which is the goal of a contenteditable input).

```jsx
<ContentEditable
  maxLength={300}
  onChange={this.onChange}
  placeholder={'Enter your text.'}
>
  <Label>selector:</Label>my <span style={{color: 'blue'}}>text</span>
</ContentEditable>
```

| Parameter | Type | Required | default | Description |
| :--- | :--- | :--- | :--- | :--- |
| onChange | Function | true | - | Change Handler. **(1)** |
| placeholder | HTMLElement | - | - | placeholder. **(2)** |
| tag | string | - | 'div' | A custom HTML tag to enforce. |
| placeholderCss | string | - | - | Custom style for the placeholder. **(3)** |
| preventPostComputing | boolean | - | false | Prevent post computing on update (such as reseting caret position). |
| linear | boolean | - | false | Make the input linear (non linear input behaves like textarea component). |
| selection | object | - | - | Mount the component with an inner selection range. |
| selection.selectionStart | number | - | - | Range start. |
| selection.selectionEnd | number | - | - | Range end. |
| keepOverflow | boolean | - | false | If content overflows maximum value, it will not be automatically capped. |

**(1)** Receives a value as a parameter and isn't expected to return anything. It can be async, in which case postComputing will be postponed until value gets updated.

```javascript
const onChange = (value, options) => {
  // Do stuff.
};
```

| Argument | Type | Description |
| :--- | :--- | :--- |
| value | string or number | Equivalent to evt.target.value . |
| options | object | Some useful information. |
| options.event | Event | The full evt object. |
| options.overflow | boolean | Custom flag to indicate value was updated above the maximum limit, before being capped. |

**(2)** Placeholder can be a HTML element (div, etc).

**(3)** Not that unlike ::placeholder selector on input elements, a contenteditable placeholder has almost every customization options available, like a regular DOM element.

## Copyright
2020 Kushuh - MIT license