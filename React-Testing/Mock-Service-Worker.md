### when to use act

So the act warning from React is there to tell us that something happened to our
component when we weren't expecting anything to happen. So you're supposed to
wrap every interaction you make with your component in act to let React know
that we expect our component to perform some updates and when you don't do that
and there are updates, React will warn us that unexpected updates happened. This
helps us avoid bugs.

Luckily for you and me, React automatically handles this for any of your code
that's running within the React callstack (like click events where React calls
into your event handler code which updates the component)

but it cannot handle this for any code running outside of it's own callstack
(like asynchronous code that runs as a result of a resolved promise you are
managing or if you're using jest fake timers). With those kinds of situations
you typically need to wrap that in act(...) or async act(...) yourself. BUT,
React Testing Library has async utilities that are wrapped in act automatically!

If you're still experiencing the act warning, then the most likely reason is
something is happening after your test completes for which you should be
waiting. (component re-render)

when you are triggering state update which coz the component to re-render then
you need act to make sure your app stablize before you test it. when you are
testing and doing some action which coz your react component to re-render . that
action need to wrapped by act function so that action first stablize. act
function will wait to re-render to complete then move forward with the code
test.

### error

# error name : provide router component

external library don't like to be run in the testing environment.

solution:

1. import the context provider(with context i mean the library that the element
   is thowing context error for).
2. provide the context they want wrap your render component with the context
   component.

# error name: provide act()

generaly coz by data fetching inside useEffect becouse useEffect update the DOM
after the render and data fetching a async task with by default take some time
to complete and testing environment warn you that the component has updated and
we were not looking for any kind of update. maybe after the update the changes
may break the current exsisting test cases.

testing environment run there code syncronusly one line at a time if it
encounter an a code that trigger async task it does not wait a to finish the
async task before moving forward to the next line. and there is the entire
problem.

1. so unexpected state update in test is a bad thing.

act function is a wrapper to wrap your async task with it so that it can provide
a waiting window for async task to finish first before moving to the next sync
task.

Note: act function is implimented by react DOM and it is a testing utility
directly provided by the react.

so when we are not using react tesing environment and making any async request
that action must be wrapped with act function call

in test when we wrap act fn on an event that coz the state update. we tell our
test that we expect state to be changed becouse of this act fn. and when test
reach that line and enter the act fn then react will make sure that all the
state update + useEffect finishes before exiting the act wrapper function.

react testing library uses act fn for you automatically. when you are using any
selecter that include async nature into count (they also provide a window to
finish the async task on the react before moving forward in the test) like
findBy, findAllBy, waitFor, userEvent

Note: so as there are selecter with automaticlly use act fn behind the seen.
when you see a warining saying use act you don't need to use act. and use your
react tesing library async selector (findBy, findAllBy, waitFor, userEvent)
which will fix that error for you automatically

## Module Mock (helps to skip over component)

module mock is used when your render componet or children of the render
component is dependent on some external dependency js file and you want to mock
it.

```
jest.fn("import adress",()=>{
    return ()=> {
        return "XYZ"
    }
})
```

now what will happen is the external commononet will not be imported from the
real import adress, do not actually import the real file insted if anyone try to
import that file from this import address just give them this code that i have
decleared here. this module mock will overwrite the real data with the provided
fn data.

what will endup happening is that where the external compoent is written will be
replaced by the XYZ text that we have decleared here.

we do module mocking when we don't want to includer that external file or our
test does not depend on that external file we just wnat to make the render
component isolated so we can test our test on it.

### Mock Service worker?

# problem with mocking a fetch request with jest.fn()

with jest.fn() mock will only be able to mimick the successful result data but
you can't check that have you written right header,body for the request that you
are making to the backend. you can't test failure case of the request. but with
MSW you can mimic the fake request more real like you can also test all the
endge cases and check the request you have made is currect or not.

MSW helps to mimic making request more aquretly and has more granuler control
over the request. so you can be confident about the reqest that you made is
currect and the response that you recive is correctly mocked.

### Request Handler?

Request handler is a function that determines whether an outgoing request should
be mocked, and specifies its mocked response (response resolver).

rest.get('URL', responseResolver)

in MWS there are two type of Request Handler:

1. rest (uses HTTP method to make request like GET,POST etc)
2. grapql

we are only focusing on rest api .

## part:1 how request handler determines whether an outgoing request should be mocked?

the answer is Request mactching.

To determine which outgoing requests should be mocked, Mock Service Worker
performs request matching—a process of executing a request handler predicate
against the actual request.(compare the actual request with the mock request)

with rest request handlers we check two things:

1. Request method.
2. Request URL.

# Request method

in rest object there are multiple method type:

1. rest.get()
2. rest.post()
3. rest.put()
4. rest.patch()
5. rest.delete()
6. rest.options()

ex: rest.method("URL", responseResolver)

there is one more method :

custom method: rest.all() Intercept any REST API request to a given path
regardless of its method. it's not strict to check which method is used it will
always intercept all the rest API regardless of method type.

# Request URL

There are multiple ways to match a REST API request by its URL:

1. Provide an exact request URL;
2. Use a path;
3. Use a RegExp.

- 1. Exact request URL: Provided an exact request URL string, only those request
     that strictly match that string are mocked.

ex: rest.get('https://api.backend.dev/users', responseResolver)

- 2. Path

a path is often used to specify the endpoint or route of a URL in a web
application. For example, in the URL https://example.com/users, the path is
/users. It identifies the specific resource or functionality that the client is
requesting from the server.

Path is an abstraction over regular expression (RegEx). MSW use two type of
abstraction :

1. wildcard
2. path parameters

- 1. Path with a Wildcard:

a path with a wildcard include wildcard symbol to match multiple segments or
values within a URL. It allows for flexible matching of URLs that follow a
certain pattern while accommodating dynamic values.

some of the wildcard symbol:

```
1. * (Asterisk):

Matches any number of characters within a single URL segment.
Example: /users/* matches /users/johndoe, /users/marysmith, etc.

2. ** (Double Asterisk):

Matches any number of URL segments, including nested segments.
Example: /users/** matches /users/johndoe, /users/profile/123, /users/admin/settings, etc.

3. ? (Question Mark):

Matches a single character within a URL segment.
Example: /users/j?hn matches /users/john, /users/jahn, etc.

4. [...] (Character Class):

Matches any single character within a specified set or range of characters.
Example: /users/[a-z] matches /users/a, /users/b, /users/c, etc.

5. + (Plus):

Matches one or more occurrences of the preceding character or group.
Example: /users/abc+ matches /users/abc, /users/abcc, /users/abccc, etc.
```

NOTE: you can provide partical url ("/api.backend/") that contain this text and
after that we don't care.

- 2. path parameters:

A path parameter is a placeholder within a URL path that allows you to capture a
specific value from the incoming URL and use it as a parameter in your
application or server logic. It is typically denoted by a colon (:) followed by
the parameter name.

```
:param (Colon + Parameter Name):

Example: /users/:username matches /users/johndoe and captures johndoe as the username parameter.
```

3. regEx: When provided a regular expression, all request URL that match that
   expression will be captured, regardless of the their origin.

ex: rest.delete(/\/posts\//, responseResolver)

## part:2 aftering matching handler should mock response with response resolver fn

The response resolver function is a callback function that determines the mocked
response for a given request. It allows you to customize the response sent back
to the client when a particular request is matched by the mock handler.

ex: rest.get('URL', responseResolver)

responseResolver fn take three parameter:

1. request: Information about the captured request.
2. response: Function to create the mocked response.
3. context: Context utilities specific to the current request handler.

# request (is object)

It represents the incoming request object and provides information about the
request, such as the request properties like URL, headers,body and request
method req.clone(), req.text(), req.json(), req.passthrough(). You can access
these properties & methods to inspect the request details and use them to
determine the appropriate response.

checkout methods:

1. req.clone(): This method creates a clone of the request object. It can be
   useful when you want to modify or manipulate the request while keeping the
   original request intact. Cloning the request allows you to perform operations
   on it without affecting the original request.

2. req.text(): This method returns a promise that resolves to the text
   representation of the request's body. It reads the body of the request as a
   text string. It is typically used when the request body is expected to be
   plain text content.

3. req.json(): This method returns a promise that resolves to the JSON
   representation of the request's body. It reads the body of the request and
   attempts to parse it as JSON. If the request body is in valid JSON format,
   the method will return the parsed JSON data. This is useful when the request
   body is expected to be JSON-encoded data.

4. req.passthrough(): This method is used to bypass the mocking logic and allow
   the request to be sent to the actual network. When req.passthrough() is
   called within a request handler, MSW will skip mocking the request and
   instead forward it to the original endpoint. This can be helpful when you
   want to mock most requests but allow certain requests to reach the real
   server for scenarios like testing against a live API or for specific
   debugging purposes.

check full list of request proprty & method : https://mswjs.io/docs/api/request

# response (is function)

A call to the res() function creates a mocked response object. that response
object will be returned to the client that made the network request. It contains
various properties and methods that define the characteristics of the response.

you can use response fn as row or with context (response transformer) to define
a response for the clint that made the network response.

1. Raw usage

The res() call can accept a function that modifies and returns a created mocked
response instance. It's recommended, however, to use standard response
transformers for faster reference and reliability.

```
ex: rest.get('/users', (req, res) => {
  return res((res) => {
    res.status = 301
    res.headers.set('Content-Type', 'application/json')
    return res
  })
})
```

Properties and methods that you can use with response objects.

1. properties

```
 Property name	Type	Description

- status	    number	Mocked response HTTP status code. (Default: 200)
- statusText	string	HTTP status text of the current status code.
- body	        string	Stringified body of the response.
- headers	    Headers	Mocked response HTTP headers.
- delay	number	Delay   duration (ms) before responding with a mocked response.
```

2. Methods

- once() : When called, marks the current request handler as used. Any
  subsequent request matches to this handler are ignored, and fall-through to
  the next suitable handler.

```
ex: rest.post('/login', (req, res, ctx) => {
  return res.once(ctx.json({ id: 'abc-123' }))
})
```

- networkError() - Emulates a network error during the response. Cancels the
  respective request. and return the network error test.

```
ex: rest.get('/books', (req, res, ctx) => {
  return res.networkError('Failed to connect')
})
```

NOTE: inside MSR using networkError() is considered as intended error meaning
you are self declearing an error to test an specific case related to error.
don't use new Error() which is concidered as non-intended error which means that
it is a unexpected error. not programatic error.

2. Standard usage with Context (response transformer)

context is called response transformer becouse it can modify or transform the
response before it is sent back to the client.

a response transformer could intercept the response and modify its status,
headers, or body. It allows you to simulate dynamic responses or add custom
logic to the mocked responses.

```
ex: rest.get('/users', (req, res, ctx) => {
  return res(
    // This response transformer sets a custom status text on the response.
    ctx.status(301),
    // While this one appends a "Content-Type" response header.
    ctx.set('Content-Type', 'application/json'),
  )
})

ctx is context
```

Note: you can modify the content directly by using res( res => res.status = 200)
or you can use context to modify the response returned data res(
ctx.status(300), ctx.json("hi")).

response with context is prefered generally becouse it is faster.

# context (is object)

context is called response transformer becouse it can modify or transform the
response before it is sent back to the client.

a response transformer could intercept the response and modify its status,
headers, or body. It allows you to simulate dynamic responses or add custom
logic to the mocked responses.

Context (ctx) argument of a response resolver contains a set of response
transformers functins to compose a mocked response.

Note: Context functions are specific to the request handler .different for rest
and graphql.

type of methods with rest context

1. ctx.status(status code, status text (optional))

ex: return res(ctx.status(404, 'Custom status text'))

2. set() : Sets one, or multiple headers on the mocked response.

Note: Headers in a fetch request are used to provide additional information
about the request to the server. like Content-Type, Authorization, Accept,
Cache-Control, Custom headers.

```
ex1:  ctx.set({
      'x-my-header': ['one', 'two'],
    }),

ex2:  ctx.set({
      'Accept-Language': 'en-US',
      'Content-Type': ['application/json', 'application/hal+json'],
      'Content-Length': '40032',
    }),
```

3. body(): Sets a raw response body.

Note: body parameter in a fetch request is used to send data from the client to
the server.

4. text: Sets a textual response body which will be send back to the clint.
   Appends a Content-Type: text/plain header on the response.

```
ex:   ctx.text(
      'The future belongs to those who believe in the beauty of their dreams.',
    )
```

5. json(): Sets a JSON response body which will be send back to the clint.
   Appends a Content-Type: application/json header on the response.

```
ex: ctx.json({
      firstName: 'John',
      age: 32,
    })
```

6. xml() : Sets an XML response body. Appends a Content-Type: text/xml header on
   the response.

7. delay(): Delays the response by the given duration (in ms). When no duration
   is provided, uses a random realistic server response time.

ex: ctx.delay(2000)

8. fetch() :Performs a real request inside a fake request handler.

```
ex: rest.get('https://api.github.com/users/:username', async (req, res, ctx) => {
  // Performs an original request to the GitHub API.
  const originalResponse = await ctx.fetch(req)

  return res(
    ctx.json({
      name: 'John Maverick',
      location: originalResponse.location,
    }),
  )
})
```

## how to setup Mock Service Worker?

there are two environment where you can run MSW?

1. in clintSide Browser (setupWorker)
2. in Node environment (setupServer)

we are going to study setupServer for Node environment becouse we will test our
app in node.

NOTE: setupWorker and setupServer both provide same functionality only
difference is the environment and how they are configured.

# setupServer() (Node environment)

A function that sets up a request interception layer in NodeJS environment.

ex:

```
import { rest } from 'msw'
import { setupServer } from 'msw/node'

const server = setupServer(
  // Describe the requests to mock.
  rest.get('/book/:bookId', (req, res, ctx) => {
    return res(
      ctx.json({
        title: 'Lord of the Rings',
        author: 'J. R. R. Tolkien',
      }),
    )
  }),
)

beforeAll(() => {
  // Establish requests interception layer before all tests.
  server.listen()
})

afterAll(() => {
  // Clean up after all tests are done, preventing this
  // interception layer from affecting irrelevant tests.
  server.close()
})

test('renders a book data', () => {
  // Render components, perform requests, API communication is covered.
})
```

1. listen(): Establishes a request interception instance on previously
   configured via setupServer.

options that you can use in listen()

- onUnhandledRequest: Specifies how to handle a request that is not listed in
  the request handlers.

- 1. Pre-defined behaviors

```
Option:name	 Description
"bypass"	   Performs an unhandled request as-is.
"warn"	     Prints a warning into stdout of the current process.(byDefault)
"error"	     Prints an error into stderr of the current process.
```

ex:

```
const server = setupServer(
  rest.get('/books', (req, res, ctx) => {
    return res(ctx.json({ title: 'The Lord of the Rings' }))
  }),
)

server.listen({
  onUnhandledRequest: 'warn',
})
```

- you can also provide custom behaviours

When passed a function as a value of this option, the library executes that
function whenever an unhandled request occurs. The behavior of such unhandled
request won't be affected, but can be prevented if the custom callback function
throws an exception.

ex:

```
const server = setupServer(
  rest.get('/books', (req, res, ctx) => {
    return res(ctx.json({ title: 'The Lord of the Rings' }))
  }),
)

server.listen({
  onUnhandledRequest(req) {
    console.error(
      'Found an unhandled %s request to %s',
      req.method,
      req.url.href,
    )
  },
})
```

NOTE: In typical a testing setup you call .listen() in a setup hook before all
your tests, making sure the request interception is at place once the test suits
run.

ex: in jest

```
import { setupServer } from 'msw/node'

const server = setupServer(/* request handlers */)

beforeAll(() => {
  server.listen()
})
```

2. Close(): resets the server instance to it's original form & stops request
   interception.

NOTE: It's recommended to call .close() after all test suites to clean up and
prevent the current interception layer from affecting irrelevant tests.

ex:

```
import { setupServer } from 'msw/node'

const server = setupServer(...)

beforeAll(() => server.listen())
afterAll(() => server.close())
```

3. use(): It is used to add or prepend request handlers to the current server
   instance. Request handlers added using use() are considered runtime request
   handlers.

- Runtime Request Handler: When you call setupServer() to create a server
  instance, you can pass initial request handlers. These initial request
  handlers define the mock behavior for specific API endpoints. However, during
  a test or at runtime, you may need to add additional request handlers to
  handle specific requests. These additional request handlers added using use()
  are referred to as runtime request handlers.

ex1:

```
import { rest } from 'msw'
import { setupServer } from 'msw/node'

const server = setupServer(
  // Request handlers given to the `setupServer` call
  // are referred to as initial request handlers.
  rest.get('/book/:bookId', (req, res, ctx) => {
    return res(ctx.json({ title: 'Lord of the Rings' }))
  }),
)

test('supports sign in user flow', () => {
  // Adds the "POST /login" request handler as a part of this test.
  server.use(
    rest.post('/login', (req, res, ctx) => {
      return res(ctx.json({ success: true }))
    }),
  )
})

test('renders a book detail', () => {
  // Request to "GET /book/:bookId" in this test will return
  // a mocked "Lord of the Rings" book detail.
})

```

explanation: here we have two test case

1. supports sign in user flow: this will use the runtime request handler.
2. renders a book detail: this will use server instance request handler.

ex2: Permanent override: When a runtime request handler overlaps with an
existing one (has the same endpoint URL), the runtime handler always takes
priority. Leverage this to provision API behavior overrides per test suite.

```
import { rest } from 'msw'
import { setupServer } from 'msw/node'

const server = setupServer(
  // Initial request handler for a book detail.
  rest.get('/book/:bookId', (req, res, ctx) => {
    return res(ctx.json({ title: 'Lord of the Rings' }))
  }),
)

test('handles server error gracefully', () => {
  server.use(
    // Runtime request handler override for the "GET /book/:bookId".
    rest.get('/book/:bookId', (req, res, ctx) => {
      return res(ctx.json({ title: 'A Game of Thrones' }))
    }),
  )

  // Any requests to "GET /book/:bookId" will return
  // the "A Game of Thrones" mocked response from
  // the runtime request handler override.
})
```

ex3: One-time override: Request handler override can be used to handle only the
next matching request. After a one-time request handler has been used, it never
affects the network again.

To declare a one-time request handler override make sure your handler responds
using res.once().

Note that restoring the handlers via .restoreHandlers() can re-activate one-time
request handlers.

```
const server = setupServer(
  // Initial request handler for a book detail.
  rest.get('/book/:bookId', (req, res, ctx) => {
    return res(ctx.json({ title: 'Lord of the Rings' }))
  }),
)

test('handles server error gracefully', () => {
  server.use(
    rest.get('/book/:bookId', (req, res, ctx) => {
      // The first matching "GET /book/:bookId" request
      // will receive this mocked response.
      return res.once(
        ctx.status(500),
        ctx.json({ message: 'Internal server error' }),
      )
    }),
  )

  // Request to "GET /book/:bookId" in this test will return
  // a mocked error response (500).
})

test('renders a book detail', () => {
  // Request to "GET /book/:bookId" in this test will return
  // a mocked "Lord of the Rings" book detail.
})
```

3. resetHandlers(): Removes any request handlers (ex: runtime request handler)
   that were added on runtime (after the initial setupServer call).

Default behavior: The .resetHandlers() function is useful as a clean up
mechanism between multiple test suites that leverage runtime request handlers.

ex:

```
import { rest } from 'msw'
import { setupServer } from 'msw/node'

const server = setupServer(
  rest.get('/book/:bookId', (req, res, ctx) => {
    return res(ctx.json({ title: 'Lord of the Rings' }))
  }),
)

beforeAll(() => {
  server.listen()
})

afterEach(() => {
  server.resetHandlers()
})

afterAll(() => {
  server.close()
})

test('adds new book review', () => {
  server.use(
    // Adds a runtime request handler for posting a new book review.
    rest.post('/book/:bookId/reviews', bookReviewResolver),
  )

  // Performing a "POST /book/:bookId/reviews" request
  // will get the mocked response from `bookReviewResolver`.
})

test('asserts another scenario', () => {
  // Since we call `server.resetHandlers()` after each test,
  // this test suite will know nothing about "POST /book/:bookId/reviews".
})
```

3. 1. resetHandlers() can also Replacing initial handlers + remove any added on
      runtime handler.

.resetHandlers() take replacemant handler as a parameter with that it will
replace the inital handler decleared in the setupserver and also it will
remove/Cleanup any additional handler .

ex:

```
const server = setupServer(rest.get('/book/:bookId', bookDetailResolver))

test('first test', () => {
  // Can handle "GET /book/:bookId".
})

test('second test', () => {
  server.resetHandlers(rest.post('/login', loginResolver))

  // This, any any subsequent tests, will NOT handle
  // "GET /book/:bookId", because it's been reset.
  // Only the "POST /login" request handler exists now.
})
```

4. restoreHandlers(): Marks all used one-time request handlers as unused,
   allowing them to affect network again.(you can re-call the one-time request )

ex:

```
import { rest } from 'msw'
import { setupServer } from 'msw/node'

const server = setupServer(
  rest.get('/book/:bookId', (req, res, ctx) => {
    return res(ctx.json({ title: 'Lord of the Rings' }))
  }),
)

beforeAll(() => {
  server.listen()
})

afterAll(() => {
  server.close()
})

test('handles server error gracefully', () => {
  server.use(
    rest.get('/book/:bookId', (req, res, ctx) => {
      return res.once(
        ctx.status(500),
        ctx.json({ message: 'Internal server error' }),
      )
    }),
  )

  // Assert your application's behavior given server error.
})

test('renders book detail', () => {
  // At this point the server error request handler has been used,
  // and will not affect network anymore.
})

test('handles server error when I posted a book review', () => {
  // Restore one-time request handlers.
  // A "GET /book/:bookId" request will return a mocked server error
  // only for the next book detail request.
  server.restoreHandlers()
})
```

5. printHandlers(): Prints the list of currently active request handlers into
   the stdout of the running process.

ex: server.printHandlers()

Each request handler outputted into the console contains the following
information:

-Request method (GET) and path (/user/:userId). -Declaration frame
(handlers.js@5:8) you can click to navigate to the place in your code where you
declare this handler.

# clintSide Browser (setupWorker)

The setupWorker() function is used for client-side mocking. You usually create a
worker instance and activate it by calling the start() method on the returned
API.

one of the great usecase of clintSide mock service worker is that you can
interupt the network request for incurrect requests send by the user to the real
API and retun an error to the user by the MSW (use in production build). and
fillter out the real currect request out and send back to the real API.

- how to setup clintSide Worker ex:

```
import { setupWorker, rest } from 'msw'

const worker = setupWorker(
  // Provide request handlers
  rest.get('/user/:userId', (req, res, ctx) => {
    return res(
      ctx.json({
        firstName: 'John',
        lastName: 'Maverick',
      }),
    )
  }),
)

// Start the Mock Service Worker
worker.start()
```

- start()

The start() function is used to register and activate the Service Worker
instance responsible for intercepting requests.

# options that start() can take

1. serviceWorker:{url: servicer worker address} Specifies the custom location of
   the Service Worker file.By default, it is set to "/mockServiceWorker.js". You
   can provide a different URL if you have a customized Service Worker file.

```
 worker.start({
  serviceWorker: {
    url: '/assets/mockServiceWorker.js' // Points to a custom location
  }
})
```

2. serviceWorker:{options:{scope: file path for intercepting }} Allows you to
   provide custom Service Worker registration options. One option is scope,
   which narrows the scope of the Service Worker to intercept requests only from
   pages under a specific path.

```
worker.start({
  serviceWorker: {
    options: {
      scope: '/product' // Only intercept requests from pages under '/product'
    }
  }
})

```

3. quiet: Disables logging of matched requests in the browser's console. By
   default, it's set to false.

```
worker.start({
  quiet: true // Disable logging of matched requests
})

```

4. waitUntilReady: Determines whether to defer application requests (user
   request) that occur while the Service Worker is activating. It's recommended
   to keep this enabled (default is true) to avoid a race condition between
   Service Worker registration and your application's runtime.

```
worker.start({
  waitUntilReady: false // Disables deferring application requests
})

```

5. findWorker: (scriptURL, mockServiceWorkerUrl) => boolean

A custom lookup function to find a Mock Service Worker in the list of all
registered Service Workers on the page.

```
const worker = setupWorker(...)

worker.start({
  // Return the first registered service worker found with the name
  // of `mockServiceWorker`, disregarding all other parts of the URL
  findWorker: (scriptURL, _mockServiceWorkerUrl) => scriptURL.includes('mockServiceWorker'),
})
```

6. onUnhandledRequest: already defined in the node service worker above

#### Javascript.split(separator, limit)

The split() method splits a string into an array of substrings.

takes two parameter:

1. separator Optional :A string or regular expression to use for splitting. If
   omitted, an array with the original string is returned.

2. limit Optional: An integer that limits the number of splits. Items after the
   limit are excluded.

ex:

```
text = stars:>10000 language: javascript
text.split("language:") will return ["stars:>10000 language:","javascript"]
```

before that i use to think i need to tell the starting index end ending index of
the text (irredeamable)
