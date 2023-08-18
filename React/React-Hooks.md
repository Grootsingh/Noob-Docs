> ## React version: 18.2.0

# chapter: 1 useState greeting

State can be defined as: data that changes over time.

---

# chapter:2 useEffect:

---

extra Info: thinking behind useEffect: "The question is not "when does this
effect run" the question is "with which state does this effect synchronize with"

knowladge from the react docs.

useEffect is a React Hook that lets you synchronize a component with an external
system.

```jsx
useEffect(setup, dependencies?)
```

## Parameters

1. setup: The function with your Effect’s logic. Your setup function may also
   optionally return a cleanup function. When your component is first added to
   the DOM, React will run your setup function. After every re-render with
   changed dependencies, React will first run the cleanup function (if you
   provided it) with the old values, and then run your setup function with the
   new values. After your component is removed from the DOM, React will run your
   cleanup function one last time.

2. optional dependencies: The list of all reactive values referenced inside of
   the setup code. Reactive values include props, state, and all the variables
   and functions declared directly inside your component body. If your linter is
   configured for React, it will verify that every reactive value is correctly
   specified as a dependency. The list of dependencies must have a constant
   number of items and be written inline like [dep1, dep2, dep3]. React will
   compare each dependency with its previous value using the Object.is
   comparison. If you omit this argument, your Effect will re-run after every
   re-render of the component.

Note: useEffect returns undefined.

## caveat

1. If you’re not trying to synchronize with some external system, you probably
   don’t need an Effect.

2. If some of your dependencies are objects or functions defined inside the
   component, there is a risk that they will cause the Effect to re-run more
   often than needed. To fix this, remove unnecessary object and function
   dependencies. You can also extract state updates and non-reactive logic
   outside of your Effect.

3. If your Effect wasn’t caused by an interaction (like a click), React will let
   the browser paint the updated screen first before running your Effect. If
   your Effect is doing something visual (for example, positioning a tooltip),
   and the delay is noticeable (for example, it flickers), replace useEffect
   with useLayoutEffect.

4. if you must block the browser from repainting the screen, you need to replace
   useEffect with useLayoutEffect.

5. Effects only run on the client. They don’t run during server rendering.

### Setup function and cleanup function general flow.

React calls your setup and cleanup functions whenever it’s necessary, which may
happen multiple times:

1. Your setup code runs when your component is added to the page (mounts).
2. After every re-render of your component where the dependencies have changed:

   . First, your cleanup code runs with the old props and state.

   . Then, your setup code runs with the new props and state.

3. Your cleanup code runs one final time after your component is removed from
   the page (unmounts).

Note: mounted meaning adding node to the DOM, unMounted means removing Node from
the DOM, update means reasigining new value to pre-exsisting node on the DOM.

SIDE note: when to use useEffects

An Effect lets you keep your component synchronized with some external system
(like a chat service). Here, external system means any piece of code that’s not
controlled by React, such as:

1. A timer managed with setInterval() and clearInterval().
2. An event subscription using window.addEventListener() and
   window.removeEventListener().
3. A third-party animation library with an API like animation.start() and
   animation.reset().

If you’re not connecting to any external system, you probably don’t need an
Effect.

---

## SOME KEY POINTS

1. useEffect is a escape hack from react eco System if you have to use it more
   oftenly then better not to use it and use some custom hook or use api
   avilible on react eco system.

   Effects are an “escape hatch”: you use them when you need to “step outside
   React” and when there is no better built-in solution for your use case. If you
   find yourself often needing to manually write Effects, it’s usually a sign that
   you need to extract some custom Hooks for common behaviors your components rely
   on.

   There are also many excellent custom Hooks for every purpose available in the
   React ecosystem.

---

2. Fetching data with Effects

You can use an Effect to fetch data for your component. Note that if you use a
framework, using your framework’s data fetching mechanism will be a lot more
efficient than writing Effects manually.

---

3. Removing unnecessary object dependencies.
4. Removing unnecessary function dependencies.

becouse on every re-render of the component object and function are re-created
from scratch andup confusion the useEffect dependency by think this is different
object/function then previous becouse it get updated due to that your useEffect
will re-render multiple time for unnessesery resion so don't put object and
function as a dependecy in the useEffect.

this is becouse in javascript

```
const obj1 = {name: raju}
const obj2 = {name: raju}

obj1 !== obj2. for react they are different you might have updated the obj
```

samething with functions too.

---

`Troubleshoot`:My Effect does something visual, and I see a flicker before it runs

If your Effect must block the browser from painting the screen, replace
useEffect with useLayoutEffect. Note that this shouldn’t be needed for the vast
majority of Effects. You’ll only need this if it’s crucial to run your Effect
before the browser paint: for example, to measure and position a tooltip before
the user sees it.

useLayoutEffect is performance inefficient so don't use if possible.

---

## from the BLOG: useState Lazy initialization

---

when you use useState with in your function component.

```jsx
const initialState = 0;
const [count, setCount] = React.useState(initialState);
```

every time your component function re-render javascript go through the code
where it also re-initalize count value to initalState (on every-re-render).even
though we don't use initalState value after inital-render.

This is normally not a big deal because JavaScript engines are very fast and can
optimize for this kindof thing. So something like this would be no problem:

However, what if the initial value for your state is computationally expensive?

```jsx
const initialState = calculateSomethingExpensive(props);
const [count, setCount] = React.useState(initialState);
```

Remember that the only time React needs the initial state is initially 😉
Meaning, it only really needs the initial state on the first render. But because
our function component body runs every time there's a re-render of our
component, we end up running that code on every render, even if its value is not
used or needed.

This is what the lazy initialization is all about. It allows you to put that
code in a function:

```jsx
const getInitialState = () => Number(window.localStorage.getItem("count"));
const [count, setCount] = React.useState(getInitialState);
```

Creating a function is fast. Even if what the function does is computationally
expensive. So you only pay the performance penalty when you call the function.
So if you pass a function to useState, React will only call the function when it
needs the initial value (which is when the component is initially rendered).

This is called "lazy initialization." It's a performance optimization. You
shouldn't have to use it a whole lot, but it can be useful in some situations,
so it's good to know that it's a feature that exists and you can use it when
needed. I would say I use this only 2% of the time. It's not really a feature I
use often.

---

## from Blog: dispatch function updates

---

lets try to understand

```jsx
function DelayedCounter() {
  const [count, setCount] = React.useState(0);

  const increment = async () => {
    await doSomethingAsync();
    setCount(count + 1);
  };

  return <button onClick={increment}>{count}</button>;
}
```

we have a DelayedCounter Component. initalState value = 0 & setCount is our
Dispatch function.

we have increase function which is nested inside DelayedCounter component.

now the situation is: when we click on button event triggers and increase
function invoked. but invoke function have two tasks:

1. doSomethingAsync() which take 500 mili-sec to complete.
2. setCount(count + 1) which is our dispatch function which update our
   initalState value and re-render our DelayedCounter component.

Note: increase function it self is a async function so even though setcount is
sync and dpSomethingAsync is async function complete function is async so untill
all the task are done. async function will not execute.

`Question`: if we click on button 3 times vary fast/continusly then what will
we the count value output.

`answer`: count will be 1 (awkword silance)

what happen under the hood? we clicked button 3 times increase function was
called 3 times. we go inside the increase function then doSomething execute
which take 500 mili-sec(X3 times) to complete and setCount(count +1) which is
sync take count =0 and add 1 , count =1 now (first time). but the problem is
initalValue has not been updated becouse increase function is not finished yet.
due to there is no re-render.

setCount(count + 1) is not be able to update value becouse it is bound(scoped)
by the increase function. untill increase function complete which take (500
mili-sec X3) time so what endup happening.

```
setCount(count +1) =  (0 + 1) so (1) first click
setCount(count +1) =  (0 + 1) so (1) second click
setCount(count +1) =  (0 + 1) so (1) third click
```

count value is not updated so previous value is still zero and 3 times we have
done the same task just becouse of scope issue coz by async function.

`solution`:

```
setCount((previosValue)=> previousValue + 1)
```

use function inside setCount which take only one argument which is initalValue
as a previousValue.

now even though we are bound by the increase function now we have a refrence to
previousvalue inside the increase function so if we click 3 time now it will not
take refrence from the inital value that we have set at the top level. it will
refer to the previous value with in the function.

```
setCount((previosValue)=> previousValue + 1) =  (0 + 1) so (1) first click
setCount((previosValue)=> previousValue + 1) =  (1 + 1) so (2) second click
setCount((previosValue)=> previousValue + 1) =  (2 + 1) so (3) third click
```

it will still take the total time but when ever the increase function get
completed it will update newSetcount value and re-render the dom.

---

# useEffect

---

React.useEffect is a built-in hook that allows you to run some custom code after
React renders (and re-renders) your component to the DOM. It accepts a callback
function which React will call after the DOM has been updated:

useEffect flow diagram.

### NOTE: <Book<span></span>/> is not a function invokation

<Book<span></span>/> this is not function/component invokation. this is a function creation.
when we say <BOOK<span></span>/> it means React.createElement(Book) NOT Book(). when to
execute/invoke the component is in the hand of react not us. react will execute
these function at the time of rendering.

now let us understand flow of React.

`case:1 Mounting means inital-render.`

1. all of lazy initalization will run.
2. after running all the code side Effect will run to sink outer world with the
   state of your app
3. in effect all type useEffect dispatch function will run.

`case:2 update re-render.`

1. now lazy initalization will not run.
2. now if you are the parent and update is to show child then first parent will
   end
3. then child will start.

4. inside Child: All the steps of Mounting will run.

   4.1. all of lazy initalization will run.

   4.2. after running all the code side. Effect will run to sink outer world with
   the state of your app.

   4.3. in effect all type useEffect dispatch function will run.

5. then child end
6. first the cleanup function of parent will run ([] will not) after that all
   the dispatch will run ([ will not])

`case:3 unmounting removing node + re-render`

1. App will run and end
2. child will run and end all effect type clean up will run including [].
3. app all the cleaning ([] not included).
4. after that all the dispatch will run ([ will not]).

`summery`:

1. lazy initalization will only run in inital render (at Mounting).
2. re-rendering: even though child might contain in the middle of the parent
   code. child will not start rendering until parent complete it's render.
3. re-render: all the cleanup function will run first before any dispatch
   function starts of any component.(in batches)
4. re-render: all the component render are isolated.
5. re-render: useEffect(()=>,[]) will not run (not dispatch,not cleanup)
6. un-mounting: that component which is unmounting will run all of it's
   useEffect cleanup function.

7. render phase is when all the states and jsx code is read .if in your jsx
   there is a Child-Component that will start rendering after the parent render
   end.

so it will be like: parent (render:start-end) then child
(render:start-end,cleanup, useEffect) then parent (cleanup,useEffect).

general ract flow is:

1. render
2. update DOM
3. cleanUp(useLayoutEffect)
4. Run useLayoutEffect function
5. browser print the DOM on screen.
6. cleanUp (useEffect)
7. Run useEffect function.

---

## Extra: Reading from localStorage is slow?

---

Local storage stores the data on your user's hard drive. It takes a bit longer
to read and write to the hard drive than it does to RAM. The conclusion to take
away from this is that you could optimize your performance by reading from local
storage on start up and only write to it when the user logs out.

so in react use Lazy-Initialization for better performace.

---

## what is a custom hook?

---

custom hook is a self build hook which uses combination of react hooks or other
custom hooks to solve a generic problem which help us to create a pattern which
can be used multiple time without re-writing the same code or solving the same
problem again and again.

react convenstion for custom hook is to use "use" keyword before initalizing the
name of custom hook.

---

# chapter 3: lifting and co-location.

---

lifting is required when two isolated component have a common functionality or
one other componenet has a dependency over those two isolated component.

lifting means picking up states from the leaf/isolated component to up in the
tree(component structure ) which is a common parent component and closser to
both the the isolated component.

this will help us to share functionality to both the isolated component.

co-location is required when there is no need of the state in the parent
component which is controlling child component. that state can be re/co-located
in the child it self. there is no need for that state to be lifted up in the
tree.

co-locating the state in the leaf component help to reduce un-nessesacy checking
for change in the component.

let take an ex:

```jsx
function App() {
  // this is "global state"
  const [dog, setDog] = React.useState("");
  const [time, setTime] = React.useState(200);
  return (
    <div>
      <DogName time={time} dog={dog} onChange={setDog} />
      <SlowComponent time={time} onChange={setTime} />
    </div>
  );
}
```

in this example: app has two state one for DOG and other for Time. when ever
re-render happen by any reason.

react will have to check both state changes in each of it's children component
so it can update them if needed. so in our ex: react we check for dog and time
state in both of the children component <dog<span></span>> & <slowComponent<span></span>>. which put
un-nessesery extra work on react. becouse we can co-locate the dog state. it is
only require for <dog<span></span>> child component. ther is no need/reqirement for dog state
to be this up (lifted) in the tree. uplifting is only reqired when we need to
share a state between two component which is not require here.

so when we co-locate the dog state to <dog<span></span>> component. when ever App the parent
componet re-render. it only need to check for time state. to check that if time
state has changed in both the child component. which helps react in performance
and improve code mainatanability.

benifit of Co-location?

1. the component's easier to maintain.
2. improve performance.

---

# chapter: 4 Don't Sync State. Derive It!

there are two type of states in our react Components.

1. unique State.
2. Drived state.

unique State are states which don't depends on other-states. they have there
unique logic.

drived State are those state that depended on other states. That means that
their value can be derived (or calculated) based on other values rather than
managed on their own.

the point of all of this is that you don't require to create a state for every
value. only create values for unique states/unique values which don't depend on
other values. those values which depend on other states can automaticaly be
drived/on the fly when depending value coz the re-render.

when depended value update entire component re-render and coz depending values
to update with them automaticly.

if you create state for every values then there are so many problem you need to
face:

1. react has to manage all the state and check all the child if all those state
   have changed or not. simply react has to do extra un-nessesery work similar
   to un-nessesery lifing problem.
2. on big app it become difficult to keep tract of all the states. becouse you
   will have to update setDepending value every time setUnique value update.
   un-nessesery complexcity and more error pron code.
3. The biggest problem with this is some of that state may fall out of sync with
   the true component state (squares). It could fall out of sync because we
   forget to update it for a complex sequence of interactions.

---

# chapter: 5 useRef and useEffect: DOM interaction

Often when working with React you'll need to integrate with UI libraries. Some
of these need to work directly with the DOM. Remember that when you do:

<div<span></span>>hi</div<span></span>> that's actually syntactic sugar for a React.createElement so you
don't actually have access to DOM nodes in your function component. In fact, DOM
nodes aren't created at all until the ReactDOM.render method is called. Your
function component is really just responsible for creating and returning React
Elements and has nothing to do with the DOM in particular.

So to get access to the DOM, you need to ask React to give you access to a
particular DOM node when it renders your component. The way this happens is
through a special prop called ref.

After the component has been rendered, it's considered "mounted." That's when
the React.useEffect callback is called and so by that point, the ref should have
its current property set to the DOM node. So often you'll do direct DOM
interactions/manipulations in the useEffect callback.

---

# chapter: 6 useEffect: HTTP requests

One important thing to note about the useEffect hook is that you cannot return
anything other than the cleanup function. This has interesting implications with
regard to async/await

This ensures that you don't return anything but a cleanup function.

🦉 I find that it's typically just easier to extract all the async code into a
utility function which I call and then use the promise-based .then method
instead of using async/await syntax:

```jsx
React.useEffect(() => {
  doSomeAsyncThing().then((result) => {
    // do something with the result
  });
});
```

## for error handling use state rather then loading/error

states can be:

1. idle: no request made yet
2. pending: request started
3. resolved: request successful
4. rejected: request failed

---

## Use ErrorBoundry

Error boundary is a React component that wraps around other components and
captures any errors that occur within its subtree during rendering. It allows
you to gracefully handle errors and display fallback UI instead of a complete
application crash(White Screen of Death).

Error boundaries are useful in production environments where unexpected errors
can occur due to various reasons, such as network issues, incorrect data, or
programming errors. By using error boundaries, you can prevent those errors from
propagating up the component tree and breaking the entire application. Instead,
you can display an error message or a fallback UI to inform the user about the
error.

in react ErroBoundry are written in class formant currently there is no hook for
that in react official Docs but there is an external api (react-error-boundary)
that solve this poblem by providing an hook that you can use to wrap around your
component.

Install it

```bash
npm install react-error-boundary
```

how to use the Error Boundary

there are several ways to render fall back in this API

1.  ErrorBoundary with fallback prop (for simple Text)

    ```jsx
    import { ErrorBoundary } from "react-error-boundary";

    <ErrorBoundary fallback={<div>Something went wrong</div>}>
      <ExampleApplication />
    </ErrorBoundary>;
    ```

2.  ErrorBoundary with fallbackRender function (for an UI)

    ```jsx
    import { ErrorBoundary } from "react-error-boundary";

    function fallbackRender({ error, resetErrorBoundary }) {
      // Call resetErrorBoundary() to reset the error boundary and retry the render.

      return (
        <div role="alert">
          <p>Something went wrong:</p>
          <pre style={{ color: "red" }}>{error.message}</pre>
        </div>
      );
    }

    <ErrorBoundary
      fallbackRender={fallbackRender}
      onReset={(details) => {
        // Reset the state of your app so the error doesn't happen again
      }}
    >
      <ExampleApplication />
    </ErrorBoundary>;
    ```

    - fallbackRender take a function component that gets render everytime
      <ExampleApplication <span></span>/> throws an error.

    - in fallbackRender take two props error and resetErrorBoundary.

      1. `error`: is the actual error that has been caught by the ErrorBoundary which
         was bubbleup from the component that has thrown an error.

      2. `resetErrorBoundary`: is a function which re-set the ErrorBoundary to it's
         initalState. resetErrorBoundary is a pure function.

    - onReset takes an function where you can define all the reset states that you
      want to reset when an error happen, so that error don't happen again. when
      someone interect with your app after error has been shown by errorBoundary.

3.  ErrorBoundary with FallbackComponent Component (for UI)

        this is exactly same as the above only difference here is that we define
        FallbackComponent={Fallback} which take a react component and above take an
        function.

        - there are some aditional props that you use

    <br/>
    <br/>
        1. Logging errors with onError:

            ```jsx
            import { ErrorBoundary } from "react-error-boundary";

            const logError = (error: Error, info) => {
              // Do something with the error, e.g. log to an external API
              console.error("An error occurred:", error);
              console.error("Component stack:", info.componentStack);
            };

            const ui = (
              <ErrorBoundary FallbackComponent={ErrorFallback} onError={logError}>
                <ExampleApplication />
              </ErrorBoundary>
            );
            ```

            onError takes a function . that function has two parameter error which contain
            the error and info. info is an object which contain a property of componentStack
            (string format). componentstack is same that you see in react devtool component

            onError helps you send error and inf.componentStack to an external API. when
            error occers after production deploy.

            NOTE: Error boundaries do not catch errors for:

            - Event handlers (onClick,onSubmit)
            - Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)
            - Server side rendering
            - Errors thrown in the error boundary itself (rather than its children)

            to solve these ErrorBoundary problems we have a neet solution.

            `case:1 with async code`

            ```jsx
            import { useErrorBoundary } from "react-error-boundary";

            function Example() {
              const { showBoundary } = useErrorBoundary();

              useEffect(() => {
                fetchGreeting(name).then(
                  response => {
                    // Set data in state and re-render
                  },
                  error => {
                    // Show error boundary----------
                    showBoundary(error);
                  }
                );
              });

              // Render ...
            }
            ```

            So when our fetchGreeting promise is rejected, the handleError function is
            called with the error and react-error-boundary will make that propagate to the
            nearest error boundary like usual.

            `case:2 event handler`

            ```jsx
            function Greeting() {
              const [name, setName] = React.useState('')
              const {status, greeting, error} = useGreeting(name)
              if (error) throw error

              function handleSubmit(event) {
                event.preventDefault()
                const name = event.target.elements.name.value
                setName(name)
              }

              return status === 'resolved' ? (
                <div>{greeting}</div>
              ) : (
                <form onSubmit={handleSubmit}>
                  <label>Name</label>
                  <input id="name" />
                  <button type="submit" onClick={handleClick}>
                    get a greeting
                  </button>
                </form>
              )
            }
            ```

            if ever error is true due to state change by onCLick then component will
            automaticly thow an error and that error will be propogated to the nearest
            errorBoundary.
