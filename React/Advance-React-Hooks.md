> ## React version: 18.2.0

# React Docs: useMemo

useMemo is a React Hook that lets you cache/memoize the result of a calculation
between re-renders.

```js
const cachedValue = useMemo(calculateValue, dependencies);
```

## Parameters:

1. `calculateValue`: The function calculating the value that you want to cache. It
   should be pure, should take no arguments, and should return a value of any
   type. React will call your function during the initial render. On next
   renders, React will return the same value again if the dependencies have not
   changed since the last render. Otherwise, it will call calculateValue, return
   its result, and store it so it can be reused later.

2. `dependencies`: The list of all reactive values referenced inside of the
   calculateValue code. Reactive values include props, state, and all the
   variables and functions declared directly inside your component body. If your
   linter is configured for React, it will verify that every reactive value is
   correctly specified as a dependency. The list of dependencies must have a
   constant number of items and be written inline like [dep1, dep2, dep3]. React
   will compare each dependency with its previous value using the Object.is
   comparison.

## Returns

On the initial render, useMemo returns the result of calling calculateValue with
no arguments.

During next renders, it will either return an already stored value from the last
render (if the dependencies haven’t changed), or call calculateValue again, and
return the result that calculateValue has returned.

### caveats

React will not throw away the cached value unless there is a specific reason to
do that.

### Usage

1. Skipping expensive recalculations

   ```js
   import { useMemo } from "react";

   function TodoList({ todos, tab, theme }) {
     const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
     // ...
   }
   ```

   You need to pass two things to useMemo:

   A calculation function that takes no arguments, like () =>, and returns what you
   wanted to calculate. A list of dependencies including every value within your
   component that’s used inside your calculation. On the initial render, the value
   you’ll get from useMemo will be the result of calling your calculation.

   On every subsequent render, React will compare the dependencies with the
   dependencies you passed during the last render. If none of the dependencies have
   changed (compared with Object.is), useMemo will return the value you already
   calculated before. Otherwise, React will re-run your calculation and return the
   new value.

   In other words, useMemo caches a calculation result between re-renders until its
   dependencies change.

2. Skipping re-rendering of components

   when you app re-render everything inside re-render and most of the time we don't
   want to re-render everything. so we can use useMemo/memo to skip re-rendering
   untill any dependency changes.

3. Memoizing a dependency of another Hook

4. Memoizing a function

   as we studied that on re-render function are re-created and as non-premitive
   data behave in javascript.

   to avoid un-nessesery re-rendering. you can use useMemo

   ```js
   const handleSubmit = useMemo(() => {
     return (orderDetails) => {
       post("/product/" + product.id + "/buy", {
         referrer,
         orderDetails,
       });
     };
   }, [productId, referrer]);

   return <Form onSubmit={handleSubmit} />;
   ```

   as we know useMemo take function and store any data type, so it will execute it
   function which returns a value that value can be function or object does not
   matter. in case of function as data type in useMemo you have to nest that
   function and this is so common ouccerence that react has created a useCallback
   hook so you don't have to nest.

   ```js
   export default function Page({ productId, referrer }) {
     const handleSubmit = useCallback(
       (orderDetails) => {
         post("/product/" + product.id + "/buy", {
           referrer,
           orderDetails,
         });
       },
       [productId, referrer]
     );

     return <Form onSubmit={handleSubmit} />;
   }
   ```

   useCallback hook don't have nested hooks.

### Note: when to use useMemo

You should only rely on useMemo as a performance optimization. If your code
doesn’t work without it, find the underlying problem and fix it first. Then you
may add useMemo to improve performance.

### Note: How to tell if a calculation is expensive?

you can add console.log and check if rendering is taking more then 1 ms then you
should consider to optimize it.

### Note: Should you add useMemo everywhere?

If in your app most interactions are coarse(constaintly changing) (like
replacing a page or an entire section), memoization is usually unnecessary. On
the other hand, if your app is more like a drawing editor, and most interactions
are granular (like moving shapes), then you might find memoization very helpful.

Optimizing with useMemo is only valuable in a few cases:

1. The calculation you’re putting in useMemo is noticeably slow, and its
   dependencies rarely change.
2. You pass it as a prop to a component wrapped in memo. You want to skip
   re-rendering if the value hasn’t changed. Memoization lets your component
   re-render only when dependencies aren’t the same.
3. The value you’re passing is later used as a dependency of some Hook. For
   example, maybe another useMemo calculation value depends on it. Or maybe you
   are depending on this value from useEffect.

There is no benefit to wrapping a calculation in useMemo in other cases.

## Troubleshooting

1. My useMemo call is supposed to return an object, but returns undefined

   This code doesn’t work:

   ```js
     // 🔴 You can't return an object from an arrow function with
     () => {
     const searchOptions = useMemo(() => {
       matchMode: 'whole-word',
       text: text
     }, [text]);
   ```

   In JavaScript, () => { starts the arrow function body, so the { brace is not a
   part of your object. This is why it doesn’t return an object, and leads to
   mistakes. You could fix it by adding parentheses like ({ and }):

   This works, but is easy for someone to break again

   ```js
   const searchOptions = useMemo(
     () => ({
       matchMode: "whole-word",
       text: text,
     }),
     [text]
   );
   ```

   However, this is still confusing and too easy for someone to break by removing
   the parentheses.

   To avoid this mistake, write a return statement explicitly:

   ```js
   // ✅ This works and is explicit
   const searchOptions = useMemo(() => {
     return {
       matchMode: "whole-word",
       text: text,
     };
   }, [text]);
   ```

2. Every time my component renders, the calculation in useMemo re-runs

   Make sure you’ve specified the dependency array as a second argument!

   If you forget the dependency array, useMemo will re-run the calculation every
   time:

   ```js
   function TodoList({ todos, tab }) {
     // 🔴 Recalculates every time: no dependency array
     const visibleTodos = useMemo(() => filterTodos(todos, tab));
     // ...
   ```

   This is the corrected version passing the dependency array as a second argument:

   ```js
   function TodoList({ todos, tab }) {
     // ✅ Does not recalculate unnecessarily
     const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
     // ...
   ```

3. I need to call useMemo for each list item in a loop, but it’s not allowed

   Suppose the Chart component is wrapped in memo. You want to skip re-rendering
   every Chart in the list when the ReportList component re-renders. However, you
   can’t call useMemo in a loop:

   ```js
   function ReportList({ items }) {
     return (
       <article>
         {items.map((item) => {
           // 🔴 You can't call useMemo in a loop like this:
           const data = useMemo(() => calculateReport(item), [item]);
           return (
             <figure key={item.id}>
               <Chart data={data} />
             </figure>
           );
         })}
       </article>
     );
   }
   ```

   Instead, extract a component for each item and memoize data for individual
   items:

   ```js
   function ReportList({ items }) {
     return (
       <article>
         {items.map((item) => (
           <Report key={item.id} item={item} />
         ))}
       </article>
     );
   }

   function Report({ item }) {
     // ✅ Call useMemo at the top level:
     const data = useMemo(() => calculateReport(item), [item]);
     return (
       <figure>
         <Chart data={data} />
       </figure>
     );
   }
   ```

   Alternatively, you could remove useMemo and instead wrap Report itself in memo.
   If the item prop does not change, Report will skip re-rendering, so Chart will
   skip re-rendering too:

   ```js
   function ReportList({ items }) {
     // ...
   }

   const Report = memo(function Report({ item }) {
     const data = calculateReport(item);
     return (
       <figure>
         <Chart data={data} />
       </figure>
     );
   });
   ```

---

# React Docs: useCallback

useCallback is a React Hook that lets you cache a function definition between
re-renders.

```js
const cachedFn = useCallback(fn, dependencies);
```

fn = function definition

## Parameters:

`fn`: The function value that you want to cache. It can take any arguments and
return any values. React will return (not call!) your function back to you
during the initial render. On next renders, React will give you the same
function again if the dependencies have not changed since the last render.
Otherwise, it will give you the function that you have passed during the current
render, and store it in case it can be reused later. React will not call your
function. The function is returned to you so you can decide when and whether to
call it.

`dependencies`: The list of all reactive values referenced inside of the fn code.
Reactive values include props, state, and all the variables and functions
declared directly inside your component body. If your linter is configured for
React, it will verify that every reactive value is correctly specified as a
depende0ncy. The list of dependencies must have a constant number of items and
be written inline like [dep1, dep2, dep3]. React will compare each dependency
with its previous value using the Object.is comparison algorithm.

## Return

Returns On the initial render, useCallback returns the fn function you have
passed.

During subsequent renders, it will either return an already stored fn function
from the last render (if the dependencies haven’t changed), or return the fn
function you have passed during this render.

### NOTE

useMemo take calculate function: which execute the caculate function and store
it's returned value as pre-value/cach.

usecallback take function defination: which stores function itself as a cach. it
don't execute that function. it returns the function direclty. it's function can
also take parameters. so it stores the function.

on inital render it returns that function and in re-render it return that
function with dependendcy changed

so in simple terms:

1. useMemo: Calls your function and caches its result
2. useCallabck: Caches your function itself

## Should you add useCallback everywhere?

Caching a function with useCallback is only valuable in a few cases:

1. You pass it as a prop to a component wrapped in memo. You want to skip
   re-rendering if the value hasn’t changed. Memoization lets your component
   re-render only if dependencies changed.
2. The function you’re passing is later used as a dependency of some Hook. For
   example, another function wrapped in useCallback depends on it, or you depend
   on this function from useEffect.

There is no benefit to wrapping a function in useCallback in other cases.

## you can make a lot of memoization unnecessary by following a few principles

1. When a component visually wraps other components, let it accept JSX as
   children. Then, if the wrapper component updates its own state, React knows
   that its children don’t need to re-render.

   (When we wrap one component around another, we call the first one the "wrapper"
   and the second one the "child". Now, if we change something in the wrapper, it
   might also affect the child.

   But sometimes, the wrapper might change something that doesn't affect the child.
   In this case, we don't want the child to change or "re-render" because it wastes
   time and slows down the website.

   So, to help React understand that the child doesn't need to change, we can write
   the wrapper so that it accepts something called "JSX as children". This means
   that we can put the child inside the wrapper like a little toy inside a bigger
   toy.

   Then, when the wrapper changes something, React will check if it affects the
   child. If it doesn't, the child won't change. If it does, React will make the
   child change.

   This is a smart way to make websites faster and easier to build!)

2. Prefer local state and don’t lift state up any further than necessary. Don’t
   keep transient state like forms and whether an item is hovered at the top of
   your tree or in a global state library.

   (No lifting state, do co-location as much as possible )

3. Keep your rendering logic pure. If re-rendering a component causes a problem
   or produces some noticeable visual artifact, it’s a bug in your component!
   Fix the bug instead of adding memoization.

4. Avoid unnecessary Effects that update state. Most performance problems in
   React apps are caused by chains of updates originating from Effects that
   cause your components to render over and over.

5. Try to remove unnecessary dependencies from your Effects. For example,
   instead of memoization, it’s often simpler to move some object or a function
   inside an Effect or outside the component.

## caveat

React will not throw away the cached function unless there is a specific reason
to do that.

## useage

1. Skipping re-rendering of components.

   When you optimize rendering performance, you will sometimes need to cache the
   functions that you pass to child components. Let’s first look at the syntax for
   how to do this, and then see in which cases it’s useful.

   To cache a function between re-renders of your component, wrap its definition
   into the useCallback Hook:

   ```js
   import { useCallback } from 'react';

   function ProductPage({ productId, referrer, theme }) {
     const handleSubmit = useCallback((orderDetails) => {
       post('/product/' + productId + '/buy', {
         referrer,
         orderDetails,
       });
     }, [productId, referrer]);
     // ...
   ```

   You need to pass two things to useCallback:

   A function definition that you want to cache between re-renders. A list of
   dependencies including every value within your component that’s used inside your
   function. On the initial render, the returned function you’ll get from
   useCallback will be the function you passed.

   On the following renders, React will compare the dependencies with the
   dependencies you passed during the previous render. If none of the dependencies
   have changed (compared with Object.is), useCallback will return the same
   function as before. Otherwise, useCallback will return the function you passed
   on this render.

   In other words, useCallback caches a function between re-renders until its
   dependencies change.

2. Updating state from a memoized callback

   Sometimes, you might need to update state based on previous state from a
   memoized callback.

   This handleAddTodo function specifies todos as a dependency because it computes
   the next todos from it:

   ```js
   function TodoList() {
     const [todos, setTodos] = useState([]);

     const handleAddTodo = useCallback(
       (text) => {
         const newTodo = { id: nextId++, text };
         setTodos([...todos, newTodo]);
       },
       [todos]
     );
     // ...
   }
   ```

   You’ll usually want memoized functions to have as few dependencies as possible.
   When you read some state only to calculate the next state, you can remove that
   dependency by passing an updater function instead:

   ```js
   function TodoList() {
     const [todos, setTodos] = useState([]);

     const handleAddTodo = useCallback((text) => {
       const newTodo = { id: nextId++, text };
       setTodos((todos) => [...todos, newTodo]);
     }, []); // ✅ No need for the todos dependency
     // ...
   }
   ```

   Here, instead of making todos a dependency and reading it inside, you pass an
   instruction about how to update the state (todos => [...todos, newTodo]) to
   React. use set(prevValue => prev)

   in simple term use useState updater function prevValue => prevValue +1 rather
   then makeing un-nessesery dependency over your useCallback fn.

3. Preventing an Effect from firing too often

   useEffect dependency of function problem can be solved with useCallback with
   dependency.

   useCallback will not re-render untill it's ddependency changes and it means it
   will be same as before making it possible to be a dependency for useEffect.

   ### note

   in Memorization non-premitive data can be equal too. becouse they are compared
   based on dependency not on the data type as in javascript does.

4. Optimizing a custom Hook

   If you’re writing a custom Hook, it’s recommended to wrap any functions that it
   returns into useCallback:

   ```js
   function useRouter() {
     const { dispatch } = useContext(RouterStateContext);

     const navigate = useCallback(
       (url) => {
         dispatch({ type: "navigate", url });
       },
       [dispatch]
     );

     const goBack = useCallback(() => {
       dispatch({ type: "back" });
     }, [dispatch]);

     return {
       navigate,
       goBack,
     };
   }
   ```

   This ensures that the consumers of your Hook can optimize their own code when
   needed.

---

# React Docs: memo

memo lets you skip re-rendering a component when its props are unchanged.

```js
const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
```

Wrap a component in memo to get a memoized version of that component. This
memoized version of your component will usually not be re-rendered when its
parent component is re-rendered as long as its props have not changed. But React
may still re-render it: memoization is a performance optimization, not a
guarantee.

## Parameters

1. Component: The component that you want to memoize. The memo does not modify
   this component, but returns a new, memoized component instead. Any valid
   React component, including functions and forwardRef components, is accepted.

2. optional arePropsEqual: A function that accepts two arguments: the
   component’s previous props, and its new props. It should return true if the
   old and new props are equal: that is, if the component will render the same
   output and behave in the same way with the new props as with the old.
   Otherwise it should return false. Usually, you will not specify this
   function. By default, React will compare each prop with Object.is.

## Returns

memo returns a new React component. It behaves the same as the component
provided to memo except that React will not always re-render it when its parent
is being re-rendered unless its props have changed.

## Usage

1. Skipping re-rendering when props are unchanged
2. Updating a memoized component using state.

   Even when a component is memoized, it will still re-render when its own state
   changes. Memoization only has to do with props that are passed to the component
   from its parent.

   If you set a state variable to its current value, React will skip re-rendering
   your component even without memo. You may still see your component function
   being called an extra time, but the result will be discarded.

   Note: memo only stop the re-rendering of component when props are un-changed.
   generaly these props comes from the parent. memo component will still execute as
   normally in other cases like when its own state changes

3. Updating a memoized component using a context

   To make your component re-render only when a part of some context changes, split
   your component in two. Read what you need from the context in the outer
   component, and pass it down to a memoized child as a prop.

   Even when a component is memoized, it will still re-render when a context that
   it’s using changes. Memoization only has to do with props that are passed to the
   component from its parent.

4. Minimizing props changes

   When you use memo, your component re-renders whenever any prop is not shallowly
   equal to what it was previously. This means that React compares every prop in
   your component with its previous value using the Object.is comparison. Note that
   Object.is(3, 3) is true, but Object.is({}, {}) is false.

   To get the most out of memo, minimize the times that the props change. For
   example, if the prop is an object, prevent the parent component from re-creating
   that object every time by using useMemo:

   ```js
   function Page() {
     const [name, setName] = useState("Taylor");
     const [age, setAge] = useState(42);

     const person = useMemo(() => ({ name, age }), [name, age]);

     return <Profile person={person} />;
   }

   const Profile = memo(function Profile({ person }) {
     // ...
   });
   ```

   A better way to minimize props changes is to make sure the component accepts the
   minimum necessary information in its props. For example, it could accept
   individual values instead of a whole object:

   ```js
   function Page() {
     const [name, setName] = useState("Taylor");
     const [age, setAge] = useState(42);
     return <Profile name={name} age={age} />;
   }

   const Profile = memo(function Profile({ name, age }) {
     // ...
   });
   ```

   Even individual values can sometimes be projected to ones that change less
   frequently. For example, here a component accepts a boolean indicating the
   presence of a value rather than the value itself:

   ```js
   function GroupsLanding({ person }) {
     const hasGroups = person.groups !== null;
     return <CallToAction hasGroups={hasGroups} />;
   }

   const CallToAction = memo(function CallToAction({ hasGroups }) {
     // ...
   });
   ```

   When you need to pass a function to memoized component, either declare it
   outside your component so that it never changes, or useCallback to cache its
   definition between re-renders.

## Specifying a custom comparison function

In rare cases it may be infeasible to minimize the props changes of a memoized
component. In that case, you can provide a custom comparison function, which
React will use to compare the old and new props instead of using shallow
equality. This function is passed as a second argument to memo. It should return
true only if the new props would result in the same output as the old props;
otherwise it should return false.

```js
const Chart = memo(function Chart({ dataPoints }) {
  // ...
}, arePropsEqual);

function arePropsEqual(oldProps, newProps) {
  return (
    oldProps.dataPoints.length === newProps.dataPoints.length &&
    oldProps.dataPoints.every((oldPoint, index) => {
      const newPoint = newProps.dataPoints[index];
      return oldPoint.x === newPoint.x && oldPoint.y === newPoint.y;
    })
  );
}
```

If you do this, use the Performance panel in your browser developer tools to
make sure that your comparison function is actually faster than re-rendering the
component. You might be surprised.

When you do performance measurements, make sure that React is running in the
production mode.

## Pitfall

If you provide a custom arePropsEqual implementation, you must compare every
prop, including functions. Functions often close over the props and state of
parent components. If you return true when oldProps.onClick !==
newProps.onClick, your component will keep “seeing” the props and state from a
previous render inside its onClick handler, leading to very confusing bugs.

Avoid doing deep equality checks inside arePropsEqual unless you are 100% sure
that the data structure you’re working with has a known limited depth. Deep
equality checks can become incredibly slow and can freeze your app for many
seconds if someone changes the data structure later.

### Note

make sure you Don't have to use coustom comparision function becouse in most of
the cases it will be slower and make your app slow. aviod as much you can.

## Troubleshooting

My component re-renders when a prop is an object, array, or function React
compares old and new props by shallow equality: that is, it considers whether
each new prop is reference-equal to the old prop. If you create a new object or
array each time the parent is re-rendered, even if the individual elements are
each the same, React will still consider it to be changed.

---

# from the Kent C Dodds chapter :2 Memorization

---

Memoization: a performance optimization technique which eliminates the need to
recompute a value for a given input by storing the original computation and
returning that stored value when the same input is provided. Memoization is a
form of caching.

## kent c dodds doc on Memorization

Another interesting aspect to memoization is the fact that the cached value you
get back is the same one you got last time.

## React's memoization

React has three APIs for memoization: memo(skip re-render of the component),
useMemo(cach calculate function return value), and useCallback(cach function it
self). The caching strategy React has adopted has a size of 1. That is, they
only keep around the most recent value of the input and result. There are
various reasons for this decision, but it satisfies the primary use case for
memoizing in a React context.

So for React's memoization it's more like this:

```js
let prevInput, prevResult;
function addTwo(input) {
  if (input !== prevInput) {
    prevResult = input + 2;
  }
  prevInput = input;
  return prevResult;
}
```

With that:

```js
addTwo(3); // 5 is computed
addTwo(3); // 5 is returned from the cache 🤓
addTwo(2); // 4 is computed
addTwo(3); // 5 is computed
```

To be clear, in React's case it's not a !== comparing the prevInput. It checks
equality of each prop and each dependency individually. Let's check each one:

1. React.memo's `prevInput` is props and `prevResult` is react elements (JSX) if
   you don't change the props then it will return the prevResult from the catch.
   it will not re-calculate the component.

   ```js
   const MemoComp = React.memo(Comp)

     // then, when you render it:
     <MemoComp prop1="a" prop2="b" /> // renders new elements

     // rerender it with the same props:
     <MemoComp prop1="a" prop2="b" /> // renders previous elements 🤓
   ```

2. React.useMemo's `prevInput` is the dependency array and `prevResult` is
   whatever your function returns

   ```js
   const posts = React.useMemo(() => getPosts(searchTerm), [searchTerm]);
   // initial render with searchTerm = 'puppies':
   // - getPosts is called
   // - posts is a new array of posts
   // rerender with searchTerm = 'puppies':
   // - getPosts is *not* called
   // - posts is the same as last time 🤓
   ```

3. React.useCallback's `prevInput` is the dependency array and `prevResult` is
   the function it self.

   ```js
   const launch = React.useCallback(
     () => launchCandy({ type, distance }),
     [type, distance]
   );
   // initial render with type = 'twix' and distance = '15m':
   // - launch is equal to the callback passed to useCallback this render
   // rerender with type = 'twix' and distance = '15m':
   // - launch is equal to the callback passed to useCallback last render 🤓
   ```

NOTE: even thought memorization is helpfull for performance boost but keep in
mind it come with a cost on memoery and processing that memory. in memorization
react has to store two extra info

1. prevInput.
2. PrevResult.

normal function get garbage collected but memorized function don't so extra
memory is required something it can also de-crease the performance of the app if
not carefull. generally speeking only use memorization is doing computation
heavy calculation.

## The value of memoization in React

There are two reasons you might want to memoize something:

1. Improve performance by avoiding expensive computations (like re-rendering
   expensive components or calling expensive functions).
2. Value stability (stabilizing fn and obj)

let's talk about value un-stability that happen in React. as we know useEffect
also take dependency array but what happen if useEffect is dependent on an
function or object. in simple terms non-premitive data type.

then as we know any time the component re-render due to any reason. useEffect
function will always be called becouse non-premitive data type are not same even
if they have the same value and as re-render happen all the function or object
are re-created again. this cozs useEffect to render on every re-render of the
component. which make's the useEffect useless to use here.

## NOTE

but useMemo, useCallback compare data on the bases of dependency not data-type
so obj1 === obj2 and fn1 === fn2. helps with stoping un-nessesery render and
value stability.

so useing useMemo or useCallback provied stability to use;

```js
const myObj = React.useMemo(() => {
  return { name: "raju" };
}, [raju]);

useEffect(() => {}, [myObj, myFunc]);
```

useMemo will only re-render if we change the value of name and even though
myObject is a nature of object useEffect will not be un-stable becouse myObj
will not get re-asign on every render untill we change the value of name. same
with useCallback.

so till now we know.

1. memo is used when we want to memorize a component if it's props changes then
   we re-render.
2. useMemo and useCallback are similar they both take function and dependency
   array and when dependecy changes we re-render.

## but what is the difference and when to use usecallback or useMemo?

let's take an example:

1. original function:

   ```js
   const dispense = (candy) => {
     setCandies((allCandies) => allCandies.filter((c) => c !== candy));
   };
   ```

2. function with useCallback.

   ```js
   const dispense = (candy) => {
     setCandies((allCandies) => allCandies.filter((c) => c !== candy));
   };
   const dispenseCallback = React.useCallback(dispense, []);
   ```

which one of the above will be more performant.

they're exactly the same except the useCallback version is doing more work. Not
only do we have to define the function, but we also have to define an array ([])
and call the React.useCallback which itself is setting properties/running
through logical expressions etc.

So in both cases JavaScript must allocate memory for the function definition on
every render and depending on how useCallback is implemented,

I'd like to mention also that on the second render of the component, the
original dispense function gets garbage collected (freeing up memory space) and
then a new one is created. However with useCallback the original dispense
function won't get garbage collected and a new one is created, so you're
worse-off from a memory perspective as well.

if you have dependency then in react useCallback will also keep track of all the
previous dependency and previous result.

## How is useMemo different, but similar?

useMemo is similar to useCallback except it allows you to apply memoization to
any value type (not just functions). It does this by accepting a function which
returns the value and then that function is only called when the value needs to
be retrieved (which typically will only happen once each time an element in the
dependencies array changes between renders).

so in simple word useMemo can be used for any non-premitive data type. how it
work. useMemo has a function which it will call to read the data inside. and
useMemo will only re-render when dependency changes.

```js
useMemo(()=> {return [can be array],{cab ne object},()=>{can be a
function}},[depenency])
```

on the otherHand useCallback is specificly used for functions.

So, if I didn't want to initialize that array of initialCandies every render, I
could make this change:

```diff
- const initialCandies = ['snickers', 'skittles', 'twix', 'milky way']

+ const initialCandies = React.useMemo(
+   () => ['snickers', 'skittles', 'twix', 'milky way'],
+   [],
+ )
```

And I would avoid that problem, but the savings would be so minimal that the
cost of making the code more complex just isn't worth it. In fact, it's probably
worse to use useMemo for this as well because again we're making a function call
and that code is doing property assignments etc.

What's the point? The point is this:

Performance optimizations are not free. They ALWAYS come with a cost but do NOT
always come with a benefit to offset that cost.

Therefore, optimize responsibly.

## So when should I useMemo and useCallback?

There are specific reasons both of these hooks are built-into React:

1. Referential equality (useCallback)
2. Computationally expensive calculations (useMemo)

Referential equality means two non-premitive data (which are refrence type) of
same type are not equal even if they both have the same values.

```js
const obj1 = {name = "dogesh"},
const obj2 = {name = "dogesh"},
// obj1!== obj2
```

Ex:

```js
function Foo({ bar, baz }) {
  const options = { bar, baz };
  React.useEffect(() => {
    buzz(options);
  }, [options]); // we want this to re-run if bar or baz change
  return <div>foobar</div>;
}

function Blub() {
  return <Foo bar="bar value" baz={3} />;
}
```

The reason this is problematic is because useEffect is going to do a referential
equality check on options between every render, and thanks to the way JavaScript
works, options will be new every time so when React tests whether options
changed between renders it'll always evaluate to true, meaning the useEffect
callback will be called after every render rather than only when bar and baz
change.

There are two things we can do to fix this:

```js
// option 1
function Foo({ bar, baz }) {
  React.useEffect(() => {
    const options = { bar, baz };
    buzz(options);
  }, [bar, baz]); // we want this to re-run if bar or baz change
  return <div>foobar</div>;
}
```

That's a great option and if this were a real thing that's how I'd fix this.

But there's one situation when this isn't a practical solution: If bar or baz
are (non-primitive) objects/arrays/functions/etc:

```js
function Blub() {
  const bar = () => {};
  const baz = [1, 2, 3];
  return <Foo bar={bar} baz={baz} />;
}
```

This is precisely the reason why useCallback and useMemo exist. So here's how
you'd fix that (all together now):

```js
function Foo({ bar, baz }) {
  React.useEffect(() => {
    const options = { bar, baz };
    buzz(options);
  }, [bar, baz]);
  return <div>foobar</div>;
}

function Blub() {
  const bar = React.useCallback(() => {}, []);
  const baz = React.useMemo(() => [1, 2, 3], []);
  return <Foo bar={bar} baz={baz} />;
}
```

## React.memo (and friends)

Check this out:

```js
function CountButton({ onClick, count }) {
  return <button onClick={onClick}>{count}</button>;
}

function DualCounter() {
  const [count1, setCount1] = React.useState(0);
  const increment1 = () => setCount1((c) => c + 1);

  const [count2, setCount2] = React.useState(0);
  const increment2 = () => setCount2((c) => c + 1);

  return (
    <>
      <CountButton count={count1} onClick={increment1} />
      <CountButton count={count2} onClick={increment2} />
    </>
  );
}
```

Every time you click on either of those buttons, the DualCounter's state changes
and therefore re-renders which in turn will re-render both of the CountButtons.
However, the only one that actually needs to re-render is the one that was
clicked right? So if you click the first one, the second one gets re-rendered,
but nothing changes. We call this an "unnecessary re-render."

MOST OF THE TIME YOU SHOULD NOT BOTHER OPTIMIZING UNNECESSARY RERENDERS. React
is VERY fast and there are so many things I can think of for you to do with your
time that would be better than optimizing things like this. In fact, the need to
optimize stuff with what I'm about to show you is so rare that I've literally
never needed to do it in the 3 years I worked on PayPal products and the even
longer time that I've been working with React.

However, there are situations when rendering can take a substantial amount of
time (think highly interactive Graphs/Charts/Animations/etc.). Thanks to the
pragmatistic nature of React, there's an escape hatch:

```js
const CountButton = React.memo(function CountButton({ onClick, count }) {
  return <button onClick={onClick}>{count}</button>;
});
```

Now React will only re-render CountButton when its props change! Woo! But we're
not done yet. Remember that whole referential equality thing? In the DualCounter
component, we're defining the increment1 and increment2 functions within the
component functions which means every time DualCounter is re-rendered, those
functions will be new and therefore React will re-render both of the
CountButtons anyway.

So this is the other situation where useCallback and useMemo can be of help:

```js
const CountButton = React.memo(function CountButton({ onClick, count }) {
  return <button onClick={onClick}>{count}</button>;
});

function DualCounter() {
  const [count1, setCount1] = React.useState(0);
  const increment1 = React.useCallback(() => setCount1((c) => c + 1), []);

  const [count2, setCount2] = React.useState(0);
  const increment2 = React.useCallback(() => setCount2((c) => c + 1), []);

  return (
    <>
      <CountButton count={count1} onClick={increment1} />
      <CountButton count={count2} onClick={increment2} />
    </>
  );
}
```

Now we can avoid the so-called "unnecessary re-renders" of CountButton.

I would like to re-iterate that I strongly advise against using React.memo (or
it's friends PureComponent and shouldComponentUpdate) without measuring because
those optimizations come with a cost and you need to make sure you know what
that cost will be as well as the associated benefit so you can determine whether
it will actually be helpful (and not harmful) in your case, and as we observe
above it can be tricky to get right all the time so you may not be reaping any
benefits at all anyway.

## Computationally expensive calculations useMemo

This is the other reason that useMemo is a built-in hook for React (note that
this one does not apply to useCallback). The benefit to useMemo is that you can
take a value like:

```js
const a = {b: props.b}

And get it lazily:
const a = React.useMemo(() => ({b: props.b}), [props.b])
```

This isn't really useful for that case above, but imagine that you've got a
function that synchronously calculates a value which is computationally
expensive to calculate (I mean how many apps actually need to calculate prime
numbers like this ever, but that's an example):

```js
function RenderPrimes({ iterations, multiplier }) {
  const primes = calculatePrimes(iterations, multiplier);
  return <div>Primes! {primes}</div>;
}
```

That could be pretty slow given the right iterations or multiplier and there's
not too much you can do about that specifically. You can't automagically make
your user's hardware faster. But you can make it so you never have to calculate
the same value twice in a row, which is what useMemo will do for you:

```js
function RenderPrimes({ iterations, multiplier }) {
  const primes = React.useMemo(
    () => calculatePrimes(iterations, multiplier),
    [iterations, multiplier]
  );
  return <div>Primes! {primes}</div>;
}
```

The reason this works is because even though you're defining the function to
calculate the primes on every render (which is VERY fast), React is only calling
that function when the value is needed. On top of that React also stores
previous values given the inputs and will return the previous value given the
same previous inputs. That's memoization at work.

---

# React Docs: useReducer

useReducer is a React Hook that lets you add a reducer to your component.

const [state, dispatch] = useReducer(reducer, initialArg, init?)

## Parameters

1. `reducer`: The reducer function that specifies how the state gets updated. It
   must be pure, should take the state and action as arguments, and should
   return the next state. State and action can be of any types.

2. `initialArg`: The value from which the initial state is calculated. It can be a
   value of any type. How the initial state is calculated from it depends on the
   next init argument.
3. `optional init`: The initializer function that should return the initial state.
   If it’s not specified, the initial state is set to initialArg. Otherwise, the
   initial state is set to the result of calling init(initialArg).

## Returns

useReducer returns an array with exactly two values:

1. The current state. During the first render, it’s set to init(initialArg) or
   initialArg fn (if there’s no init).
2. The dispatch function that lets you update the state to a different value and
   trigger a re-render.

## dispatch function

The dispatch function returned by useReducer lets you update the state to a
different value and trigger a re-render. You need to pass the action as the only
argument to the dispatch function:

```js
const [state, dispatch] = useReducer(reducer, { age: 42 });

function handleClick() {
  dispatch({ type: "incremented_age" });
  // ...
}
```

React will set the next state to the result of calling the reducer function
you’ve provided with the current state and the action you’ve passed to dispatch.

## Parameters

1. `action`: The action performed by the user. It can be a value of any type. By
   convention, an action is usually an object with a type property identifying
   it and, optionally, other properties with additional information.

## Returns

dispatch functions do not have a return value.

## Caveats

1. The dispatch function only updates the state variable for the next render. If
   you read the state variable after calling the dispatch function, you will
   still get the old value that was on the screen before your call.

2. If the new value you provide is identical to the current state, as determined
   by an Object.is comparison, React will skip re-rendering the component and
   its children. This is an optimization. React may still need to call your
   component before ignoring the result, but it shouldn’t affect your code.

3. React batches state updates. It updates the screen after all the event
   handlers have run and have called their set functions. This prevents multiple
   re-renders during a single event. In the rare case that you need to force
   React to update the screen earlier, for example to access the DOM, you can
   use flushSync.

## Usage

1. Adding a reducer to a component.

   Call useReducer at the top level of your component to manage state with a
   reducer.

   ```js
   import { useReducer } from 'react';

   function reducer(state, action) {
     // ...
   }

   function MyComponent() {
     const [state, dispatch] = useReducer(reducer, { age: 42 });
     // ...
   ```

   useReducer returns an array with exactly two items:

   The current state of this state variable, initially set to the initial state you
   provided. The dispatch function that lets you change it in response to
   interaction. To update what’s on the screen, call dispatch with an object
   representing what the user did, called an action:

   ```js
   function handleClick() {
     dispatch({ type: "incremented_age" });
   }
   ```

   React will pass the current state and the action to your reducer function. Your
   reducer will calculate and return the next state. React will store that next
   state, render your component with it, and update the UI.

   useReducer is very similar to useState, but it lets you move the state update
   logic from event handlers into a single function outside of your component.

2. Avoiding recreating the initial state.

   React saves the initial state once and ignores it on the next renders.

   ```js
   function createInitialState(username) {
     // ...
   }

   function TodoList({ username }) {
     const [state, dispatch] = useReducer(
       reducer,
       createInitialState(username)
     );
     // ...
   }
   ```

   Although the result of createInitialState(username) is only used for the initial
   render, you’re still calling this function on every render. This can be wasteful
   if it’s creating large arrays or performing expensive calculations.

   To solve this, you may pass it as an initializer function to useReducer as the
   third argument instead:

   ```js
   function createInitialState(username) {
     // ...
   }

   function TodoList({ username }) {
     const [state, dispatch] = useReducer(reducer, username, createInitialState);
     // ...
   ```

   Notice that you’re passing createInitialState, which is the function itself, and
   not createInitialState(), which is the result of calling it. This way, the
   initial state does not get re-created after initialization.

   In the above example, createInitialState takes a username argument. If your
   initializer doesn’t need any information to compute the initial state, you may
   pass null as the second argument to useReducer.

## Troubleshooting

1. I’ve dispatched an action, but logging gives me the old state value

   becouse This is because states behaves like a snapshot. Updating state requests
   another render with the new state value, but does not affect the state
   JavaScript variable in your already-running event handler.

2. I’ve dispatched an action, but the screen doesn’t update

   React will ignore your update if the next state is equal to the previous state,
   as determined by an Object.is comparison.

3. A part of my reducer state becomes undefined after dispatching

   Make sure that every case branch copies all of the existing fields when
   returning the new state:

   ```js
   function reducer(state, action) {
     switch (action.type) {
       case "incremented_age": {
         return {
           ...state, // Don't forget this!
           age: state.age + 1,
         };
       }
       // ...
     }
   }
   ```

   Without ...state above, the returned next state would only contain the age field
   and nothing else.

4. My entire reducer state becomes undefined after dispatching.

   If your state unexpectedly becomes undefined, you’re likely forgetting to return
   state in one of the cases, or your action type doesn’t match any of the case
   statements. To find why, throw an error outside the switch:

   ```js
   function reducer(state, action) {
     switch (action.type) {
       case "incremented_age": {
         // ...
       }
       case "edited_name": {
         // ...
       }
     }
     throw Error("Unknown action: " + action.type);
   }
   ```

   You can also use a static type checker like TypeScript to catch such mistakes.

---

# React docs :Extracting State Logic into a Reducer

## why we use reducer function?

Components with many state updates spread across many event handlers can get
overwhelming. For these cases, you can consolidate all the state update logic
outside your component in a single function, called a reducer.

## Consolidate state logic with a reducer

As your components grow in complexity, it can get harder to see at a glance all
the different ways in which a component’s state gets updated.

For example, the TaskApp component below holds an array of tasks in state and
uses three different event handlers to add, remove, and edit tasks: add, remove
and edit are three events that updates same task state depending on the event
call.

Each of its event handlers calls setTasks in order to update the state. As this
component grows, so does the amount of state logic sprinkled throughout it. To
reduce this complexity and keep all your logic in one easy-to-access place, you
can move that state logic into a single function outside your component, called
a “reducer”.

Managing state with reducers is slightly different from directly setting state.
Instead of telling React “what to do” by setting state, you specify “what the
user just did” by dispatching “actions” from your event handlers. (The state
update logic will live elsewhere!) So instead of “setting tasks” via an event
handler, you’re dispatching an “added/changed/deleted a task” action. This is
more descriptive of the user’s intent.

## Note: my observation

use useReducer when one state needed to be updated by multiple types of user
interaction. you can batch all the state change logic at one place with
useReducer.

## Note: about action

An action object can have any shape.

By convention, it is common to give it a string type that describes what
happened, and pass any additional information in other fields. The type is
specific to a component, so in this example either 'added' or 'added_task' would
be fine. Choose a name that says what happened!

```js
dispatch({
  // specific to component
  type: "what_happened",
  // other fields go here
});
```

## how to write reducer

Write a reducer function A reducer function is where you will put your state
logic. It takes two arguments, the current state and the action object, and it
returns the next state:

```js
function yourReducer(state, action) {
  // return next state for React to set
}
```

React will set the state to what you return from the reducer.

To move your state setting logic from your event handlers to a reducer function
in this example, you will:

1. Declare the current state (tasks) as the first argument.
2. Declare the action object as the second argument.
3. Return the next state from the reducer (which React will set the state to).

Because the reducer function takes state (tasks) as an argument, you can declare
it outside of your component. This decreases the indentation level and can make
your code easier to read.

## Why are reducers called reducer?

Although reducers can “reduce” the amount of code inside your component, they
are actually named after the reduce() operation that you can perform on arrays.

The reduce() operation lets you take an array and “accumulate” a single value
out of many:

```js
const arr = [1, 2, 3, 4, 5];
const sum = arr.reduce((result, number) => result + number); // 1 + 2 + 3 + 4 + 5
```

The function you pass to reduce is known as a “reducer”. It takes the result so
far and the current item, then it returns the next result. React reducers are an
example of the same idea: they take the state so far and the action, and return
the next state. In this way, they accumulate actions over time into state.

## Comparing useState and useReducer

Reducers are not without downsides! Here’s a few ways you can compare them:

1. Code size: Generally, with useState you have to write less code upfront. With
   useReducer, you have to write both a reducer function and dispatch actions.
   However, useReducer can help cut down on the code if many event handlers
   modify state in a similar way.
2. Readability: useState is very easy to read when the state updates are simple.
   When they get more complex, they can bloat your component’s code and make it
   difficult to scan. In this case, useReducer lets you cleanly separate the how
   of update logic from the what happened of event handlers.
3. Debugging: When you have a bug with useState, it can be difficult to tell
   where the state was set incorrectly, and why. With useReducer, you can add a
   console log into your reducer to see every state update, and why it happened
   (due to which action). If each action is correct, you’ll know that the
   mistake is in the reducer logic itself. However, you have to step through
   more code than with useState.
4. Testing: A reducer is a pure function that doesn’t depend on your component.
   This means that you can export and test it separately in isolation. While
   generally it’s best to test components in a more realistic environment, for
   complex state update logic it can be useful to assert that your reducer
   returns a particular state for a particular initial state and action.
5. Personal preference: Some people like reducers, others don’t. That’s okay.
   It’s a matter of preference. You can always convert between useState and
   useReducer back and forth: they are equivalent!

We recommend using a reducer if you often encounter bugs due to incorrect state
updates in some component, and want to introduce more structure to its code. You
don’t have to use reducers for everything: feel free to mix and match! You can
even useState and useReducer in the same component.

## Recap

1. To convert from useState to useReducer

   - Dispatch actions from event
     handlers

   - Write a reducer function that returns the next state for a given state and action

   - Replace useState with useReducer.

2. Reducers require you to write a bit more code, but they help with debugging
   and testing.
3. Reducers must be pure.
4. Each action describes a single user interaction.
5. Use Immer library if you want to write reducers in a mutating style.

### NOTE

useReducer take:

1. reducer function
2. initalState
3. optional: initalState fn which take initalState/prevState as argunment and
   what ever init Fn return will be set to be the initalState. you can use init
   fn as lazy initilizer funtion.

---

## chaper: 2 extra my Observation

---

video: 0078

`action`: action can be anything, it can be an object, a function, an expression,
general convension is to make action an object with property as type of user
action and other extra info you may wana pass to your reducer function.

in other words action is just a variable you can asign any data type to it.

Ex: here use used action as a function or string both

```js
function useDarkMode() {
  const preferDarkQuery = '(prefers-color-scheme: dark)'
-  const [mode, setMode] = React.useReducer(
    (prevMode, nextMode) =>
      typeof nextMode === 'function' ? nextMode(prevMode) : nextMode,
    'light',
    () =>
      window.localStorage.getItem('colorMode') ||
      (window.matchMedia(preferDarkQuery).matches ? 'dark' : 'light'),
-  )

  React.useEffect(() => {
    const mediaQuery = window.matchMedia(preferDarkQuery)
-    const handleChange = () => setMode(mediaQuery.matches ? 'dark' : 'light')
    mediaQuery.addListener(handleChange)
    return () => mediaQuery.removeListener(handleChange)
  }, [])

  React.useEffect(() => {
    window.localStorage.setItem('colorMode', mode)
  }, [mode])

  return [mode, setMode]
}
```

## when to use reducer vs state

1. when to use useState = When it's just an independent element of state you're
   managing: useState
2. when to use useReducer = When one element of your state relies on the value
   of another element of your state in order to update: useReducer

## useReducer refacter to support useState

1. `useReducer`

   ```js
   const useStateReducer = (prevState, newState) =>
     typeof newState === "function" ? newState(prevState) : newState;

   const useStateInitializer = (initialValue) =>
     typeof initialValue === "function" ? initialValue() : initialValue;

   function useState(initialValue) {
     return React.useReducer(
       useStateReducer,
       initialValue,
       useStateInitializer
     );
   }
   ```

2. `useState: initalization`

   ```js
   useState(); // no initial value
   useState(initialValue); // a literal initial value
   useState(() => initialValue); // a lazy initial value
   ```

3. `useState : return`

   ```js
   const [state, setState] = useState();
   setState(newState);
   setState((previousState) => newState);
   ```

### Note:

useReducer dispatch function is stable by default it's not re-created on every render. due to that we can directly use it in useEffect dependency if we want.

---

# React Docs: createContext

createContext lets you create a context that components can provide or read.

```js
const SomeContext = createContext(defaultValue);
```

we are creating a refrence/context point which is an object and provide two
things

1. provider with value props
2. counsumer which is replaced with useContext hook.

create context take a default value which is used when things go wrong.

## how to create a context?

Call createContext outside of any components to create a context.

```JS
import { createContext } from 'react';

const ThemeContext = createContext('light');
```

## Parameters

`defaultValue`: The value that you want the context to have when there is no
matching context provider in the tree above the component that reads context. If
you don’t have any meaningful default value, specify null. The default value is
meant as a “last resort” fallback. It is static and never changes over time.

## Returns

createContext returns a context object.

The context object itself does not hold any information. It represents which
context other components read or provide. Typically, you will use
SomeContext.Provider in components above to specify the context value,and use
useContext to get the value.

## SomeContext.Provider

Wrap your components into a context provider to specify the value of this
context for all components inside:

```JS
function App() {
  const [theme, setTheme] = useState('light');
  // ...
  return (
    <ThemeContext.Provider value={theme}>
      <Page />
    </ThemeContext.Provider>
  );
}
```

## Props

`value`: The value that you want to pass to all the components reading this
context inside this provider, no matter how deep. The context value can be of
any type. A component calling useContext(SomeContext) inside of the provider
receives the value of the innermost corresponding context provider above it.

## useContext

```JS
function Button() {
  // ✅ Recommended way
  const theme = useContext(ThemeContext);
  return <button className={theme} />;
}
```

Props children: A function. React will call the function you pass with the
current context value determined by the same algorithm as useContext() does, and
render the result you return from this function. React will also re-run this
function and update the UI whenever the context from the parent components
changes.

## usecase of createContext

1. Creating context.

   Context lets components pass information deep down without explicitly passing
   props. Call createContext outside any components to create one or more contexts.

   ```js
   import { createContext } from "react";

   const ThemeContext = createContext("light");
   const AuthContext = createContext(null);

   function App() {
     const [theme, setTheme] = useState("dark");
     const [currentUser, setCurrentUser] = useState({ name: "Taylor" });
     return (
       <ThemeContext.Provider value={theme}>
         <AuthContext.Provider value={currentUser}>
           <Page />
         </AuthContext.Provider>
       </ThemeContext.Provider>
     );
   }
   ```

   ```js
   function Page() {
     const theme = useContext(ThemeContext);
     const currentUser = useContext(AuthContext);
   }
   ```

2. Importing and exporting context from a file

   components in different files will need access to the same context. This is why
   it’s common to declare contexts in a separate file. Then you can use the export
   statement to make context available for other files.

   ```js
   import { createContext } from "react";

   export const ThemeContext = createContext("light");
   export const AuthContext = createContext(null);

   function App() {
     const [theme, setTheme] = useState("dark");
     const [currentUser, setCurrentUser] = useState({ name: "Taylor" });
     return (
       <ThemeContext.Provider value={theme}>
         <AuthContext.Provider value={currentUser}>
           <Page />
         </AuthContext.Provider>
       </ThemeContext.Provider>
     );
   }
   ```

   ```js
   import { useContext } from "react";
   import { ThemeContext, AuthContext } from "App";

   function Page() {
     const theme = useContext(ThemeContext);
     const currentUser = useContext(AuthContext);
   }
   ```

## Note:

createContext has two different useCase

1. createContext as a refrence. if you create a context and wrap the child
   componet with that context then in that child it doesn't matter how deep it
   is. we can accese the context provider value without export and import.

   to keep in mind is that: child will take refrence to the closest parent who have
   the context name provided.

2. you can also export the context to other file while is a non-child component
   for that you need to use import and export.

---

# useContext

useContext is a React Hook that lets you read and subscribe to context from your
component.

```js
const value = useContext(SomeContext);
```

here value will read the data that we have asigned to <<span></span>SomeContext.provider
value(...here...)> and useContext is acutally subscribing to the SomeContext
that we have exported/imported from the other files.

## useContext(SomeContext)

Call useContext at the top level of your component to read and subscribe to
context.

```js
import { useContext } from "react";

function MyComponent() {
  const theme = useContext(ThemeContext);
  // ...
}
```

## Parameters

`SomeContext`: The context that you’ve previously created with createContext. The
context itself does not hold the information, it only represents the kind of
information you can provide or read from components.

## Returns

useContext returns the context value for the calling component. It is determined
as the value passed to the closest SomeContext.Provider above the calling
component in the tree. If there is no such provider, then the returned value
will be the defaultValue you have passed to createContext for that context. The
returned value is always up-to-date. React automatically re-renders components
that read some context if it changes.

## Caveats

1. useContext() call in a component is not affected by providers returned from
   the same component. The corresponding <Context.Provider> needs to be above
   the component doing the useContext() call.
2. React automatically re-renders all the children that use a particular context
   starting from the provider that receives a different value. The previous and
   the next values are compared with the Object.is comparison. Skipping
   re-renders with memo does not prevent the children receiving fresh context
   values.
3. If your build system produces duplicates modules in the output (which can
   happen with symlinks), this can break context. Passing something via context
   only works if SomeContext that you use to provide context and SomeContext
   that you use to read it are exactly the same object, as determined by a ===
   comparison.

## Note (don't use useContext)

reference = [React Training](https://www.youtube.com/watch?v=3XaXKiXtNjw&feature=youtu.be)

we think useContext is the solution for the prop drilling.

prop drilling is when you have to pass props from parent to more then 1 level
deep when level that are in the middle don't require the prop for them self they
just pass to the next child who ever needs it.

even though useContext is a valid solution. you should keep in mind that it's
not the best solution

useContext is more of a implisit value refrencial. what i mean is. the component
you need to pass the prop need to be wrapped with the useContext.provider
wrapper. let say we wrap the child componet and the prop that we are useing for
context is require 5 level deep with in the parent->wrap child wrap->

```
child ->
child level-1->
child level-2->
child level-3->
child level-4->
----------------------
child           <------
----------------------
level-5->
```

even though useContext solve the problem. but it can also create problem if you
want to use let's say child level-2 some where else in the file. then you move
the child level-2.

```
child level-2->
child level-3->
child level-4->
child level-5->
(now child level-5 is out-Of-Context)
```

and it will fail to acess the data so to keep the app in working
child from level-1 to level-5 need to be locked with in the parent Component for
the context to work. this is a big problem and can be easyliy be solved with the
help of component composition.

## Component composition.

```js
const [name, setName] = useState("Bhola");
```

```js
function Parent(){
  return (
    <Child >
     <Child-Level-1>
      <Child-Level-2>
       <Child-Level-3>
        <Child-Level-4>
         <Child-Level-5 prop={name}>
        </Child-Level-4>
       </Child-Level-3>
      </Child-Level-2>
     </Child-Level-1>
    </Child>
  )
}

```

now we can accese the data as in child-level-5 without any prop drilling and
without useContext and it's not implisit anymore we can move child-level-2 any
where we want (use Children props from child-level-1 to child-level-5) don't
need to worry about child-level-5 to get the data or not becouse it is getting
explisitly from the parent.

so in my opinion try to solve the problem with component compostion.

---

# React Docs: useLayout Hook

## Note:

useLayoutEffect can hurt performance. Prefer useEffect when possible.

## useLayout Hook

useLayoutEffect is a version of useEffect that fires before the browser
re-paints the screen.

```js
useLayoutEffect(setup, dependencies?)
```

## caveat

1. Effects only run on the client. They don’t run during server rendering.
2. The code inside useLayoutEffect and all state updates scheduled from it block
   the browser from repainting the screen. When used excessively, this makes
   your app slow. When possible, prefer useEffect.

## when to use uselayout hook?

1. Render the initial content.
2. Measure the layout before the browser repaints the screen.
3. Render the final content using the layout information you’ve read.

## kent c dodds blogs about useLayoutEffect

This runs synchronously immediately after React has performed all DOM mutations.
This can be useful if you need to make DOM measurements (like getting the scroll
position or other styles for an element) and then make DOM mutations or trigger
a synchronous re-render by updating state.

## difference between uselayoutEffect vs useEffect

1. useLayoutEffect: If you need to mutate the DOM and/or do need to perform
   measurements.
2. useEffect: If you don't need to interact with the DOM at all or your DOM
   changes are unobservable (seriously, most of the time you should use this).

---

# React Docs: useDebugValue

useDebugValue is a React Hook that lets you add a label to a custom Hook in
React DevTools.

useDebugValue(value, format?)

## Parameters

1. `value`: The value you want to display in React DevTools. It can have any type.
2. `optional format`: A formatting function. When the component is inspected,
   React DevTools will call the formatting function with the value as the
   argument, and then display the returned formatted value (which may have any
   type). If you don’t specify the formatting function, the original value
   itself will be displayed.

## Returns

useDebugValue does not return anything.

## Usage

Adding a label to a custom Hook

## Note

1. Don’t add debug values to every custom Hook. It’s most valuable for custom Hooks
   that are part of shared libraries and that have a complex internal data
   structure that’s difficult to inspect.

2. Deferring formatting of a debug value

   You can also pass a formatting function as the second argument to useDebugValue:

   ```js
   useDebugValue(date, (date) => date.toDateString());
   ```

   Your formatting function will receive the debug value as a parameter and should
   return a formatted display value. When your component is inspected, React
   DevTools will call this function and display its result.

This lets you avoid running potentially expensive formatting logic unless the
component is actually inspected. For example, if date is a Date value, this
avoids calling toDateString() on it for every render.

## youtube video : The Perfect Hook For Debugging Custom React Hooks

```js
useDebugvalue(value, format fn)
```

1. useDebugValue is only used with in custom hooks.

2. useDebugValue used to add a label to the custom hook.

which can be seen in React devTool. when you see your component which has a
useDebugValue it will be visible alongside it.

useDebug hook is used becouse sometimes custom hooks can be complicated to debug
becouse of there complexity so you can asign a value to the custom hook which
help us understand the functioning of the custom hook when we are debuging. in
the react dev tool.

## value

value can be any data type you want (string,number,array,object etc)

## format fn

format fn take value as a parameter and you can do whatever you want with the
value whatever be the return value that will be the label for your custom
component.

format fn has a special quark that it will only execute when you are useing your
react devTool(when react dev tool is open on the browser) otherwise it will not
execute. helps us avoid un-nesseory re-render of ur useDebugValue hook due to
your custom component re-render.

## kent c dodds

`Note`: useDebugValue values will not show in production, because the production
build of useDebugValue does nothing.

---

## percive performance by useDifferedValue and useTransition.

## React docs : useDifferedValue improve user Experience

---

## Dictionary meaning of Differ?

- put off (an action or event) to a later time. in simple words postpone.

## Docs

useDeferredValue is a React Hook that lets you defer(postpone) updating a part
of the UI.

const deferredValue = useDeferredValue(value)

## Parameters

`value`: The value you want to defer. It can have any type.

## Returns

During the initial render, the returned deferred value will be the same as the
value you provided. During updates, React will first attempt a re-render with
the old value (so it will return the old value), and then try another re-render
in background with the new value (so it will return the updated value).

## Caveats

1. The values you pass to useDeferredValue should either be primitive values
   (like strings and numbers) or objects created outside of rendering. If you
   create a new object during rendering and immediately pass it to
   useDeferredValue, it will be different on every render, causing unnecessary
   background re-renders.

2. When useDeferredValue receives a different value (compared with Object.is),
   in addition to the current render (when it still uses the previous value), it
   schedules a re-render in the background with the new value. The background
   re-render is interruptible: if there’s another update to the value, React
   will restart the background re-render from scratch. For example, if the user
   is typing into an input faster than a chart receiving its deferred value can
   re-render, the chart will only re-render after the user stops typing.

3. useDeferredValue is integrated with <Suspense<span></span>>. If the background update
   caused by a new value suspends the UI, the user will not see the fallback.
   They will see the old deferred value until the data loads.

4. useDeferredValue does not by itself prevent extra network requests.

5. There is no fixed delay caused by useDeferredValue itself. As soon as React
   finishes the original re-render, React will immediately start working on the
   background re-render with the new deferred value. Any updates caused by
   events (like typing) will interrupt the background re-render and get
   prioritized over it.

6. The background re-render caused by useDeferredValue does not fire Effects
   until it’s committed to the screen. If the background re-render suspends, its
   Effects will run after the data loads and the UI updates.

## Usage

1. Showing stale content while fresh content is loading

   During updates, the deferred value will “lag behind” the latest value. In
   particular, React will first re-render without updating the deferred value, and
   then try to re-render with the newly received value in background.

2. Indicating that the content is stale.

   In the example above, there is no indication that the result list for the latest
   query is still loading. This can be confusing to the user if the new results
   take a while to load. To make it more obvious to the user that the result list
   does not match the latest query, you can add a visual indication when the stale
   result list is displayed:

   ```js
   <div
     style={{
       opacity: query !== deferredQuery ? 0.5 : 1,
     }}
   >
     <SearchResults query={deferredQuery} />
   </div>
   ```

   With this change, as soon as you start typing, the stale result list gets
   slightly dimmed until the new result list loads.

3. Deferring re-rendering for a part of the UI.

   You can also apply useDeferredValue as a performance optimization. It is useful
   when a part of your UI is slow to re-render, there’s no easy way to optimize it,
   and you want to prevent it from blocking the rest of the UI.

## How does deferring a value work under the hood?

You can think of it as happening in two steps:

First, React re-renders with the new query ("ab") but with the old deferredQuery
(still "a"). The deferredQuery value, which you pass to the result list, is
deferred: it “lags behind” the query value.

In background, React tries to re-render with both query and deferredQuery
updated to "ab". If this re-render completes, React will show it on the screen.
However, if it suspends (the results for "ab" have not loaded yet), React will
abandon this rendering attempt, and retry this re-render again after the data
has loaded. The user will keep seeing the stale deferred value until the data is
ready.

The deferred “background” rendering is interruptible. For example, if you type
into the input again, React will abandon it and restart with the new value.
React will always use the latest provided value.

Note that there is still a network request per each keystroke. What’s being
deferred here is displaying results (until they’re ready), not the network
requests themselves. Even if the user continues typing, responses for each
keystroke get cached, so pressing Backspace is instant and doesn’t fetch again.

## How is deferring a value different from debouncing and throttling?

debouncing and throttling are explisit in nature. defferred value is implisit

There are two common optimization techniques you might have used before in this
scenario:

- Debouncing means you’d wait for the user to stop typing (e.g. for a second,in
  time) before updating the list.
- Throttling means you’d update the list every once in a while (e.g. at most
  once a second).

While these techniques are helpful in some cases, useDeferredValue is better
suited to optimizing rendering because it is deeply integrated with React itself
and adapts to the user’s device.

Unlike debouncing or throttling, it doesn’t require choosing any fixed delay. If
the user’s device is fast (e.g. powerful laptop), the deferred re-render would
happen almost immediately and wouldn’t be noticeable. If the user’s device is
slow, the list would “lag behind” the input proportionally to how slow the
device is.

Also, unlike with debouncing or throttling, deferred re-renders done by
useDeferredValue are interruptible by default. This means that if React is in
the middle of re-rendering a large list, but the user makes another keystroke,
React will abandon that re-render, handle the keystroke, and then start
rendering in background again. By contrast, debouncing and throttling still
produce a janky experience because they’re blocking: they merely postpone the
moment when rendering blocks the keystroke.

If the work you’re optimizing doesn’t happen during rendering, debouncing and
throttling are still useful. For example, they can let you fire fewer network
requests. You can also use these techniques together.

## my observation

- useDifferedvalue take a value and make it a low priority-render.

so when you update the value react will render the value first and as soon as
that render completes react then imidiatly starts the differedValue(low
priority) render and when differed-render completes differedValue variable get
updated.

so by the use of differed we now have two type of render.

1. High priority rander.(the value)
2. low priorty render.(the differedValue)

Note: when the DifferedValue is the process of differed-render and during that
time High priority value get updated then the old differed-render will be
suspanded and now differedValue with new Value will differed-render.

Note: all task in react are High priority rander by default.

There is no fixed delay caused by useDeferredValue itself. As soon as React
finishes the original re-render, React will immediately start working on the
background re-render with the new deferred value. Any updates caused by events
(like typing) will interrupt the background re-render and get prioritized over
it.

---

# React Docs: useTransition

---

useTransition is a React Hook that lets you update the state without blocking
the UI.

```js
const [isPending, startTransition] = useTransition();
```

## Parameters

useTransition does not take any parameters.

## Returns

useTransition returns an array with exactly two items:

- The isPending flag that tells you whether there is a pending transition.
- The startTransition function that lets you mark a state update as a
  transition.

## startTransition function

The startTransition function returned by useTransition lets you mark a state
update as a transition.

```js
function TabContainer() {
  const [isPending, startTransition] = useTransition();
  const [tab, setTab] = useState("about");

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```

## Parameters

- `scope`: A function that updates some state by calling one or more set
  functions. React immediately calls scope with no parameters and marks all
  state updates scheduled synchronously during the scope function call as
  transitions. They will be non-blocking and will not display unwanted loading
  indicators.

## Returns

startTransition does not return anything.

## Caveats

- useTransition is a Hook, so it can only be called inside components or custom
  Hooks. If you need to start a transition somewhere else (for example, from a
  data library), call the standalone startTransition instead.

- You can wrap an update into a transition only if you have access to the set
  function of that state. If you want to start a transition in response to some
  prop or a custom Hook value, try useDeferredValue instead.

- The function you pass to startTransition must be synchronous. React
  immediately executes this function, marking all state updates that happen
  while it executes as transitions. If you try to perform more state updates
  later (for example, in a timeout), they won’t be marked as transitions.

- A state update marked as a transition will be interrupted by other state
  updates. For example, if you update a chart component inside a transition, but
  then start typing into an input while the chart is in the middle of a
  re-render, React will restart the rendering work on the chart component after
  handling the input update.

- Transition updates can’t be used to control text inputs.

- If there are multiple ongoing transitions, React currently batches them
  together. This is a limitation that will likely be removed in a future
  release.

## Usage

1. Marking a state update as a non-blocking transition

   Call useTransition at the top level of your component to mark state updates as
   non-blocking transitions.

   ```js
   import { useState, useTransition } from "react";

   function TabContainer() {
     const [isPending, startTransition] = useTransition();
     // ...
   }
   ```

   useTransition returns an array with exactly two items:

   - The isPending flag that tells you whether there is a pending transition.
   - The startTransition function that lets you mark a state update as a
     transition. You can then mark a state update as a transition like this:

   ```js
   function TabContainer() {
     const [isPending, startTransition] = useTransition();
     const [tab, setTab] = useState("about");

     function selectTab(nextTab) {
       startTransition(() => {
         setTab(nextTab);
       });
     }
     // ...
   }
   ```

   Transitions let you keep the user interface updates responsive even on slow
   devices.

   With a transition, your UI stays responsive in the middle of a re-render. For
   example, if the user clicks a tab but then change their mind and click another
   tab, they can do that without waiting for the first re-render to finish.

2. Updating the parent component in a transition.
3. Displaying a pending visual state during the transition.
4. Preventing unwanted loading indicators.
5. Building a Suspense-enabled router. Note: use Transtion over useDefferedValue
   becouse

   - Transitions are interruptible, which lets the user click away without waiting
     for the re-render to complete.
   - Transitions prevent unwanted loading indicators, which lets the user avoid
     jarring jumps on navigation.

## my observation

useTransition is helpfull to use low-end devices who's computation power is
slow. ex: slow CPU throttling

how this help it help use show loading/data when there device taking time to do
the computation of the task;

helps make non-blocking ui expreance; while computing;

- useTransition is a React Hook that lets you update the state without blocking
  the UI.

Ex1: when you are filtering million of cart when you use search box; so every
character you type in search input box; a list of card data get filtered if the
data list is to big then it will take time to compute the filtered data andup
blocking the ui for the user which is not ideal to solve this problem we can use
useTransition;

Ex2: as we know when react re-render it will bundle out all the re-rendering
element together; so if one of them is slow the re-rendering will not be
completed onece that once slow rendering get's completed;

```
render 1 takes 10 sec
render 2 takes 1 min
render 3 takes 10 sec
render 4 takes 10 sec
```

bundle the render (render 1+2+3+4) total time to re-render = 1min 30 sec then
the render get's completed and the code get's updated;

due to bundling our fast render are also struggling;

side Note: react when bundle for re-rendering set priority to high all the
re-rendering element/component are high priority by Default is react; thats why
slow and fast rendering component are bundleup together;

but with the help of useTransion you can set the priority to be low for those
component which take time to compute;

side note: only use useTransion if you find some performace like slow CPU
computation; becouse now react will make two bundle

1. high priority (by default)
2. low priority (useTransition)

if you mis-use it that you app become slower and react need to do multiple
re-render and also low priorty component run in background once they get
completed they are updated

it's like this react look for re-render makes two bundle hight priority
component which imidiatble updated low priority bundle which execute in
backgeround just like Async handle by browser

low priority task will happen in background onece done and queue is empity it
get updated;

just like browser handle micro and macro queues and callstack same

how to use UseTransition

```js
import { useTranstion } from "./React";

const [isPanding, setTranstion] = useTransition();
```

isPending is a boolean by default set to true which means this is a low priority component
execute it once the pending is over

setTranstion is a function which is used to wrap the computationaliy expensive code

setTranstion ( heavy computation)

```js
  //jsx
  return (
    {isPanding ? by default True (use it for Loading): once the computation is over execute this}
  )
```

## When should they be used, and when not?

As previously stated, useTransition() wraps the state updating code, whereas
useDeferredValue() wraps a value affected by the state change. You don’t have to
(and shouldn’t) utilize both at the same time because they accomplish the same
thing.

Instead, if you have access to the state updating code and have some state
updates that should be treated with a lower priority, it makes sense to use
useTransition(). Use useDeferredValue() if you don’t have that access.

UseTransition() and useDeferredValue() should not be used to wrap up all of your
state updates or values. If you have a complex user interface or component that
can’t be optimized any other way, you should use the hooks. Other performance
enhancements, such as lazy loading, pagination, and performing work in worker
threads or on the back end, should always be considered.

## throttling and debouncing

Throttling and debouncing are two techniques used in software development to
manage the frequency of function execution, particularly in response to frequent
events such as user input or API calls. Although they serve similar purposes,
there is a fundamental difference between throttling and debouncing:

1. `Throttling`:

   Throttling limits the execution of a function to a certain rate or frequency. It
   ensures that the function is called at most once during a specified time
   interval, regardless of how many times the event triggering the function occurs
   within that interval.

   For example, let's say you have a scroll event that fires multiple times as the
   user scrolls down a webpage. By applying throttling with a time interval of 200
   milliseconds, you can ensure that the associated function is executed at most
   once every 200 milliseconds, even if the scroll event fires multiple times
   within that period. This can help prevent performance issues caused by an
   excessive number of function calls.

2. `Debouncing`:

   Debouncing, on the other hand, enforces a delay before executing a function
   after the last occurrence of the triggering event. It is used to handle
   scenarios where the event is triggered frequently, but you only want to respond
   to the final event after a certain period of inactivity.

   For instance, if you have an input field with an "onkeyup" event that triggers
   an API call for search suggestions, debouncing can be employed to ensure that
   the API call is made only when the user pauses typing for a specific duration
   (e.g., 300 milliseconds). If the user continues typing within that duration, the
   timer resets, and the function execution is delayed again.

   In summary, throttling limits the frequency of function calls, ensuring they
   occur at a specified rate, while debouncing delays function execution until a
   period of inactivity occurs after the last event.

## My Obsservation:

Throttling: limit the number of time a fn can execute with in a time frame.

Debouncing: defines the pause/delay/gap between each fn execution.

# Suspense

<<span></span>Suspense> lets you display a fallback until its children have finished loading.

```js
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```

## Props

- `children`: The actual UI you intend to render. If children suspends while
  rendering, the Suspense boundary will switch to rendering fallback.

- `fallback`: An alternate UI to render in place of the actual UI if it has not
  finished loading. Any valid React node is accepted, though in practice, a
  fallback is a lightweight placeholder view, such as a loading spinner or
  skeleton. Suspense will automatically switch to fallback when children
  suspends, and back to children when the data is ready. If fallback suspends
  while rendering, it will activate the closest parent Suspense boundary.

## Caveats

- React does not preserve any state for renders that got suspended before they
  were able to mount for the first time. When the component has loaded, React
  will retry rendering the suspended tree from scratch.
- If Suspense was displaying content for the tree, but then it suspended again,
  the fallback will be shown again unless the update causing it was caused by
  startTransition or useDeferredValue.
- If React needs to hide the already visible content because it suspended again,
  it will clean up layout Effects in the content tree. When the content is ready
  to be shown again, React will fire the layout Effects again. This ensures that
  Effects measuring the DOM layout don’t try to do this while the content is
  hidden.
- React includes under-the-hood optimizations like Streaming Server Rendering
  and Selective Hydration that are integrated with Suspense. Read an
  architectural overview and watch a technical talk to learn more.

## Usage

- Displaying a fallback while content is loading.

- suspense enable data fetching is not enabled in react it self becouse suspense
  can't detect when data is fetched inside an Effect or event handler. if you
  want to use suspense for data fecthing then use Suspense-enabled framwork like
  Relay and Next.js

- Lazy-loading component code with lazy with suspense

- Revealing content together at once: wrap the suspense to multiple component
  and when all the component are ready suspense will load the ui untill it will
  show the fallback.

- Revealing nested content as it loads : When a component suspends, the closest
  parent Suspense component shows the fallback. This lets you nest multiple
  Suspense components to create a loading sequence. Each Suspense boundary’s
  fallback will be filled in as the next level of content becomes available.

- don't use suspense with useTransition & useDifferedValue: Both deferred values
  and transitions provide ways to prioritize rendering and ensure a smoother
  user experience. They allow you to avoid displaying a fallback UI (such as a
  loading spinner or placeholder) and instead provide inline indicators or
  updates that indicate the progress or status of the non-urgent part of the UI.

  By marking a transition as non-urgent, the framework or router library can
  handle the navigation process in a way that doesn't block or delay other UI
  updates. It ensures that the user interface remains responsive during the
  transition and can continue rendering other components or handling user
  interactions while the navigation is being performed.

- Preventing already revealed content from hiding (use with use Suspense-enabled
  framwork) : When a component in React is waiting for some data to load, it can
  temporarily show a loading spinner or placeholder. However, if this happens
  while the user is already seeing some content, it can be jarring to suddenly
  replace that content with the spinner.

  To avoid this jarring experience, React provides a feature called "transitions."
  Transitions allow you to mark certain updates as non-urgent, meaning they don't
  need to happen immediately. Instead of replacing the already visible content
  with a spinner, React will keep showing the existing content while the new data
  is being loaded.

  In the code example you provided, when you press the button to navigate to a
  different page, the transition is not marked. As a result, the existing content
  is replaced by a spinner. However, by using the startTransition function
  provided by React, you can mark the navigation as a transition. This tells React
  to wait a bit longer before replacing the existing content with the spinner,
  allowing the already revealed content to remain visible for a smoother user
  experience.

`My Observation` in Preventing already revealed content from hiding (use
Suspense-enabled framwork): when you have used suspense over an ui (Ex: Navbar)
and navBar has multiple ui as children

Ex:

```js
<suspense fallback={loading}>
  <Navbar>
    <logo />
    <Home />
    <About />
    <Article />
    <Contect />
  </Navbar>
</suspense>
```

then if any of the children ui (ex:Article) takes time to load then entire
Navbar will show the loading state from suspense falback. the problem here is
that navbar was visible to the user and all the children were visible too only
the the Article ui got re-render and when to fetch some data requre to load. due
to that entrie Navbar went to loading state which gives the user a jarring
expirence.

to solve this use transition over Navbar which will delay the loading state
becouse transiton is non-blocking fallback will not execute. due all the
component will be visible to the user. and only the article will show the
suspense fallback.

---
