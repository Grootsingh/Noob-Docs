> ## React version: 18.2.0

# 1. Context-Module-pattern

to understand we have to go through an ex:

```js
// src/context/counter.js

//created a context

const CounterContext = React.createContext();

function CounterProvider({ step = 1, initialCount = 0, ...props }) {
  //create a reducer

  const [state, dispatch] = React.useReducer(
    (state, action) => {
      const change = action.step ?? step;
      switch (action.type) {
        case "increment": {
          return { ...state, count: state.count + change };
        }
        case "decrement": {
          return { ...state, count: state.count - change };
        }
        default: {
          throw new Error(`Unhandled action type: ${action.type}`);
        }
      }
    },
    { count: initialCount }
  );

  // we are providing reducer state and dispatch function directily to the childrens

  const value = [state, dispatch];
  return <CounterContext.Provider value={value} {...props} />;
}

// just a context value checker

function useCounter() {
  const context = React.useContext(CounterContext);
  if (context === undefined) {
    throw new Error(`useCounter must be used within a CounterProvider`);
  }
  return context;
}

export { CounterProvider, useCounter };

//-----------------------------------------------------
// src/screens/counter.js

// here we are using reducer that we have passed throught the context value
import { useCounter } from "context/counter";

function Counter() {
  const [state, dispatch] = useCounter();

  // here we have to use dispatch to give action to our reducer to do the task?
  // here is the problem that we are going to solve?-------

  const increment = () => dispatch({ type: "increment" });
  const decrement = () => dispatch({ type: "decrement" });
  return (
    <div>
      <div>Current Count: {state.count}</div>
      <button onClick={decrement}>-</button>
      <button onClick={increment}>+</button>
    </div>
  );
}
//-----------------------------------------------------
// src/index.js
import { CounterProvider } from "context/counter";
import Counter from "screens/counter";

function App() {
  return (
    <CounterProvider>
      <Counter />
    </CounterProvider>
  );
}
```

## Problem with this simple approch?

- when you have a sequence of dispatch functions.

it becomes t-di-us when you have to repeat the sequence at multiple places.
which become more error pron becouse dispatch take action string;

## temp solution is to use a healper function to incapsulate the dispatch

```js
// src/context/counter.js

// created a context

const CounterContext = React.createContext();

function CounterProvider({ step = 1, initialCount = 0, ...props }) {
  // create a reducer

  const increment = React.useCallback(
    () => dispatch({ type: "increment" }),
    [dispatch]
  );
  const decrement = React.useCallback(
    () => dispatch({ type: "decrement" }),
    [dispatch]
  );

  // here we are sending increment, decrement helper fn rather then sending dispatch directly. we are incapsulating the dispatch (sequence) and send the healper.

  const value = { state, increment, decrement };
  return <CounterContext.Provider value={value} {...props} />;
}
// now users can consume it like this:

const { state, increment, decrement } = useCounter();
```

here the problem is solved, healper fn do the job write but for creating healper
fn we need to use useCallback hook so the hook don't re-render. still on every
render react will do comapre the depandency of useCallback hook. which can be
saved by simply useing heaper + Modules.

## Context + (Heaper Fn + Modules) = Context Module Pattern

> final solution

```js

// src/context/counter.js
const CounterContext = React.createContext()

function CounterProvider({step = 1, initialCount = 0, ...props}) {
  const [state, dispatch] = React.useReducer(
    (state, action) => {
      const change = action.step ?? step
      switch (action.type) {
        case 'increment': {
          return {...state, count: state.count + change}
        }
        case 'decrement': {
          return {...state, count: state.count - change}
        }
        default: {
          throw new Error(`Unhandled action type: ${action.type}`)
        }
      }
    },
    {count: initialCount},
  )

  const value = [state, dispatch]

  return <CounterContext.Provider value={value} {...props} />
}

function useCounter() {
  const context = React.useContext(CounterContext)
  if (context === undefined) {
    throw new Error(`useCounter must be used within a CounterProvider`)
  }
  return context
}

// here we are creating healper fn in open not inside CounterProvider fn and export these helper fn with export and import modules which saves us un-nessesory useCallback depandency check on every re-render of CounterProvider fn.

// in helper fn we are taking dispatch fn as a argument and inside helper we provide the dispatch it's argunment.

// before that we are taking distach and it's argunment (complete) as a argunment.

const increment = dispatch => dispatch({type: 'increment'})
const decrement = dispatch => dispatch({type: 'decrement'})

- we export { counterProvider : <CounterContext.Provider value={value} {...props} />
              useCounter:  const [state, dispatch] = useCounter(),
              increase: increment(dispatch),
              decrease: decrease(dispatch)
            }

// helps us reducer errors and makes the encapsulation of the component more flexible.


export {CounterProvider, useCounter, increment, decrement}
//------------------------------------------------------
// src/screens/counter.js
import {useCounter, increment, decrement} from 'context/counter'

function Counter() {
  const [state, dispatch] = useCounter()
  return (
    <div>
      <div>Current Count: {state.count}</div>
      <button onClick={() => decrement(dispatch)}>-</button>
      <button onClick={() => increment(dispatch)}>+</button>
    </div>
  )
}

```

### summerly:

`create a component with Context + (Heaper Fn + Modules) + reducer and export`

1.  Contextproivder to wrap the component.
2.  Context Value will be reducer [state,dispatch]
3.  healper fn for performing actions, helper fn incapsulate the functionality
    of dispatch and provider smaller code which makes this utility to be more
    easy to use less error pron.

---

# 2. Compound Components

subTopic you need to learn before understanding Compound Component.

1. Childen API
2. Clone.Element()

## Children API

---

Children api is similer to Array data structure API but specific for react
Children props.

Children API Contain many mathods that you can use over the children props of
any component.

mrthods are:

1. React.Children.map(children, function , thisArg): This method maps over the
   children of a React component and returns a new array with the results of
   calling the specified function on each child element.

2. React.Children.forEach(children, function, thisArg): This method iterates
   over the children of a React component and calls the specified function on
   each child element.

3. React.Children.count(children): This method returns the total number of
   children in a React component.

4. React.Children.only(children): This method returns the only child in a React
   component. If the component has more than one child or no children at all, an
   error is thrown.

5. React.Children.toArray(children): This method returns an array of the
   children of a React component. This can be useful for manipulating the
   children array directly.

6. React.isValidElement(object): This method returns true if the specified
   object is a valid React element.

Note: that these methods are part of the React.Children namespace, so they need
to be accessed using React.Children.methodName().

- fist parameter "Children" is the children props which we use in
  react.createElement we need to refer that.

- second parameter "Fn" is same as array methods takes

  1. Children (chid prop) as a array
  2. index

- third parameter is of "this" to tell which object our Fn is refering
  too.(Optional) (was required in class based react app, you can igonre it in
  function based)

## why use Children Api methods when you can directly use Array Methods over child props

- The React.Children.map() method can handle both single child elements and
  arrays of child elements, while the children.map() method can only handle
  arrays. This means that if your component has a single child element, you can
  still use the React.Children.map() method without having to check if the
  children prop is an array or not.

---

# Clone.Element()

---

cloneElement lets you create a new React element using another element as a
starting point.

const clonedElement = cloneElement(element, props, ...children)

## Parameters

1. `element`: The element argument must be a valid React element. For example, it
   could be a JSX node like <Something />, the result of calling createElement,
   or the result of another cloneElement call.

2. `props`: The props argument must either be an object or null. If you pass null,
   the cloned element will retain all of the original element.props. Otherwise,
   for every prop in the props object, the returned element will “prefer” the
   value from props over the value from element.props. The rest of the props
   will be filled from the original element.props. If you pass props.key or
   props.ref, they will replace the original ones.

3. `optional ...children`: Zero or more child nodes. They can be any React nodes,
   including React elements, strings, numbers, portals, empty nodes (null,
   undefined, true, and false), and arrays of React nodes. If you don’t pass any
   ...children arguments, the original element.props.children will be preserved.

## Returns

cloneElement returns a React element object with a few properties:

- type: Same as element.type.
- props: The result of shallowly merging element.props with the overriding props
  you have passed.
- ref: The original element.ref, unless it was overridden by props.ref.
- key: The original element.key, unless it was overridden by props.key.

Usually, you’ll return the element from your component or make it a child of
another element. Although you may read the element’s properties, it’s best to
treat every element as opaque after it’s created, and only render it.

## My observation

as we know when we create an element with createElement in react it return an
object

```js
React.createElement(type, props, children);
```

```js
<div id="root">Hello world</div>
{
  type: "div",
  key: null,
  ref: null,
  props: { id: "root", children: "Hello world" },
  _owner: null,
  _store: {}
}
```

so when we clone an element we take the same component type and either add or
replace props and overWrite children

```js
//ex:
import { cloneElement } from "react";

const clonedElement = cloneElement(
  <Row title="Cabbage">Hello</Row>,
  { isHighlighted: true },
  "Goodbye"
);
```

- previous element/component = <Row<span></span> title="Cabbage"> Hello </Row<span></span>>

```js
Row = {
type: ƒ Row() {}
key: null
ref: null
props: {
    title: "Cabbage",
    children: "Hello"
    }
_owner: null
_store: Object
}

```

- new Props = { isHighlighted: true }
- new Children = 'Goodbye'

```js
new Row/Cloned Row = {
   type: ƒ Row() {}
key: null
ref: null
props: {
    title: "Cabbage",
    isHighlighted: true,
    children: "Goodbye"
    }
_owner: null
_store: Object
}
```

- as you can notice porps either get re-asigined if porp already exist or new
  prop is added to the cloned element and children are always
  replaced/overWritten

- this is normal this is how an object works in javascript.

## Note

- Using cloneElement is uncommon and can lead to fragile code.

the reson behind making the code fragile is that useing cloneElement makes it
harder to keep track of the data flow. which makes it hard to debugg your code.

---

## now underStand Compound component

Compound components is a design pattern in React that allows you to create a set
of components that work together to achieve a common goal ( to create a utility
UI). A compound component is made up of multiple smaller components that are
closely related to each other and work together to create a more complex
component.

In a compound component, there is usually one main component (Parent) that acts
as a container for the other components. This main component is responsible for
rendering the other components and passing them the necessary props.(handle all
the states and pass them to smaller/Children component implicitly )

One of the benefits of using a compound component is that it provides a simple
and intuitive API for users of your component. Instead of having to manage
multiple components themselves, they can use a single component and rely on the
compound component to manage the details.

lets take an ex: to understand Compound component.

```js
import * as React from "react";
import { Switch } from "../switch";

// Parent Component (manage all the States at top level and share them implicitly to it's children )

function Toggle({ children }) {
  const [on, setOn] = React.useState(false);
  const toggle = () => setOn(!on);

  // we are usinng React.Children.map to itr over all of the children of the Parent Component

  // Parent Component = fn Toggle

  // children Component = [ToggleOn, ToggleOff, ToggleButton]

  // we are using React.cloneElement to pass the props to all the childrens implicitly

  // props = {on, toggle}

  return React.Children.map(children, (child) => {
    let newChild = React.cloneElement(child, { on, toggle });
    return newChild;
  });
}

// ToggleOn, ToggleOff, ToggleButton are Children component you can see in APP component they are nested in the Toggle (parent) componet.

// here in all the chidlren we are accessing the props that we have shared from the parent to the childrens.

// props were = {on, toggle}

const ToggleOn = ({ on, children }) => (on ? children : null);

const ToggleOff = ({ on, children }) => (on ? null : children);

const ToggleButton = ({ on, toggle }) => <Switch on={on} onClick={toggle} />;

// whatever you put between Toggle component it becomes a child of the Toggle and those props will automaticly be shared with them.

function App() {
  return (
    <div>
      <Toggle>
        <ToggleOn>The button is on</ToggleOn>
        <ToggleOff>The button is off</ToggleOff>
        <ToggleButton />
      </Toggle>
    </div>
  );
}
export default App;
```

compound Composition require a setup of

- parent who mainatian all the states uses (React.Children.map,
  React.cloneElement) to share props to it's children.

- then utilise those props in children.

## limitation

only be able to share props to 1 level deep, only to the children.

## Note

i think you should not use Compound component becouse the props that the parent
share to children are Implicit in nature, even though it can be made explicit if
you want , due to implicit nature props we are giving to Children component will
be givin to all the children of the parent component which can be problomatic if
not carefull.

but the banifit of compound component also comes with the implicit nature of the
Pattern.

## Side quest: 1 (HTML tag as a child)

if you add HTML tag as a child of the parent them those props will be shared to
the HTML tag and as HTML can't understand those props React will thow an error
to solvew this simple check:

typeof child.type === "string" in React.Chidlren.map()

```js
return React.Children.map(children, (child) => {
  if (typeof child.type === "string") return child;

  let newChild = React.cloneElement(child, { on, toggle });
  return newChild;
});
```

## Side quest: 2 (make it Expilit)

you can make an array of all the childen and share that array and check if child
is === to anyof the array element.

```js
return React.Children.map(children, (child) => {
  if (ExpilitFn.includes(children)) {
    let newChild = React.cloneElement(child, { on, toggle });
    return newChild;
  }
});
```

```js
const ExplitFn = [ToggleOn, ToggleOff, ToggleButton];
```

## 3. Flexible Compound Components (Imporved oversion of Compound Component)

`Compound Component + share props to anyLevel (useContext) + Explicit
(useContext)`

in flexible Compound Component we use useContext to share props to any level
Deep Child component and due to useContext it become explicit.

we don't use (React.Children.map, React.cloneElement) to share props

lets take an Ex:

```js
import * as React from "react";
import { Switch } from "../switch";

// here simply we created a context

const ToggleContext = React.createContext();

function Toggle({ children }) {
  const [on, setOn] = React.useState(false);
  const toggle = () => setOn(!on);

  // we pass props to value which will be shared to all the children who ever consume with useContext

  // wrapping arround childen makes it share state to all the childrens  at any level this is how Context works. pritty cool hehh...

  // on the other hand, (React.Children.map, React.cloneElement) only manage to share one level deep.

  return <ToggleContext value={{ on, toggle }}>{children}</ToggleContext>;
}

// now we just use useContext to get the value that we have passed from the parent wrapper to chidlren to get the props out.

function useToggle() {
  return React.useContext(ToggleContext);
}

function ToggleOn({ children }) {
  const { on } = useToggle();
  return on ? children : null;
}

function ToggleOff({ children }) {
  const { on } = useToggle();

  return on ? null : children;
}

function ToggleButton({ props }) {
  const { on, toggle } = useToggle();
  return <Switch on={on} onClick={toggle} {...props} />;
}

// here <ToggleButton /> can not be access props with compound component becouse it is nested more then 1 level deep from the Parent (Toggle component) so we need useContext. to go beyound 1 level or any level deep.

function App() {
  return (
    <div>
      <Toggle>
        <ToggleOn>The button is on</ToggleOn>
        <ToggleOff>The button is off</ToggleOff>
        <div>
          <ToggleButton />
        </div>
      </Toggle>
    </div>
  );
}

export default App;
```

## side quest: provide display name to Context for inspect Component

when you inspect your Context in inspect React in dev tool you will see
Context.Provider which is less imformative (which context is this context
provider blong too)

```js
const ToggleContext = React.createContext();
ToggleContext.displayName = "ToggleContext";
```

now ToggleContext will be displayed rather then Context.Provider which makes it
more imformative.

---

## Props collection (Object) and Getters (Function) (custom hook pattern)

### Props collection (Object)

In React, a prop is a way to pass data from a parent component to a child
component. Prop collections are simply a collection of props that are passed
down to a child component.

Prop collections can be created by defining an object with multiple key-value
pairs, where each key represents the name of a prop and its corresponding value
is the data that is being passed down to the child component.

Using prop collections can make the code cleaner and more organized, especially
when passing multiple props to a component. It can also make it easier to pass a
group of related props, as they can be grouped together in a single object.
Additionally, prop collections can make it easier to add or remove props, as
they can be modified as a single unit.

we use props collection to group together closly related props in a single
object so that it will be easer to organise the props and we don't forget group
of props which are generally works/ require to be applied together.

Ex:

```js
import * as React from "react";
import { Switch } from "../switch";

function useToggle() {
  const [on, setOn] = React.useState(false);
  const toggle = () => setOn(!on);

  // here we are grouping together toggerProps becouse there props are generaly require to be used together.

  return { on, toggle, togglerProps: { "aria-pressed": on, onClick: toggle } };
}

// here is <Switch on={on} {...togglerProps} /> we are sharing these grouped props which we used together.

function App() {
  const { on, togglerProps } = useToggle();
  return (
    <div>
      <Switch on={on} {...togglerProps} />
      <hr />
      <button aria-label="custom-button" {...togglerProps}>
        {on ? "on" : "off"}
      </button>
    </div>
  );
}

export default App;
```

## limitation of prop collection

props collection does not provided the flexibility to the prop user to use there
own custom props, and only one prop will work either the prop collection object
provided by parent or the custom child prop becouse object can not have two keys
with the same name due to that we can not have both props (child custom porp and
parent collection props object) together.

Ex:

```js
function App() {
  const { on, togglerProps, toggle } = useToggle();
  return (
    <div>
      <Switch on={on} {...togglerProps} />
      <hr />
      <button
        aria-label="custom-button"
        onClick={() => console.info("onButtonClick")}
        {...togglerProps}
      >
        {on ? "on" : "off"}
      </button>
    </div>
  );
}
```

in this Ex: look at the button Tag

```js
 <button
        aria-label="custom-button"
        onClick={() => console.info('onButtonClick')}
        {...togglerProps}
      >
```

we are using our old prop Collection (togglerProps) object which contain two
property togglerProps: {'aria-pressed': on, onClick: toggle} and button want to
use it's own onClick event (not the one that is provided by the togglerProps) as
we spread the togglerProps onClick will be overwritten by the togglerProps
object and only one onClick will work. there is no way around that both of the
onClick work here untill we use Props Getters.

props collection don't have the flexibility that a child can specify its own
values for certain props.

## Props Getters (Function)

props getter use fn which eleminate the problem of overwritting coz by Object.

props getter also provide the flexibility that a child can specify it's own
value for certain props

Ex:

```js
import * as React from "react";
import { Switch } from "../switch";

function useToggle() {
  const [on, setOn] = React.useState(false);
  const toggle = () => setOn(!on);

  //-------------------- original from the kent C Dodds --------------------------------
  // function callALL(...Fns) {
  // ...Fns in parameter act as rest operator not spread and rest will collect all the
  //  parameters can create an array.
  //   return (...arg) => {
  //     Fns.forEach(fn => fn?.(arg))
  //   }
  // }

  function getTogglerProps({ onClick, ...props } = {}) {
    return {
      "aria-pressed": on,
      onClick: () => callALL(onClick, toggle),
      ...props,
    };
  }

  //----------------------- My version -----------------------------------------

  // callAll take the array of all the Fns that we want to Compose together, check if the child has provided the fns (onClick) or not , if it's provided then we will execute it with the toggler fn,if not then otherwise we only trigger toggle fn of ours (from the parent).

  function callALL(Fns) {
    Fns.forEach((fn) => fn?.());
  }

  //------ getTogglerProps version 2 when you have too many props to compose togther --------
  // here we are reciving props from the children and seprating onClick sepratly so that we can compose it together to our onClick so that both onClick works together.

  // getTogglerProps returns an object which contain props that the parent want to send and the props that child want to use, in this ex: we are composing [onClick, toggle] functions when child trigger onClick callAll will execute

  function getTogglerProps({ onClick, ...props } = {}) {
    return {
      "aria-pressed": on,
      onClick: () => callALL([onClick, toggle]),
      ...props,
    };
  }

  //------ getTogglerProps version 1 when you have 1 or 2 props to compose togther --------

  function getTogglerProps({ onClick, ...props } = {}) {
    return {
      "aria-pressed": on,
      onClick: () => {
        onClick?.();
        toggle();
      },
      ...props,
    };
  }

  //---------------------------------------------------------------------------
  return {
    on,
    toggle,
    togglerProps: { "aria-pressed": on, onClick: toggle },
    getTogglerProps,
  };
}

// in <button> now we are using getTogglerProps() which is a function that we are passing as a prop from the parent to the child.

// getTogglerProps() take an object of key and value (props) that child want to apply on the button and getTogglerProps() recive that object and compose it together with the props that Parent want to send.

// we use spread operator on getTogglerProps() which returns an object.
//   <button
//    {
//      'aria-pressed': on,
//      onClick: () => callALL([onClick, toggle]),
//      'aria-label': 'custom-button',
//    }

// now in parent both onClick are compose together so that both works.

function App() {
  const { on, getTogglerProps } = useToggle();
  return (
    <div>
      <Switch {...getTogglerProps({ on: on })} />
      <hr />
      <button
        {...getTogglerProps({
          "aria-label": "custom-button",
          onClick: () => console.info("onButtonClick"),
        })}
      >
        {on ? "on" : "off"}
      </button>
    </div>
  );
}

export default App;
```

- only place prop getter shine over prop collection is that it is better at
  compostion of props, you can run multiple same name props together without
  overWritting isssues.

## State Reducer Pattern (inversion of Control)

In software development, inversion of control refers to a design principle where
a component's dependencies are "injected" into it from the outside, rather than
being created internally by the component itself. This allows the component to
be more flexible and easier to customize, since its behavior can be modified by
changing its dependencies rather than having to modify its internal
implementation.

The State Reducer pattern with React Hooks can be seen as a specific
implementation of the IoC principle, where the reducer function that manages
state is passed in as a prop to the component, rather than being created
internally. This allows the component to expose a customizable "state reducer"
API to the user, which can be used to modify the behavior of the component.

For example, imagine a component that manages a list of items. Instead of
tightly coupling the internal state of the component with its logic for adding
and removing items from the list, the component could expose a "state reducer"
prop that takes a reducer function as input. This reducer function could be used
to customize the behavior of the component, for example by filtering the list
based on certain criteria or modifying the list in other ways.

By using this IoC approach, the component becomes more flexible and easier to
reuse, since its behavior can be modified by changing its dependencies (i.e. the
reducer function) rather than having to modify its internal implementation. This
can make the component more useful and adaptable to a wider range of use cases.

## My Observation

`(useReduer + invesion of Control)` allow you to create universaly useable
component that are highly customizable for the user rather then creating an API
At author level which contain all the posibile side and use cases a user might
want to use the API for.

`(useReduer + invesion of Control)` API allow us to create an API which is small
in size and contain only the nessesory code that can be used by wide range of
user rather then having all the possible side cases which select few of the user
ever uses and make the API bulkie in Size.

Ex: When you want to provide your own Complete reducer fn

```js
import * as React from 'react'
import {Switch} from '../switch'

const callAll =
  (...fns) =>
  (...args) =>
    fns.forEach(fn => fn?.(...args))

- this is our Default reducer fn that we have created

function toggleReducer(state, {type, initialState}) {
  switch (type) {
    case 'toggle': {
      return {on: !state.on}
    }
    case 'reset': {
      return initialState
    }
    default: {
      throw new Error(`Unsupported type: ${type}`)
    }
  }
}

// how we make an API that allow invasion of control

// we allow user to provide there own version of reducer method so they can customize the behaviour as they like, and those user who don't want to customize the behavious we provide a default reducer so that they don't need to worry about it.

// as you can see function useToggle({reducer = toggleReducer} = {}) we are allowing the user to share there form of reducer we pass this reducer to our useReducer Hook

function useToggle({initialOn = false, reducer = toggleReducer} = {}) {
  const {current: initialState} = React.useRef({on: initialOn})

// here we are passing that reducer fn

  const [state, dispatch] = React.useReducer(reducer, initialState)
  const {on} = state

  const toggle = () => dispatch({type: 'toggle'})
  const reset = () => dispatch({type: 'reset', initialState})

  function getTogglerProps({onClick, ...props} = {}) {
    return {
      'aria-pressed': on,
      onClick: callAll(onClick, toggle),
      ...props,
    }
  }

  function getResetterProps({onClick, ...props} = {}) {
    return {
      onClick: callAll(onClick, reset),
      ...props,
    }
  }


  return {
    on,
    reset,
    toggle,
    getTogglerProps,
    getResetterProps,
  }
}

function App() {
  const [timesClicked, setTimesClicked] = React.useState(0)
  const clickedTooMuch = timesClicked >= 4

// this is the "toggleStateReducer" reducer function that user want to uses over our API


  function toggleStateReducer(state, action) {
    switch (action.type) {
      case 'toggle': {
        if (clickedTooMuch) {
          return {on: state.on}
        }
        return {on: !state.on}
      }
      case 'reset': {
        return {on: false}
      }
      default: {
        throw new Error(`Unsupported type: ${action.type}`)
      }
    }
  }

// here we are re-asigining user "toggleStateReducer" to our default reducer fn

  const {on, getTogglerProps, getResetterProps} = useToggle({
    reducer: toggleStateReducer,
  })

  return (
    <div>
      <Switch
        {...getTogglerProps({
          disabled: clickedTooMuch,
          on: on,
          onClick: () => setTimesClicked(count => count + 1),
        })}
      />
      {clickedTooMuch ? (
        <div data-testid="notice">
          Whoa, you clicked too much!
          <br />
        </div>
      ) : timesClicked > 0 ? (
        <div data-testid="click-count">Click count: {timesClicked}</div>
      ) : null}
      <button {...getResetterProps({onClick: () => setTimesClicked(0)})}>
        Reset
      </button>
    </div>
  )
}

```

Ex: when auther reducer is too complicated and you don't want to write an
complicated reducer, you just want to add few side cases only then

- export autherReducer, autherFn that uses useReducer hook

- then import it to the place where you want to create your own custom reducer
  and do like this

```js
function userReducer(state, action) {
  const change = autherReducer(state, action);
  if (action.type === "toggle" && clickedTooMuch === true) {
    return { ...change, on: state.on };
  } else {
    return { ...change };
  }
}
```

- here you are just overWriting the change to a specific condition and keeping
  all the rest of the condition as they were. becouse reducer also returns an
  object

## complete implementation

```js
import * as React from 'react'
import {Switch} from '../switch'

const callAll =
  (...fns) =>
  (...args) =>
    fns.forEach(fn => fn?.(...args))

function toggleReducer(state, {type, initialState}) {
  switch (type) {
    case 'toggle': {
      return {on: !state.on}
    }
    case 'reset': {
      return initialState
    }
    default: {
      throw new Error(`Unsupported type: ${type}`)
    }
  }
}

function useToggle({initialOn = false, reducer = toggleReducer} = {}) {
  const {current: initialState} = React.useRef({on: initialOn})

  const [state, dispatch] = React.useReducer(reducer, initialState)
  const {on} = state

  const toggle = () => dispatch({type: 'toggle'})
  const reset = () => dispatch({type: 'reset', initialState})

  function getTogglerProps({onClick, ...props} = {}) {
    return {
      'aria-pressed': on,
      onClick: callAll(onClick, toggle),
      ...props,
    }
  }

  function getResetterProps({onClick, ...props} = {}) {
    return {
      onClick: callAll(onClick, reset),
      ...props,
    }
  }

  return {
    on,
    reset,
    toggle,
    getTogglerProps,
    getResetterProps,
  }
}
//------------------------------------------------------------------------
export{toggleReducer,useToggle}

import{toggleReducer,useToggle}
//------------------------------------------------------------------------
function App() {
  const [timesClicked, setTimesClicked] = React.useState(0)
  const clickedTooMuch = timesClicked >= 4
//----------------------------------------------------------------------
  function toggleStateReducer(state, action) {
    const change = toggleReducer(state, action)
    if (action.type === 'toggle' && clickedTooMuch === true) {
      return {...change, on: state.on}
    } else {
      return {...change}
    }
  }
//--------------------------------------------------------------------
  const {on, getTogglerProps, getResetterProps} = useToggle({
    reducer: toggleStateReducer,
  })

  return (
    <div>
      <Switch
        {...getTogglerProps({
          disabled: clickedTooMuch,
          on: on,
          onClick: () => setTimesClicked(count => count + 1),
        })}
      />
      {clickedTooMuch ? (
        <div data-testid="notice">
          Whoa, you clicked too much!
          <br />
        </div>
      ) : timesClicked > 0 ? (
        <div data-testid="click-count">Click count: {timesClicked}</div>
      ) : null}
      <button {...getResetterProps({onClick: () => setTimesClicked(0)})}>
        Reset
      </button>
    </div>
  )
}

export default App
```

## Side Note:

when using reducer make sure create an actionType object which contain all your
action type so people don't do mistake while typing action "string" in dispatch
and inside reduccer fn condtions.

ex:

```js
const actionTypes = {
  toggle: "toggle",
  reset: "reset",
};
```

---

## Controled Props for (Custom Component)

you probably have seen controlled props in react where react has buit in system
for controlling the Form input.

lets take a look how we use controlled props in react Form input.

1. we create a input tag in Form and add attributes like onChange and value.

   - `value` : allow us to control the user input that we want to show to the user
     not what react suggest the value for this input.

     but we don't take responsiblility of keeping the state updated and you also want
     to know when react will be updating the state it self this is what onChange
     attributes comes in.

   - `onChange` : trigger when react updates the states + we use e.target.value to
     get the react suggested value that react will show if we don't interfear. we
     take that value and then control it (modifily it) and pass it to the value
     attribute to tell this is the value we want you to show to the user not the
     one that you sugeested.

2. to sharing the value state during the render we use a useState that we pass
   to value and setusestate value in onChange which cozes a re-render of the
   component and we endup seeing the newly Modified value.

so how the cycle moves.

1. react take the user input.
2. we take that value from the react by
   onChange={setUpdateValue(e.target.value)}
3. which cozes a re-render of entire component with the newly updateValue.
4. which then we pass it to the value attribute of the input
5. then react will render that value and show it to the user.

with state reducer pattern you can modify how the state changes should be made
but you can't determined when those state changes happens so controlled Props
kind of like a stepUp above state reducer pattern.

so if we compare state reducer pattern in our controlled input of the Form.
state reducer act as a useState that we use to modify the state. now we need a
system to keep tract of when the state is happening like onChange and we need a
prop like value which changes the customInputComponent behaviour

## How control props works for Component

to create a control props you need?

1. you need to take the suggested out (react will return the output if you don't
   control it)
2. you need a prop to provide/Control the resulting output like a value={}
3. you need onChange to know when the react will render and to be in sync with
   it.

lets take an Ex:

```js
// Control Props

import * as React from "react";
import { Switch } from "../switch";

const callAll =
  (...fns) =>
  (...args) =>
    fns.forEach((fn) => fn?.(...args));

const actionTypes = {
  toggle: "toggle",
  reset: "reset",
};

function toggleReducer(state, { type, initialState }) {
  switch (type) {
    case actionTypes.toggle: {
      return { on: !state.on };
    }
    case actionTypes.reset: {
      return initialState;
    }
    default: {
      throw new Error(`Unsupported type: ${type}`);
    }
  }
}

// 7. here we recive the ( onChange, on: controlledOn) onChange to Sync, we need to  take the suggested on (initalOn) and then control it with our on (controlledOn);

// 8. here we need to take one more thing into concideration which makes this code a little difficalt to understand. which is we have two type of Toggle component

//     8.1. toggle we want to control with our on (controlledOn).

//      8.2. toggle that we don't want to control so we want to use there suggeseted value on (initalOn).

function useToggle({
  initialOn = false,
  reducer = toggleReducer,
  onChange,
  on: controlledOn,
} = {}) {
  const { current: initialState } = React.useRef({ on: initialOn });
  const [state, dispatch] = React.useReducer(reducer, initialState);

  // 9. so first we need to check that have user clicked on the toggle btn that have on={bothOn} on it or not this will help us to distinguesh that we need to show the controlledoutput or suggested output

  // 10. onIsControlled will be true when user clicked over toggle btn that has on={bothOn} (defined) otherwise it will show falsy which mean this trigger is coz by a toggle btn that don't have on={bothOn} (undefined,null)

  const onIsControlled = controlledOn != null;

  // 11. here we are saying is onIsControlled then on value will be controlledOn otherwise state.on (suggested on);

  const on = onIsControlled ? controlledOn : state.on;

  function dispatchWithOnChange(action) {
    // 12. dispatch trigger reducer fn and return state = {on: !state.on} which is a boolean true/false but this is sugested on value meaning this will be the result if we don't control it.

    // 13. so we will only call disptach when we are not controlling the on value and let the non controlled toggle btn behave as it should be.

    if (!onIsControlled) {
      dispatch(action);
    }

    // 14. onChange will take state (suggested state and modify it) and  action (to verify that it is a toggle).

    // 15. how to take the suggested state u can call the reducer(previous state,action) =  to get the nextstate

    // 16. {...state, on} we are just overWritting state.on value (we are giving our new Controlled value from the parent, we don't want to use suggested on value) with on and then onChange will re-render the parent with the newvalue in the Controlled Toggle Component. if we only provide state which only get's updated when we dispatch then our state will return the same value becouse we are not triggering dispach becouse it only trigger when user tap on controlled toggle btn.

    onChange?.(reducer({ ...state, on }, action), action);
  }

  const toggle = () => dispatchWithOnChange({ type: actionTypes.toggle });
  const reset = () =>
    dispatchWithOnChange({ type: actionTypes.reset, initialState });

  function getTogglerProps({ onClick, ...props } = {}) {
    return {
      "aria-pressed": on,
      onClick: callAll(onClick, toggle),
      ...props,
    };
  }

  function getResetterProps({ onClick, ...props } = {}) {
    return {
      onClick: callAll(onClick, reset),
      ...props,
    };
  }

  return {
    on,
    reset,
    toggle,
    getTogglerProps,
    getResetterProps,
  };
}

// 5. Toggle recive on (controlledOn) and onChange, toggle is our custom component which will provide us the suggested value and we need to modify it with our on (controlledOn) value.

// 6. Toggle also sand this value to useToggle who manage the state of on (inital on or the suggested on) which is the state we need to control with our own controlled on;

function Toggle({ on: controlledOn, onChange, initialOn, reducer }) {
  const { on, getTogglerProps } = useToggle({
    on: controlledOn,
    onChange,
    initialOn,
    reducer,
  });
  const props = getTogglerProps({ on });
  return <Switch {...props} />;
}

// 1. start reading the code from here

// 2. we are using bothOn (value) to control the custom component

// 3. handleToggleChange is (onChange) to sink our result with the react

function App() {
  const [bothOn, setBothOn] = React.useState(false);
  const [timesClicked, setTimesClicked] = React.useState(0);

  // 17. here we are changing the suggested on (state.on) value with our controlled on value

  function handleToggleChange(state, action) {
    if (action.type === actionTypes.toggle && timesClicked > 4) {
      return;
    }
    setBothOn(state.on);
    setTimesClicked((c) => c + 1);
  }

  function handleResetClick() {
    setBothOn(false);
    setTimesClicked(0);
  }

  // 4. in Toggle we are providing as on (value) and onChange as (onChange)

  return (
    <div>
      <div>
        <Toggle on={bothOn} onChange={handleToggleChange} />
        <Toggle on={bothOn} onChange={handleToggleChange} />
      </div>
      {timesClicked > 4 ? (
        <div data-testid="notice">
          Whoa, you clicked too much!
          <br />
        </div>
      ) : (
        <div data-testid="click-count">Click count: {timesClicked}</div>
      )}
      <button onClick={handleResetClick}>Reset</button>
      <hr />
      <div>
        <div>Uncontrolled Toggle:</div>
        <Toggle
          onChange={(...args) =>
            console.info("Uncontrolled Toggle onChange", ...args)
          }
        />
      </div>
    </div>
  );
}

export default App;
// we're adding the Toggle export for tests
export { Toggle };

/*
eslint
  no-unused-expressions: "off",
*/
```

## Extra (remove warning code in production)

when you create code in development mode you endup creating warning code which
trigger when some importent condition are not met (every type of self
declearation code for error) but you don't need that for Production that is just
of developemnt so use if(process.env.NODE_ENV !== 'production') which means this
code is only for development environmnet not for production your compiler will
automaticly remove that code from the codebase and only take the nessesery code.
