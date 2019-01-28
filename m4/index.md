# Module 4 React

*TODO:*
- [] finish calculator tutorial
- [] pass state setting to children
- [] add useReducer
- [] consider context to pass state setter to deep children
- [] host the final app somewhere
- [] add bonus: 2nd page that fetches countries from module 3 and renders a list of countries
- [] add questions

## 1 What is React
React is a technology for creating a the "view" of an applicaiton that is based on web technologies.

React is a framework that renders html from javascript. Related to react is the JSX language. JSX allows mixing javascript and html markup in the same file. React tools will transpile JSX files into React objects. The transpiled code is plain JavaScript, coReact is a technology for creating a the "view" of an applicaiton that is based on web technologies.

As mentioned before react is simply the "view" of an application. The other tools and frameworks that are required build an application with React can be called the React ecosystem. For this exercise, we will not spend much time reviewing all of the tools necessary to create an application with react. Instead we will focus on learning how to build a user interface using react.

## What React is not

## JSX

## Components
React projects are divided into components. Components allow developers to re-use and share code. Components also allow encapsulation of logic.

A component allows us to organize the logic (JavaScript) and display (HTML) into a single element.

There are 3 primary ways to write components:

1. Calling the react JavaScript functions directly.
1. Writing classes that inherit from React and implement a `render()` function which returns JSX.
1. Functional components that return JSX.

## Properties and 1-way data binding



## State

## Setup

Create a react project and update the version of react.
```bash
yarn create react-app m4
cd m4
yarn add react@next react-dom@next
```

Delete all of the files inside your `src` directory. Either with Windows Explorer or in the console:

```cmd
cd src
del *
```

Add a file `src/index.js`:
```jsx
import React from 'react';
import ReactDOM from 'react-dom';

// YOUR CODE HERE

ReactDOM.render(
  <Button />,
  document.getElementById('root')
);
```

## 2 Components

React allows us to organize our code into small resuable pieces called "components". Compentents are composed together to create complex user intefaces.

There several ways to write components. The tradidtional way is a class based component that extends `React.component`:

```jsx
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />
```

Components tell react what to render on the screen.

A few important points:
* A component takes a parementer named `props` and returns a hierarchy of views to display to the user in the `render` method
* The `render` function returns a _description_ of what you want to see on the screen. React uses that description and displays the result.
* `render` actually returns a **React element**, however, most developers use JSX (the HTML looking tags in the above example)
* JSX mixes HTML syntax and JavaScript! Inside JSX we can put _any_ JavaScript inside `{ }` braces.
* React elements are JavaScript script objects that you store in variables and pass around inside your app.

The JSX compiler transforms return of the `render` function above example into this:

```javascript
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

☝️ Here we are only rendering typical HTML elements (`<div>`, `<h1>`, etc.), but we can render other react components in the same way. For example we can now render the entire shopping list with `<ShoppingList />`. This is the magic of React!

## 3 Functional Components

While class components are very common, they require a lot of typing and have pitfalls around using the `this` pointer that require a extra care. A more modern way to write components is using a function syntax, also known as **Functional Components**.

**For this course, we will use functional components.**

The above component can be simplified by using functional syntax:

```jsx
const ShoppingList = (props) => (
  <div className="shopping-list">
    <h1>Shopping List for {props.name}</h1>
    <ul>
      <li>Instagram</li>
      <li>WhatsApp</li>
      <li>Oculus</li>
    </ul>
  </div>
);
```

☝️ The above _functional_ syntax requires less typing and removes some of the strange problems involving `this`, since we don't use it!

## 4 Calculator

We are going to build a small calculator appliction using React. Here is the goal:

![calculator](./images/calculator.jpg)

Inside `src/index.js` add the following code:

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

// YOUR CODE HERE

const Button = (props) => <button type="button">{props.name}</button>

ReactDOM.render(
  <Button name='1'/>,
  document.getElementById('root')
);
```

In the terminal run:
```cmd
npm start
```

The above command should open a window in Chrome with a singel button.

Open the Chrome developer tools.

Insepct the html generated on the page: TODO:...

Install the [React Developer Tools for Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=fr) and open the "React" tab in the developers tools.

#### Question 4.1 TODO:


Add new `KeyRow` component after the declaration of the `Button` component:

```jsx
const KeyRow = (props) => {
  const buttons = props.row.map(k => <Button name={k} />);
  return <div>{buttons}</div>;
}
```

Update the call to `ReactDOM.render()` to use the new `KeyRow` component:

```jsx
ReactDOM.render(
  <KeyRow row={['1', '2', '3']}/>,
  document.getElementById('root')
);
```

_Note:_
* The component is simply a function that can either return JSX directly `() => <div>...</div>` or can calculate components with JavaScript and return the JSX at the end of the function ☝️.
* Inside JSX, we can put any JavaScript inside the `{ }` curly braces.

The above code should result in this:

![keyrow](./images/keyrow.png)


#### Exercise 4.1 Add `KeyPad` component that takes property names `keys` that is a nested array of keys:
* the `keys` property should except a 2-dimensional array of keys, i.e. `[['1', '2', '3'], ['4', '5', '6']]`
* each array, i.e. `['1', '2', '3']` in the keys property should render a `KeyRow`
* the goal is to build on the previous components
* update your call to `ReactDOM.render()` to instantiate your `KeyPad`.

The above exercise should look like this:

![keypad](./images/keypad.png)

Next we will add a final `Calculator` component that will encapsulate all of the calculator logic, render the `KeyPad` component and the results.

```jsx
const Calculator = () => {
  const keys = [
    ['1', '2', '3', '+'],
    ['4', '5', '6', '-'],
    ['7', '8', '9', '='],
    ['C', '0']
  ];
  return ( 
    <div>
      <KeyPad keys={keys} />
      <h1>0</h1>
    </div>
  );
}
```

_Note:_
* the component has no properties so there is no `props` parameter

The results are ugly:

![calculator without css](./images/calculator-no-css.png)

We can fix the button alignment by adding some CSS style to our `Button` componenet:

```jsx
const Button = (props) => <button type="button" style={{fontSize: '2em', width: '3em'}}>{props.name}</button>
```

* Remember, in JSX we can put any JavaScript inside the `{ }` curly braces. Here we are putting an object `{fontSize: '2em'...}` inside the `{ }` curly braces. That is why there are double braces `{{...}}`.
* React lets us write CSS using JavaScript style camelCase. Instead of css `font-size` we use `fontSize`.

The results:

![calculator with css](./images/calculator-with-css.png)

## State

So far we have seen properties. Properties cannot be modified by a component, they are *immutable*. Properties let React efficiently decide when something needs to be rendered. If the properties have not changed, React does not need to re-render the component.

Components can also have state. State can be changed by a component. When the state changes, React re-renders the componenet. **React needs to know when state changes in order for it to decide if a component and it's children need to be re-rendered.** Therefore, when working with state, we have to make React aware.

There are of course multiple ways to manage state. In this course we will use React hooks. A new feature in React! **Hooks let us use functional components and work with state.**

To use hooks we need to import the hooks functions from React.

In `index.js`, modify `import React from 'react'` to:
```javascript
import React, {useState, useReducer} from 'react';
```

Next, modify the `Calculator` component to store the results in state:

```jsx
const Calculator = () => {
  const [results, setResults] = useState('0');
  const keys = [
    ['1', '2', '3', '+'],
    // ...
  ];
  // ...
  return // ...
}
```

The `useState()` function tells react we want to manage state. It takes a single parameter, the initial value of the state: `'0'` in our case. It has 2 return values, the current value of the state and function we must call to modify state. **We should never modify the state directly, instead we must modify the state through the function that React provides.** This keeps react aware of any changes in state, allow it to decide when something must be re-rendered.

The `setResults()` function will update the value of the state. However, it is only accessible inside the the `Calculator` component. However, we want to update the state when a `Button` is pressed (the `onClick` event).

Fortunatetly, we can pass the function as a property to a components children. One solution will be to pass the funtion through each child:

The Calculator:
```jsx
const Calculator = () => {
  const [results, setResults] = useState('0');
  const keys = [
    ['1', '2', '3', '+'],
    ['4', '5', '6', '-'],
    ['7', '8', '9', '='],
    ['C', '0']
  ];

  // Include setResults as a property to the KeyPad component
  return ( 
    <div>
      <KeyPad keys={keys} setResults={setResults} />
      <h1>{results}</h1>
    </div>
  );
}
```

The goal is to be able to access `setResults()` inside the `Button` component, therefore, each intermedaite component (`KeyPad` and `KeyRow`) must pass the function to it's children.

The verbose solution:

`KeyPad`:
```jsx
const KeyPad = (props) => {
  const rows = props.keys.map(r => <KeyRow keys={r} setResults={props.setResults} />);
  return <div>{rows}</div>;
}
```

☝️ However, we can take advantage of ES6 JavaScript, simply "relay" the properties using the `...` operator:
```jsx
const KeyRow = (props) => {
  const buttons = props.keys.map(k => <Button name={k} {...props} />);
  return <div>{buttons}</div>;
}

const KeyPad = (props) => {
  const rows = props.keys.map(r => <KeyRow keys={r} {...props} />);
  return <div>{rows}</div>;
}
```

_Finally..._ `Button`:
```jsx
const Button = (props) => <button
    type="button"
    style={{fontSize: '2em', width: '3em'}}
    onClick={() => props.setResults(props.name)}
  >
      {props.name}
  </button>
```

☝️ _Note: to set the state with the value of the button, we use `onClick={() => props.setResults(props.name)}`._

#### Exercise ??: Test the calculator in the browser. Clicking on a button should update the character displayed below the keypad.

Now we have an "interactive" keypad. There is still a problem. **The calculator is useless!** It does not calculate anything. Setting the key is not enough... we actually need _calculate_ something when a key is pressed.

One option would be to inspect the current state of the calculator inside the `Button` component (requiring us to pass the state to the children) and adding some logic to update the state as needed. _This would be a poor design, separating the state logic (in the `Button` component) from where it is stored (in the `Calculator` component)._

A better, more maintainable, design would be to perform the logic inside in the `Calculator` component. Fortuneatly, there is another type of React hook that allows us to do just that. A "reducer" in react a function that has tow prameters, the current state and an operation to perform on the state: `reducer(state, operation)`. The "reducer" should return the new state.

However, the calculator logic is more complex than simply storing a single number. We will need a more complex state. Something like:

```javascript
{
  current: '0',    // The current value
  next: null,      // The next value
  operation: null, // The operation to perform
}
```

Next we need a reducer function to contain our calculator logic. 

```javascript
function calculate(state, button) {
  // ...
}
```

For our calculator, the operation is the button that was pressed. The reducer should decide what to do based on the operation and must always return the new state (could be the same as the old state). The `switch` statement is perfect since it lets us "fall through" multiple buttons to the same logic.

Here is template of your `calculate` function.

```javascript
function calculate(state, button) {
  switch(button) {
    case '1':
    case '2':
    case '3':
    case '4':
    case '5':
    case '6':
    case '7':
    case '8':
    case '9':
    case '0':
      // TODO: RETURN THE MODIFIED STATE
      // return {current, next, operation};
      return state
    case '+':
    case '-':
      return {
        current: state.next || '0',
        next: null,
        operation: button,
      }
    case '=':
      // TODO: PERFORM THE CALCULATION AND REUTRN THE MODIFIED STATE
      // return {current, next, operation};
      return state;
    case 'C':
      // When C is presed, we reset the state
      return {
        current: '0',
        next: null,
        operation: null,
      }
    default:
      // If a button occured that we did not expect, don't change anything
      return state;
  }
}
```

In our `Calculator` component, we will replace our call to `useState` to `useReducer`:

```jsx
const Calculator = () => {
  const [state, buttonPressed] = useReducer(calculate, {current: '0', next: null, operation: null});
  console.log(state);  // Log the state for debugging

  const keys = [
    ['1', '2', '3', '+'],
    ['4', '5', '6', '-'],
    ['7', '8', '9', '='],
    ['C', '0']
  ];

  // NOTE! we pass buttonPressed to the KeyPad
  return ( 
    <div>
      // Pass the buttonPressed reducer to the children
      <KeyPad keys={keys} buttonPressed={buttonPressed} />
      <h1>{state.next || state.current}</h1>
    </div>
  );
}
```

We renamed the function that changes the state to `buttonPressed`. Update the `Button` component to call this function when the `onClick` event occurs:

```jsx
const Button = (props) => <button
    type="button"
    style={{fontSize: '2em', width: '3em'}}
    onClick={() => props.buttonPressed(props.name)}
  >
      {props.name}
  </button>
```

Open the console view in the Chrome developer tools and observe what happens when you press the button on the calculator. Specifically, the buttons `'+'`, `'-'`, and `'='`. For now the other buttons don't do anything.

#### Exercise 4.3: Complete the logic inside the calculate function to handle the buttons 0-9 and =.

## State Management

## Styling

Functionally, the calculator is now working, but it is _ugly_! We can easily add some style with CSS. One option would be to improve the `style` property of the `Button` component. Or we can use some preconfigured styling.

[Bootstrap]() from twitter provides some excellent CSS templates. We will only use the button styling, but Bootstrap offers so much more.

Add some more depenencies to our project.

```cmd
yarn add bootstrap jquery popper.js
```

Bootstrap requires jquery and popper.js, but they are not installed automatically. Therefor, we have installed them explicitly.

`src/index.js`:
```javascript
import React, {useReducer} from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap';
```

React lets us add CSS classes with the `className` property. **`class` is a reserved word in JavaScript, so always use `className` in Reat.** We will also simplify the previous `style` that we added:

```jsx
const Button = (props) => <button
    type="button"
    className='btn btn-outline-primary btn-lg'
    style={{width: '3em'}}
    onClick={() => props.buttonPressed(props.name)}
  >
      {props.name}
  </button>
```

Now we should have some beautiful style!

![calculator with bootstrap](./images/calculator.jpg)

Checkout the bootstrap docs for other possibilities, starting with the [Button docs](https://getbootstrap.com/docs/4.0/components/buttons/).

#### Exercise 4.4: Modify the CSS classes to give the calculator the following style.

![](./images/calculator-orange.jpg)














Clone this github repository: xxx

git clone xxx

cd xxx

npm run dev

The above repository is an application based on the next.js framework. Next.js is framework for working with react 
nsisting of classes and function calls.

As mentioned before react is simply the "view" of an application. The other tools and frameworks that are required build an application with React can be called the React ecosystem. For this exercise, we will not spend much time reviewing all of the tools necessary to create an application with react. Instead we will focus on learning how to build a user interface using react.

Clone this github repository: xxx

git clone xxx

cd xxx

npm run dev

The above repository is an application based on the next.js framework. Next.js is framework for working with react 
nsisting of classes and function calls.

As mentioned before react is simply the "view" of an application. The other tools and frameworks that are required build an application with React can be called the React ecosystem. For this exercise, we will not spend much time reviewing all of the tools necessary to create an application with react. Instead we will focus on learning how to build a user interface using react.

Clone this github repository: xxx

git clone xxx

cd xxx

npm run dev

The above repository is an application based on the next.js framework. Next.js is framework for working with react 
nsisting of classes and function calls.

As mentioned before react is simply the "view" of an application. The other tools and frameworks that are required build an application with React can be called the React ecosystem. For this exercise, we will not spend much time reviewing all of the tools necessary to create an application with react. Instead we will focus on learning how to build a user interface using react.

Clone this github repository: xxx

git clone xxx

cd xxx

npm run dev

The above repository is an application based on the next.js framework. Next.js is framework for working with react 

```bash
yarn create next-app module4
cd module4
npm run dev
```

Navigate to http://localhost:1234

