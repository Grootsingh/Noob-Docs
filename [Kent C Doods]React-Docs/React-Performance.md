### chapter 1: Code Splitting

---

# side knowladge

# difference between <script type="text/javascript"> vs <script type="modules">

1. <script type="text/javascript"> is the traditional way of including 
   JavaScript in a web page. It is a non-modular script and can be used 
   to load JavaScript code that is not in module format. The code is loaded 
   and executed in the order it appears in the document.


2. <script type="module"> is a newer way to include JavaScript in a web page,
   introduced in ECMAScript 6. It is a modular script and can be used to load 
   JavaScript code that is written as a module. Modules are a way to organize 
   and encapsulate code, making it easier to manage and reuse.


3. When a module script is loaded, its contents are not executed immediately (it
   is defered),but are instead deferred until all of the module's dependencies
   have been loaded and evaluated. This ensures that modules are loaded in the
   correct order and avoids issues with code that relies on other code that
   hasn't been loaded yet. Additionally, modules have a scoped execution
   context, which means that variables and functions defined in one module are
   not visible in other modules, unless they are explicitly exported and
   imported.

In summary, <script type="text/javascript"> is used for non-modular JavaScript
code, while <script type="module"> is used for modular JavaScript code.

<script type="module"> allows for better organization, reusability, and
dependency management of JavaScript code in web applications.

# Dynamic Module

import("location").then((data)=> data.recivedInfo(),(error)=> thow new
Error(error))

in this data that you recive is an object,error occurs when your provided
location is incurrect

ex:

```
// async-script.js
import {appendDiv} from './append-div.js'

function go() {
  appendDiv('Hello from async script')
}

export {go}

-------------------------------------------------

// script-src.js
import {appendDiv} from './append-div.js'

appendDiv('Hello from external script')

import('./async-script.js').then(
  moduleExports => {
    moduleExports.go()
  },
  error => {
    console.error('there was an error loading the script')
    throw error
  },
)

```

The dynamic import likewise also must point directly to a JavaScript file (with
the extension). And to be clear, what's important is not the extension, but the
fact that when the browser makes a request to that URL, it receives back a text
file which it can execute as JavaScript.

This means that if you happen to have a URL that returns a JavaScript file but
doesn't end in .js you are fine omitting that.

```
import * as d3 from 'https://unpkg.com/d3?module'
```

The point is, the thing you put in the quotes in your import statements has to
point to a JavaScript resource on some server somewhere.

---

# why we do code splitting

Code splitting is a technique used in software development to break up a large
codebase into smaller, more manageable pieces. The goal of code splitting is to
reduce the amount of code that needs to be loaded by a web page or application
at any given time, which can improve the page or application's performance and
reduce its load time.

In a web development context, code splitting typically involves dividing a
JavaScript bundle into smaller, more specific chunks that can be loaded (on demand)
as-needed basis. For example, if a web page has a large JavaScript bundle that
contains code for multiple features, code splitting can be used to load only the
code for the feature that is currently being used, rather than loading the
entire bundle up-front.

There are several techniques for implementing code splitting in a web
development context. One common approach is to use dynamic imports, which allow
you to load modules on-demand, rather than up-front. Another approach is to use
a build tool or module bundler that can automatically split your code into
smaller chunks based on certain criteria, such as feature boundaries or code
dependencies.

# different ways to do code splitting

1. Dynamic Imports: Dynamic imports are a native JavaScript feature that allow
   you to load modules on-demand, rather than up-front. You can use the import()
   function to load a module dynamically, and the resulting module can be used
   just like any other module.

2. Build Tools and Module Bundlers: Build tools and module bundlers like Webpack
   and Rollup can automatically split your code into smaller chunks based on
   certain criteria, such as feature boundaries or code dependencies. These
   tools typically use a combination of static analysis and dynamic imports to
   determine the optimal code splitting strategy.

3. Server-Side Rendering: Server-side rendering (SSR) can be used to pre-render
   a web page on the server and send the resulting HTML to the client, rather
   than sending a JavaScript bundle that needs to be loaded and executed on the
   client. This can improve the page load time and provide a better user
   experience, especially on slow or unreliable networks.

## Code Splitting with React lazy Loading + Suspence + Dynamic Import (Conditional Import)

lazy loading is a technique used to improve the performance of the application
by splitting it into smaller chunks and loading them only when needed. It is
especially useful for larger applications that have a lot of components, as it
can significantly reduce the initial load time and improve the overall user
experience.

Lazy loading is achieved using the React.lazy function and the Suspense
component. Here's how it works:

Use the React.lazy function to import a component lazily. This function takes a
function that returns a dynamic import() statement that loads the component when
it is needed. Here's an example:

```
const Globe = React.lazy(() => import('../globe'))
```

In this example, the Globe component is loaded lazily using the dynamic import()
statement.

Wrap the lazily loaded component with the Suspense component. This component
displays a fallback UI while the lazily loaded component is being loaded. Here's
an example:

```
function App() {
  const [showGlobe, setShowGlobe] = React.useState(false)

  return (
    <div
      style={{
        display: 'flex',
        alignItems: 'center',
        flexDirection: 'column',
        justifyContent: 'center',
        height: '100%',
        padding: '2rem',
      }}
    >
      <label style={{marginBottom: '1rem'}}>
        <input
          type="checkbox"
          checked={showGlobe}
          onChange={e => setShowGlobe(e.target.checked)}
        />
        {' show globe'}
      </label>
      <div style={{width: 400, height: 400}}>
        <React.Suspense fallback={<div>...loading</div>}>
          {showGlobe ? <Globe /> : null}
        </React.Suspense>
      </div>
    </div>
  )
}
```

In this example, the Globe component is wrapped with the Suspense component,
which displays the Loading... message while the component is being loaded.

Lazy loading can be especially useful in scenarios where a particular feature or
section of the application is rarely used, as it can help to reduce the overall
size of the application and improve the initial load time.

# some Extra key points to remember while useing lazy Loading.

1. the component you want to lazy load need to be export by Default
2. lazy Load happen certain condition are met then component is loaded untill
   import is on standby.(conditional import)
3. wrap your lazy component with suspance and suspance take a fallback which
   will be displayed while lazy component is beeing imported.
4. when we import Module files browser is actually making a network request.
   lazy loading helps with minimize network load which makes the page load
   faster and when needed more network request can be made with lazy load

## Eger Loading

Eger loading is a variation of Lazy loading. Eger Loading reduce the percived
waiting time for the user to intertect with the Imported Component

Egger loading take advantage of browser cashing machemism. the thing is once the
file has been imported. the file has been recived then when you next time make a
request for import it will first check it's browser catch if it has the file
then it will not make any request.

it means then it does not matter how many time you request for the import if
it's the same file from the same address. then there will be only one network
request. rest will be recived from the browser catch.

Eger loading take advantage of that what we generally happens. In lazy loading
we wait for the user to take action like click on Btn. once the user has clicked
on the btn we start to make a request for the import of the component untill the
component is loaded then user has to wait a whil once the file has been loaded
then user can interact with the lazy loaded component.

but what Eger Loading does it don't wait for any action from the user. it take
any clue it get's from the user. from the intent of the user if he is interested
to click on the btn? like onMouseEnter (Hover on Btn) and onFocus (focus on
Btn).

when user hover or focus on the btn we start importing the component in advance
and when the user actually take action to clickit we already have the file in
browser catch so the user don't have to wait for the intereacting with the
component.

ex:

```
import * as React from 'react'

- we created a EgerLoad fn which trigger on onMouseEnter and onFocus.
- with that we load Globe component in advance.

const EagerLoad = () => import('../globe')
const Globe = React.lazy(EagerLoad)

function App() {
  const [showGlobe, setShowGlobe] = React.useState(false)

  return (
    <div
      style={{
        display: 'flex',
        alignItems: 'center',
        flexDirection: 'column',
        justifyContent: 'center',
        height: '100%',
        padding: '2rem',
      }}
    >
      <label
        style={{marginBottom: '1rem'}}
        --------------------------
        onMouseEnter={EagerLoad}
        onFocus={EagerLoad}
        ------------------------
      >
        <input
          type="checkbox"
          checked={showGlobe}
          onChange={e => setShowGlobe(e.target.checked)}
        />
        {' show globe'}
      </label>
      <div style={{width: 400, height: 400}}>
        <React.Suspense fallback={<div>...loading</div>}>
          {showGlobe ? <Globe /> : null}
        </React.Suspense>
      </div>
    </div>
  )
}

export default App

```

## prefetching

Prefetching is a technique used to improve the performance of web applications
by loading resources, such as scripts, stylesheets, or images, in advance of
when they are actually needed. This can reduce the perceived latency of the
application and improve the overall user experience.

Prefetching works by using the link tag in HTML to indicate that a particular
resource should be fetched in advance. The link tag can be used with the rel
attribute set to "prefetch", "preload", or "prerender". Here's what each of
these attributes does:

rel="prefetch": This attribute indicates that the resource should be fetched in
the background, after the current page has finished loading. This is useful for
resources that are likely to be needed in the near future, but are not required
for the current page.(require in near future but not immidiatly)

rel="preload": This attribute indicates that the resource that are critical for 
the current page and should be loaded as a priority. This is useful
for critical resources that are needed for current page, such as essential assets 
like CSS files, JavaScript files, fonts, or images required for main page.
(reqiure now for current page with the highest priority)

rel="prerender": This attribute indicates that the resource should be loaded and
rendered in advance of when it is actually needed. This is useful for pages that
are likely to be visited next, as it can significantly reduce the perceived
latency of the application.(Done with SSR or SSG)

we use prefetch in lazyLoading when we know that this componet is required 100%
by the user. its not a maybe case. so we load the the lazy Component imidiatly
after the current page finishes loading just like differ in (react priority
render).

```
in HTML head
<link rel="prefetch" href="/path/to/resource.js">

```

# for webpack

but if you use webPack bundler it can do this for us with a simple command

```
import(/* webpackPrefetch: true */ './some-module.js')
```

this command will automaticlly read by the webpack and it will generate The
prefetch link in HTML Head for us.

ex:

```

import * as React from 'react'
-----------------------------------------------------------------------------------
const Globe = React.lazy(() => import(/* webpackPrefetch: true */ '../globe'))
------------------------------------------------------------------------------------
function App() {
  const [showGlobe, setShowGlobe] = React.useState(false)

  return (
    <div
      style={{
        display: 'flex',
        alignItems: 'center',
        flexDirection: 'column',
        justifyContent: 'center',
        height: '100%',
        padding: '2rem',
      }}
    >
      <label style={{marginBottom: '1rem'}}>
        <input
          type="checkbox"
          checked={showGlobe}
          onChange={e => setShowGlobe(e.target.checked)}
        />
        {' show globe'}
      </label>
      <div style={{width: 400, height: 400}}>
        <React.Suspense fallback={<div>loading globe...</div>}>
          {showGlobe ? <Globe /> : null}
        </React.Suspense>
      </div>
    </div>
  )
}

export default App

```

# in <script> difference between async and defer.

1. async attribute:

- When the async attribute is present, the browser will download the script 
  file asynchronously while continuing to parse the HTML document. Once the file 
  is downloaded, it will be executed immediately, regardless of whether the HTML 
  parsing is complete or not.

- The script is executed as soon as it's available, which means it can interrupt 
  the rendering of the page and other scripts. Multiple scripts with the async 
  attribute may load and execute in an arbitrary order, depending on their download speed.

- It is commonly used for scripts that don't have dependencies on other scripts or 
  the DOM and can run independently.

2. defer attribute:

- When the defer attribute is present, the browser will download the script 
  file asynchronously, similar to async. However, the execution of the script 
  is deferred until after the HTML parsing is complete.
  
- Scripts with the defer attribute maintain their order of appearance in the 
  HTML document, meaning they are executed in the order they appear. This allows 
  the scripts to access the DOM and execute in a predictable manner.
  
- The defer attribute is suitable for scripts that need to manipulate or interact 
  with the DOM and rely on the DOM structure being fully parsed and available.


# coverage (Crome Dev tool- Coverage)

The Chrome DevTools Coverage panel is a tool in the Chrome web browser that
allows developers to analyze the code coverage of a web page. It shows which
parts of the code were executed when the page was loaded and which parts were
not, making it easier to identify unused code.

To use the Coverage panel, developers can open the Chrome DevTools, navigate to
the "Coverage" tab, and click the "Start" button to begin recording code
coverage. They can then interact with the web page as usual and perform actions
such as clicking buttons or scrolling. Once they are finished, they can click
the "Stop" button to end the recording and see the code coverage report.

The report shows a breakdown of the code by file, including the percentage of
code that was executed, and the total number of lines of code. It also allows
developers to view the code itself and see which lines were executed and which
were not, making it easier to identify areas of the code that can be optimized
or removed.

The Coverage panel can be a useful tool for improving the performance and
efficiency of web pages, as it allows developers to identify and remove
unnecessary code that can slow down page load times.

# my observation with Coverage

Coverage helps us to understand that how many file has been loaded (when you
load the page, doing certain actions) and how much code is used to displaying the
page from all the files.

you can see big files comes on top and show how much code is loaded or how much
is not. the not loaded part help us to think that can we obtimize the
un-required code becouse that un-required code is not needed to display what we
are displaying at this movement. maybe we can do more code splitting (reduce the
file size to speedup the page, remove some un-nessesery code becouse it's not
required to display what we want).

### chapter:2 useMemo for expensive calculations

when we have expensive computation with in an component. when ever the fn
component re-render the expensive computation will be re-evaluated on every
render. re-render can happen in a component for many reason (parent render so
all of it's child re-render,any hook value changes so re-render).

doing the same expensive computation on every render is un-nessesery and make
the site slugish to not to do the same expensive computation we use useMemo.

what useMemo does that useMemo(()=> ,[]) can take any type of data and it will
evaulate it by calling the fn and store the resulting value in catch and untill
any of the dependency changes it will not re-evaluate the returned value and
provide you the value from the catch.

simillarly useCallback hook catch the fn.

how to know that which fn component are slow and are needed to be memoized

# dev tool Performance (Crome Dev tool- Performance)

Chrome DevTools Performance is a built-in tool in the Google Chrome browser that
provides developers with powerful features for analyzing and optimizing the
performance of web applications. The tool includes several panels that provide a
detailed view of application performance, including the Timeline, Network,
Memory, and CPU panels.

The Timeline panel is the primary panel used for profiling application
performance. It records and analyzes the timeline of events that occur during
the loading and rendering of a web page, providing a detailed view of the
various phases of page rendering such as parsing, layout, painting, and
scripting. The Timeline panel also displays information about CPU and network
activity during page rendering, allowing developers to identify performance
issues related to these resources.

The Flame Chart view in the Timeline panel provides a graphical representation
of the time spent on each function call in the application. This view allows
developers to quickly identify the most time-consuming parts of the application
and optimize them for better performance.

# my observation

start record and do some kind of intereaction on the page and stop recording. in
TimeLine panel you can see the screenshort of the timeline of action that you
perform. select a particuler range of TimeLine.which contain a perticular action
that you have performed and you will see that what type of various phases your
page go through to perforom that action.

in that you can check flame graph which is a pictorial representation of how
much time each of your component took to render on the page. this is a good
place to observe which component are slow & time taking and then optimizing them
with memoization.

in performance you should check your event. when you intereact with your page
does that event happening as intented to do or there is some kind of leak.
ex: you are clicking on a component and it might re-render other component which 
are not intented to be render with this event.   

# note (for better performance check)

use throtling slowdown to 6X so that you can check for people with low end
device(in performance panel)

when you are checking performance of your app make sure to use production build
becouse in dev build react make a lot of rendering to give more controlled
environmnet (more option in crome dev control,porp type validation, additional
development worning ) so that you can test the code which is not required in
production build and also in production build your code is also minified and
compressed and optimized so your production code is always smaller then dev
build. due to that we can not really mazor during developemnt and what real user
experiencing always compare your production code so that you can get as closer
expreance as possible to the user.

always use the incognito mode becouse your browser can catch some data so that
you can have better performance but we want to check for user to better to use
incognito

for better exprence of the user if takeing action on the page take less then 60
FPS it means its a smooth expreance for the user Good Job. 60 FPS means (1000/60
= 16 miliSec) should be the optimum time for the page to loaded.

## WebWorker for expensive calculations

Web Workers are a feature of modern web browsers that allow developers to run
JavaScript code in a separate background thread, independent of the main UI
thread. This allows for computationally expensive tasks to be off-loaded to the
background thread, freeing up the main thread to handle user interactions and
rendering tasks, resulting in a more responsive and smoother user experience.

Web Workers were introduced with HTML5 and are supported by most modern web
browsers, including Chrome, Firefox, Safari, and Edge. There are two types of
Web Workers: Dedicated Web Workers and Shared Web Workers.

Dedicated Web Workers are specific to a single script and communicate with the
script that created them through a messaging system. Dedicated Web Workers have
their own global execution context, meaning they don't share memory with the
main thread or other Web Workers.

Shared Web Workers, on the other hand, allow multiple scripts to share the same
worker instance and communicate with each other through a messaging system.
Shared Web Workers can be more efficient than Dedicated Web Workers in some
scenarios where multiple scripts need to share data and resources.

Web Workers are created and controlled using the Worker interface in JavaScript.
Here's an example of how to create a Dedicated Web Worker:

```
// main.js

// create a new worker
const myWorker = new Worker('worker.js');

// send a message to the worker
myWorker.postMessage('Hello from the main thread!');

// listen for messages from the worker
myWorker.onmessage = function(event) {
  console.log('Message received from worker: ', event.data);
};
-----------------------------------------------------------------
// worker.js

// listen for messages from the main thread
self.onmessage = function(event) {
  console.log('Message received from main thread: ', event.data);

  // send a message back to the main thread
  self.postMessage('Hello from the worker thread!');
};
```

In the example above, main.js creates a new Dedicated Web Worker by passing the
URL of the worker script to the Worker constructor. It then sends a message to
the worker using the postMessage method and listens for messages from the worker
using the onmessage event handler.

worker.js listens for messages from the main thread using the onmessage event
handler, logs the received message to the console, and sends a message back to
the main thread using the postMessage method.

In summary, Web Workers are a powerful feature of modern web browsers that allow
developers to run JavaScript code in a background thread, separate from the main
UI thread. They can improve the performance and responsiveness of web
applications by offloading computationally expensive tasks to a background
thread, freeing up the main thread to handle user interactions and rendering
tasks.

# if Javascript is single threaded then how worker create extra Thread

Although JavaScript is traditionally considered to be a single-threaded
language, Web Workers allow for the creation of additional threads for executing
JavaScript code. This is because Web Workers are implemented as separate
instances of the JavaScript engine running in parallel, each with its own event
loop and memory space.

When a Web Worker is created, a separate thread is spawned in the browser's
runtime environment. This thread has its own event loop and can execute
JavaScript code independently of the main thread. Communication between the main
thread and the Web Worker is achieved through message passing, which involves
serializing and deserializing data between the threads.

The use of Web Workers allows for computationally intensive tasks to be
offloaded from the main thread, improving the performance and responsiveness of
web applications. For example, image processing, video decoding, and large-scale
data processing can be performed in a Web Worker without blocking the UI thread.

It's worth noting that although Web Workers provide additional threads for
executing JavaScript code, they have certain limitations. For example, Web
Workers cannot access the DOM directly, and they cannot share variables or state
with the main thread or other Web Workers. Communication between Web Workers is
also limited to message passing, which can add overhead and increase complexity.

ex:2

```
//in HTML
<script src="main.js"></script>

```
---

//in main.js
const worker = new Worker('worker.js')
worker.postMessage('Hello Worker')
worker.onmessage = e => { console.log('main.js: Message received fromworker:', e.data) }
// if you want to "uninstall" the web worker then use: //
worker.terminate()

---

// worker.js
this.onmessage = e => { console.log('worker.js: Message receivedfrom main script', e.data)
this.postMessage('Hello main') }

```

you can check in browser source > Thread > Now you have main and worker Thread
there

# Extra infro about web Worker

- web worker are async in nature so once you provide a task to an web worker
  then when you provide the second task then it's importent to terminate the
  worker becouse as it is async and it will not work on your second task untill
  it complete and first task. always terminate worker before asiging next task
  to it.

- when we share data from main to worker the data is copied. so when you have to
  work with large data ex: doing filter on large dataset then first you share
  the data from main to the worker then worker will do the filtering and send
  back the data to main at this point we have 2 copies of large data. one on
  worker and other one on main which is not a good option.(but still web worker
  helps to of load expensive calculations ) which is what web worker is good for
  making the main thread free for UI.

- Worker can not access your DOM becouse it is actually a different JS (using
  different JS Engine) but it can do network request and send data to your main
  thread which is cool.

### difference between reconciliation vs React Fiber?

in react 15 the tree looks like

```

             app (Root node)
              |
            navbar
              |
            hero
              |
           footer

```

before react v16 was release the life cycle of react app has only 2 phases

```
             render --> reconciliation

```

1. render => read the code (component) and create React element then append
   those element to the HTML node (parent) so that you can see the react app on
   the screen.

React creates a tree of nodes when the UI renders for the very first time, with
each node representing a React element. React creates a virtual tree called
virtualDOM. which is a clone of the rendered DOM tree but have additional
performance benifit.

2. Stack reconciliation => Stack reconciliation happens when any update arrises
   reconciliation is a algorithim which uses STACK data structure & sync in
   nature. reconciliation algorithim travese through virtual DOM tree compare
   both old element node with the update (new element node) and check if any
   change has happend.if yes, then update the virtual DOM (commit) (happen
   immidiatly).

# problems before React v16

before Fiber, reconciliation and commiting to the DOM weren't separated, and
React couldn't pause its traversal to jump to other higher priority update in
between. this is becouse we use stack data structure which is last in fast out
you can't jump on different level in stack you have to follow lifo order
(sequence). and also it was synchronous in nature so it is UI blocking This
often resulted in lagging inputs and choppy frame rates.

- there was no sense of priority.
- sync nature (bad exprense) (happen on the main thread)(always follow a squence
  there is no concept of concurency)
- not interuptable

# after React v16 React fiber was introduced

React Fiber is a reimplementation of React reconciler algorithm. which is a new
reconciliation algorithm. which is async in nature and use fiber data structure
which is react own implimentation.

in fiber Data strucutre - a fiber means unit of work which in litral sense a
javascript object which contain info about the react element (nodes) and some
aditional info like relation of node with each other.

React Fiber could help you prioritise functions that originate from user actions
while delaying logic of less-important background or offscreen functions to
avoid frame rate drops.

# benifits of React fiber:

- set priority (differed task)
- abort task if no longer needed.(inturptable)
- async run in memory in background.(non UI-Blocking)(concurrency)

- you can take advantage of fiber while using, suspense, Error boundary,
  useDifferedValue, useTransition.

# How does React Fiber work?

there are three phases : → render → reconciliation → commit

in render phase of react v16 we create a virtual DOM but it is not a tree of
element(node). it is a tree of fibers( element + relational info). and each node
has two fiber nodes

fiber need to maintain two trees:

1. current fiber (visible on the screen).
2. work in process fiber (background in the memory) (and it is a copy of current
   fiber)

in react 16 the tree looks like

```
  invisible Fiber Root node
                /\
       (current)  (workInProcess)
             /        \
           app      app
            |         |
          navbar    navbar
            |         |
          hero      hero
            |         |
         footer     footer

```

in render phase:

on inital render this fiber tree is created during render phase: we read the
code create element node and then we create fibers (unit of work) each node has
two fibers one is current fiber which is actually displayed on the userbrowser.
and then there is workinprocess fiber which is a copy of the current fiber. work
in the background.

if state changes the change happens in the workin process fiber. react has it's
own workLoop (similar to eventLoop) which check any update happened or not if
happen then fiber-reconciliation starts

in fiber-reconcialiation phase:

fiber-reconcialiation is an algorithem which take current fiber and
workinprogress fiber and priority as parameter. which traverse from upto bottom
in the tree compare current fiber with workinprocess fiber and set flags on any
workinprogeess fiber that has been updated (change found flag).

then react will create a list of all the flaged workin progess fibers called
effect list. and set there prioirty as defined by the author.(low priority or
high priority)

the Reconciliation phase can be interrupted it is async in nature. this is the
phase where we check that anyupdate is needed or not set priority and if any
task needed to be abendend or not.

Note: that React doesn't make any actual changes in this phase.

in commit phase:

React tells the DOM to render the effect list that was created in the previous
phase. this is done by simply changing the node direction from current (old
element) to workinporcess (new element). or you can say current and
workinprocess fiber were swiped with each other.

While the Reconciliation phase can be interrupted, the Commit phase cannot.
commit phase is sync in nature.

### chapter:3 React.memo for reducing unnecessary re-renders

# Life Cycle of React app

```

    →  render → reconciliation → commit
         ↖                   ↙
              state change

```

1. The "render" phase: create React elements React.createElement
2. The "reconciliation" phase: compare previous elements with the new ones
3. The "commit" phase: update the DOM (if needed).(appending the changed element
   to the Document)

A "render" is when React calls your function to get React elements.
"Reconciliation" is when React compares those React elements with the previously
rendered elements. A "commit" is when React takes those differences and makes
the DOM updates.

- A React Component can re-render for any of the following reasons:

1. Its props change
2. Its internal state changes
3. It is consuming context values which have changed
4. Its parent re-renders

Typically, the slowest part of this is the "commit" phase when the DOM is
updated. But not all DOM updates are slow. In fact, it's probably a bit
misleading to state simply that "the DOM is slow" because it's more nuanced than
that. DOM updates like adding/removing event listeners are really fast. The slow
part of the DOM is "layout".

# Unnecessary re-renders

Just because a component is re-rendered, doesn't mean that will result in a DOM
update. Here's a quick contrived example of that:

```

function Foo() { return <div>FOO!</div> }

function Counter() { const [count, setCount] = React.useState(0)
const increment = () => setCount(c => c + 1)
return ( <>
          <Foo />
          <button onClick={increment}>{count}</button>
          </>
  ) }

```

Every time you click on the button, the Foo function is called, but the DOM that
it represents is not re-rendered. Because of that, there's no DOM update for
that component at all. This is commonly referred to as an "unnecessary
re-render."

Unfortunately, there's been a fair amount of confusion around the difference
between "renders" and "commits." Many people know (or at least they've heard)
that "the DOM is slow," but plenty don't realize that just because a component
is re-rendered, doesn't mean the DOM will be updated. Because of this
misunderstanding, they believe it is a performance bottleneck that a component
renders when it doesn't actually need to update the DOM.

This can definitely be a problem in some cases, but in general even mobile
browsers on low-end devices are very fast at creating objects (render phase) and
comparing them (reconciliation phase). So what's the problem with re-renders?

ex:2

```

function CountButton({count, onClick}) {
  return <button onClick={onClick}>{count}</button>
}

function NameInput({name, onNameChange}) {
  return ( <label>
              Name:
                <input value={name} onChange={e => onNameChange(e.target.value)} />
            </label>
  ) }

function Example() {
  const [name, setName] = React.useState('')
  const [count,setCount] = React.useState(0)
  const increment = () => setCount(c => c + 1)
  return (
            <div>
            <div>
            <CountButton count={count} onClick={increment} />
            </div>
            <div>
            <NameInput name={name} onNameChange={setName} />
            </div> {name ? <div>{`${name}'s favorite number is ${count}`}
            </div> : null}
            </div>
            ) }

```

Based on how this is implemented, when you click on the counter button, the
<CountButton /> re-renders (so we can update the count value). But the
<NameInput /> is also re-rendered. If you have Record why each component
rendered while profiling. enabled in React DevTools, then you'll see that under
"Why did this render?" it says "The parent component rendered."

React does this because it has no way of knowing whether the NameInput will need
to return different React elements based on the state change of its parent. In
our case there were no changes necessary, so React didn't bother updating the
DOM. This is what's called an "unnecessary rerender" and if that
render/reconciliation process is expensive, then it can be worthwhile to prevent
it.

You can instruct React when to re-render. React.memo is a function components
and your component will not re-render simply because its parent re-rendered
which could improve the performance of your app overall.

# Slow renders

Given that JavaScript is really fast at handling the render and reconciliation
phases, then why is my app freezing up when I'm getting unnecessary re-renders?
In that situation, I'd suggest that your problem might be unnecessary
re-renders, but it's more likely a problem with slow renders in general. There's
something that your code is doing during the render phase (that is hearting the
performance) that's making things slow. You should diagnose and fix that first.
Once you've fixed that problem, then you can profile your app again and see if
you still have issues with unnecessary re-renders.

In fact, if you leave in a slow render and just reduce re-renders instead, then
you could wind up with a worse situation, and you'll likely wind up with more
complicated code.

# How to fix slow renders

So we've concluded we want to fix slow renders first. Then we can determine
whether re-renders are still a problem. So how do we fix the slow render. Often
you already know which interaction is causing a "janky" experience for the user.
Often it's when you open a tab, click a button, or type in a text field.

Here's what you do: Using your browser Dev tool Performance , React dev tool
profiler

It doesn't matter if 100% of your renders are necessary, if those renders are
slow, it will still produce a bad experience for the user. Stop punching
yourself in the face every time you blink (check what is cozing the slow down
during re-render). Fix your slow renders first. Then deal with the "unnecessary
re-renders" (with the help of React.memo).

# how to use React.memo

```

function CountButton({count, onClick}) { return
<button onClick={onClick}>{count}</button> }

function NameInput({name, onNameChange}) { return ( <label> Name: <input
value={name} onChange={e => onNameChange(e.target.value)} /> </label> ) }
NameInput = React.memo(NameInput)

```

wrap the component with react.memo() that is un-nesseserily re-rendering.
react.Memo will stop the re-rendering coz by the parent rendering. it will only
be re-render by the parent re-render if the props passing from parent to that
component changes. and the component can also be re-render coz by other factors

1. Its props change
2. Its internal state changes
3. It is consuming context values which have changed

# React dev Tool profiler

React DevTools Profiler is a tool that helps developers to identify performance
bottlenecks and optimize the rendering performance of React applications. it
allows developers to visualize the performance of their React components in
real-time.

The React DevTools Profiler works by measuring the rendering performance of each
component and providing insights into how long each component takes to render,
update, and commit changes to the DOM. It also provides a flame graph
visualization that shows the call stack of each component and the time spent on
each function call.

Using the React DevTools Profiler, developers can identify components that are
causing performance issues and optimize their rendering performance. They can
also analyze the performance impact of different props, state changes, and other
factors that affect the rendering performance of their components.

To use the React DevTools Profiler, you can open the Profiler tab in the React
DevTools panel and start profiling their application. They can then interact
with their application and analyze the performance of each component in
real-time.

# My observation

First Fix the slow render (component that making slow your rendering due to big
computation) before you fix the re-render(with React.memo).

openup your profiler in devtool and intereact with the app (becouse interaction
is what coz the re-render to the page). record it and see the flames it will
show what was re-renderd when you interteacted with the page (in cyan color) and
what was commited on the DOM (in orange color).

you can check what are the re-render that are un-nessesery and needed to be
memoized

generally you can solve the un-nessesory render by using default React.memo()
comparitor which is react check prev props to next props if it find any changes
it re-render the react.memo(Component) otherwise it will not re-render. 99% of
the time you don't require your custom comparitor but you can use it

react.memo(component,(prev,next)=>{})

it take the pre props and you can compare it with next props and write your own
logic for comparision. if comparitor fn returns false then till will re-render
if true then it will not.

# case: when we are dealing with list itr and object as props

When you use memo, your component re-renders whenever any prop is not shallowly
equal to what it was previously. This means that React compares every prop in
your component with its previous value using the Object.is comparison. Note that
Object.is(3, 3) is true, but Object.is({}, {}) is false.

non-primitve data type comparision will also coz trouble for you better to use
primitive value time

in case of array or objects as a props inside memoized component will coz
un-neesery re-render even after being memoized better to use primitive values or
do calculation up in the tree and pass primitve value to the component that you
want to stop from re-rendering.

by doing that you can avoid un-nessesory complication of custom comparitor and
un-nessesory re-rendering even after memoization

### Window large lists with react-virtual

As we learned in the last exercise, React is really optimized at updating the
DOM during the commit phase.

Unfortunately, there's not much React can do if you simply need to make huge
updates to the DOM. And as fast as React is in the reconciliation phase, if it
has to do that for tens of thousands of elements that's going to take some time
("perf death by a thousand cuts"). In addition, our own code that runs during
the "render" phase may be fast, but if you have to do that tens of thousands of
times, you're going to have a hard time being fast on low-end devices.

There's no UI that reveals these problems more than dataviz, grids, tables, and
lists with lots of data. There's only so much you can do before we have to
conclude that we're simply running too much code (or running the same small
amount of code too many times).

But here's the trick. Often you don't need to actually display tens of thousands
of list items, table cells, or data points to users. So if that content isn't
displayed, then you can kinda cheat by doing some "lazy" just-in-time rendering.

So let's say you had a grid of data that rendered 100 columns and had 5000 rows.
Do you really need to render all 500000 cells for the user all at once? They
certainly won't see or be able to interact with all of that information at once.
You'll only display a "window" of 10 columns by 20 rows (so 200 cells for
example), and the rest you can delay rendering until the user starts scrolling
around the grid.

Maybe you can render a few cells outside the view just in case they're a really
fast scroller. In any case, you'll save yourself a LOT of computational power by
doing this "lazy" just-in-time rendering.

This is a concept called "windowing" and in some cases it can really speed up
your components that render lots of data. There are various libraries in the
React ecosystem for solving this problem. My personal favorite is called
react-virtual.

# My observation

the thing is when you need to commit a large data on the browser it will take to
much time to load all the data all at once and it will take a while and on
screen you will see white screen of death untill all the data has been loaded.

The Solution is: don't load all the data all at once becouse browser has a
certain screen size and user can only be able to intereact with the visible
window data rest of the data will be outside of the window( which you see when
you scroll down)

so only load the data that can be visible so that user can interect with it and
load rest of the data as user scroll down (mean only load that can be visible to
the user ) that makes UX really better becouse we are not loading a lot of data
all at once. this technique is called Windowing (to load only data that the user
can see and interect)

it save user data becouse in most of the cases user don't need all the data to
begian with.

the sad part is that React itself is not capabile of doing that so we uses some
APIs like React virtual, windows to achive this optimization.

### chapter:5 Optimize context value

# case :1 when your contextProvider value is an array or object

contextProvider value is also compared with object.is so if it is a non
primitive value like array and object then it will be concidered as changed
eventhough they are the same

how to stabilized the non-primitive value use useMemo it will return the
returned value which can be any type of value (non-primitive too) it will only
update when it's dependency changes which will not and return a stable value
which we can use in contextProvider so that we don't face un-nessesery
re-render.

Note: even though you are using memo on the component to not re-render when the
prop changes it will not stop the re-render coz by context change. all the
children of context provider will re-render eventhough they are optimized by
memo.

# case:2 when your contextProvider value is an array or object

# but Childrens don't use the all the values of the array

when only one value is changed in context provider value and value is an array
or object. this will still coz re-render of all the children who are using
useContext to consume the value. but in our case all the children are not using
all the values of the context provider values array then it is better to splitup
one context provider to multiple context context provider and wrap all the
children with it so those who are using a specific context only those childrens
will be re-render

ex:

```

return (
  <AppStateContext.Provider value={state}>
    <AppDispatchContext.Provider value={dispatch}>
      {children}
     </AppDispatchContext.Provider>
  </AppStateContext.Provider>
)

```

### Chapter: 6 Fix "perf death by a thousand cuts"

"perf death by a thousand cuts" is a problem which refers to when multiple
component are running at the same time . eventhough those multiple component run
fast in isolation but the problem comes when they run together.

So how do we fix this performance problem? Remember that every perf problem is
solved by less code. In this case, the perf problem is coming from running too
much code. Often you have components responding to a state change that don't
need to. Often we memoize these with React.memo, and we could do that to all the
components in our app, but there are two problems with this:

1. It increases the complexity of our app (because we have to start using
   useCallback and useMemo for literally everything to take advantage of that,
   meaning you have a bunch of dependency arrays to manage).

2. React's still doing a bunch of work to check whether these components should
   be re-rendered.

So how do we fix this? What if we just put less of our state in the global
store? This is called colocation and it's a really great way to both improve
performance and maintenance of our app at the same time.

## aprroch:1 colocation

colocation is required when multiple state change happen up in the tree.

colocation mean placing state closer to where it needed not up in the tree. this
can help react to not to do extra work on checking that this state make any
changes to all the childrens or not (if it is up in the tree). it helps to
remove un-nessesery load on the component. which helps in improved performance.

## aproch:2 for some reason that state needed to be in parent (Global)

then spitup that specific component context logic.

for Ex: you are using context and reducer dispatch for whole app then create a
seprate context and reducer dispatch and wrap that specific component that is
using it. now other component will not re-render becouse there context will not
change.

## problem: useContext re-reder coz un-nessesery load on other states of the component

a component is using useContext but only using one value out of array of value
that contextProvider provides and as we are consuming the whole useContext value
array anytime any value changes the component re-renders.

the big problem here is the "re-render" coz by the context becouse sure there is
one place we are actually using that contextValue but rest of the code (which is
big) of the component is re-rendering for no reason and rest of the code has to
check if there is any update to them (on every render) so they can commit new
changes in reality there is no changes but it need to check becouse a re-render
happens.

eventhough you have used memo it does not stop a re-render coz by context
change.

solution: why not seprate out the useContext and the value that your component
is using logic and pass that value as a prop so we don't need to worry about
re-render and other states of the component don't have to re-render and checkup
any update it will definatly improve the performance.

so even though we are re-rendering the useContext but react don't have to check
un-nessery states which don't required a check for update to begain with.

ex:

```

- here callImpl require cell and cell requre the useContext to update it so we
  seprate out the cell logic and pass it as a prop to callimpl

function Cell({row, column}) {
  const state = useAppState()
  const cell = state.grid[row][column]
  return <CellImpl cell={cell} row={row} column={column} />
}

Cell = React.memo(Cell)

- here now cellimpl don't re-render and all of it's states don't need to check
  for any state update. (impoved performance removed un-nessesery re-render of
  states)

function CellImpl({cell, row, column}) {
  const dispatch = useAppDispatch()
  const handleClick = () => dispatch({type: 'UPDATE_GRID_CELL', row, column})
  return (
    <button
      className="cell"
      onClick={handleClick}
      style={{
        color: cell > 50 ? 'white' : 'black', backgroundColor:
`rgba(0, 0, 0, ${cell / 100})`, }} > {Math.floor(cell)}
    </button>
  ) }

```

here cellimpl will only update when cell prop value is change.

Note: cellimpl is a list item which we itr over. so every cellImpl don't require
cell value only the button clicked one requred which is only one.

## when your context is global and contain array of states as a value

## and multiple Component are utilizing the context;

context generaly take a state that we pass from global to some specific
component that may reqire it. when there are multiple component useing single
context then it is better to use a state management library over simple react
context which provide better performance and easy to manitan over context.

here we are using React recoil state management library to mange states

Note: there are many state management library like Redux,Recoil so on.

benifits of React recoil API over useContext;

Better performance: React Recoil uses a novel approach to managing state that
leverages the underlying mechanisms of React's rendering engine. Recoil uses a
dependency graph to efficiently track which components depend on which state
variables, and only re-renders the components that are affected by a change in
state. This approach can result in faster rendering times and better performance
compared to React Context, especially for larger and more complex applications.

Global state management: React Recoil provides a global state management
solution that is easy to use and integrates well with React's component-based
architecture. Recoil allows you to define and manage state at a global level,
making it easy to share state between components and reducing the need for
complex prop drilling or other state management solutions.

### chapter:7 performance tracking after production build has been launched

We should always ship fast experiences to our users, but sometimes something
slips through our PR review process and our users start having a slow
experience. Unless they complain to us, we have no way of knowing that things
are going so slow for them. User complaints is not a great policy for quality
control.

Because we can't make every user install the React DevTools and profile the app
for us as they interact with it, it would be nice if we could somehow track some
of the render times and get that information sent to our servers for us to
monitor.

Well, the React team has created an API specifically for this. It doesn't give
us quite as much information as the React DevTools do, but it does give us some
useful information.

# React profiler react Docs

The Profiler measures how often a React application renders and what the “cost”
of rendering is. Its purpose is to help identify parts of an application that
are slow and may benefit from optimizations such as memoization.

Note:

Profiling adds some additional overhead, so it is disabled in the production
build. To opt into production profiling, React provides a special production
build with profiling enabled.

# Docs

<Profiler> lets you measure rendering performance of a React tree
programmatically.

```
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

# Props

1. id: A string identifying the part of the UI you are measuring.
2. onRender: An onRender callback that React calls every time components within
   the profiled tree update. It receives information about what was rendered and
   how much time it took.

# Caveats

Profiling adds some additional overhead, so it is disabled in the production
build by default.

# onRender callback

React will call your onRender callback with information about what was rendered.

```
function onRender(id, phase, actualDuration, baseDuration, startTime, commitTime) {
  // Aggregate or log render timings...
}
```

# Parameters

- id: The string id prop of the <Profiler> tree that has just committed. This
  lets you identify which part of the tree was committed if you are using
  multiple profilers.

- phase: "mount", "update" or "nested-update". This lets you know whether the
  tree has just been mounted for the first time or re-rendered due to a change
  in props, state, or hooks.

- actualDuration: The number of milliseconds spent rendering the <Profiler> and
  its descendants for the current update. This indicates how well the subtree
  makes use of memoization (e.g. memo and useMemo). Ideally this value should
  decrease significantly after the initial mount as many of the descendants will
  only need to re-render if their specific props change.

- baseDuration: The number of milliseconds estimating how much time it would
  take to re-render the entire <Profiler> subtree without any optimizations. It
  is calculated by summing up the most recent render durations of each component
  in the tree. This value estimates a worst-case cost of rendering (e.g. the
  initial mount or a tree with no memoization). Compare actualDuration against
  it to see if memoization is working.

- startTime: A numeric timestamp for when React began rendering the current
  update.
- endTime: A numeric timestamp for when React committed the current update. This
  value is shared between all profilers in a commit, enabling them to be grouped
  if desirable.

# Usage

1. Measuring rendering performance programmatically Wrap the <Profiler>
   component around a React tree to measure its rendering performance.

```
<App>
  <Profiler id="Sidebar" onRender={onRender}>
    <Sidebar />
  </Profiler>
  <PageContent />
</App>
```

It requires two props: an id (string) and an onRender callback (function) which
React calls any time a component within the tree “commits” an update.

2. Measuring different parts of the application You can use multiple <Profiler>
   components to measure different parts of your application:

```
<App>
  <Profiler id="Sidebar" onRender={onRender}>
    <Sidebar />
  </Profiler>
  <Profiler id="Content" onRender={onRender}>
    <Content />
  </Profiler>
</App>
```

You can also nest <Profiler> components:

```
<App>
  <Profiler id="Sidebar" onRender={onRender}>
    <Sidebar />
  </Profiler>
  <Profiler id="Content" onRender={onRender}>
    <Content>
      <Profiler id="Editor" onRender={onRender}>
        <Editor />
      </Profiler>
      <Preview />
    </Content>
  </Profiler>
</App>
```

Although <Profiler> is a lightweight component, it should be used only when
necessary. Each use adds some CPU and memory overhead to an application.

# Kent C dodds

```
function onRenderCallback(
  id, // the "id" prop of the Profiler tree that has just committed
  phase, // either "mount" (if the tree just mounted) or "update" (if it re-rendered)
  actualDuration, // time spent rendering the committed update
  baseDuration, // estimated time to render the entire subtree without memoization
  startTime, // when React began rendering this update
  commitTime, // when React committed this update
  interactions, // the Set of interactions belonging to this update
) {
  // Aggregate or log render timings...
}
```

It's important to note that unless you build your app using react-dom/profiling
and scheduler/tracing-profiling this component won't do anything.

From here, you'll want to send the onRenderCallback data to a monitoring tool
(like Grafana for example). Because re-renders can happen a LOT, I'd personally
suggest batching them up and sending them together every 5 seconds or so. For
example:

```
let queue = []

// sendProfileQueue every 5 seconds
setInterval(sendProfileQueue, 5000)

function onRenderCallback(
  id,
  phase,
  actualDuration,
  baseDuration,
  startTime,
  commitTime,
  interactions,
) {
  queue.push({
    id,
    phase,
    actualDuration,
    baseDuration,
    startTime,
    commitTime,
    interactions,
  })
}

function sendProfileQueue() {
  if (!queue.length) {
    return Promise.resolve()
  }
  const queueToSend = [...queue]
  queue = []
  // here's where we'd actually make the server call to send the queueToSend
  // data to our backend...
  console.info('sending profile queue', queueToSend)
  return Promise.resolve()
}
```

Something to keep in mind is that because this is running in production, React
does it's best to not hurt performance in the measuring of it (which is pretty
sensible). Because of this, we're limited in the information we can receive. So
you'll probably want to strategically place <Profiler /> components in your app
with sensible id props so you can determine the source of the performance issue
more easily.

Here's an example of what the data looks like:

```
{
  id: "Navigation",
  phase: "update",
  actualDuration: 0.09999994654208422,
  baseDuration: 0.3799999540206045,
  startTime: 104988.11499998556,
  commitTime: 104988.45000000438,
  interactions: [ // this is actually a Set, not an array
    {
      __count: 0
      id: 3,
      name: "menu click",
      timestamp: 104978.33499999251,
    }
  ],
}
```

Note: when using react profiler make sure you deploy your app in phases first to
1000 customer then 2500 like this so that you can solve all the performance
issue in the starting phase and your min user suffer untill the full relise of
the app.

### react 18 (useDifferedValue,useTransition)

use these API for setting the priority of which task is importent and needed to
be render imidiatly and which can wait and render later (immidiatly after the
importent render finishesh)
