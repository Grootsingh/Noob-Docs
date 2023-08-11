chapter 1:Basic JavaScript-rendered Hello World

The browser takes this HTML code and generates the DOM (the Document Object
Model) out of it. The browser then exposes the DOM to JavaScript so you can
interact with it to add a layer of interactivity to your web-page.

---

DOM is an web API presented to us by the browser that allow us to intertect us
with the browser generated DOM from our HTML file with the Help of JavaScript

DOM api consist of element like addEventListener,querySelector etc. Note: DOM
api is not a part of javascript it is an API given us by the browser.

---

---

JavaScript frameworks were created to address some of the challenges by
"programmatically creating the DOM "rather than defining it in hand-written
HTML.

---

---

difference between element.apped() vs element.appendChild(); append() allow us
to append text as well as nodes; appendChild() only allow us to append nodes;

---

---

element.className is a property of element interface allow us to set class
Attribute value. element here is an html node ex: const element =
document.getElementByid("root"), element.className = "container"

element interface comes from DOM api

---

Chapter 2: Intro to raw React APIs.

---

Imperative vs Declarative Programming

imperative programming focus more over how the task is going to happen, more
focus over the niity gritty of the steps.

Declarative programming is more about focusing on what you want to do and expect
methods, functions to take care of all the imperative part (steps).

Declarative is more about having easy to understand declaration what you want to
do ex: react componet we import and use are more of a descriptive programming

<widget>
widget component is descriptive, once you see it you know what is this and what it will do.
we used or declear this here means we want to show widget here. declearing it.
and as it is an widget component we must have do the imperitive nitigrity part writing the 
code in this widget.jsx file.

widget.jsx file is Imperative which focus on steps how this will happen.
<widget> is Decleartive focus on what it will do.

---

---

for Every react app you need two library from the react

1. the react library it self to create react element/component.
2. react renderer to render your element into your page.

for different type of render react comes with different renderer like:

1. reactDOM for web
2. reactNative for the mobile
3. reactVR

for us we need react and reactDOM for the web.

in simple term: react === javascript

1. react.createElement === document.createElement comes from React API

<!-- outdated -->

ReactDOM.render(childElement,parentElement) ===
parentElement.append(childElement)

reactDOM.render has been depricated.

<!-- updated and devided into two property -->

1. ReactDOM.createRoot() to select the parentElement comes from ReactDOM.
2. Parent.render() === parent.append() comes from reactDOM

---

---

understand react.createElement

react.CreateElement(type,props,children)

1. type = which type of node/element you want to create. ex:"div","p".
2. props = props is an object which take key value pair for attributes you want
   to add to the node.

ex1: react.createElement("div",{className:"container",key:"1",children:"hello
world"});

ex2: react.createElement("div",{className:"container",key:"1"},"hello world")

ex1 === ex2

1. key is like an id but only for react react use key attribute to keep tract of
   the state of the component/element. if element have not changed then there is
   no need of re-rendering.(hot module re-loading)

1. 1. key is require when ever you deal with arrays.

1. children === what you want to put inside the div tag that you are creating.

children can be anything you want to put inside opening and closing element

<div>...here...</div>

<div class="container">hello world</div>

Note: you can use props to declear children or you can use children parameter
which is third in createElement function

---

chapter 3: Using JSX

---

JSX just provides syntactic sugar for the React.createElement(component, props,
...children) function.

---

---

JSX is HTML-ish type syntex and JSX is used to create Element in react;

JSX code for ex: const ui = <div id ="container">"hello world"</div>

JSX is not actually JavaScript language, you have to convert it using something
called a code compiler. Babel is one such tool.

Note: Babel is a compiler written in javascript language.

Normally you'll compile all of your code at build-time before you ship your
application to the browser, but because Babel is written in JavaScript we can
actually run it in the browser to compile our code on the fly

we can give that compiler comand of bable to the browser and browser will do the
compilation for us on fly.

so bable will convert our JSX code of ui to JavaScript that browser understand :

const ui = React.createElemenet("div",{id= "container"},"hello world")

---

---

Info about JSX from REACT documantation;

Each React component is a JavaScript function that may contain some markup that
React renders into the browser. React components use a syntax extension called
JSX to represent that markup. JSX looks a lot like HTML, but it is a bit
stricter and can display dynamic information.

---

---

info about JSX rule

1. Return a single root element

To return multiple elements from a component, wrap them with a single parent
tag. you can use <div></div> or <Fragment></Fragment>

what is fragment?

Fragment is an ulternate to div given us by React api.

<Fragment></Fragment> to wrap your JSX if you don't want to have an extra div
wrapper use <Fragment> or use short hand <></> but in case of props (key) with
Fragment you can't use shorthand you require complete syntex.

<Fragment key ="1"></Fragment>

2. Close all the tags

JSX requires tags to be explicitly closed: self-closing tags like <img> must
become <img />, and wrapping tags like <li>oranges must be written as

<li>oranges</li>.

3. camelCase in most of the things!

JSX turns into JavaScript and attributes written in JSX become keys of
JavaScript objects. In your own components, you will often want to read those
attributes into variables. But JavaScript has limitations on variable names. For
example, their names can’t contain dashes or be reserved words like class.

This is why, in React, many HTML and SVG attributes are written in camelCase.
For example, instead of stroke-width you use strokeWidth. Since class is a
reserved word, in React you write className instead, named after the
corresponding DOM property:

re-cap:

React components group rendering logic together with markup because they are
related. JSX is similar to HTML, with stricter rules.

---

---

React is used to group together rendering Logic (Javascript) with markup
language (HTML) and all the logic remain in sync comes from the same file.

react file use JSX (JSX stands for JavaScript XML) file format indicate this
component contain JSX and Javascript in the same file. react is a library that
we use in javascript language and JSX is a markup language like HTML but has
strickter rules to follow.

---

---

libraries target a specific functionality, while a framework tries to provide
everything required to develop a complete application.

react is a library it is used to provide component like structure to the code
but it is not good at other things like fetching,state managemnt etc.

---

---

JSX with curly braces

to interpulate (insert (something of a different nature) into something else) in
JSX

You can only use curly braces in two ways inside JSX:

1. As text directly inside a JSX tag: <h1>{name}'s To Do List</h1> works, but
   <{tag}>Gregorio Y. Zara's To Do List</{tag}> will not.
2. As attributes immediately following the = sign: src={avatar} will read the
   avatar variable, but src="{avatar}" will pass the string "{avatar}".

JSX and Object

In addition to strings, numbers, and other JavaScript expressions, you can even
pass objects in JSX. Objects are also denoted with curly braces, like { name:
"Hedy Lamarr", inventions: 5 }. Therefore, to pass a JS object in JSX, you must
wrap the object in another pair of curly braces:
person={{ name: "Hedy Lamarr", inventions: 5 }}.

similar to writing CSS in Javascript, Inline style properties in JSX {{}} are
written in camelCase. For example, HTML <ul style="background-color: black">
would be written as <ul style={{ backgroundColor: 'black' }}> in your component.

---

---

from the video:0018

in react React.createElement(type,props,children)

1. React.createElement("div,{className:"container"},"hello world")
2. React.createElement("div,{className:"container",children:"hello world"})

in react 1 === 2

in jsx React.createElement(type,props,children)

1. type === <div></div>
2. props === attributes <div className="container"></div>
3. children === div child element <div>"hello world"</div>

const myObject = { className:"Container", children:"hello world" }

so if i do this: <div {...myObject} />  
as we can write children inside props or sepratly both are currect.

so <div {...nyObject}/> will interprit by bebal (converted into javascript) as
React.createElement("div,{className:"container",children:"hello world"})

myObject works becouse it has the current nameing:

1. className = "container"
2. children = "hello world"

which is correct this code will work becouse it is jsx not html and jsx is
converted to Javascript.

---

chapter:3 Creating custom components

---

JSX just provides syntactic sugar for the React.createElement(component, props,
...children) function.

anything your write inside JSX is converted into React.createElement()

now re-understood react.createElement() in depth so that we can have a better
understanding of how JSX is used properly.

react is an object and that comes with ton of methods/functions

function createElement(elementType, props, ...children) {}

createElement is one of them.

let understand the parameter of createElement.

1. elementType. ---------------------------------

can be a string or a function (class) for the type of element to be created.

string is used to describe the HTML Tags/component which are pre-defined like
"div","h1" etc

functions are regular function which we use to return a JSX. function nameimg
convention is to start the name with the capitalCase Character to differenciate
function Component from the HTML componet becouse as you can see that html tags
by nature starts with lowercase.

1. JSX <div>"hello world"<div>
2. React React.createElement("div",null,"hello world")

if function name is in lower case (it will not work)

1. JSX <book>"hello world"<book>
2. React React.createElement("book",null,"hello world")

the currect way ex:1

1. JSX <Book>"hello world"<Book>
2. React React.createElement(Book,null,"hello world")

as you can notice lowerCase function name is mistaken for HTML tag name and thow
an error becouse there is no HTML tag/component with a name of book.

the currect way ex:2

JSX

```
  function MyComponent(props) {
         return (
          <div>
             Imagine a {props.color} datepicker here.
           </div>
         )
  }

    const element = <MyComponent color="blue">helloo world</MyComponent>
    ReactDOM.createRoot(document.getElementById('root')).render(element)
```

React

```

function MyComponent(props) {
  return React.createElement("div", null, "Imagine a ", props.color, datepicker here.");
}

version:1
const element = React.createElement(MyComponent, {
   color: 'blue',
   children: 'hello world',
})

version:2
var element = React.createElement(MyComponent, {
  color: "blue"
}, "hello world");

version 1 === version 2

ReactDOM.createRoot(document.getElementById('root')).render(element);


```

as you can notice we used Mycomponet as elementType in React.createElement what
you can observe here is that our Tag/Component is MyComponent and props are
{color:"blue",children:"hello world"} our porps act as parameter for our
component Tag which is Mycomponent Function. these props are pass down to
Mycomponent function as used with javascript object.dot notation.

our result of upper code is : Imagine a blue datepicker here.

Note: React.createElement() return an object which contains a lot of
key/properts and values. it means when we use createElement() we are just
asigning values to an object. in that object keys that we should keep in mind
are

1. props which store two things {all of your attributes as key value pair,
   children: all of your children}

1. if there is only one children then children: "hello world"
1. if multile children then children: ["hello world" ,"go goa gone"] converted
   into array automaticaly.

so above code props:{color:"blue",children:"hello world"} in createElement()
object and we use these props in Mycomponent function where we return a new JSX
the reson children are not prinited on the screen becouse we have not used it
from the prop.

How to think in terms of custom Components???

1. we create a custom Component

```
function MyComponent(props) {
         return (
          <div>
             Imagine a {props.color} datepicker here.
           </div>
         )
  }
```

2. we use that custom component in JSX

```
JSX:
const element = <MyComponent color="blue">helloo world</MyComponent>

<MyComponent {anything you declear here will be props/parameter
                    of your MyComponent (props.parameter:value)}>
{anything here will be our props.childen parameter}
</MyComponent>

converted in react:
const element = React.createElement(MyComponent, {
   color: 'blue',
   children: 'hello world',
})

createElement object of element will store {
    props:{color:blue,children:"hello wolrd"}
}

```

here what we are doing is passing the props to the Mycomponent Function and
accessing those props with in Mycomonent function to return a new JSX

so,

1. function Mycomponent(){} is component/function expression

2. <Mycomponent> === Mycomponent() here we are just asiging the props here to
   our component and that component will take our props and use them to return a
   JSX who's react.createElement type is HTML. and that JSX is what is the final
   JSX that will be used when we render the DOM and when to call is in the hand
   of react.render() it self.

---

2. Props --------------------------------------------

---

props is an object for the props we want applied to the element (or null if we
specify no props)

---

3. children ---------------------------------------

---

...children is all the children we want applied to the element too. chidlren are
what we want to nest/palce between the opening and closing Tags/components.

1. <MyComponent children = "hello world" />
2. <MyComponent> "hello world"</MyComponent>

1 === 2

multiple childrens

we can pass ass many children we want. in JSX it is automaticaly converted to an
array of childrens;

<MyComponent>"hellow world","hey everybody","namste india"</MyComponent>

```
props:{
    color: "red",
    children:["hellow world","hey everybody","namste india"]
}
```

in React.createElement

1. react.createElement("div",null,"hellow world","hey everybody","namste india")
2. react.createElement("div",null,["hellow world","hey everybody","namste
   india"])
3. react.createElement("div",{children:["hellow world","hey everybody","namste
   india"]})

all are stored/converted into

```
props:{
    color: "red",
    children:["hellow world","hey everybody","namste india"]
}
```

---

---

React.createElement() returns an object

```
// <div id="root">Hello world</div>
{
  type: "div",
  key: null,
  ref: null,
  props: { id: "root", children: "Hello world" },
  _owner: null,
  _store: {}
};
```

and When you pass an object like that to ReactDOM.render(), it's the renderer's
job to interpret that element object and create DOM nodes.

---

Extra info from the doc's about JSX and React

---

1. Choosing the Type at Runtime

You cannot use a general expression as the React element type. If you do want to
use a general expression to indicate the type of the element, just assign it to
a capitalized variable first. This often comes up when you want to render a
different component based on a prop:

```
Not allowed
<components[props.storyType] story={props.story} />;

Allowed
const SpecificStory = components[props.storyType];
return <SpecificStory story={props.story} />;
```

2. JavaScript Expressions as Props {}

You can pass any JavaScript expression as a prop, by surrounding it with {}. For
example, in this JSX:

```
<MyComponent foo={1 + 2 + 3 + 4} />
```

For MyComponent, the value of props.foo will be 10 because the expression 1 +
2 + 3 + 4 gets evaluated.

if statements and for loops are not expressions in JavaScript, so they can’t be
used in JSX directly.

```
function NumberDescriber(props) {
  let description;
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
}
```

3. String Literals

You can pass a string literal as a prop value. These two JSX expressions are
equivalent:

```
<MyComponent message="hello world" />
<MyComponent message={'hello world'} />
```

When you pass a string literal, its value is HTML-unescaped. So these two JSX
expressions are equivalent:

```
<MyComponent message="&lt;3" />
<MyComponent message={'<3'} />
```

This behavior is usually not relevant. It’s only mentioned here for
completeness.

4. Props Default to “True” If you pass no value for a prop, it defaults to true.
   These two JSX expressions are equivalent:

```
<MyTextBox autocomplete />
<MyTextBox autocomplete={true} />
```

In general, we don’t recommend not passing a value for a prop, because it can be
confused with the ES6 object shorthand {foo} which is short for {foo: foo}
rather than {foo: true}. This behavior is just there so that it matches the
behavior of HTML.

5. Children in JSX

you can pass Strings,HTML unescaped, JavaScript Expressions as Children with {}.

Booleans, Null, and Undefined Are Ignored

```
<div />
<div></div>
<div>{false}</div>
<div>{null}</div>
<div>{undefined}</div>
<div>{true}</div>
```

This can be useful to conditionally render React elements. This JSX renders the

<Header /> component only if showHeader is true:

```
<div>
  {showHeader && <Header />}
  <Content />
</div>
`

One caveat is that some “falsy” values, such as the 0 number, are still rendered by React. For example, this code will not behave as you might expect because 0 will be printed when props.messages is an empty array:
```

<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>
```
To fix this, make sure that the expression before && is always boolean:
```
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
```
Conversely, if you want a value like false, true, null, or undefined to appear in the output, you have to convert it to a string first:
```
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```
JSX removes whitespace at the beginning and ending of a line. It also removes
blank lines. New lines adjacent to tags are removed; new lines that occur in the
middle of string literals are condensed into a single space. So these all render
to the same thing

You can provide more JSX elements as the children. This is useful for displaying
nested components

---

video: 0022

---

we can directly execute our function too

```
ex: function message(props) {
      return <div className="message">{props.children}</div>
    }
```

const element = <>{message}</> this works but not a very good apporch and not
very declearitive in nature;

Note: if we directly execute function in our JSX with the help of interpolation
{}. then we enter inside Javascript land and we execute that function and return
JSX. it's like we are just refrencing another JSX code with in the
interpolation. due to that this is not be seen in react.Component dev tool
becouse here it's just a refrence its like we are writing jsx inside another
JSX.

you will not find message as a component in react component dev tool.

---

---

but we if use that function as a argunmnet in react.createElement(type) then it
will be consider as a component.

```
function message(props) {
      return <div className="message">{props.children}</div>
}

const element = React.createElement(message, {children: 'hello world'})
return <div>{element}</div>
```

now message will be shown as a component in dev-tool. becouse ract.createElement
creates an element that here is message message is an element.
<message><div className="message">{props.children}</div></message>

you can see that in dev tool. but for HTML there is not message tag there so
don't worry.

when browser reach to the return <div>{element}</div> it go to element then
React.createElement(message, {children: 'hello world'}) it reads message is an
component it goes to message function with-the props{children:"hello world"} and
then function message(props) { return

<div className="message">{props.children}</div> } and return but before that it
convert <div> JSX to react.createElement and the code is done execution.

---

---

Note: React.createElement(message,{children:"hello world"})

react can take lowercase function name in createElement menthod but JSX can't
for jsx sake we need to convert our function name to Capitalize

<message>hello world</message> is not allowed becouse jsx mis interprit
lowercase name with HTML tags

<Message>hello world</Message> is allowed now JSX understand this is not a HTML
tag it is a function/component

---

---

Destructuring an Objects in component props

```
Mycomponent({children}){
  return <h1>{children}</div>
}

<Mycomponent>"hello"</Mycomponent>
```

react.createElement is an object that contain a props key which it self is an
object which return all the attributes and children property asign to the
component during JSX fn.

```
// <Mycomponent>"hello"</Mycomponent>
{
  type: "Mycomponent fn",
  key: null,
  ref: null,
  props: { children: "Hello" },
  _owner: null,
  _store: {}
};
```

that Mycomponent component can access all the props as a parameter.

Mycomponent(props) and here props = { children: "Hello" }.

if we destructure the props in the parameter then Mycomponent({children}) = {
children: "Hello" }

here let {children} = {children:"hello"}

in destructuring if the let {children} matches with key of the object (of the
other side of equal) then that value will be assigined to the matched let
variable this is how destructuring work.

---

Chapter: 4 Styling

---

in HTML we say class for selection class attribute but fun fact it when HTML
code converted into dom it becomes className and second fun fact is in react we
refer to className which is what dom render so JSX is much closer to DOM syntex
then HTML.

similarly in HTML for inline style we go like this style (style="margin-top:
20px; background-color: blue ) which is a strinmg type but in DOM it is an
object type and fun fact in react it is also an object type
(style={{marginTop: 20, backgroundColor: 'blue'}})

---

chapter: 5 Form

---

### useRef hook?

useRef is a React Hook that lets you reference a value that’s not needed for
rendering.

You can mutate the ref.current property. Unlike state, it is mutable. However,
if it holds an object that is used for rendering (for example, a piece of your
state), then you shouldn’t mutate that object.

When you change the ref.current property, React does not re-render your
component. React is not aware of when you change it because a ref is a plain
JavaScript object.

Do not write or read ref.current during rendering, except for initialization.
This makes your component’s behavior unpredictable.

first: import { useRef } from 'react';

then const intervalRef = useRef(0);

useRef return an javascript Object with only one property that is current. on
your initial render that value is set to useRef(...this..) value.

the main focus of useRef is useRef returns a ref object with a single current
property initially set to the initial value you provided.

On the next renders, useRef will return the same object. You can change its
current property to store information and read it later. This might remind you
of state, but there is an important difference.

Changing a ref does not trigger a re-render. This means refs are perfect for
storing information that doesn’t affect the visual output of your component. For
example, if you need to store an interval ID and retrieve it later, you can put
it in a ref. To update the value inside the ref, you need to manually change its
current property:

By using a ref, you ensure that:

1. You can store information between re-renders (unlike regular variables, which
   reset on every render).
2. Changing it does not trigger a re-render (unlike state variables, which
   trigger a re-render).
3. The information is local to each copy of your component (unlike the variables
   outside, which are shared).

Changing a ref does not trigger a re-render, so refs are not appropriate for
storing information you want to display on the screen. Use state for that
instead.

### Some basic rules to use useRef

Do not write or read ref.current during rendering.

React expects that the body of your component behaves like a pure function:

If the inputs (props, state, and context) are the same, it should return exactly
the same JSX. Calling it in a different order or with different arguments should
not affect the results of other calls. Reading or writing a ref during rendering
breaks these expectations.

```
function MyComponent() {
  // ...
  // 🚩 Don't write a ref during rendering
  myRef.current = 123;
  // ...
  // 🚩 Don't read a ref during rendering
  return <h1>{myOtherRef.current}</h1>;
}
```

You can read or write refs from event handlers or effects instead.

```
function MyComponent() {
  // ...
  useEffect(() => {
    // ✅ You can read or write refs in effects
    myRef.current = 123;
  });
  // ...
  function handleClick() {
    // ✅ You can read or write refs in event handlers
    doSomething(myOtherRef.current);
  }
  // ...
}
```

If you have to read or write something during rendering, use state instead. if
you want to read only and don't want to re-render then use ref.

When you break these rules, your component might still work, but most of the
newer features we’re adding to React will rely on these expectations. Read more
about keeping your components pure.

### Manipulating the DOM with a ref

It’s particularly common to use a ref to manipulate the DOM. React has built-in
support for this.

First, declare a ref object with an initial value of null:

import { useRef } from 'react';

```
function MyComponent() {
  const inputRef = useRef(null);
  ....
```

Then pass your ref object as the ref attribute to the JSX of the DOM node you
want to manipulate:

```
  return <input ref={inputRef} />;
```

After React creates the DOM node and puts it on the screen, React will set the
current property of your ref object to that DOM node. Now you can access the
<input>’s DOM node and call methods like focus():

```
  function handleClick() {
    inputRef.current.focus();
  }
```

### Avoiding recreating the ref contents

React saves the initial ref value once and ignores it on the next renders.

```
function Video() {
  const playerRef = useRef(new VideoPlayer());
  // ...
```

Although the result of new VideoPlayer() is only used for the initial render,
you’re still calling this function on every render. This can be wasteful if it’s
creating expensive objects.

To solve it, you may initialize the ref like this instead:

```
function Video() {
  const playerRef = useRef(null);
  if (playerRef.current === null) {
    playerRef.current = new VideoPlayer();
  }
  // ...
```

Normally, writing or reading ref.current during render is not allowed. However,
it’s fine in this case because the result is always the same, and the condition
only executes during initialization so it’s fully predictable.

### ref to a custom component

If you try to pass a ref to your own component like this:

```
const inputRef = useRef(null);
return <MyInput ref={inputRef} />;
```

You might get an error in the console:

```
Console Warning: Function components cannot be given refs.
Attempts to access this ref will fail. Did you mean to use React.forwardRef()?
```

By default, your own components don’t expose refs to the DOM nodes inside them.
To fix this, find the component that you want to get a ref to:

```
export default function MyInput({ value, onChange }) {
  return (
    <input
      value={value}
      onChange={onChange}
    />
  );
}
```

And then wrap it in forwardRef like this:

```
import { forwardRef } from 'react';

const MyInput = forwardRef(({ value, onChange }, ref) => {
  return (
    <input
      value={value}
      onChange={onChange}
      ref={ref}
    />
  );
});
export default MyInput;
```

Then the parent component can get a ref to it.

### Myobservation

think of useRef as an javascript Object which don't get affected by re-rendering
like a variable does. variable value get re-set every time when re-render
happens. but useRef maintain it's values. it don't get affected by re-rendering.

1. don't use it for displaying any content on the screen for that useState.
2. you are not allowed to change/re-asign value duing rendering/re-render only
   at the inital state you are allowed to set value for useRef.
3. if you want to change value for useRef then do it in useEffect state or
   inside eventHandler.
4. useRef is good of HTML node reference ref={useRef varaiable name } with this
   you can refer HTML node.
5. if you want to refer your own component as HTML nodes then you need to wrap
   your Component with forwardRef.
6. don't pace function as a inital value in useRef becouse that function by
   default will re-render and useRef initial value will be set on every
   re-render which is not good.

If you have to read or write something during rendering, use state instead.

if you want to read only and don't want to re-render then use ref.(if you want
to re-write ref value then you can do that too but ref is good for read the
refrence value)

### React docs: forwardRef

---

# side knowladge

ref is used to refrence HTML DOM element with in your Component so that you can
access those DOM element after react render and you can manupulate the HTML
Nodes.

but ref can only be used to refrence HTML DOMs if you want to ref your own child
Component with in your Parent Component then ref stand alone can't work.

so you need to forward the ref to your child Componet.

how to forward ref you just need to wrap your childComponent with forwardRef and
thats help you with referencing ref.

ex:

```
function ParentComponent(){
  const inputRef = React.ref()

  // here we can now access the ChildComponent input DOM node which is in
  // other file

  return (<ChildComponent ref={inputRef} type="text"/>)

}
----------------------------------------------------------------------
export Default React.forwardRef(function ChildComponent(props,ref){
  return <input {...props} ref={ref}>
})
```

---

forwardRef lets your component expose a DOM node to parent component with a ref.

const SomeComponent = forwardRef(render)

Call forwardRef() to let your component receive a ref and forward it to a child
component

import { forwardRef } from 'react';

```
const MyInput = forwardRef(function MyInput(props, ref) {
  // ...
});

```

# kent C Dodds : why use forwardRef

You'll actually get an error if you try to pass a ref prop to a function
component. So how do we solve this? Well, React has had this feature called
forwardRef for this specific reasion.

# Parameters

1. render: The render function for your component. React calls this function
   with the props and ref that your component received from its parent. The JSX
   you return will be the output of your component.

# Returns

forwardRef returns a React component that you can render in JSX. Unlike React
components defined as plain functions, a component returned by forwardRef is
also able to receive a ref prop.

# render function

forwardRef accepts a render function as an argument. React calls this function
with props and ref:

```
const MyInput = forwardRef(function MyInput(props, ref) {
  return (
    <label>
      {props.label}
      <input ref={ref} />
    </label>
  );
});
```

# Parameters

1. props: The props passed by the parent component.

2. ref: The ref attribute passed by the parent component. The ref can be an
   object or a function. If the parent component has not passed a ref, it will
   be null. You should either pass the ref you receive to another component, or
   pass it to useImperativeHandle.

# Returns

forwardRef returns a React component that you can render in JSX. Unlike React
components defined as plain functions, the component returned by forwardRef is
able to take a ref prop.

# Usage

1. Exposing a DOM node to the parent component.
2. Forwarding a ref through multiple components. you can do nested
   childComponent forwording.
3. Exposing an imperative handle instead of a DOM node. with the help of
   imperative Handle you can ref a function rather then exposing the entire DOM
   node.

---

Extra : own observation

```
function ParentComponent(){
  const inputRef = React.ref()

  return (<ChildComponent ref={inputRef} type="text"/>)

}
-----------------------------------------------------
import { forwardRef, useRef, useImperativeHandle } from 'react';

const ChildComponent = forwardRef(function ChildComponent(props, ref) {
  const inputRef = useRef(null);

// useImperativeHandle function is used as a refrence point
// rather then  useing DOM in the ChildComponent we are ref the
// useImperativeHandle function which here is returning two methods
// which we can use in parentComponent.
  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});
```

---

# pitfall

Do not overuse refs. You should only use refs for imperative behaviors that you
can’t express as props: for example, scrolling to a node, focusing a node,
triggering an animation, selecting text, and so on.

If you can express something as a prop, you should not use a ref. For example,
instead of exposing an imperative handle like { open, close } from a Modal
component, it is better to take isOpen as a prop like <Modal isOpen={isOpen} />.
Effects can help you expose imperative behaviors via props.

---

# React Docs : useImperativeHandle

---

# my observation

as i said useImperativeHandle is a way to reference method to parent from the
Child component. useImperativeHandle helps to reference fn to a ref point. ref
itself can only refrence DOM node but with the help of useImperativeHandle you
can reference function too.

useImperativeHandle is used to have more control over DOM and don't expose the
DOM node to the ParentComponent rather give them methods/fnHandler that they can
use to trigger some functionallity

# rect Docs

useImperativeHandle is a React Hook that lets you customize the handle exposed
as a ref.

useImperativeHandle(ref, createHandle, dependencies?)

```
import { forwardRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  useImperativeHandle(ref, () => {
    return {
      // ... your methods ...
    };
  }, []);
  // ...
})
```


# Parameters

1. ref: The ref you received as the second argument from the forwardRef render
   function.

2. createHandle: A function that takes no arguments and returns the ref handle
   you want to expose. That ref handle can have any type. Usually, you will
   return an object with the methods you want to expose.

3. optional dependencies: normal Dependency array like useEffect

# Returns

useImperativeHandle returns undefined.

# UseCase

1. Exposing a custom ref handle (methods to control DOM) to the parent
   component.

# myObservation usecase

create a ref point in parentComponent (parentRef) then pass that parefRef to
ChildComponent. to acess that parentRef in ChildComponent wrap your
childComponent with forwardRef. now your code look's like this:
forwradRef(function(props,parentRef){ return <HTMLtag {...props}
ref={parentRef}>})

now use impreritiveHandle hook if you don't want to expose node directily you
can return an object of method/handle to parentComponent that you can use in
your parentComponent to control your childComponent HTMLtags/nodes.

```
Before: forwradRef(function(props,parentRef){ return <HTMLtag {...props} ref={parentRef}>})

Now: import {useRef,useImpreritiveHandle} from "React"

forwradRef(function(props,parentRef){
  const childRefNode = useRef()
  useImparitivehandle(parentRef,()=>{
    return {onFocus:()=> childRefNode.current.focus()}
  })
   return <HTMLtag {...props} ref={childRefNode}>

   })

```

now when you say parentRef.current.focus() inside parentComponent then
childRefNode will be in focus

---

### useState hook

first argunment:initalState

initialState: The value you want the state to be initially. It can be a value of
any type, but there is a special behavior for functions. This argument is
ignored after the initial render.

If you pass a function as initialState, it will be treated as an initializer
function. It should be pure, should take no arguments, and should return a value
of any type. React will call your initializer function when initializing the
component, and store its return value as the initial state.

second argunment:nextState or changeState function

The set function returned by useState lets you update the state to a different
value and trigger a re-render. You can pass the next state directly, or a
function that calculates it from the previous state

```
const [name, setName] = useState('Edward');

function handleClick() {
  setName('Taylor');
  setAge(a => a + 1);
  // ...
```

Parameters

nextState: The value that you want the state to be. It can be a value of any
type, but there is a special behavior for functions.

If you pass a function as nextState, it will be treated as an updater function.
It must be pure, should take the pending state as its only argument, and should
return the next state. React will put your updater function in a queue and
re-render your component. During the next render, React will calculate the next
state by applying all of the queued updaters to the previous state.

Note: set functions do not have a return value.

Caveats

The set function only updates the state variable for the next render. If you
read the state variable after calling the set function, you will still get the
old value that was on the screen before your call.

If the new value you provide is identical to the current state, as determined by
an Object.is comparison, React will skip re-rendering the component and its
children. This is an optimization. Although in some cases React may still need
to call your component before skipping the children, it shouldn’t affect your
code.

React batches state updates. It updates the screen after all the event handlers
have run and have called their set functions. This prevents multiple re-renders
during a single event. In the rare case that you need to force React to update
the screen earlier, for example to access the DOM, you can use flushSync.

Calling the set function during rendering is only allowed from within the
currently rendering component. React will discard its output and immediately
attempt to render it again with the new state. This pattern is rarely needed,
but you can use it to store information from the previous renders.

### Updating objects and arrays in state

You can put objects and arrays into state. In React, state is considered
read-only, so you should replace it rather than mutate your existing objects.
For example, if you have a form object in state, don’t mutate it:

```
// 🚩 Don't mutate an object in state like this:
form.firstName = 'Taylor';
```

Instead, replace the whole object by creating a new one:

```
// ✅ Replace state with a new object
setForm({
  ...form,
  firstName: 'Taylor'
});
```

### Avoiding recreating the initial state

React saves the initial state once and ignores it on the next renders.

```
function TodoList() {
  const [todos, setTodos] = useState(createInitialTodos());
  // ...
```

Although the result of createInitialTodos() is only used for the initial render,
you’re still calling this function on every render. This can be wasteful if it’s
creating large arrays or performing expensive calculations.

To solve this, you may pass it as an initializer function to useState instead:

```
function TodoList() {
  const [todos, setTodos] = useState(createInitialTodos);
  // ...
```

Notice that you’re passing createInitialTodos, which is the function itself, and
not createInitialTodos(), which is the result of calling it. If you pass a
function to useState, React will only call it during initialization.

### Resetting state with a key

You’ll often encounter the key attribute when rendering lists. However, it also
serves another purpose.

You can reset a component’s state by passing a different key to a component. In
this example, the Reset button changes the version state variable, which we pass
as a key to the Form. When the key changes, React re-creates the Form component
(and all of its children) from scratch, so its state gets reset.

```
import { useState } from 'react';

export default function App() {
  const [version, setVersion] = useState(0);

  function handleReset() {
    setVersion(version + 1);
  }

  return (
    <>
      <button onClick={handleReset}>Reset</button>
      <Form key={version} />
    </>
  );
}
```

when the key={version} changes the entire form component re-render. which re-set
the form to its original state.

### Troubleshooting

### I’ve updated the state, but logging gives me the old value

Calling the set function does not change state in the running code:

```
function handleClick() {
  console.log(count);  // 0

  setCount(count + 1); // Request a re-render with 1
  console.log(count);  // Still 0!

  setTimeout(() => {
    console.log(count); // Also 0!
  }, 5000);
}
```

This is because states behaves like a snapshot. Updating state requests another
render with the new state value, but does not affect the count JavaScript
variable in your already-running event handler.

If you need to use the next state, you can save it in a variable before passing
it to the set function:

```
const nextCount = count + 1;
setCount(nextCount);

console.log(count);     // 0
console.log(nextCount); // 1
```

### I’ve updated the state, but the screen doesn’t update

React will ignore your update if the next state is equal to the previous state,
as determined by an Object.is comparison. This usually happens when you change
an object or an array in state directly:

```
obj.x = 10;  // 🚩 Wrong: mutating existing object

here you already re-asigined the value

setObj(obj); // 🚩 Doesn't do anything

so here it's like asking that new value is equal to the new value
and render don't happen
```

You mutated an existing obj object and passed it back to setObj, so React
ignored the update. To fix this, you need to ensure that you’re always replacing
objects and arrays in state instead of mutating them:

```
// ✅ Correct: creating a new object
setObj({
  ...obj,
  x: 10
});
```

don't mutate the array/object becouse it's like reasigining the value to the
same variable and you are not changing the value you are just re-asigning the
value due to that re-render don't happen.

you should always asign complete new object/array if you want to change value

### I’m getting an error: “Too many re-renders”

this means that you’re unconditionally setting state during render, so your
component enters a loop: render, set state (which causes a render), render, set
state (which causes a render), and so on. Very often, this is caused by a
mistake in specifying an event handler:

```
// 🚩 Wrong: calls the handler during render
return <button onClick={handleClick()}>Click me</button>

// ✅ Correct: passes down the event handler
return <button onClick={handleClick}>Click me</button>

// ✅ Correct: passes down an inline function
return <button onClick={(e) => handleClick(e)}>Click me</button>
```

### I’m trying to set state to a function, but it gets called instead

You can’t put a function into state like this:

```
const [fn, setFn] = useState(someFunction);

function handleClick() {
  setFn(someOtherFunction);
}
```

Because you’re passing a function, React assumes that someFunction is an
initializer function, and that someOtherFunction is an updater function, so it
tries to call them and store the result. To actually store a function, you have
to put () => before them in both cases. Then React will store the functions you
pass.

```
const [fn, setFn] = useState(() => someFunction);

function handleClick() {
  setFn(() => someOtherFunction);
}
```

---

### video 0033

---

when you access event in react when acessing input/form you are not accessing
the real event it is a synthaticEvent which is similar to real event created for
react.

react created synthatic event for performance reasons if you ever want to access
native/javascript event just go for (event.native).

you can target form input by there input name that you have asigned.

```
ex: event.target.elements.name = <input name="name" type="text"/>
ex: event.target.elements.firstName = <input name="firstName" type="text"/>
```

event.target.elements.(you can tell id,name,className) it will select that input

### Control the input value

we create a state

const [inputValue,setInputValue] = useState("")

initalstate is empity (that was it is for any input value)

then we set inputvalue as a value in our input tag

```
<input type="text" value={inputValue}/>
```

what we did is we asign a state to value if we leave it at it is then the input
value will be shown as empity. eventhought the user is typing the input value.
you can check it as event.target.value. value is being updated but state is not
changing becouse we have set the value as initalState and we are not updateing
it. to update it we want change the initalState and re-render too.

```
<input type="text" value={inputValue} onClick={(e)=> setInputValue(e.target.value)}/>
```

now we can update the value too here when ever input value is added to the input
field we are taking that value by e.target.value and setting it to our
initalstate and re-rendering it after so that user can see the input change
while typing. as you can notice we now have control over what user can see in
the input feild we can set some parameter to manupulate the value as we like.

this is how we take more control over the input value that user can give us for

ex: we only want to accept lowercase values in our form so we will do

```
<input type="text" value={inputValue} onClick={(e)=> setInputValue(e.target.value.toLowerCase())}/>
```

so now we have the total control over the user input. not no matter what user
can only give lower case values

with the help of react we have added an extra layer of security/ functionality
over the input by taking control over the user input.

---

chapter: 5 Rendering Arrays and why we need key prop?

---

let's take an array.

```
const items = [
  {id: 'apple', value: '🍎 apple'},
  {id: 'orange', value: '🍊 orange'},
  {id: 'grape', value: '🍇 grape'},
  {id: 'pear', value: '🍐 pear'},
]

function App() {
  return (
    <ul>
      {items.map((item) => (
        <li>{item.value}</li>
      ))}
    </ul>
  )
}
```

when we do The inital render on this array list without key props added to the
list. it will create a reactDOM out of it and render it on the web.

when we update the array?

react compare the previous renderedDOM with the updated new DOM and compare it
and check what are the changes? and update only the changes to the previous DOM
which endup becoming the New Updated re-render.

now when we say update the array it can mean multiple things

1. you have added an item (begnning,middle,end).
2. you have removed an item (begnning,middle,end).
3. you just have updated value to an index.

it's hard to know what kind of update you have did. react just do the guess work
and try to come out with the best output.

let's take an example to better our understanding.

```
ex:1
previusDOM array = [apple,orange,grape,pear]
updatedDOM array = [apple,orange,grape]
```

now when it comes to array array have two try of info

1. index
2. data

react compare index and it's data to check for new Update and update the dom.

```
in ex:1 react will see
index:0 was apple and new is also apple,
index:1 was orange and new is also orange,
index:2 was grape and new is also grape,
index:3 was pear and new!!! oh index 3 was remove ok.
```

in new update remove the index:3

here react was able to solve the problem with out any issue.

```
ex:2
previusDOM array = [apple,orange,grape]
updatedDOM array = [apple,grape]

```

new challenge? check index and data comparision?

```
index:0 was apple and new is also apple.
index:1 was orange and new is grape update.
index:2 was grape and new!!! oh index 3 was remove ok.
```

what react understood is that index:2 data was changed and index:3 was removed.
but in reality index 2 was removed. and as index 2 was removed index 3 fall back
to index:2.

one other problem with index base comparision is that if we remove index:2 item
then all the index's that comes after index:2 will also be re-calculated as the
index value get shifted. which is waste of additional-work.

but what's the problem here both means the same thing? isn't it?? NO.

it work's in some case where we just need to print data on the page but when we
use data in JSX/Component that's a problem?

react take the previous DOM and comapre it to new DOM and update the changes but
few property only get asigned / ignored on re-render evern if the data mutate.
ex: input defaultValue.

Input DefaultValue?

defaultValue is meant to be uncontrolled. So, why would you want to control it?

defaultValue is supposed to allow an input to receive some starting data but
then React usually will forget that it was ever set. Why reset it?

so if your component contain defualt value it will not update on re-render and
that will create a mismatch in UI with cozez problems for the user.

react only update the data of those which was changed in new update it wil not
even bother to take a look for rest of the component like(defaultValue).

to tell the react that update the entrie component not just the list data part
we need key which are more reliable then index as we see if we remove index:1
and index:1 data will be replaced by index:2 and index:2 look like it was
removed.

so when it comes to component use key on the component or/ our most part of the
UI component so that any time key changes it will update/swipe enitre new
component not just the small peices.

```
ex:3
items.map(item => (
          <li>
            <button onClick={() => removeItem(item)}>remove</button>{' '}
            <label htmlFor={`${item.id}-input`}>{item.value}</label>{' '}
            <input id={`${item.id}-input`} defaultValue={item.value} />
          </li>
          )
)
```

in ex:3 when ever list item get updated the default value will not be updated
even though value has been changed. this is how react works it only update
what's importent. and ignore rest of the code.

but if we set item list with unique id:key then it's easyer for react to know
what happend.

let's take a look.

```
ex:2
previusDOM array = [{key:1,fruit:apple},{key:2,fruit:orange},{key:3,fruit:grape}]
updatedDOM array = [{key:1,fruit:apple},{key:3,fruit:grape}]

```

now react have more-reliable piece of info key with don't get changes and mutate
as the array changes.

now react will compare key and fruit data.

```
before= key was 1 and fruit was apple. After= key is 1 and fruit was apple. no update
before= key was 2 and fruit was orange. Update ohh key 2 was removed.
beofore= key was 3 and fruit was grape. After= key is 3 and fruit was grape. no update
```

now react have more control over the data manupulation help us in better data/
upate management.

- if we add key to the outer shell/component then when ever any key get updated
  entire component re-render rahter then re-rendering specific part of the
  component.

```
items.map(item => (
          <li key={item.id}>
            <button onClick={() => removeItem(item)}>remove</button>{' '}
            <label htmlFor={`${item.id}-input`}>{item.value}</label>{' '}
            <input id={`${item.id}-input`} defaultValue={item.value} />
          </li>
          )
)
```

anytime id changes re-render happen to the entire component.

as a side Note: everyCompoent has a inbuild prop of key , React.createElement()
object as a proprty by the name of key spicialy created for haveing more control
over updated on re-rendering.

---
