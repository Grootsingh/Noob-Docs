> Resource: from the Docs

tanStack version: 4.3

## phase of fetching.

1. `Idle`: The initial phase where no data fetching is happening. The query is created, but it hasn't been triggered yet.

    Idle state is when the fetching request is created by the author but it is dormanted meaning the request making (fatching) has not been started yet.

2. `Loading`: When the query is triggered, it transitions to the loading phase. This phase indicates that the data fetching is in progress. The loading phase is where you can show loading indicators or placeholders in your UI.

    Loading state is tarnsition state (in the process of fetching). the phase after the idle and before the settle state.

3. `settle state: Either Success or Error`:

    1. `Success`: If the data fetching is successful, the query moves to the success phase. In this phase, the fetched data is available, and you can use it in your components. The success phase persists until the query is refetched or invalidated.

        Success state is when the request has reached to the server and server has sent back the data to the clint and the data is ready to be used.

        Note: once the data is fetched completly then the data is cached and marked as stale(outOfDate) immediately. by Default.

      2. `Error`: If an error occurs during the data fetching process, the query transitions to the error phase. The error phase provides information about the error that occurred. You can use this information to display error messages or handle error scenarios.  

          Error state is during the process of fetching (request reaching the server,request coming back from the server) something went wrong.

4. `Idle (Settled)`: Once the query has reached either the success or error phase, it settles back to the idle phase. This means the query has completed its operation, and it's ready to be triggered again.

    Idle after settlement means that fetching is completed and you can re-trigger the fetching process becouse browser has the Code for making request (written by the author.)

## Chapter:1 Important Default (Docs)

1. `Query instances via useQuery or useInfiniteQuery by default consider cached data as stale(outOfDate).`

    In React Query, when you use the useQuery or useInfiniteQuery hooks to create query instances, the default behavior is that they consider the cached data as stale. This means that even if there is cached data available for a query, React Query will still attempt to fetch fresh data from the server.

    The reasoning behind this default behavior is to ensure that the data displayed in your application is up to date and reflects the latest changes. By considering the cached data as potentially stale, React Query promotes a real-time experience and helps avoid displaying outdated information.

    When you execute a query using useQuery or useInfiniteQuery, React Query will first check the cache for any existing data related to that query. If there is cached data available, React Query will immediately return that data to your component, allowing you to display it while the fresh data is being fetched.

    However, React Query will also trigger a background network request to fetch the latest data from the server. If the fresh data differs from the cached data, React Query will automatically update the cache and notify your component of the new data, triggering a re-render.

    This default behavior ensures that your application remains responsive and provides a consistent user experience by showing the most recent data. If the fresh data is identical to the cached data, React Query optimizes the process by avoiding unnecessary re-renders.

    Note: you can check any query to be outofdate or not by queryObject.isStale returns a true if it is stale.

### you can opt out of the default behaviour of re-fetching

1. `staleTime`

    The staleTime option in React Query allows you to control how long the cached data is considered fresh before it becomes stale. It specifies a duration in milliseconds after which React Query will automatically consider the cached data stale and trigger a background fetch for fresh data.

    By default, the staleTime is set to 0, which means that the data is considered stale immediately after it is fetched. This ensures that React Query always attempts to fetch fresh data from the server.

    You can provide a staleTime value when defining your query using the useQuery or useInfiniteQuery hooks. Here's an example that sets a staleTime of 5 minutes (300,000 milliseconds) for a query:

    ```js
    import { useQuery } from 'react-query';

    const fetchData = async () => {
      // Perform data fetching logic here
    };

    const MyComponent = () => {
      const { data } = useQuery('myQueryKey', fetchData, {
        staleTime: 300000, // 5 minutes
      });

      return <div>{data}</div>;
    };
    ```

    In this example, the query with the key 'myQueryKey' will initially attempt to fetch fresh data. Once the data is fetched, it will be considered fresh for 5 minutes (300,000 milliseconds). During this duration, React Query will use the cached data and won't trigger a background fetch.

    After the staleTime duration has passed, if the component tries to access the data, React Query will mark it as stale and automatically trigger a background fetch for fresh data. This ensures that the displayed data remains up to date.

    You can adjust the staleTime according to your specific needs. Setting a longer staleTime can help reduce unnecessary network requests and improve performance when the data doesn't change frequently. On the other hand, setting a shorter staleTime ensures more real-time data updates at the cost of more frequent network requests.

    Additionally, you can also override the staleTime on a per-query basis by using the staleTime option when calling the queryClient.invalidateQueries function to manually invalidate a query and force a refetch.

    Note: you can use staleTime Globally with (queryClint) or per-Query Bases by using (UseQuery,useInfinityQuery).

### reasons when stale data is re-fetching in the background?

1. `Mounting and re-render of Component`: When a component that uses a stale query data mounts or renders, React Query automatically initiates a background refetch for that query. This ensures that the displayed data remains up to date when the component is re-rendered.

    to opt out of this default functionality = refetchOnMount:false

2. `The window/app is refocused`: when user switchback from (other tabs,devTool) Windows to the (Application) website tab. or in simple terms when you switchback to your website tab from anywhere. this will coz a background refetch.

    to opt out of this default functionality = refetchOnWindowFocus:false

3. `The network is reconnected`: when the user network connection is not stable. internet connection lost and re-connect. this will coz a background refetch.

    to opt out of this default functionality = refetchOnReconnect :false

4. `manual re-fetching` by using any of these options:

    refetchInterval or refetchIntervalInBackground : When enabled, React Query will automatically refetch stale queries in the background at the specified intervals.

    queryClient.invalidateQueries: explicitly invalidating a query, React Query marks the query's data as stale and triggers a background fetch for fresh data.

    queryClient.refetchQueries function: This function accepts one or more query keys as arguments and triggers a background refetch for the corresponding queries.

### active queries and inactive queries

1. `Active queries`: these are those queries which are used in an component and that component is Mounted,rendered on the browser (is used)

2. `Inactive queires`: these are those queries which are used in an component and that component is un-mounted,not being rendered on the browser. (not being used)

If a Query instances via useQuery or useInfiniteQuery is inactive and present in the catch then it will get garbage collected after 5 minutes by default.

you can opt out of this default for inactive queries by setting catchTime option in milliseconds. which set a time for inactive quries to wait before it get garbage collected.

### When Query fails

Queries that fail are silently retried 3 times, with exponential backoff delay before capturing and displaying an error to the UI.

the process behind the scene:

1. `Initial Request`: When a query is first executed and the request fails (e.g., due to a network error or an unsuccessful HTTP response), React Query captures the error internally.

2. `Retries`: React Query will then automatically initiate retry attempts for the failed request, following an exponential backoff strategy. The retry attempts are made behind the scenes without any interruption to your application's UI or user experience.

3. `Exponential Backoff Delay`: The retry attempts are spaced out using increasing delay durations, following an exponential backoff pattern. This means that each subsequent retry is delayed for a longer duration than the previous one. The delay helps prevent overwhelming the server and allows potential temporary issues to resolve.

4. `Error Capturing`: If any of the retry attempts succeed, React Query updates the query's data with the successful response, and the UI components using the query are refreshed with the new data.

5. `Error Display`: If all retry attempts fail, React Query captures the error and provides it through the error object and use it to deplay the error message to the user.

Note: you can customize the number of retry you want to make when error occers with (retry in number) and you can also specify retry delay between each retry attempt with (retryDelay in milliseconds)

### structural comparison of response data

structural comparison refer to comparing two data response object and checking each and every key:value and if all the key:value matches are have the same nesting structure then both response are consider as same data.

In React Query, query results are structurally shared (structural compared) by default. This means that when a query fetches and receives new data, React Query checks if the data has actually changed by performing a structural comparison. If the data is determined to be the same, React Query maintains the existing data reference instead of creating a new reference.

This structural sharing of query results has several benefits:

1. `Value Stabilization`: By reusing the existing data reference when the data hasn't changed, React Query helps with value stabilization. Value stabilization is important for optimizing the performance of React components that use the useMemo and useCallback hooks. These hooks rely on stable dependencies and avoid unnecessary re-evaluations when the input values remain the same. By keeping the data reference unchanged when the data is structurally the same, React Query helps ensure the stability of computed values derived from the query result.

2. `Memory Efficiency`: Sharing the data reference for unchanged data helps reduce memory consumption. Instead of creating new copies of the data on each fetch, React Query minimizes memory usage by reusing the existing reference. This is particularly useful when working with large or complex data structures, as it avoids unnecessary memory allocations.

3. `Performance Optimization`: By detecting when the data hasn't changed, React Query can optimize rendering performance. When a component relies on a query and receives a new data reference, React Query will trigger re-rendering of the component to update with the fresh data. However, when the data is structurally the same, React Query can skip unnecessary re-renders, improving the overall performance of your application.

    By default, React Query uses a structural comparison algorithm to determine if the data has changed. This comparison is performed at a deep level, ensuring that nested objects and arrays are properly checked for changes. If the data is found to be structurally the same, React Query preserves the existing reference, and if the data has changed, React Query updates the reference to the new data.

    It's worth noting that React Query provides additional options to customize the comparison behavior, such as structuralSharing: 'always' or structuralSharing: 'never', allowing you to explicitly control when structural sharing is applied or disabled for specific queries.

Note: Structural sharing only works with JSON-compatible values, any other value types will always be considered as changed. If you are seeing performance issues because of large responses for example, you can disable this feature with the config.structuralSharing flag. If you are dealing with non-JSON compatible values in your query responses and still want to detect if data has changed or not, you can define a data compare function with config.isDataEqual or provide your own custom function as config.structuralSharing to compute a value from the old and new responses, retaining references as required.

## Chapter:2 Queries

A query is a declarative dependency on an asynchronous source of data (fetch,promise) that is tied to a unique key. A query can be used with any Promise based method (including GET and POST methods) to fetch data from a server. If your method modifies data on the server, we recommend using Mutations instead.

To subscribe to a query in your components or custom hooks, call the useQuery hook with at least:

1. A unique key for the query

2. A function that returns a promise that: Resolves the data, or Throws an error

```js
import { useQuery } from '@tanstack/react-query'

function App() {
  const info = useQuery({ queryKey: ['todos'], queryFn: fetchTodoList })
}
```

The unique key you provide is used internally for refetching, caching, and sharing your queries throughout your application.

The result object returned by useQuery Hook contains all of the information that you need from managing the fetch request and displaying the data.

const result = useQuery({ queryKey: ['todos'], queryFn: fetchTodoList })

### result object

1. contain states properties:

    1. `isLoading or status === 'loading'` - The query has no data yet
    
    2. `isError or status === 'error'` - The query encountered an error
    
    3. `isSuccess or status === 'success'` - The query was successful and data is available

Note: there is no idle state so from the start loading will be the inital state.

state dependent Properties:

  `error` - If the query is in an isError state, the error is available via the error property.

  `data` - If the query is in an isSuccess state, the data is available via the data property.

For most queries, it's usually sufficient to check for the isLoading state, then the isError state, then finally, assume that the data is available and render the successful state:

```js
function Todos() {
  const { isLoading, isError, data, error } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodoList,
  })

  if (isLoading) {
    return <span>Loading...</span>
  }

  if (isError) {
    return <span>Error: {error.message}</span>
  }

  // We can assume by this point that `isSuccess === true`
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```

you can use status as well if you don't like booleans for states.

2. contain FetchStatus properties:

    1. `isFetching or fetchStatus === 'fetching'` - The query is currently fetching.

    2. `isPaused or fetchStatus === 'paused'` - The query wanted to fetch, but it is paused. usually becouse the network is offline.

    3. `isIdle or fetchStatus === 'idle'` - The query is not doing anything at the moment.

isIdle means we are not fetching. which can mean two things

- either we have not started to fetch
- or we completed the fetching and now we are done.

Pause state happen when user is offline and try to fetch data. once the user is again online pause state will converted to fetching state automatically.

### important

loading state only trigger for the first time (initial Mount) when the query has No data on it. and in the process of fetching. once the data is fetched successfully. now query has the data in the catch. so there is no loading need state.

Note: if the query is inactive and removed from the catch then next time it fetches it will show the loading state.

fetchStatus trigger everytime when fetch fn executes on every render (initial Mount) and re-render (re-fetch).

### common convension regarding status and fetchstatus

The status gives information about the data: Do we have any data in the catch or not?
The fetchStatus gives information about the queryFn: Is queryFn running or not?

### isInitialLoading (boolean)

Lazy queries will be in status: 'loading' right from the start because loading means that there is no data yet. This is technically true, however, since we are not currently fetching any data (as the query is not enabled), it also means you likely cannot use this flag to show a loading spinner.

`isInitialLoading = isLoading && isFetching`

isInitialLoading will be true when you are fetching for the first time.

### Status Combinations you should know

1. `isIdle && isLoading` : we don't have the data + we have not started to fetch.
   (used when one query is disabled to fetch with enable:false)

2. `isLoading && isFetching` (isInitalLoading) means we don't have the data + we are fetching the data (in the fly)

3. `isIdle && !isLoading` : we do have the data + we have fieshed fetching the data completed

## Chapter:3 Query Keys

React Query manages query caching for you based on query keys. Query keys have to be an Array at the top level, and can be as simple as an Array with a single string, or as complex as an array of many strings and nested objects. As long as the query key is serializable(convert to JsonFormat), and unique to the query's data, you can use it!

Note: don't use fn which can't be converted to Json.

### static naming

simple string name

```js
// A list of todos
useQuery({ queryKey: ['todos'], ... })
```

### Dynamic naming

When a query needs more information to uniquely describe its data, you can use an array with a string and any number of serializable objects to describe it.

use object to descibe any dynamic data or use dynamic variable name (props).

```js
// An individual todo in a "preview" format
useQuery({ queryKey: ['todo', 5, { preview: true }], ...})

// An individual todo with todoId
useQuery({ queryKey: ['todo', todoId], ...})
```

### Rules of queyKeys

1. Array item order matters in QueryKey array.

    ```js
    // there querykey are Equal
    useQuery({ queryKey: ['todos', status, page], ... })
    useQuery({ queryKey: ['todos', status, page], ... })
    ```
    ```js
    // there querykey are not Equal
    useQuery({ queryKey: ['todos', page, status], ...})
    useQuery({ queryKey: ['todos', undefined, page, status], ...})
    ```

2. object keys order does not matter.

    ```js
    // these queyKeys are Equal
    useQuery({ queryKey: ['todos', { status, page }], ... })
    useQuery({ queryKey: ['todos', { page, status }], ...})
    useQuery({ queryKey: ['todos', { page, status, other: undefined }], ... })
    ```

### QueryKey as dependency Array

if your QueryFunction is dependent on some variable you should add that variable inside your querykey Array. so when the variable changes the queryfunction can re-fetch.

```js
function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ['todos', todoId],
    queryFn: () => fetchTodoById(todoId),
  })
}
```

### My observation:

QueryKey has two main features:

1. once the queryKey Array is serializable(convert to JsonFormat). then the key is used for storing data in catch once the data is sucessful.

2. Query Key array also act as a dependency array. in which if any of the index value changes it coz the queryFn to re-fetch the data.

## chapter:3 QueryFunction

A query function can be literally any function that returns a promise. The promise that is returned should either resolve the data or throw an error.

```js
useQuery({ queryKey: ['todos'], queryFn: fetch(URL) })
```

### Handling and Throwing Errors

For TanStack Query to determine a query has errored, the query function must throw or return a rejected Promise. Any error that is thrown in the query function will be persisted on the error state of the query.

```js
const{data, error} = useQuery({ queryKey: ['todos'], queryFn: fetch(URL) })
```

Note: Fetch doesn't throw Error for network request fail ex: 404

so for that you need to throw Error by yourself.

```js
useQuery({
  queryKey: ['todos', todoId],
  queryFn: async () => {
    const response = await fetch('/todos/' + todoId)
    if (!response.ok) {
      throw new Error('Network response was not ok')
    }
    return response.json()
  },
})
```

or simply use fetching library like Axios who are better at handling Error.

### QueryFunctionContext

Query keys are not just for uniquely identifying the data you are fetching, but are also conveniently passed into your query function as part of the QueryFunctionContext. While not always necessary, this makes it possible to extract your query functions if needed:

```js
function Todos({ status, page }) {
  const result = useQuery({
    queryKey: ['todos', { status, page }],
    queryFn: fetchTodoList,
  })
}

// Access the key, status and page variables in your query function!
function fetchTodoList({ queryKey }) {
  const [_key, { status, page }] = queryKey
  return new Promise()
}
```

The QueryFunctionContext is an object that is passed as an argument to the query function defined in React Query. It provides important information and context about the query being executed. Let's explore the properties of the QueryFunctionContext in detail:

`queryKey`: The queryKey property represents the key or keys that were passed to the query hook (useQuery or useInfiniteQuery). It is an essential property because it uniquely identifies the data being fetched. The query key can be any value or an array of values. For example, ['todos', { status, page }] is a query key that consists of the string 'todos' and an object { status, page }. It allows you to distinguish different queries and cache them separately.

`pageParam (only for Infinite Queries)`: The pageParam property is specific to infinite queries (useInfiniteQuery). It represents the value of the pageParam used for pagination. When fetching paginated data, the pageParam is typically an identifier or value that indicates the next page to be fetched. The pageParam value can be accessed within the query function through the pageParam property of the QueryFunctionContext. It enables you to use the current page parameter value for fetching subsequent pages or implementing custom pagination logic.

`signal (AbortSignal)`: The signal property provides an AbortSignal instance that can be used for query cancellation. It is useful when you want to manually cancel a running query. The AbortSignal allows you to abort or cancel the query request before it completes. You can listen to the abort event of the signal and take appropriate actions to cancel the query or cleanup any resources.

`meta`: The meta property is an optional field where you can attach additional metadata or information about your query. It is a flexible space where you can store any custom data related to the query. For example, you can use it to include information about the origin of the query, timestamps, or any other contextual data that might be useful for your application.

By utilizing the QueryFunctionContext and its properties, you can access the query key, pagination parameters, cancellation signal, and custom metadata within your query function. This allows you to customize the behavior of your query function based on the specific context of the query being executed.

## Chapter:4 Network Mode

TanStack Query provides three different network modes to distinguish how Queries and Mutations should behave if you have no network connection. This mode can be set for each Query / Mutation individually, or globally via the query / mutation defaults.

Since TanStack Query is most often used for data fetching in combination with data fetching libraries, the default network mode is online.

there are three Modes of Network:

1. `networkMode: 'online' (By default)`

    In this mode, queries will be fetched as normal when there is an active network connection. If the network connection is lost, queries will be paused and will not make further network requests until the connection is restored.

    fetchStatus are exposed to know if the network is offline or online:

    - in offline : fetchStatus will be paused
    - in online : fetchStatus will be fetching/idle

    Note: keep in mind that status:Loading is showing (becouse when you were fetching for the4 first time network was online, but then network get lost in the middle of fetching ) then fetchStatus:Paused so just checking of loading state is not good enough for showing <Spinner/>.

2. `networkMode: 'always'`

    In this mode, queries will always make network requests, even if there is no network connectivity. The queries will attempt to fetch data regardless of the network status.

    This is likely the mode you want to choose if you use TanStack Query in an environment where you don't need an active network connection for your Queries to work - e.g. if you just read from AsyncStorage, or if you just want to return Promise.resolve(5) from your queryFn.

3. `Network Mode: 'offlineFirst'`

    In this mode, the query function (queryFn) will be executed once (inital render), but subsequent retries will be paused.

    Here's how the offlineFirst network mode behaves:

    Initial Query Execution: When the query is executed for the first time, React Query will run the queryFn and attempt to fetch the data. If the data is already available in an offline storage (ServiceWorker), the query will succeed without making a network request.

    Subsequent Retries: If the data is not available in an offline storage (ServiceWorker) during the initial query execution then a network request will be made but after that subsequent retries will be paused. This means that React Query will not automatically retry to refetch the failed request. Instead for failed Request, it will wait for explicit triggering, such as when the network is reconnected (inital query execution fail becouse of network failure) or manually invoking a refetch.

### TanStack Query Devtool

The TanStack Query Devtools will show Queries in a paused state if they would be fetching, but there is no network connection. There is also a toggle button to Mock offline behavior.

## Chapter:5 Parallel Queries

"Parallel" queries are queries that are executed in parallel, or at the same time so as to maximize fetching concurrency.

When the number of parallel queries does not change, there is no extra effort to use parallel queries. Just use any number of TanStack Query's useQuery and useInfiniteQuery hooks side-by-side!

```js
function App () {
  // The following queries will execute in parallel
  const usersQuery = useQuery({ queryKey: ['users'], queryFn: fetchUsers })
  const teamsQuery = useQuery({ queryKey: ['teams'], queryFn: fetchTeams })
  const projectsQuery = useQuery({ queryKey: ['projects'], queryFn: fetchProjects })
  ...
}
```

### problem with React suspense

In suspense mode, if you use multiple useQueries hooks to make parallel requests, and any of those queries are still loading (fetching) or encounter an error (rejected), the entire component will be suspended. This means that the rendering of the component will be paused until all the queries have resolved or errored.

The purpose of this behavior is to ensure that the component only renders when all the data is available and ready to be displayed. It helps maintain consistency in the UI, as rendering the component with incomplete or partial data could lead to an inconsistent UI or incorrect user experience.

So, if any of the parallel queries are still loading or encounter an error, React Query suspends the component to wait for all the queries to finish. Once all the queries are resolved or errored, the component is unsuspended and can continue with rendering.

### Solution for React suspense

there are two ways you cansolve this problem:

1. create multiple Component with one useQuery each (Lame solution)

you can create separate components for each query. This way, if one query suspends, it won't affect the rendering of other components.

2. useQueries Hook

The useQueries hook in React Query allows you to execute multiple queries in parallel. It returns an array of query result objects, each contains all the status and data of an individual query.

```js
import { useQueries } from 'react-query';

function MyComponent() {
  const queries = useQueries([
    { queryKey: 'query1', queryFn: fetchQuery1 },
    { queryKey: 'query2', queryFn: fetchQuery2 },
    // Add more query configurations as needed
  ]);

  // Access the results of each query
  const query1Result = queries[0].data;
  const query2Result = queries[1].data;
  // ...

  // Render the component based on the query results
  return (
    <div>
      <h1>Query 1 Result: {query1Result}</h1>
      <h1>Query 2 Result: {query2Result}</h1>
      {/* Render other components based on the query results */}
    </div>
  );
}
```

The useQueries hook will return the array of query result objects once all the queries have finished executing. This means that all the queries in the provided array will either succeed or fail, and their corresponding results will be available in the returned array.

If an error occurs during the execution of any of the queries, the error will be captured in the error property of the respective query result object.

## Chapter:6 Sequential fetching (dependent Quries)

Dependent (or serial) queries depend on previous ones to finish before they can execute. To achieve this, it's as easy as using the enabled option to tell a query when it is ready to run:

```js
// Get the user (first)
const { data: user } = useQuery({
  queryKey: ['user', email],
  queryFn: getUserByEmail,
})

const userId = user?.id

// Then get the user's projects (second)
const {
  status,
  fetchStatus,
  data: projects,
} = useQuery({
  queryKey: ['projects', userId],
  queryFn: getProjectsByUser,
  // The query will not execute until the userId exists
  enabled: !!userId,
})
```

first the user will fetch then userProject will fetch becouse userProject has a option enabled
which mean don't run the useQuries Hook untill the enabled is True.

## Chapter:7 Background Fetching Indicators

A query's status === 'loading' state is sufficient enough to show the initial hard-loading state for a query, but sometimes you may want to display an additional indicator that a query is refetching in the background. To do this, queries also supply you with an isFetching boolean that you can use to show that it's in a fetching state, regardless of the state of the status variable

```js
function Todos() {
  const {
    status,
    data: todos,
    error,
    isFetching,
  } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  })

  return status === 'loading' ? (
    <span>Loading...</span>
  ) : status === 'error' ? (
    <span>Error: {error.message}</span>
  ) : (
    <>
      {isFetching ? <div>Refreshing...</div> : null}

      <div>
        {todos.map((todo) => (
          <Todo todo={todo} />
        ))}
      </div>
    </>
  )
}
```

### Show indicator when fetching happen anywhere in your app (Global indicator for refetching)

In addition to individual query loading states, if you would like to show a global loading indicator when any queries are fetching (including in the background), you can use the useIsFetching hook which also return an boolean.

```js
import { useIsFetching } from '@tanstack/react-query'

function GlobalLoadingIndicator() {
  const isFetching = useIsFetching()

  return isFetching ? (
    <div>Queries are fetching in the background...</div>
  ) : null
}
```

`isFetching`: for individual query fetching state.

`useIsFetching`: for Every Query fetching state.

## Chapter:8 Window Focus Refetching

If a user leaves your application and returns and the query data is stale, TanStack Query automatically requests fresh data for you in the background. You can disable this globally or per-query using the refetchOnWindowFocus option

```js
// Disabling Globally

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      refetchOnWindowFocus: false, // default: true
    },
  },
})

function App() {
  return <QueryClientProvider client={queryClient}>...</QueryClientProvider>
}

```

```js
//Disabling Per-Query

useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
  refetchOnWindowFocus: false,
})
```

### Provide your own Custom Window Focus Event

In rare circumstances, you may want to manage your own window focus events that trigger TanStack Query to revalidate. To do this, TanStack Query provides a focusManager.setEventListener function that supplies you the callback that should be fired when the window is focused and allows you to set up your own events. When calling focusManager.setEventListener, the default Window focus will be overWriten by the new one.

```JS
focusManager.setEventListener((handleFocus) => {
  // Listen to visibilitychange and focus
  if (typeof window !== 'undefined' && window.addEventListener) {
    window.addEventListener('visibilitychange', handleFocus, false)
    window.addEventListener('focus', handleFocus, false)
  }

  return () => {
    // Be sure to unsubscribe if a new handler is set
    window.removeEventListener('visibilitychange', handleFocus)
    window.removeEventListener('focus', handleFocus)
  }
})
```

### Ignoring Iframe Focus Events

A great use-case for replacing the focus handler is that of iframe events. Iframes present problems with detecting window focus. when your mouse is over Iframe it act as you have leave the Window/App and when to leave the Iframe it act as you come abck to the Window/App.

use this Preexsiting solution:

```js
import { focusManager } from '@tanstack/react-query'
import onWindowFocus from './onWindowFocus' // The gist above

focusManager.setEventListener(onWindowFocus) // Boom!
```

### you can also on and off the focus state with focusManager

```js
import { focusManager } from '@tanstack/react-query'

// Override the default focus state
focusManager.setFocused(true)

// Fallback to the default focus check
focusManager.setFocused(undefined)
```

but this is lame solution just use refetchOnWindowFocus option.

### Pitfalls & Caveats

Some browser internal dialogue windows, such as spawned by alert() or file upload dialogues (as created by <input <span></span>type="file" />) might also trigger focus refetching after they close. you should use FocusManager.setEventListener to provide a custom filter.

check tenstak page for more info.

## Chapter:9 Disable automatically fetching (How to use enabled Option)

If you ever want to disable a query from automatically running (fetching,background fethcing), you can use the enabled = false option

When enabled is false:

### permanent State set

- If the query has cached data, then the query will be initialized in the status === 'success' or isSuccess state.

- If the query does not have cached data, then the query will start in the status === 'loading' and fetchStatus === 'idle' state.

### Automatic fetching is disabled

- The query will not automatically fetch on mount.

- The query will not automatically refetch in the background.

- The query will ignore query client invalidateQueries and refetchQueries calls that would normally result in the query refetching.

### only Manual fetching

- refetch returned from useQueryOjbect can be used to manually trigger the query to fetch.

```js
function Todos() {
  const { isInitialLoading, isError, data, error, refetch, isFetching } =
    useQuery({
      queryKey: ['todos'],
      queryFn: fetchTodoList,
      enabled: false,
    })

  return (
    <div>
      <button onClick={() => refetch()}>Fetch Todos</button>

      {data ? (
        <>
          <ul>
            {data.map((todo) => (
              <li key={todo.id}>{todo.title}</li>
            ))}
          </ul>
        </>
      ) : isError ? (
        <span>Error: {error.message}</span>
      ) : isInitialLoading ? (
        <span>Loading...</span>
      ) : (
        <span>Not ready ...</span>
      )}

      <div>{isFetching ? 'Fetching...' : null}</div>
    </div>
  )
}
```

Permanently disabling a query opts out of many great features that TanStack Query has to offer (like background refetches), and it's also not the idiomatic way. It takes you from the declarative approach (defining dependencies when your query should run) into an imperative mode (fetch whenever I click here). It is also not possible to pass parameters to refetch. Oftentimes, all you want is a lazy query that defers the initial fetch.

### Conditional fetching

The enabled option can not only be used to permanently disable a query, but also to enable / disable conditionaly. A good example would be a filter form where you only want to fire off the request once the user has entered a filter value:

```js
function Todos() {
  const [filter, setFilter] = React.useState('')

  const { data } = useQuery({
      queryKey: ['todos', filter],
      queryFn: () => fetchTodos(filter),
      // ⬇️ disabled as long as the filter is empty
      enabled: !!filter
  })

  return (
      <div>
        // 🚀 applying the filter will enable and execute the query
        <FiltersForm onApply={setFilter} />
        {data && <TodosTable data={data}} />
      </div>
  )
}
```

### isInitialLoading (boolean)

Lazy queries will be in status: 'loading' right from the start because loading means that there is no data yet. This is technically true, however, since we are not currently fetching any data (as the query is not enabled), it also means you likely cannot use this flag to show a loading spinner.

`isInitialLoading = isLoading && isFetching`

isInitialLoading will be true when you are fetching for the first time.

## Chapter:10 Query Retries (How to use retry,retryDealy Option)

### retry Option

When a useQuery query fails (the query function throws an error), TanStack Query will automatically retry the query if that query's request has not reached the max number of consecutive retries (defaults to 3) or a function is provided to determine if a retry is allowed.

You can configure retries both on a GLOBAL level and an INDIVIDUAL query level.

- `retry = true` will infinitely retry failing requests.

- `retry = false` will disable retries.

- `retry = 6 (Number)` will retry failing requests 6 times before showing the final error thrown by the function.

- `retry = (failureCount, error) => ...` allows for custom logic based on why the request failed.

```js
import { useQuery } from '@tanstack/react-query'

// Make a specific query retry a certain number of times
const result = useQuery({
  queryKey: ['todos', 1],
  queryFn: fetchTodoListPage,
  retry: 10, // Will retry failed requests 10 times before displaying an error
})
```

### retryDelay Option

By default, retries in TanStack Query do not happen immediately after a request fails. As is standard, a back-off delay is gradually applied to each retry attempt.

The default retryDelay is set to double (starting at 1000ms) with each attempt, but not exceed 30 seconds

so for retry: 3 it will go from dealy: 1sec,2sec,4sec.

You can configure retryDelay both on a GLOBAL level and an INDIVIDUAL query level.

`retryDelay = (attemptIndex) => ...` allows for custom logic based on attemptIndex.

attemptIndex means number of attempt made to make a request. attemptIndex start from 0 means 0 attempt.

`retryDelay = 1000 (Time in milisecond)` allow you to specify Fix retryDelay time.

```js
const result = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
  retryDelay: 1000, // Will always wait 1000ms to retry, regardless of how many retries
})
```

## Chapter: 11 UI shift problem (Paginated / Lagged Queries)

### for pagination

Rendering paginated data is a very common UI pattern and in TanStack Query, it "just works" by including the page information in the query key:

```js
const result = useQuery({
  queryKey: ['projects', page],
  queryFn: fetchProjects
})
```

the problem is that The UI jumps in and out (you see a layoutShift) as Page changes from one page to another, the state changes from success to loading. different queries are created and destroyed for each page or cursor.

This experience is not optimal and TanStack Query comes with an awesome feature called keepPreviousData that allows us to get around this.

### keepPreviousData (Solve layoutShift problem)

keepPreviousData helps to show previousData while newData is fetching. help to display previous data on UI rahter than destroying previous data as soon as new Data start fetching. and shifting between success to loading state.

when `keepPreviousData: true`,

- The data from the last successful fetch is available while new data is being requested, even though the query key has changed.

- When the new data arrives, the previous data is seamlessly swapped to show the new data.

- isPreviousData is made available to know what data the query is currently providing you.

- isPreviosData: true, means currently we are showing previous Data.

```js
function Todos() {
  const [page, setPage] = React.useState(0)

  const fetchProjects = (page = 0) => fetch('/api/projects?page=' + page).then((res) => res.json())

  const {
    isLoading,
    isError,
    error,
    data,
    isFetching,
    isPreviousData,
  } = useQuery({
    queryKey: ['projects', page],
    queryFn: () => fetchProjects(page),
    keepPreviousData : true
  })

  return (
    <div>
      {isLoading ? (
        <div>Loading...</div>
      ) : isError ? (
        <div>Error: {error.message}</div>
      ) : (
        <div>
          {data.projects.map(project => (
            <p key={project.id}>{project.name}</p>
          ))}
        </div>
      )}
      <span>Current Page: {page + 1}</span>
      <button
        onClick={() => setPage(old => Math.max(old - 1, 0))}
        disabled={page === 0}
      >
        Previous Page
      </button>{' '}
      <button
        onClick={() => {
          if (!isPreviousData && data.hasMore) {
            setPage(old => old + 1)
          }
        }}
        // Disable the Next Page button until we know a next page is available
        disabled={isPreviousData || !data?.hasMore}
      >
        Next Page
      </button>
      {isFetching ? <span> Loading...</span> : null}{' '}
    </div>
  )
}
```

### for infiniteScrolling

the keepPreviousData option also works flawlessly with the useInfiniteQuery hook,from the problem of layoutShift, so you can seamlessly allow your users to continue to see cached data while infinite query keys change over time.

## Chapter:12 Infinite Queries

Rendering lists that can additively "load more" data onto an existing set of data or "infinite scroll" is also a very common UI pattern. TanStack Query supports a useful version of useQuery called useInfiniteQuery for querying these types of lists.

The useInfiniteQuery hook in React Query is used to fetch data in a paginated or infinite-scrolling manner. It allows you to retrieve data in chunks or pages and provides a convenient way to manage the pagination state.

The useInfiniteQuery hook takes three main arguments:

1. queryKey
2. queryFn (with params)
3. Options

```JS
  const {
    data,
    error,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isFetchingNextPage,
    status,
  } = useInfiniteQuery({
    queryKey: ['projects'],
    queryFn: fetchProjects,
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  })

```

When using useInfiniteQuery, you'll notice a few things are different:

- data is an object which contain key:pages value:array each index of page array is the data recived from that server.

- data.pageParams is an array which contain parameters used in the current fetch request.

### config next or previous params

getNextPageParam & getPreviousPageParam used to define next set of parameter which we should use when calling QueryFn.

- `getNextPageParam: (lastPage,allpage) => ...` the returned value will be used as nextPageParams

- `getPreviousPageParam: (firstPage,allpage) => ...` the returned value will be used as nextPageParams

- `lastPage`: This parameter represents the last page of data that was fetched in the previous request. It contains the data of the last page, which can be used to extract information needed for the next page request.

- `FirstPage`: This parameter represents the first page of data that was fetched in the inital request. It contains the data of the first page, which can be used to extract information needed for the previous page request.

- `allPages`: This parameter is an array of all the pages that have been fetched so far, including the lastPage. It provides access to the complete set of data retrieved in previous requests, allowing you to examine or manipulate the data if needed.

### Call next fetch or previous fetch

- The fetchNextPage functions is used to call queryFn with nextset of param (getNextPageParam)

- The fetchPreviousPage functions is used to call queryFn with previousset of param (getPreviousPageParam)

Note: generally fetchNextPage & fetchPreviousPage fn are called without any agrunments and we set params for these fn in getNextPageParam or getPreviousPageParam but you can also provide these params direclty inside fetchNextPage(params) & fetchPreviousPage(params). but it is recommanded to use getNextPageParam or getPreviousPageParam for configuring the params.

### indicator

- A hasNextPage boolean is now available and is true if getNextPageParam returns a value other than undefined

- A hasPreviousPage boolean is now available and is true if getPreviousPageParam returns a value other than undefined

- The isFetchingNextPage and isFetchingPreviousPage booleans are now available to distinguish between a background refresh state and a loading more state

Note: When using options like initialData or select in your query, make sure that when you restructure your data that it still includes data.pages and data.pageParams properties, otherwise your changes will be overwritten by the query in its return!

```js
// manual
// fetching infinity scroll with curser params

fetch('/api/projects?cursor=0')
// { data: [...], nextCursor: 3}
fetch('/api/projects?cursor=3')
// { data: [...], nextCursor: 6}
fetch('/api/projects?cursor=6')
// { data: [...], nextCursor: 9}
fetch('/api/projects?cursor=9')
// { data: [...] }
```

```js
// with useInfinityQuery
// fetching infinity scroll with curser params
// setting next set of parameter in getNextPageParam automatically

import { useInfiniteQuery } from '@tanstack/react-query'

function Projects() {
  const fetchProjects = async ({ pageParam = 0 }) => {
    const res = await fetch('/api/projects?cursor=' + pageParam)
    return res.json()
  }

  const {
    data,
    error,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isFetchingNextPage,
    status,
  } = useInfiniteQuery({
    queryKey: ['projects'],
    queryFn: fetchProjects,
    getNextPageParam: (lastPage, pages) => lastPage + 3,
  })

  return status === 'loading' ? (
    <p>Loading...</p>
  ) : status === 'error' ? (
    <p>Error: {error.message}</p>
  ) : (
    <>
      {data.pages.map((group, i) => (
        <React.Fragment key={i}>
          {group.data.map((project) => (
            <p key={project.id}>{project.name}</p>
          ))}
        </React.Fragment>
      ))}
      <div>
        <button
          onClick={() => fetchNextPage()}
          disabled={!hasNextPage || isFetchingNextPage}
        >
          {isFetchingNextPage
            ? 'Loading more...'
            : hasNextPage
            ? 'Load More'
            : 'Nothing more to load'}
        </button>
      </div>
      <div>{isFetching && !isFetchingNextPage ? 'Fetching...' : null}</div>
    </>
  )
}
```

### What happens when an infinite query needs to be refetched?

When an infinite query needs to be refetched due to becoming stale, React Query follows a specific process to ensure data integrity and accurate pagination. Here's an explanation of what happens:

1. Sequential Fetching: When an infinite query becomes stale and needs to be refetched, the refetching process starts with the first group of data. Each group is fetched sequentially, meaning that the fetch requests for subsequent groups will not be initiated until the previous group has been fetched and processed.

2. Avoiding Stale Cursors: The sequential fetching ensures that even if the underlying data has been mutated or modified, the query doesn't use stale cursors. This prevents potential issues such as fetching duplicate records or skipping over certain records.

3. Fetching Groups: Each group is fetched by making a request to the server using the appropriate parameters or cursor values. The query's queryFn is executed for each group, and the response data is retrieved.

4. Merging Results: As each group is fetched, the query merges the newly fetched data with the existing data. This process ensures that the query's data.pages array is updated with the latest pages and maintains the correct ordering.

5. Handling Removed Results: In cases where the results of an infinite query have been completely removed from the query cache (e.g., due to cache invalidation or eviction), the pagination restarts from the initial state. This means that only the initial group of data will be requested, and subsequent groups will be fetched (if requested again).

By following this approach, React Query ensures that when an infinite query needs to be refetched, the fetching process occurs sequentially to avoid using stale cursors. This helps maintain data consistency and prevents issues like duplicate or missing records. Additionally, if the query's results are removed from the cache, the pagination restarts from the beginning to retrieve the initial data group.

This behavior ensures that infinite queries can handle refetching scenarios effectively and provide accurate and up-to-date data to your application.

### Example for better understanding

1. When an infinite query is initially executed with the initial parameters, the query function (queryFn) is called with those parameters. This fetches the first group of data.

2. After that, if you call fetchNextPage() on the infinite query, the query function will be executed again, but this time with the parameters obtained from getNextPageParam(). These parameters are typically used to determine the cursor or page information needed to fetch the next group of data.

3. If the data from the subsequent fetchNextPage() call becomes stale, meaning it is outdated and needs to be refetched, the infinite query will restart from scratch. It will first execute the query function with the initial parameters again to fetch the initial group of data.

4. After fetching the initial group of data, subsequent fetchNextPage() calls will continue using the parameters provided by getNextPageParam() to fetch the remaining groups of data.

So, in summary, the infinite query starts by fetching the initial group of data using the initial parameters. Then, when fetchNextPage() is called, it fetches subsequent groups using the parameters from getNextPageParam(). If the data becomes stale, the process starts from scratch by fetching the initial group again before fetching the remaining groups.

### how to do selective refetch with refetchPage

If you only want to actively refetch a subset of all pages, you can pass the refetchPage function to refetch returned from useInfiniteQuery.

```js
const { refetch } = useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
})

// only refetch the first page
refetch({ refetchPage: (page, index) => index === 0 })
```

1. The useInfiniteQuery hook returns an object that includes a refetch function. This function can be used to manually trigger a refetch of the query.

2. The refetchPage function is executed for each page of the infinite query.refetchPage, refetchpage is like a forEach fn for each data.pages page of the useInfinityQuery.

    It receives three arguments: page, index, and allPages.

    - The page argument represents the data of the current page being processed.
    - The index argument indicates the index of the current page.
    - The allPages argument is an array containing the data of all previously fetched pages.

3. The refetchPage function should return a boolean value. If it returns true for a specific page, that page will be included in the refetch process. If it returns false, the page will be skipped.

In the provided example, the refetchPage function checks if the index is equal to 0, which means it only returns true for the first page. This ensures that only the first page is refetched when the refetch function is called.

### I Don't Know

Additionally, you can also use the refetchPage function as part of the second argument (queryFilters) when calling queryClient.refetchQueries, queryClient.invalidateQueries, or queryClient.resetQueries. This allows you to selectively refetch specific pages of the infinite query using those methods as well.

### if you want to pass custom information (params) to my QueryFn?

By default, the variable (params) returned from getNextPageParam will be supplied to the query function, but in some cases, you may want to override this. You can pass custom variables (params) to the fetchNextPage function which will override the default variable

- use fetchNextPage(custom params)

```js
function Projects() {
  const fetchProjects = ({ pageParam = 0 }) =>
    fetch('/api/projects?cursor=' + pageParam)

  const {
    status,
    data,
    isFetching,
    isFetchingNextPage,
    fetchNextPage,
    hasNextPage,
  } = useInfiniteQuery({
    queryKey: ['projects'],
    queryFn: fetchProjects,
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  })

  // Pass your own page param
  const skipToCursor50 = () => fetchNextPage({ pageParam: 50 })
}
```

### What if I want to implement a bi-directional infinite list?

generally we want only one directional inifnte scrolling so we use getNextPageParam with fetchNextPage().

for bi-directional use both (getNextPageParam, fetchNextPage()) with (getPreviousPageParam, fetchPreviousPage())

```js
useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  getPreviousPageParam: (firstPage, pages) => firstPage.prevCursor,
})
```

### What if I want to show the pages in reversed order?

Sometimes you may want to show the pages in reversed order.

in that case, you can use the select option:

```js
useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  select: (data) => ({
    pages: [...data.pages].reverse(),
    pageParams: [...data.pageParams].reverse(),
  }),
})
```

### when you want to manually update useInfinityQuery

you can update useInfinityQuery Pages array data by updating the catch of that useInfinityQuery.

you can do it by using useInfinityQuery queryKey in queryClient.setQueryData(['QueryKey'],updaterFn)

```js
//Manually removing first page:
// data is the data stored at this key in the catchs obect

queryClient.setQueryData(['projects'], (data) => ({
  pages: data.pages.slice(1),
  pageParams: data.pageParams.slice(1),
}))

```
```js
//Manually removing a single value from an individual page:

const newPagesArray =
  oldPagesArray?.pages.map((page) =>
    page.filter((val) => val.id !== updatedId),
  ) ?? []

queryClient.setQueryData(['projects'], (data) => ({
  pages: newPagesArray,
  pageParams: data.pageParams,
}))
```

make sure the structure of data remain the same as original.

### I Don't Know

queryClient.setQueryData(['QueryKey'],updaterFn)


## Chapter: 13 Initial Query Data

There are many ways to supply initial data for a query to the cache before you need it:

1. Declaratively:

    Provide initialData to a query to prepopulate its cache if empty

2. Imperatively:

    Prefetch the data using queryClient.prefetchQuery

    Manually place the data into the cache using queryClient.setQueryData

### I Don't Know

- queryClient.prefetchQuery
- queryClient.setQueryData

### Using initialData to prepopulate a query

There may be times when you already have the initial data for a query available in your app and can simply provide it directly to your query. If and when this is the case, you can use the initialData option to set the initial data for a query and skip the initial loading state!

`state = sucessfull`

```js
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: initialTodos,
})
```

# I Don't Know

IMPORTANT: initialData is persisted to the cache, so it is not recommended to provide placeholder, partial or incomplete data to this option and instead use placeholderData

### initalData in context of staleTime and initialDataUpdatedAt  staleTime

if you are not using staleTime option then, staleTime is zero by default so your initalData that you have set in your cash, will automatically becomes stale and new data will be fetched for that queue so that user can exprience uptogate data.

if you have provided staleTime then your initalData will stay fresh till the staleTime expires. so, no data will be fetched.

but what if your initalData has been drived from someOther data and it is not fresh and you want to refatch that data but you have staleTime option active too then what you will do?

### initialDataUpdatedAt

use initialDataUpdatedAt to define that how old/stale the data is and let the query decide the data needed to fetch or not.

initialDataUpdatedAt is defined in milisec.

initialDataUpdatedAt always define how old the data is in refrence to current Time (new Date()).

```js
//ex:1 
const initialDataUpdatedAt = Date.now() - 2 * 60 * 1000; // 2 minutes ago
//ex:2 
const initialDataUpdatedAt = Date.now() // current
```

ex:1 will be refetched and ex:2 will not be

```js
// Show initialTodos immediately, but won't refetch until another interaction event is encountered after 1000 ms
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: initialTodos,
  staleTime: 60 * 1000, // 1 minute
  // This could be 10 seconds ago or 10 minutes ago
  initialDataUpdatedAt: initialTodosUpdatedTimestamp, // eg. 1608412420052
})
```

### initialDataUpdatedAt deeper understanding

initialDataUpdatedAt define how much time has passed since the data has been updated or this data is this much times old. compared from the current time new Date().

staleTime tell that how much time later this data will be consider as stale old data.

so if initalDataUpdatedAt time is less than the new Date time then the data is consider as old.

```
initialDataUpdatedAt -> currentTime (new Date()) -> staleTime
 |-------------------------------|----------------------|

 initialDataUpdatedAt        ref Point       staleTime
 relm                                        relm
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<||>>>>>>>>>>>>>>>>>>>>>>|
              past           current    future
              stale          current    fresh           |          stale

```

the data is consider as fresh if data has been updated Between currentTime -> staleTime after and before that it is stale (old).

- if initialDataUpdatedAt is less than currentTime then it means that data has not been updated resently it is old stale.

- if initialDataUpdatedAt is greater than currentTime but lessThan staleTime it is fresh.
- if initialDataUpdatedAt is greater than currentTime and greaterThan staleTime. then it will getupdated after the staleTIme has passed.

we try to refrence initialDataUpdatedAt from the current time and if it is lessthan the current time then staleTime has no saying here and the data is consider as old. staleTime compare data from current time and from the current time to staleTime that is defined is consider fresh after the data is outdated.

### I Don't Know

If you would rather treat your data as prefetched data, we recommend that you use the prefetchQuery or fetchQuery APIs to populate the cache beforehand, thus letting you configure your staleTime independently from your initialData

### Initial Data Function (for large data set) (stop unnessesery re-rendering)

If the process for accessing a query's initial data is intensive or just not something you want to perform on every render, you can pass a function as the initialData value. This function will be executed only once when the query is initialized, saving you precious memory and/or CPU

```js
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: () => getExpensiveTodos(),
})
```

### Initial Data from Cache

In some circumstances, you may be able to provide the initial data for a query from the cached result of another query.

```js
const result = useQuery({
  queryKey: ['todo', todoId],
  queryFn: () => fetch('/todos'),
  initialData: () => {
    // Use a todo from the 'todos' query as the initial data for this todo query
    return queryClient.getQueryData(['todos'])?.find((d) => d.id === todoId)
  },
})
```

### I Don't Know

queryClient.getQueryData

### use dataUpdatedAt with initialDataUpdatedAt to know catch data is old or not

when you are set initalData from catch (from other query data) but you don't know that data is outofdate or not use dataUpdatedAt property with the Catchquery which will return time of how old it is and use it in initialDataUpdatedAt which will tell the queue that this data is old or not. if it is old then it will be refetched.

```js
const result = useQuery({
  queryKey: ['todos', todoId],
  queryFn: () => fetch(`/todos/${todoId}`),
  initialData: () =>
    queryClient.getQueryData(['todos'])?.find((d) => d.id === todoId),
  initialDataUpdatedAt: () =>
    queryClient.getQueryState(['todos'])?.dataUpdatedAt,
})
```

### conditional update the catch data for initalData

If the source query you're using to look up the initial data from is old, you may not want to use the cached data at all and just fetch from the server. To make this decision easier, you can use the queryClient.getQueryState method instead to get more information about the source query, including a state.dataUpdatedAt timestamp you can use to decide if the query is "fresh" enough for your needs

```js
const result = useQuery({
  queryKey: ['todo', todoId],
  queryFn: () => fetch(`/todos/${todoId}`),
  initialData: () => {
    // Get the query state
    const state = queryClient.getQueryState(['todos'])

    // If the query exists and has data that is no older than 10 seconds...
    if (state && Date.now() - state.dataUpdatedAt <= 10 * 1000) {
      // return the individual todo
      return state.data.find((d) => d.id === todoId)
    }

    // Otherwise, return undefined and let it fetch from a hard loading state!
  },
})
```

## Chapter:14 Placeholder Query Data

### What is placeholder data?

When you provide placeholderData to a query, it serves as temporary data used for rendering purposes while the actual data is being fetched in the background. The purpose of placeholder data is to provide a visual representation or simulate the behavior of real data until the actual data arrives.

However, placeholder data does not persist in the cache. It is not stored or saved as part of the query result in the cache. Instead, it is used only for initial rendering and replaced with the actual data once it is fetched.

### difference between placeholderdata vs initialData

- placerholderData don't get cached. but initalData get cached in the memory.

- placeholderData is just for show while the actual data is fetching in the background, once the actual data get fetched it is immidiatly replaced by the actualData. placeholderData has no concept of fresh or stale becouse it does not get stored in the memory.

- initalData is actual data. it will be shown till it is fresh.

### how to provide placeholder data to the queue

There are a few ways to supply placeholder data for a query to the cache before you need it:

1. Declaratively:

    Provide placeholderData to a query to prepopulate its cache if empty

2. Imperatively:

    Prefetch or fetch the data using queryClient and the placeholderData option

### Placeholder Data as a Value

```js
function Todos() {
  const result = useQuery({
    queryKey: ['todos'],
    queryFn: () => fetch('/todos'),
    placeholderData: placeholderTodos,
  })
}
```

### Placeholder Data Memoization

If the process for accessing a query's placeholder data is intensive or just not something you want to perform on every render, you can memoize the value:

```js
function Todos() {
  const placeholderData = useMemo(() => generateFakeTodos(), [])
  const result = useQuery({
    queryKey: ['todos'],
    queryFn: () => fetch('/todos'),
    placeholderData,
  })
}
```

### Placeholder Data from Cache

In some circumstances, you may be able to provide the placeholder data for a query from the cached result of another.

```js
function Todo({ blogPostId }) {
  const result = useQuery({
    queryKey: ['blogPost', blogPostId],
    queryFn: () => fetch(`/blogPosts/${blogPostId}`),
    placeholderData: () => {
      // Use the smaller/preview version of the blogPost from the 'blogPosts'
      // query as the placeholder data for this blogPost query
      return queryClient
        .getQueryData(['blogPosts'])
        ?.find((d) => d.id === blogPostId)
    },
  })
}
```

## Chapter: 15 prefetching

In React Query, the prefetchQuery function is used to proactively fetch and cache data in the background, without actually using the fetched data immediately. It allows you to fetch data ahead of time, anticipating that it will be needed in the future.

The prefetchQuery function is provided by the queryClient.

```js
// prefetch with EagerEvent
const prefetchTodos = async () => {
  // The results of this query will be cached like a normal query
  await queryClient.prefetchQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  })

}
```
```js
// later call it for real but with the same key.
const { data, isLoading, isError } = useQuery(['todos'], fetchTodos)

//call this fn with an Eager-Event and later call it by useQuery with the same key.

//prefetch will catch the data in advance and you can later call it with useQuery which first check

//the cach as use the that data.
```

The prefetchQuery function initiates the data fetching process but doesn't return a promise or provide access to the fetched data directly. Instead, React Query takes care of caching the fetched data internally, making it available for subsequent use.

Prefetching queries can be useful in scenarios where you expect a particular query to be required soon, such as preloading data for an upcoming page transition or prefetching data for an anticipated user action. By prefetching the data in advance, you can improve the perceived performance of your application by reducing the waiting time when the data is actually needed.

Note that while the prefetchQuery function fetches and caches the data in the background, it doesn't trigger any UI updates automatically. To use the prefetched data, you would typically call useQuery with the same queryKey in the component where you actually need to consume the data. React Query will then utilize the cached data if available, or fetch it if necessary.

### keypoint about prefetch caching

- If data for this query is already in the cache and not invalidated, the data will not be fetched

- If a staleTime is passed eg. prefetchQuery({queryKey: ['todos'], queryFn: fn, staleTime: 5000 }) and the data is older than the specified staleTime, the query will be fetched

- If no instances of useQuery appear for a prefetched query then the prefetched catched data become inactive and it will be deleted and garbage collected after the time specified in cacheTime.

### don't prefetch if you already have the data

if you already have the data for your query synchronously available, you don't need to prefetch it. You can just use the Query Client's setQueryData method to directly add or update a query's cached result by Querykey.

```js
queryClient.setQueryData(['todos'], todos)
```

## Chapter:15 useMutation

useMutation is used when you want to change (create,update,delete) server State.

A mutation can only be in one of the following states at any given moment:

1. `isIdle or status === 'idle'` - The mutation is currently idle or in a fresh/reset state

2. `isLoading or status === 'loading'` - The mutation is currently running

3. `isError or status === 'error'` - The mutation encountered an error

4. `isSuccess or status === 'success'` - The mutation was successful and mutation data is available

Note: useQuery don't have idle State.

Beyond those primary states, more information is available depending on the state of the mutation:

- `error` - If the mutation is in an error state, the error is available via the error property.

- `data` - If the mutation is in a success state, the data is available via the data property.

```js
const CreateTodo = () => {
  const mutation = useMutation({
    mutationFn: (formData) => {
      return fetch('/api', formData)
    },
  })
  const onSubmit = (event) => {
    event.preventDefault()
    mutation.mutate(new FormData(event.target))
  }

  return <form onSubmit={onSubmit}>...</form>
}
```

useMutate take an object and that object has mutationFn which will be used to mutate server state.
and useMutate provide (isloading, isError, isSuccess) boolean to check the state cycle and mutate variable/object which is used to provide data to the mutationFn and some additional parameter.

### Resetting Mutation State

In React Query, you can reset the state of a mutation using the reset function provided by the useMutation hook. The reset function allows you to reset the mutation state, including clearing any error or result data, and reverting the status back to its initial state.

```js
import { useMutation } from 'react-query';


function EditUserForm({ userId }) {
  const { mutate, reset, isLoading, isError, isSuccess } = useMutation(updateUser);

  const handleSubmit = (event) => {
    event.preventDefault();
    const formData = new FormData(event.target);
    const userData = Object.fromEntries(formData);

    mutate(userId, userData);
  };

  const handleReset = () => {
    reset(); // Reset the mutation state
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}
      {/* ... */}

      {/* Submit button */}
      <button type="submit" disabled={isLoading}>
        {isLoading ? 'Updating...' : 'Update User'}
      </button>

      {/* Reset button */}
      <button type="button" onClick={handleReset}>
        Reset
      </button>

      {isError && <div>Error updating user</div>}
      {isSuccess && <div>User updated successfully</div>}
    </form>
  );
}
```

The handleReset function is a custom function that calls the reset function provided by the useMutation hook. It resets the mutation state, clearing any error or result data, and resetting the status to its initial state.

The reset button in the form triggers the handleReset function when clicked, allowing the user to reset the form and clear any existing mutation state.

By using the reset function, you can programmatically reset the state of a mutation when needed, providing a way to reset form fields or clear any previous mutation results.

Note: MutateFn passed to useMutate is async becouse it has to make an API Call.

### Mutation Side Effects (side Effect functions)

useMutation comes with some helper options that allow quick and easy side-effects at any stage during the mutation lifecycle. These come in handy for both invalidating and refetching queries after mutations and even optimistic updates

```js
useMutation({
  mutationFn: addTodo,

  onMutate: (variables) => {
    // A mutation is about to happen!

    // Optionally return a context containing data to use when for example rolling back
    return { id: 1 }
  },
  onError: (error, variables, context) => {
    // An error happened!
    console.log(`rolling back optimistic update with id ${context.id}`)
  },
  onSuccess: (data, variables, context) => {
    // Boom baby!
  },
  onSettled: (data, error, variables, context) => {
    // Error or success... doesn't matter!
  },
})
```

mutations can have side effects that you can handle using the onSuccess, onError, and onSettled options provided by the useMutation hook. These options allow you to perform additional actions or update the UI based on the outcome of the mutation.

### overview each side effect option:

- `onMutate`: This option allows you to define custom logic or actions to be executed before the mutation is performed. It gives you a way to perform tasks such as updating the UI, setting loading states, or managing local state before the actual mutation request is sent to the server.

- `onError`: This option allows you to specify a callback function that will be called when the mutation encounters an error (i.e., the promise returned by the mutation is rejected). The onError callback receives the error object as its argument. You can use this callback to handle and display the error, perform error logging, or trigger any necessary error-related actions.

- `onSuccess`: This option allows you to specify a callback function that will be called when the mutation succeeds (i.e., the promise returned by the mutation resolves successfully). The onSuccess callback receives the result of the mutation as its argument. You can use this callback to update the UI, trigger additional actions, or perform any other logic needed.

- `onSettled`: This option allows you to specify a callback function that will be called when the mutation is settled, whether it succeeds or encounters an error. The onSettled callback receives the result or error object (whichever is available) as its argument. You can use this callback to perform actions that need to be executed regardless of the mutation outcome, such as clearing loading spinners or performing cleanup tasks.

Note: if you use side effect option as normal callback they will run concurrently/sideByside. but if you specify these helper fn as promises (async fn), then they will be executed in sequence as they were defined or in simple word next promise will wait for prev promise to finesh befroe starting.

```js
useMutation({
  mutationFn: addTodo,
  onSuccess: async () => {
    console.log("I'm first!")
  },
  onSettled: async () => {
    console.log("I'm second!")
  },
})
```

### component-specific side effects

You might find that you want to trigger additional callbacks beyond the ones defined on useMutation when calling mutate. This can be used to trigger component-specific side effects. To do that, you can provide any of the same callback options to the mutate function after your mutation variable/object. Supported options include: onSuccess, onError and onSettled. Please keep in mind that those additional callbacks won't run if your component unmounts before the mutation finishes.

```js
// global sideEffect
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, variables, context) => {
    // I will fire first
  },
  onError: (error, variables, context) => {
    // I will fire first
  },
  onSettled: (data, error, variables, context) => {
    // I will fire first
  },
})

// component specific sideEffect
mutate(todo, {
  onSuccess: (data, variables, context) => {
    // I will fire second!
  },
  onError: (error, variables, context) => {
    // I will fire second!
  },
  onSettled: (data, error, variables, context) => {
    // I will fire second!
  },
})
```

### difference between useMutation onError vs mutation onError

When using useMutate from React Query and providing an onError callback, the onError callback will be called for each observer that is affected by a failed mutation. Each observer will handle the error independently, and the error will not propagate to other observers.

In React Query, mutations are executed independently for each observer, and the onError callback is specific to each observer. If a mutation fails for a particular observer, only that observer's onError callback will be called, allowing you to handle the error in a customized way for that specific observer.

Other observers that are not affected by the failed mutation will not be impacted or receive the error. They will continue to operate normally without any interruption.

This behavior allows you to handle errors on a per-observer basis, providing flexibility in how you handle and display errors in different parts of your application.

### Consecutive mutations

If you define these callbacks (sideEffect helperFn) directly in the useMutation options, they will execute for each individual mutation. For example, if you have three mutations, the callbacks will be called three times, once for each mutation.

However, if you define these callbacks within the mutate function when performing consecutive mutations, they will execute only once, and that too for the latest mutation of that sequence. The callbacks won't be called separately for each mutation in the sequence.

This behavior is because React Query removes and re-subscribes the mutation observer every time the mutate function is called. So, the callbacks defined within the mutate options are associated with the latest mutation and executed accordingly.

```js
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, error, variables, context) => {
    // Will be called 3 times
  },
})

[('Todo 1', 'Todo 2', 'Todo 3')].forEach((todo) => {
  mutate(todo, {
    onSuccess: (data, error, variables, context) => {
      // Will execute only once, for the last mutation (Todo 3),
      // regardless which mutation resolves first
    },
  })
})

```

mutate will be called 3 times with varible and onsucess (sideEffect helperFn). due to the passing of variable useMutation mutationFn will be called 3 times with 3 different variable. and sideEffect helperFn defined inside useMutation mange the state for mutationFn then they will also be called 3 times. but sideEffect helperFn defined inside mutate will only be execute only once and that also only for the last variable (Todo 3)

mutation observer is removed and resubscribed every time when the mutate function is called what it means

### explain mutate observer behavious

it means that when mutate is called it is created and when mutate is called again previous mutate is removed (like cleanupFn) and new mutate is created so if we call mutate 3 time only the last mutate will be present and only his onSucess sideEffect helperFn will be executed.

Note: Be aware that most likely, mutationFn passed to useMutation is asynchronous. In that case, the order in which mutations are fulfilled may differ from the order of mutate function calls.

### mutateAysnc (Promise version)

Use mutateAsync instead of mutate to get a promise which will resolve on success or throw on an error. This can for example be used to compose side effects.

```js
const mutation = useMutation({ mutationFn: addTodo })

try {
  const todo = await mutation.mutateAsync(todo)
  console.log(todo)
} catch (error) {
  console.error(error)
} finally {
  console.log('done')
}
```

### Retry

By default TanStack Query will not retry a mutation on error, but it is possible with the retry option:

```js
const mutation = useMutation({
  mutationFn: addTodo,
  retry: 3,
})
```

If mutations fail because the device is offline, they will be retried in the same order when the device reconnects.

### serverSide rendering (Hydration) Topics

- Persist mutations
- Persisting Offline mutations

## Chapter:16 Query Invalidation

Waiting for queries to become stale before they are fetched again doesn't always work, especially when you know for a fact that a query's data is out of date because of something the user has done. For that purpose, the QueryClient has an invalidateQueries method that lets you intelligently mark queries as stale and potentially refetch them too!

invalidateQueries method mark a query as stale in the catch and it can overwrite the staleTime too. so if a query is invalidated by invalidateQueries method it will become stale even if staleTime tell to be fersh.

```js
// Invalidate every query in the cache
queryClient.invalidateQueries()
// Invalidate every query with a key that starts with `todos`
queryClient.invalidateQueries({ queryKey: ['todos'] })
```

When a query is invalidated with invalidateQueries, two things happen:

- It is marked as stale. This stale state overrides any staleTime configurations being used in useQuery or related hooks

- If the query is currently being rendered via useQuery or related hooks, it will also be refetched in the background

### Query Matching with invalidateQueries (with QueryKey)

When using APIs like invalidateQueries and removeQueries (and others that support partial query matching), you can match multiple queries by their prefix, or get really specific and match an exact query. For information on the types of filters you can use

Ex:1 We can use the todos prefix to invalidate any queries that start with todos in their query key:

```js
import { useQuery, useQueryClient } from '@tanstack/react-query'

// Get QueryClient from the context
const queryClient = useQueryClient()

queryClient.invalidateQueries({ queryKey: ['todos'] })

// Both queries below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})
const todoListQuery = useQuery({
  queryKey: ['todos', { page: 1 }],
  queryFn: fetchTodoList,
})
```

Ex:2 You can even invalidate queries with specific variables by passing a more specific query key to the invalidateQueries method:

```js
queryClient.invalidateQueries({
  queryKey: ['todos', { type: 'done' }],
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { type: 'done' }],
  queryFn: fetchTodoList,
})

// However, the following query below will NOT be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})
```

Ex:3 For exact matching use exact:true with invalidateQueries method

```js
queryClient.invalidateQueries({
  queryKey: ['todos'],
  exact: true,
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})

// However, the following query below will NOT be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { type: 'done' }],
  queryFn: fetchTodoList,
})
```

Ex:4 use predicate function with invalidateQueries method to define your own filter mathod. predicate function take entire catch object and you can acess all the queryKeys to filter out.

```js
queryClient.invalidateQueries({
  predicate: (query) =>
    query.queryKey[0] === 'todos' && query.queryKey[1]?.version >= 10,
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { version: 20 }],
  queryFn: fetchTodoList,
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { version: 10 }],
  queryFn: fetchTodoList,
})

// However, the following query below will NOT be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { version: 5 }],
  queryFn: fetchTodoList,
})
```

## Chapter:17 Invalidations from Mutations

when you mutate state of serverSide you now want to acess the updated serverDate and for that you can specify invalidateQueries method to tell which querys needed to be refetch to get the fresh data from the server that we have mutated recently.

you can use useMutate sideEffect helperFn onSuccess to define your invalidateQueries method when the mutation is sucessful.

```js
import { useMutation, useQueryClient } from '@tanstack/react-query'

const queryClient = useQueryClient()

// When this mutation succeeds, invalidate any queries with the `todos` or `reminders` query key
const mutation = useMutation({
  mutationFn: addTodo,
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] })
    queryClient.invalidateQueries({ queryKey: ['reminders'] })
  },
})
```

## Chapter:18 Updates from Mutation Responses

there is no need to re-fetch for updating data in . becouse when we mutate data by useMutation, it is common for the sever to send the newly updated data back to useMutation fn once the mutation was sucessful. you can direclty use that data and update the UI. which save some network request.

you can use Client's setQueryData method to set data to a pre-exisiting queryKey catch.

```js
const queryClient = useQueryClient()

const mutation = useMutation({
  mutationFn: editTodo,
  onSuccess: data => {
    queryClient.setQueryData(['todo', { id: 5 }], data)
  }
})

mutation.mutate({
  id: 5,
  name: 'Do the laundry',
})
```

### data object immutability problem

- either overWrite the entire data by providing the completly new object (Done in previous ex)

- or overWrite it by copiing the previous object and proving new data after that.

Don't re-assign object key with newValue it will coz problem.

```js
queryClient.setQueryData(
  ['posts', { id }],
  (oldData) => {
    if (oldData) {
      // ❌ do not try this
      oldData.title = 'my new post title'
    }
    return oldData
  })

queryClient.setQueryData(
  ['posts', { id }],
  // ✅ this is the way
  (oldData) => oldData ? {
    ...oldData,
    title: 'my new post title'
  } : oldData
)
```

## Chapter:19 Optimistic Updates

Optimistic update is a technique used in client-server interactions, where the client assumes that a mutation (such as creating, updating, or deleting data) will succeed and updates the local state immediately, without waiting for the server response. This provides an instant response to the user and improves the perceived performance of the application.

### Roleback

When you optimistically update your state before performing a mutation, there is a chance that the mutation will fail. In most of these failure cases, you can just trigger a refetch for your optimistic queries to revert them to their true server state. In some circumstances though, refetching may not work correctly and the mutation error could represent some type of server issue that won't make it possible to refetch. In this event, you can instead choose to rollback your update.

### How to Roleback

to roleback use onMutate (sideEffect helperFn) that will execute before the mutation request has been send to the server.

onMutate option which when return anyValue can be acesses by other helperFn which execute after the request has been send (onError,onSucess,onSettle) that value can be acess by context argument passed to these helperFn.

```js
const queryClient = useQueryClient()

useMutation({
  mutationFn: updateTodo,
  // When mutate is called:
  onMutate: async (newTodo) => {
    // Cancel any outgoing refetches
    // (so they don't overwrite our optimistic update)
    await queryClient.cancelQueries({ queryKey: ['todos'] })

    // Snapshot the previous value
    const previousTodos = queryClient.getQueryData(['todos'])

    // Optimistically update to the new value
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo])

    // Return a context object with the snapshotted value
    return { previousTodos }
  },
  // If the mutation fails,
  // use the context returned from onMutate to roll back
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(['todos'], context.previousTodos)
  },
  // Always refetch after error or success:
  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] })
  },
})
```

Note: You can also use the onSettled function in place of the separate onError and onSuccess handlers to both in single fn.

## Chapter: 20 Query Cancellation

### why to cancel request?

canceling the request saves network resources both for the user and for the server.

- if the network request get cancelled before it is sent the server wouldn't have to spent resources responding to it.

- and even if the server gets the request before it was canceled the client wouldn't boughter to process it when server returns the response. andup saving user CPU cycle and any work CPU doesn't have to do means improved battery life for the user.

there are two ways to cancel a query.

1. Manual Cancellation with queryClient.cancelQueries({ queryKey })

    You might want to cancel a query manually. For example, if the request takes a long time to finish, you can allow the user to click a cancel button to stop the request. To do this, you just need to call queryClient.cancelQueries({ queryKey }), which will cancel the query and revert it back to its previous state.

Note: but if you don't have data in the catch (meaning first time sending request) then query will never resolve (that promise gets terminated prematurely) and it will be in idle state. untill the query refetches.

If you have consumed the signal passed to the query function, TanStack Query will additionally also cancel the Promise.

### what it means to cancel the promise

By canceling the promise, it means that the ongoing asynchronous operation, such as a fetch request, will be forcefully interrupted and stopped. This can be done using the AbortSignal and the AbortController API, which provide a mechanism for canceling ongoing requests.

When a promise is canceled, it means that the resolution of the promise, whether successful or failed, will not be processed further. Any subsequent code or callbacks associated with the promise, such as onSuccess or onError handlers, will not be executed. Instead, the promise is terminated prematurely, and the associated query is marked as canceled.

```js
// use queryClient.cancelQueries({ queryKey }) to manually cancel a request

const query = useQuery({
  queryKey: ['todos'],
  queryFn: async ({ signal }) => {
    const resp = await fetch('/todos', { signal })
    return resp.json()
  },
})

const queryClient = useQueryClient()

return (
  <button
    onClick={(e) => {
      e.preventDefault()
      queryClient.cancelQueries({ queryKey: ['todos'] })
    }}
  >
    Cancel
  </button>
)
```

2. automatically cancel with AbortSignal

### By default behavious of the query (without AbortSignal)

By default, queries that unmount or become unused before their promises are resolved (fetching state) are not cancelled. This means that if a promise is resolved and the data is available in the cache, it will still be accessible even if the component unmounts. This behavior is helpful when you start a query but unmount the component before it completes. If you mount the component again and the query data is still in the cache, it will be available without refetching.

### how query Behave after ading AbortSignal

there are cases when AbortSignal will trigger automatically to abort the query.

1. the query get unMounted or becomes inactive. before the query resolves (before fetching get completed)

2. queryKey might change during mid request (fetching). making returned response un-useable

in these two scenerios abortSignal will be triggered.

now it's on you to tell if you want to activate this automatic abortSingal functionalility to react query by using single or you want to go with the default.

to be fair you should use abort single on every request you make. becouse it will save your network request. in both the case (unmount,keyChange).

if you consume the AbortSignal, the Promise will be cancelled (e.g. aborting the fetch when you are fetching but the query get unmounted) and therefore, also the Query must be cancelled. Cancelling the query will result in its state being reverted to its previous state.

```js
import axios from 'axios'

const query = useQuery({
  queryKey: ['todos'],
  queryFn: ({ signal }) =>
    axios.get('/todos', {
      // Pass the signal to `axios`
      signal,
    }),
})
```

## Chapter:21 Scroll Restoration

Traditionally, when you navigate to a previously visited page on a web browser, you would find that the page would be scrolled to the exact position where you were before you navigated away from that page. This is called scroll restoration.

Out of the box, "scroll restoration" for all queries (including paginated and infinite queries) Just Works™️ in TanStack Query. The reason for this is that query results are cached and able to be retrieved synchronously when a query is rendered. As long as your queries are being cached long enough (the default time is 5 minutes) and have not been garbage collected, scroll restoration will work out of the box all the time.

## Chapter:22 Filters

filters are used when you want to filterout catch data with keys to select specific keys.

there are two type filter objects for queries (QueryFilters) and for mutation (MutationFilters).

### QueryFilters

A query filter is an object with certain conditions to match a query

1. `queryKey?: QueryKey`

   Set this property to define a query key to match on.

2. `exact?: boolean`

   If you don't want to search queries inclusively by query key, you can pass the exact: true option to return only the query with the exact query key you have passed.

3. `type?: 'active' | 'inactive' | 'all'`

    Defaults to all

    When set to active it will match active queries.

    When set to inactive it will match inactive queries.

4. `stale?: boolean`

    When set to true it will match stale queries.

    When set to false it will match fresh queries.

5. `fetchStatus?: FetchStatus`

    When set to fetching it will match queries that are currently fetching.

    When set to paused it will match queries that wanted to fetch, but have been paused.

    When set to idle it will match queries that are not fetching.

6. `predicate?: (query: Query) => boolean`

    This predicate function will be used as a final filter on all matching queries. If no other filters are specified, this function will be evaluated against every query in the cache.

    ```js
    // Cancel all queries
    await queryClient.cancelQueries()

    // Remove all inactive queries that begin with `posts` in the key
    queryClient.removeQueries({ queryKey: ['posts'], type: 'inactive' })

    // Refetch all active queries
    await queryClient.refetchQueries({ type: 'active' })

    // Refetch all active queries that begin with `posts` in the key
    await queryClient.refetchQueries({ queryKey: ['posts'], type: 'active' })
    ```

### MutationFilters

A mutation filter is an object with certain conditions to match a mutation

1. `mutationKey?: MutationKey`

    Set this property to define a mutation key to match on.

2. exact?: boolean

    If you don't want to search mutations inclusively by mutation key, you can pass the exact: true option to return only the mutation with the exact mutation key you have passed.

3. `fetching?: boolean`

    When set to true it will match mutations that are currently fetching.

    When set to false it will match mutations that are not fetching.

4. `predicate?: (mutation: Mutation) => boolean`

    This predicate function will be used as a final filter on all matching mutations. If no other filters are specified, this function will be evaluated against every mutation in the cache.

## Chapter:23 How Catching behaves

This caching example illustrates the story and lifecycle of:

- Query Instances with and without cache data
- Background Refetching
- Inactive Queries
- Garbage Collection

Let's assume we are using the default cacheTime of 5 minutes and the default staleTime of 0.

1. A new instance of useQuery({ queryKey: ['todos'], queryFn: fetchTodos }) mounts.

    Since no other queries have been made with the ['todos'] query key, this query will show a hard loading state and make a network request to fetch the data.

    When the network request has completed, the returned data will be cached under the ['todos'] key.

    The hook will mark the data as stale after the configured staleTime (defaults to 0, or immediately).

2. A second instance of useQuery({ queryKey: ['todos'], queryFn: fetchTodos }) mounts elsewhere.

    Since the cache already has data for the ['todos'] key from the first query, that data is immediately returned from the cache.

    The new instance triggers a new network request using its query function.

    - Note that regardless of whether both fetchTodos query functions are identical or not, both queries' status are updated (including isFetching, isLoading, and other related values) because they have the same query key.
<BR>
<BR>

    When the request completes successfully, the cache's data under the ['todos'] key is updated with the new data, and both instances are updated with the new data.

3. Both instances of the useQuery({ queryKey: ['todos'], queryFn: fetchTodos }) query are unmounted and no longer in use.

    Since there are no more active instances of this query, a cache timeout is set using cacheTime to delete and garbage collect the query (defaults to 5 minutes).

4. Before the cache timeout has completed, another instance of useQuery({ queryKey: ['todos'], queryFn: fetchTodos }) mounts. The query immediately returns the available cached data while the fetchTodos function is being run in the background. When it completes successfully, it will populate the cache with fresh data.

5. The final instance of useQuery({ queryKey: ['todos'], queryFn: fetchTodos }) unmounts.

6. No more instances of useQuery({ queryKey: ['todos'], queryFn: fetchTodos }) appear within 5 minutes.

    The cached data under the ['todos'] key is deleted and garbage collected.

### MyObservation

```js
const instance1 = useQuery(["list"],()=> axios(URL))
const instance2 = useQuery(["list"],()=> axios(URL))
--------------------------------------------------------
// in catchStorage
catchStorge = {
  list:{
    data,
    isLoading,
    isError,
    isSucess,
    isSettle
  }
}
```

when were anyof the instance get stale new data will be fetched and updated in both the instances. there is only one variable key name that store a common catch for both of them.

## Chapter:24 Global Default QueryFn

you can define a global QueryFn as a default in queryClint and then when you call query with the same QueryKey but without any queryFn the default queryFn will be called.

if you want to overWrite Global Default QueryFn then simply provide a query with different Querykey and provide a QueryFn.

```js
//setup

// Define a default query function that will receive the query key
const defaultQueryFn = async ({ queryKey }) => {
  const { data } = await axios.get(
    `https://jsonplaceholder.typicode.com${queryKey[0]}`,
  )
  return data
}

// provide the default query function to your app with defaultOptions
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      queryFn: defaultQueryFn,
    },
  },
})

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <YourApp />
    </QueryClientProvider>
  )
}
```

```js
// Useage

// All you have to do now is pass a key!
function Posts() {
  const { status, data, error, isFetching } = useQuery({ queryKey: ['/posts'] })

  // ...
}

// You can even leave out the queryFn and just go straight into options
function Post({ postId }) {
  const { status, data, error, isFetching } = useQuery({
    queryKey: [`/posts/${postId}`],
    enabled: !!postId,
  })

  // ...
}
```

## Chapter: 25 API Reference

### 1. useQuery
```js
const {
data,
dataUpdatedAt,
error,
errorUpdateCount,
errorUpdatedAt,
failureCount,
failureReason,
fetchStatus,
isError,
isFetched,
isFetchedAfterMount,
isFetching,
isInitialLoading,
isLoading,
isLoadingError,
isPaused,
isPlaceholderData,
isPreviousData,
isRefetchError,
isRefetching,
isStale,
isSuccess,
refetch,
remove,
status
} = useQuery({
queryKey,
queryFn,
cacheTime,
enabled,
networkMode,
initialData,
initialDataUpdatedAt,
keepPreviousData,
meta,
notifyOnChangeProps,
onError,
onSettled,
onSuccess,
placeholderData,
queryKeyHashFn,
refetchInterval,
refetchIntervalInBackground,
refetchOnMount,
refetchOnReconnect,
refetchOnWindowFocus,
retry,
retryOnMount,
retryDelay,
select,
staleTime,
structuralSharing,
useErrorBoundary,
})
```

### Option object

1. `meta`:

    In React Query, the meta option allows you to attach additional metadata to a query. It provides a way to associate custom information with a query that can be accessed and utilized throughout the query lifecycle.

    The meta option can be set when defining a query using the useQuery, useInfiniteQuery, or usePaginatedQuery hooks. It takes an object as its value, where you can include any custom properties or values you want to associate with the query.

    The main purpose of the meta option is to provide a convenient way to store and access additional contextual information about a query. This can be useful in various scenarios, such as:

    Tracking or identifying specific queries: You can include identifiers or labels in the meta object to easily distinguish different queries. For example, you might include a type property to indicate the type of data being fetched.

    Storing auxiliary data: You can attach extra data to the meta object that is not part of the query result but might be relevant for the component or application logic. This can include flags, timestamps, or any other auxiliary information.

    Customizing query behavior: You can use the meta object to store configuration or customization options specific to a particular query. This allows you to extend the default behavior of React Query based on the metadata associated with the query.

    Throughout the lifecycle of the query, you can access the meta object from various places, such as the query result, the query cache, or the query functions like onSuccess, onError, and onSettled handlers. This enables you to leverage the metadata and make decisions or perform actions based on the custom information attached to the query.

2. `notifyOnChangeProps`:

    In React Query, the notifyOnChangeProps option allows you to specify an array of property names that, when changed, will trigger a re-fetch of the query. It provides a way to configure which specific props should be monitored for changes and automatically trigger a query update when those props are modified.

    By default, React Query compares the entire query key array for changes to determine when a query should be refetched. However, in some cases, you may want more granular control over which props trigger a re-fetch. This is where the notifyOnChangeProps option comes in.

    When you provide an array of property names to the notifyOnChangeProps option, React Query will only consider those specific props when checking for changes. If any of the specified props are modified, the associated query will be refetched.

    Note: property names are the properties that we get from the query

    ex: const {data,dataUpdatedAt,error,errorUpdateCount,errorUpdatedAt,failureCount,failureReason,fetchStatus,isError,isFetched,isFetchedAfterMount,isFetching,isInitialLoading,isLoading,isLoadingError,isPaused,isPlaceholderData,isPreviousData,isRefetchError,isRefetching,isStale,isSuccess,refetch,remove,status} = useQuery(queryKey,queryFn)

    all of these are properties that we can use data,error,isError etc

    Ex:

    ```js
    useQuery({
        queryKey: ['todos'],
        queryFn: fetchTodos,
        select,
        notifyOnChangeProps: ["data"],
      })
    ```

    ### some additional info from docs

    - If set, the component will only re-render if any of the listed properties change.
    
    - If set to ['data', 'error'] for example, the component will only re-render when the data or error properties change.
    
    - If set to "all", the component will opt-out of smart tracking and re-render whenever a query is updated.
    
    - By default, access to properties will be tracked, and the component will only re-render when one of the tracked properties change.

3. `queryKeyHashFn`:

    In React Query, the queryKeyHashFn option allows you to customize how the query key is hashed to generate a unique identifier for the query. The query key is an array of values that determines the identity of the query and is used to cache and retrieve query results.

    By default, React Query uses a built-in hash function to generate a unique hash for the query key. However, in certain cases, you may need more control over how the hash is computed. That's where the queryKeyHashFn option comes in.

    You can provide a custom hash function to the queryKeyHashFn option, which will be used instead of the default hash function. This function should take the query key array as its input and return a string representing the hash.

4. `refetchInterval: number | false`

    If set to a number, all queries will continuously refetch at this frequency in milliseconds

    If set to a function, the function will be executed with the latest data and query to compute a frequency

5. `refetchIntervalInBackground: boolean`

    If set to true, queries that are set to continuously refetch with a refetchInterval will continue to refetch while their tab/window is in the background

6. `retryOnMount: boolean`

    If set to false, the query will not be retried on mount if it contains an error. Defaults to true.

7. `useErrorBoundary`

    useErroBoundary is used to share the error to nearest Errorboundary set by react errorBoundary.

    `useErrorBoundary: undefined | boolean`

    - Defaults to the global query config's useErrorBoundary value, which is undefined

    - Set this to true if you want errors to be thrown in the render phase and propagate to the nearest error boundary

    - Set this to false to disable suspense's default behavior of throwing errors to the error boundary.

    Note: if you want to use onError to handle error individually then don't use useErrorBoundary but if you want to handover the Error to nearest errorBoundary which you have set to handle error then use it.

### return properties

1. `errorUpdateCount: number`

    The total number of times Error occers.

2. `failureCount: number`

    - The failure count for the query.
    
    - Incremented every time the query fails.
    
    - Reset to 0 when the query succeeds.

3. `failureReason: null | TError`

    - The failure reason for the query retry.
    
    - Reset to null when the query succeeds.

4. `isFetchedAfterMount: boolean`

    - Will be true if the query has been fetched after the component mounted.
    
    - This property can be used to not show any previously cached data.

5. `isLoadingError: boolean`

    Will be true if the query failed while fetching for the first time.

6. `isPlaceholderData: boolean`

    Will be true if the data shown is the placeholder data.

7. `isPreviousData: boolean`

    Will be true when keepPreviousData is set and data from the previous query is returned.

8. `isRefetchError: boolean`

    Will be true if the query failed while refetching.

9. `isRefetching: boolean`

    - Is true whenever a background refetch is in-flight, which does not include initial loading
    
    - Is the same as isFetching && !isLoading

10. `refetch`

    The refetch function in React Query allows you to manually trigger a refetch of the query data. It is a function that you can call to initiate a new request to fetch the latest data for the query.

    Here's an explanation of the options that can be passed to the refetch function:

    throwOnError (optional, default: false): By default, if the refetch request encounters an error, the error will be logged but not thrown. However, if you want the error to be thrown and potentially caught by an error boundary, you can pass throwOnError: true as an option.

    cancelRefetch (optional, default: true): By default, if there is already a request running for the query when refetch is called, the currently running request will be cancelled before making a new request. This ensures that only the latest request is active and avoids unnecessary network traffic. If you want to disable this behavior and allow multiple concurrent requests, you can set cancelRefetch to false.

    The refetch function returns a promise that resolves to a UseQueryResult object. This object provides information and status about the query, such as the current data, loading state, error state, and more.

    By using the refetch function, you have manual control over when to trigger a fresh request for the query data. It allows you to explicitly initiate a refetch, customize error handling behavior, and control whether ongoing requests should be cancelled before making a new request.

11. `remove: () => void`

    A function to remove the query from the cache.

### 2.useMutate

```js
const {
data,
error,
isError,
isIdle,
isLoading,
isPaused,
isSuccess,
failureCount,
failureReason,
mutate,
mutateAsync,
reset,
status,
} = useMutation({
mutationFn,
cacheTime,
mutationKey,
networkMode,
onError,
onMutate,
onSettled,
onSuccess,
retry,
retryDelay,
useErrorBoundary,
meta
})

mutate(variables, {
onError,
onSettled,
onSuccess,
})
```

### 3. QueryClint

The QueryClient can be used to interact with a cache:

```js
import { useQueryClient } from '@tanstack/react-query'

const queryClient = useQueryClient({ context })
```

you can provide the context of the queryClint if you have multiple queryClient otherwise default queryClient will be taken as context.

### methods availible to use in clintQuery

### ClintQuery (get methods)

1. `queryClient.getQueryData`

    getQueryData is a synchronous function that can be used to get an existing query's cached data by providing the queryKey. If the query does not exist, undefined will be returned.
    
    ```js
    const data = queryClient.getQueryData(queryKey)
    ```

2. `queryClient.getQueriesData`

    getQueriesData is a synchronous function that can be used to get the cached data of multiple queries. Only queries that match the passed queryKey or queryFilter will be returned. If there are no matching queries, an empty array will be returned.

    ```js
    const data = queryClient.getQueriesData(filters)
    ```

3. `queryClient.getQueryState`

    getQueryState is a synchronous function that can be used to get an existing query's state object. If the query does not exist, undefined will be returned.

    ```js
    const state = queryClient.getQueryState({ queryKey })
    console.log(state.dataUpdatedAt)
    ```

4. `queryClient.getDefaultOptions`

The getDefaultOptions method returns the default options which have been set when creating the client or with setDefaultOptions

```js
const defaultOptions = queryClient.getDefaultOptions()
```

5. `queryClient.getQueryDefaults`

The getQueryDefaults method returns the default options which have been set for specific queries:

```js
const defaultOptions = queryClient.getQueryDefaults(['posts'])
```

Note: that if several query defaults match the given query key, the first matching one is returned. This could lead to unexpected behaviours. See setQueryDefaults.

6. `queryClient.getMutationDefaults`

The getMutationDefaults method returns the default options which have been set for specific mutations:

```js
const defaultOptions = queryClient.getMutationDefaults(['addPost'])
```

7. `queryClient.getQueryCache`

The queryClient.getQueryCache method in React Query returns the reference to the underlying query cache object associated with the queryClient instance. The query cache is responsible for storing and managing the cached query data.

By accessing the query cache, you gain direct access to the underlying data structures and methods provided by React Query for managing the cached queries. This allows you to perform more advanced operations or customizations on the query cache if needed.

```js
const queryCache = queryClient.getQueryCache()
```

8. `queryClient.getMutationCache`

getMutationCache same as getQueryCatch but for mutation

```js
const mutationCache = queryClient.getMutationCache()
```
---



### ClintQuery (set methods)

1. `queryClient.setQueryData`

queryClient.setQueryData is used to set the catch for a query manually by providing the data. for setting multiple queries use queryClient.setQueriesData

```js
queryClient.setQueryData(queryKey, updater)
```
### Using an updater value

```js
setQueryData(queryKey, newData)
```
If the value is undefined, the query data is not updated.

### Using an updater function

For convenience in syntax, you can also pass an updater function which receives the current data value and returns the new one:

```js
setQueryData(queryKey, oldData => newData)
```
If the updater function returns undefined, the query data will not be updated. If the updater function receives undefined as input, you can return undefined to bail out of the update and thus not create a new cache entry.

Note: Immutability

Updates via setQueryData must be performed in an immuatable way. DO NOT attempt to write directly to the cache by mutating oldData or data that you retrieved via getQueryData in place.

Note: difference between using setQueryData and fetchQuery is that setQueryData is sync and assumes that you already synchronously have the data available. If you need to fetch the data asynchronously, it's suggested that you either refetch the query key or use fetchQuery to handle the asynchronous fetch.

2. `queryClient.setQueriesData`

setQueriesData is a synchronous function that can be used to immediately update cached data of multiple queries by using filter function or partially matching the query key. Only queries that match the passed queryKey or queryFilter will be updated - no new cache entries will be created. Under the hood, setQueryData is called for each existing query.

```js
queryClient.setQueriesData(filters, updater)
```
3. `queryClient.setDefaultOptions`

The setDefaultOptions method can be used to dynamically set the default options for this queryClient. Previously defined default options will be overwritten.

```js
queryClient.setDefaultOptions({
queries: {
staleTime: Infinity,
  },
})
```

4. `queryClient.setQueryDefaults`

setQueryDefaults can be used to set default options for specific queries:

```js
queryClient.setQueryDefaults(['posts'], { queryFn: fetchPosts })

function Component() {
  const { data } = useQuery({ queryKey: ['posts'] })
}
```

5. `queryClient.setMutationDefaults`

setMutationDefaults can be used to set default options for specific mutations:

```js
queryClient.setMutationDefaults(['addPost'], { mutationFn: addPost })

function Component() {
  const { data } = useMutation({ mutationKey: ['addPost'] })
}
```

---

### ClintQuery (fetch methods)

1. `queryClient.prefetchQuery`

    prefetchQuery is an asynchronous method that can be used to prefetch a query before it is needed or rendered with useQuery and friends. The method works the same as fetchQuery except that it will not throw or return any data.

2. `queryClient.prefetchInfiniteQuery`

    same as queryClient.prefetchQuery but for inifnityQuery

3. `queryClient.fetchQuery`

    The queryClient.fetchQuery method is used to manually fetch and retrieve data for a specific query. It allows you to trigger a network request and obtain the latest data for a query.

4. `queryClient.fetchInfiniteQuery`

    The queryClient.fetchInfiniteQuery method is used to manually fetch and retrieve data for a specific infinityQuery. It allows you to trigger a network request and obtain the latest data for a infinityQuery.

5. `queryClient.refetchQueries (conditionally)`

    The refetchQueries method can be used to refetch queries conditionally (based on certain conditions).

    ```js
    // refetch all queries:
    await queryClient.refetchQueries()

    // refetch all stale queries:
    await queryClient.refetchQueries({ stale: true })

    // refetch all active queries partially matching a query key:
    await queryClient.refetchQueries({ queryKey: ['posts'], type: 'active' })

    // refetch all active queries exactly matching a query key:
    await queryClient.refetchQueries({ queryKey: ['posts', 1], type: 'active', exact: true })
    ```

6. `queryClient.ensureQueryData`

    ensureQueryData is an asynchronous function that can be used to get an existing query's cached data. If the query does not exist, queryClient.fetchQuery will be called and its results returned.

    ```js
    const data = await queryClient.ensureQueryData({ queryKey, queryFn })
    ```
7. `queryClient.invalidateQueries`

    The invalidateQueries method can be used to invalidate and refetch single or multiple queries in the cache based on their query keys or any other functionally accessible property/state of the query. By default, all matching queries are immediately marked as invalid and active queries are refetched in the background.

    - If you do not want active queries to refetch, and simply be marked as invalid, you can use the refetchType: 'none' option.

    - If you want inactive queries to refetch as well, use the refetchType: 'all' option

    ```js
    await queryClient.invalidateQueries({
      queryKey: ['posts'],
      exact,
      refetchType: 'active',
    }, { throwOnError, cancelRefetch })
    ```

    `options?: InvalidateOptions`:

    - `throwOnError?: boolean`

    When set to true, this method will throw if any of the query refetch tasks fail.

    - `cancelRefetch?: boolean //Defaults to true`

    Per default, a currently running request will be cancelled before a new request is made
    When set to false, no refetch will be made if there is already a request running.

---

### ClintQuery (subscribe methods)

1. `queryClient.isFetching`

    This isFetching method returns an integer representing how many queries, if any, in the cache are currently fetching (including background-fetching, loading new pages, or loading more infinite query results)

    ```js
    if (queryClient.isFetching()) {
    console.log('At least one query is fetching!')
    }
    ```

    TanStack Query also exports a handy useIsFetching hook that will let you subscribe to this state in your components without creating a manual subscription to the query cache.

2. `queryClient.isMutating`

    This isMutating method returns an integer representing how many mutations, if any, in the cache are currently fetching.

    ```js
    if (queryClient.isMutating()) {
    console.log('At least one mutation is fetching!')
    }
    ```

    TanStack Query also exports a handy useIsMutating hook that will let you subscribe to this state in your components without creating a manual subscription to the mutation cache.

---

### ClintQuery (remove from catch methods)

1. `queryClient.removeQueries`

  The removeQueries method can be used to remove queries from the cache based on their query keys or any other functionally accessible property/state of the query.

  ```js
  queryClient.removeQueries({ queryKey, exact: true })
  ```
2. queryClient.clear

    The queryClient.clear method in React Query is used to clear the entire query cache, removing all cached query data. When you call queryClient.clear(), it will remove all queries from the cache, including their data and metadata.

    ```js
    queryClient.clear()
    ```
---

### ClintQuery (others methods)

1. `queryClient.cancelQueries`

    The cancelQueries method can be used to cancel outgoing queries based on their query keys or any other functionally accessible property/state of the query.

    This is most useful when performing optimistic updates since you will likely need to cancel any outgoing query refetches so they don't clobber your optimistic update when they resolve.

    ```js
    await queryClient.cancelQueries({ queryKey: ['posts'], exact: true })
    ```

2. `queryClient.resetQueries`

    The queryClient.resetQueries method in React Query is used to reset specific queries in the cache to their initial state (set to initalData). It allows you to restore the queries to their original data and trigger a refetch if necessary.

    ```js
    queryClient.resetQueries({ queryKey, exact: true })
    ```
3. `queryClient.resumePausedMutations`

    The queryClient.resumePausedMutations method in React Query is used to resume any paused mutations. Paused mutations occur when you use the pause option in the useMutation hook or when mutations are automatically paused due to the pauseOnVisibilityChange or pauseOnNetworkError global query config options.

    When you call queryClient.resumePausedMutations(), it will resume any paused mutations, allowing them to proceed and send requests to the server. Resuming mutations can be useful in scenarios where mutations were temporarily paused and need to be resumed, such as when the visibility of the page changes or when network connectivity is restored.

    ```js
    queryClient.resumePausedMutations()
    ```
### 4. useIsFetching (subscribe)

useIsFetching is an optional hook that returns the number of the queries that your application is loading or fetching in the background (useful for app-wide loading indicators).

```js
import { useIsFetching } from '@tanstack/react-query'
// How many queries are fetching in the entire app?
const isFetching = useIsFetching()
// How many queries matching the posts prefix are fetching in the entire app?
const isFetchingPosts = useIsFetching({ queryKey: ['posts'] })
```

### 5. useIsMutating

useIsMutating is an optional hook that returns the number of mutations that your application is fetching (useful for app-wide loading indicators).

```js
import { useIsMutating } from '@tanstack/react-query'
// How many mutations are fetching?
const isMutating = useIsMutating()
// How many mutations matching the posts prefix are fetching?
const isMutatingPosts = useIsMutating({ mutationKey: ['posts'] })
```

### 6. QueryCache

usually you will use queryClint to access catch object to get/set catch data. but queryCatch provide advance methods to acess catch object

```js
import { QueryCache } from '@tanstack/react-query'

const queryCache = new QueryCache({
  onError: (error) => {
    console.log(error)
  },
  onSuccess: (data) => {
    console.log(data)
  },
  onSettled: (data, error) => {
    console.log(data, error)
  },
})

const query = queryCache.find(['posts'])
```

### options avilible by QueryCatch

### Options

- `onError?: (error: unknown, query: Query) => void`

  This function will be called if some query encounters an error.

- `onSuccess?: (data: unknown, query: Query) => void`

  This function will be called if some query is successful.

- `onSettled?: (data: unknown | undefined, error: unknown | null, query: Query) => void`

  This function will be called if some query is settled (either successful or errored).

Note: all options are optional.

### methods

1. `QueryCatch.find`

queryCache.find method is used to get an existing query instance from the cache.

the queryCache.find method returns the first query that matches the criteria. If multiple queries match the criteria, only the first one will be returned. If no query matches the criteria, it returns undefined.

```js
const query = queryCache.find(queryKey)
```

2. `queryCache.findAll`

queryCache.find method is used to get an existing query instance from the cache that partially match query key. If queries do not exist, empty array will be returned.

```js
const queries = queryCache.findAll(queryKey)
```
3. `queryCache.subscribe`

The queryCache.subscribe method is used to subscribe to changes in the query cache. It allows you to register a callback function that will be called whenever there are updates to the cache, such as when queries are fetched, invalidated, or mutated.

The queryCache.subscribe method returns a function that can be used to unsubscribe from the cache updates. When called, it removes the registered callback from the subscription list.

```js
import { useQueryCache } from 'react-query';

function MyComponent() {
  const queryCache = useQueryCache();

  const handleCacheUpdate = () => {
    console.log('Cache updated');
    // Do something with the updated cache
  };

  // Subscribe to cache updates
  useEffect(() => {
    const unsubscribe = queryCache.subscribe(handleCacheUpdate);

    // Clean up the subscription when the component unmounts
    return () => {
      unsubscribe();
    };
  }, []);

  return <div>My Component</div>;
}

```

4. `queryCache.clear`

The clear method can be used to clear the cache entirely and start fresh.

```js
queryCache.clear()
```

### 7. MutationCache

The MutationCache is the storage for mutations.

Normally, you will not interact with the MutationCache directly and instead use the QueryClient.

```js
import { MutationCache } from '@tanstack/react-query'

const mutationCache = new MutationCache({
  onError: error => {
    console.log(error)
  },
  onSuccess: data => {
    console.log(data)
  },
})
```

### Options

- `onError?: (error: unknown, variables: unknown, context: unknown, mutation: Mutation)`

    This function will be called if some mutation encounters an error.

- `onSuccess?: (data: unknown, variables: unknown, context: unknown, mutation: Mutation)`

  This function will be called if some mutation is successful.

- `onSettled?: (data: unknown | undefined, error: unknown | null, variables: unknown, context: unknown, mutation: Mutation)`

  This function will be called if some mutation is settled (either successful or errored).

  If you return a Promise from it, it will be awaited

- `onMutate?: (variables: unknown, mutation: Mutation)`

  This function will be called before some mutation executes.

Note: all options are not request/optional to use

### Methods

1. mutationCache.getAll
2. mutationCache.subscribe
3. mutationCache.clear

### 6. QueryErrorResetBoundary

When you're using features like suspense or error boundaries in React Query, you may encounter situations where you want to retry a query after an error has occurred. The QueryErrorResetBoundary component provides a way to reset any query errors within its boundaries, allowing you to trigger a fresh query attempt when re-rendering.

Here's an explanation of what it means:

Let's say you have a component that uses React Query to fetch data with a query.

If an error occurs during the query, React Query will capture that error and set it as the error state of the query.

Now, when you re-render the component, React Query will see the error state and will not attempt to re-fetch the data by default. This behavior is to prevent endless error loops.

However, there might be cases where you want to retry the query after an error has occurred. For example, you might have a "Retry" button in your UI that allows the user to manually trigger the query again.

This is where the QueryErrorResetBoundary component comes in. By wrapping your component or a specific part of your application with QueryErrorResetBoundary, you create a boundary where query errors can be reset.

When an error occurs within the QueryErrorResetBoundary, such as during a query, the boundary component will catch the error and reset the error state of the queries within its boundaries.

Resetting the error state allows the affected queries to retry when re-rendering happens, either triggered by suspense or by explicitly re-rendering the component.

In summary, QueryErrorResetBoundary provides a way to control how query errors are handled within a specific boundary. It allows you to reset the error state of queries so that they can retry when re-rendering occurs, giving you more control over how errors are handled and enabling better error recovery mechanisms in your application.

### 7. useQueryErrorResetBoundary

The useQueryErrorResetBoundary is a hook provided by React Query that allows you to create a boundary where query errors can be reset within a function component. It simplifies the usage of the QueryErrorResetBoundary component by abstracting away the rendering logic.

```js
import { useQueryErrorResetBoundary } from 'react-query';

const App = () => {
  const { reset } = useQueryErrorResetBoundary();

  return (
    <ErrorBoundary
      onReset={reset}
      fallbackRender={({ resetErrorBoundary }) => (
        <div>
          There was an error!
          <Button onClick={() => resetErrorBoundary()}>Try again</Button>
        </div>
      )}
    >
      <Page />
    </ErrorBoundary>
  );
};
```

it provide a reset fn which you want pass to your ErrorBoundary so when were user retry to fetch the page by clicking on Try again. onRest will be called which will reset the Error occered in your query and refetch happen.

### 8. FocusManagmer

FocusManger is use to customize focus states.

### Methods

1. `focusManager.setEventListener`

    setEventListener can be used to set a custom event listener

2. `focusManager.setFocused`

    define focusState by booleans

    ```js
    import { focusManager } from '@tanstack/react-query'

    // Set focused
    focusManager.setFocused(true)

    // Set unfocused
    focusManager.setFocused(false)

    // Fallback to the default focus check
    focusManager.setFocused(undefined)
    ```

3. `focusManager.isFocused`

isFocused can be used to get the current focus state.

```js
const isFocused = focusManager.isFocused()
```
### 9. OnlineManager

onlineManger is use to customize online states.

### methods

1. `onlineManager.setEventListener`

    setEventListener can be used to set a custom event listener

2. `onlineManager.setOnline (boolean)`

    ```js
    import { onlineManager } from '@tanstack/react-query'

    // Set to online
    onlineManager.setOnline(true)

    // Set to offline
    onlineManager.setOnline(false)

    // Fallback to the default online check
    onlineManager.setOnline(undefined)
    ```

3. `onlineManager.isOnline`

    isOnline can be used to get the current online state.

    ```js
    const isOnline = onlineManager.isOnline()
    ```

## TK Dodo's Blog

## 1: Practical React Query

First of all: React Query does not invoke the queryFn on every re-render, even with the default staleTime of zero. Your app can re-render for various reasons at any time, so fetching every time would be insane!

### Server State vs. Client State

### Clint State

Client State is state that the frontend has full control over, is mostly synchronous and where we know the accurate value of it at all times.

- Non-presisted (data remain in the memeory of the browser,as session expires data goes away)
- Owned Completely
- Synchronously available
- always up-to-date

Because state we own completely and that is synchronously available (like, when I click that dark mode toggle button) has totally different needs than state that is persisted remotely and asynchronously available, like a list of issues.

### Server State

Server State is state that we do not own, that is mostly async and where we only see a snapshot of how the data looked like the last time we fetched it.

- presisted remotily (stored in a remote server, out-of-control from the frontend)

- shared Ownership (data can be modify/updated by the server (data base maintaner,backend developer),  
  any clint user (by adding comments,likeing the video etc)).

- Potentially Out-of-date

- Asynchronously available

- Client only sees a "Snapshot"

- Manage Async Lifycycle

With async state or "server state”, we only see a snapshot in time of when we fetched it. It can get out of date, because we are not the only owner of that state. The backend, probably our database owns it. We have just borrowed it to display that snapshot.

You might notice this when you leave a browser tab open for half an hour, and then come back to it. Wouldn't it be nice to automatically see fresh and accurate data? That means WE have to keep it up-to-date, because other users can make changes in the meantime as well. And because state is not synchronously available, meta-information around that state, like loading and error states, need to be managed as well.

So, keeping your data up-to-date automatically and managing async lifecycles isn't something you would get or need from a traditional, all-purpose state manager (redux). But since we have a tool that is geared towards async state, we can make all that happen, and more. We just need to use the right tool for the right job.

### Extra (#21: Thinking in React Query)

### When does react Query automaticlly refetch?

Condtitons required for a query to even consider to be refetch automatically.

- query needed to be active
- query needed to be stale

Note: inactive query will not fetch even if you mark them as stale untill they become active.

automatic refetch triggers by one of these reasons:

- component mount
- window focus
- regaining network connection
- QueryKey change.

whenEver one of these events occurs, React Query will refetch that query automatically.

But that's not the whole story. The thing is: React Query will not do this for all Queries - only for Queries that are considered stale (out-Dated)

Now Defining staleTime is up to you - it highly depends on your resource and your needs. There is also no "correct" value for staleTime.

If you are querying config settings that will only change when the server restarts, staleTime: Infinity can be a good choice.

On the other hand, if you have a highly collaborative tool where multiple users update things at the same time, you might be happy with staleTime: 0.

### Manual refetching

generally you want to fetch based on the automatic setting of query but some times you want to manually refetch

1. `queryClient.refetchQueries(["queryFilter"])`

it will refetch all the query that matches these queryKeys. it will refetch all type of quries fresh,stale,active,inactive.

Note: it will also refetch all the inactive queries too.

2. `queryClinent.invalidateQueries(["queryFilter"])`

`fresh >> stale >> fetching`

it will mark all the query that matches the queryKey as stale. and let the react query decide when to refetch. it will refetch fresh (which now you have marked as stale),stale,active.

Note: it will follow the rule of automatic refetching.

Note: when you invalidate a query it will first go to stale position then later it will go to fetch state.

if you want to go directly from fresh to fetching use refetch

3. `refetch()`

`fresh >> fetching`

it will refetch the query with the same params.

### filterQuery

helps us to have granuler control over which queries you want to refetch.

1. refetch all the quries present in the catch

    ```js
    queryClient.refetchQueries()
    ```

2. refetch quries that matches the query keys

    react query doesn't try to mach the exact key, it try to match any key which matches the provider key order

    ```js
    const org = "facebook";
    const repo = "react-native"

    const repoQuery = useQuery(['repo',org,repo]);
    const repoIssuesQuery = useQuery(['repo',org,repo,'issues']);
    const openIssuesQuery = useQuery(['repo',org,repo,'issues',{status:'open'}]);
    ```

    case:1 `queryClient.refretchQuries(['repo',org,repo])`

    ```js
    ✅ const repoQuery = useQuery(['repo',org,repo]);
    ✅ const repoIssuesQuery = useQuery(['repo',org,repo,'issues']);
    ✅ const openIssuesQuery = useQuery(['repo',org,repo,'issues',{status:'open'}]);
    ```

    case:2 `queryClient.refretchQuries(['repo',org,repo,'issues'])`
    
    ```js
    ❌ const repoQuery = useQuery(['repo',org,repo]);
    ✅ const repoIssuesQuery = useQuery(['repo',org,repo,'issues']);
    ✅ const openIssuesQuery = useQuery(['repo',org,repo,'issues',{status:'open'}]);
    ```

3. refetch exact queries with filter options

    case:1 `queryClient.refretchQuries(['repo',org,repo,'issues'],{exact:true})`
    
    ```js
    ❌ const repoQuery = useQuery(['repo',org,repo]);
    ✅ const repoIssuesQuery = useQuery(['repo',org,repo,'issues']);
    ❌ const openIssuesQuery = useQuery(['repo',org,repo,'issues',{status:'open'}]);
    ```

4. refetch quries with filter states options

    type of state available

    - stale: boolean (true = stale, false = fresh)
    - type: "active"
    - type: "inactive"
    - fetchStatus: fetching,paused,idle
    - type: "all" (both active and inactive)(by Default)

    case:1 `queryClient.refretchQuries({stale:true, type: "active"})`

    all the stale and active quries will be refetched.

### treat parameters of (queryFn) as dependency on (queryKeys)

if you have parameters, like the filters for QueryFn, that you want to use inside your queryFn to make a request, you have to add them to the queryKey.

This ensures a lot of things that make React Query great to work with:

- Cached Separatly
- Avoid Race Condition
- Automatic refetching
- Avoid state closure problem

For one, it makes sure that entries are cached separately depending on their input, so if we have different filters, we store them under different keys in the cache, which avoids race conditions.

It also enables automatic refetches when filters changes, because we go from one cache entry to the other. And it avoids problems with stale closures, which are usually pretty hard to debug.

It's so important that we've released our own eslint plugin. It can check if you’re using something inside the queryFn and tells you to add it to the key. It's also auto fixable, and I can highly recommend using it.

If you want, you can think about the queryKey like the dependency Array for useEffect, but without the drawbacks, because we don't have to think about referential stability.

There's no need for useMemo or useCallback to get involved here - not for thequeryFn and not for the queryKey.

## 2: React Query Data Transformations

1. In the queryFn

    The queryFn is the function that you pass to useQuery. It expects you to return a Promise, and the resulting data winds up in the query cache. But it doesn't mean that you have to absolutely return data in the structure that the backend delivers here. You can transform it before doing so:

    ```js
    // Runs on Every fetch

    const fetchTodos = async (): Promise<Todos> => {
      const response = await axios.get('todos')

    // Transform data inside QueryFn before storing in the catch
      const data: Todos = response.data
      return data.map((todo) => todo.name.toUpperCase())
    }

    export const useTodosQuery = () =>
      useQuery({ queryKey: ['todos'], queryFn: fetchTodos })
    ```

    🟢 very "close to the backend" in terms of co-location

    🟡 the transformed structure winds up in the cache, so you don't have access to the original structure

    🔴 runs on every fetch

    🔴 not feasible if you have a shared api layer that you cannot freely modify

2. In the render function

    if you create custom hooks, you can easily do transformations there:

    ```js
    // Runs on Every render

    const fetchTodos = async (): Promise<Todos> => {
      const response = await axios.get('todos')
      return response.data
    }

    export const useTodosQuery = () => {
      const queryInfo = useQuery({ queryKey: ['todos'], queryFn: fetchTodos })

      return {
        ...queryInfo,
        data: queryInfo.data?.map((todo) => todo.name.toUpperCase()),
      }
    }
    ```

    this means the transformation will run on every render, even if there are no data fetching operations involved. To optimize this, the useMemo hook can be used to memoize the transformed data and prevent unnecessary re-computations.

    ```js
    export const useTodosQuery = () => {
      const queryInfo = useQuery({ queryKey: ['todos'], queryFn: fetchTodos })

      return {
        ...queryInfo,
        // 🚨 don't do this - the useMemo does nothing at all here!
        data: React.useMemo(
          () => queryInfo.data?.map((todo) => todo.name.toUpperCase()),
          [queryInfo]
        ),

        // ✅ correctly memoizes by queryInfo.data
        data: React.useMemo(
          () => queryInfo.data?.map((todo) => todo.name.toUpperCase()),
          [queryInfo.data]
        ),
      }
    }
    ```

    By using useMemo correctly, you can optimize the rendering process, avoid unnecessary transformations, and ensure that the transformed data is memoized and updated only when needed.

    Especially if you have additional logic in your custom hook to combine with your data transformation, this is a good option. Be aware that data can be potentially undefined, so use optional chaining when working with it.

    🟢 optimizable via useMemo
    🟡 exact structure cannot be inspected in the devtools
    🔴 a bit more convoluted syntax
    🔴 data can be potentially undefined

3. using the select option

    v3 introduced built-in selectors, which can also be used to transform data:

    ```js
    export const useTodosQuery = () =>
      useQuery({
        queryKey: ['todos'],
        queryFn: fetchTodos,
        select: (data) => data.map((todo) => todo.name.toUpperCase()),
      })
    ```

    selectors will only be called if data exists, so you don't have to care about undefined here. Select fn will render on every render becouse if you define your selector function inline directly in the select option, it will be treated as a new function instance on every render. If your transformation is expensive, you can memoize it either with useCallback, or by extracting it to a stable function reference:

    ```js
    const transformTodoNames = (data: Todos) =>
      data.map((todo) => todo.name.toUpperCase())

    export const useTodosQuery = () =>
      useQuery({
        queryKey: ['todos'],
        queryFn: fetchTodos,
        // ✅ uses a stable function reference
        // this fn will not get re-created on every render
        select: transformTodoNames,
      })

    export const useTodosQuery = () =>
      useQuery({
        queryKey: ['todos'],
        queryFn: fetchTodos,
        // ✅ memoizes with useCallback
        select: React.useCallback(
          (data: Todos) => data.map((todo) => todo.name.toUpperCase()),
          []
        ),
      })
    ```

### subscribe to only parts of the data to minimize re-render

Further, the select option can also be used to subscribe to only parts of the data. This is what makes this approach truly unique.

```js
//  base query
export const useTodosQuery = (select) =>
  useQuery({ queryKey: ['todos'], queryFn: fetchTodos, select })

// Transfomer
export const useTodosCount = () => useTodosQuery((data) => data.length)
export const useTodo = (id) =>
  useTodosQuery((data) => data.find((todo) => todo.id === id))
```

we have created custom hooks useTodosQuery, useTodosCount, and useTodo using React Query. These hooks allow us to fetch and manage todo-related data from a server.

The useTodosQuery hook is the base query hook that fetches the todos data from the server. It takes an optional select parameter, which is a selector function used to select specific data from the fetched todos. If select is not provided, the hook returns the entire todos data.

The useTodosCount hook is a derived hook that uses the useTodosQuery hook internally. It selects only the length of the todos data by passing a selector function as a prop to the useTodosQuery (data) => data.length. This hook is useful when we only need the count of todos and want to avoid unnecessary re-renders if the individual todo details change.

The useTodo hook is another derived hook that uses the useTodosQuery hook internally. It selects a specific todo object based on the provided id by passing a selector function (data) => data.find((todo) => todo.id === id). This hook is helpful when we want to retrieve a single todo based on its ID.

The power of using these custom hooks with selectors is that they allow for fine-grained control over which parts of the fetched data trigger re-renders. For example, if we update the name of a todo, components using useTodosCount will not re-render because the count value remains the same. React Query optimizes the updates and only informs the necessary observers about the changes.

🟢 best optimizations
🟢 allows for partial subscriptions
🟡 structure can be different for every observer
🟡 structural sharing is performed twice

Note: structural sharing is a comparision algorithm which checks for json that you have in catch and new json that you have gotten after fetching for new data. both json objects are comared level by level (meaning each key and properties). at any level data is different then the already present data in catch that level will be replaced by the new data and if the new data at anylevel is same as previous catch data then it will not be replaced and previous data remain the same. this is a performance optiomiation.

in case of useing select Option in query. structural sharing is performed two times. one for queryFn and second one for selectFn.

## 3: React Query Render Optimizations

`isFetching transition`

when query render for the first time it get the data and store it in the catch and update the UI by cozing a render to all those component who are using that query.

but in case of background re-fetchinng (coz by stale data, dataInvalidation,refetch etc) component consuming data will be re-render twice.

### but why is that

Every time you make a background refetch, this component will re-render twice with the following query info:

```js
// inital mount one render
{ status: 'success', data: 2, isFetching: false }

// background re-fetching state chage happen two times

// first isfatching true (fetching on the fly)
{ status: 'success', data: 2, isFetching: true }
// second isfatching false (fetching completes)
{ status: 'success', data: 2, isFetching: false }
```

That is because React Query exposes a lot of meta information for each query, and isFetching is one of them. This flag will always be true when a request is in-flight. This is quite useful if you want to display a background loading indicator. But it's also kinda unnecessary if you don't do that.

as we use isfatching to show loading spinner if the render (first) does not happens then how the hell we are going to show the spinner. and render (second) will not happen then how we will stop showing the spinner.

so in refetching case: our previous select (subscribe for min-render will still happen two times)

```js
export const useTodosQuery = (select) =>
  useQuery({ queryKey: ['todos'], queryFn: fetchTodos, select })
export const useTodosCount = () => useTodosQuery((data) => data.length)

function TodosCount() {
  const todosCount = useTodosCount()

  return <div>{todosCount.data}</div>
}
```

but in tanstack V4. "By default, access to properties will be tracked, and the component will only re-render when one of the tracked properties change"

which means select useTodosCount is using data which will be tracked by default. only if the tracked properies (properties used by the component) changes then only then it will render. other meta data like isFetching will not affect the component to re-render. (already taken care by the tanStack)

if you want to explicitly set which query properties to track and tell the observer (Component) that it need to re-render due to tracked property that you have set have changed then use notifyOnChangeProps

### notifyOnChangeProps

For this use-case, React Query has the notifyOnChangeProps option. It can be set on a per-observer level to tell React Query: Please only inform this observer about changes if one of these props change. By setting this option to ['data'], we will find the optimized version we seek:

```js
export const useTodosQuery = (select, notifyOnChangeProps) =>
  useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
    select,
    notifyOnChangeProps,
  })
export const useTodosCount = () =>
  useTodosQuery((data) => data.length, ['data'])
```

Note: there is no need for using notifyOnChangeProps property to define expilict props neededs for tracking. the down side is it is easy to get out of sync of app. it's easier to make a mistake by using notifyOnChangeProps then just letting the default tracker do the job.

### how to use tracking properly

1. when you destructure properties out of any query those destructured properties are the one gettting tract for that component.

    ```js
    // 🚨 will track all fields
    const { isLoading, ...queryInfo } = useQuery(...)

    // ✅ this is totally fine
    const { isLoading, data } = useQuery(...)
    ```

2. Edge Case: Tracked queries only work "during render". If you only access fields during effects, they will not be tracked. This is quite the edge case though because of dependency arrays:

    ```js
    const queryInfo = useQuery(...)

    // 🚨 will not corectly track data
    React.useEffect(() => {
        console.log(queryInfo.data)
    })

    // ✅ fine because the dependency array is accessed during render
    React.useEffect(() => {
        console.log(queryInfo.data)
    }, [queryInfo.data])
    ```

3. Tracked queries don't reset on each render, so if you track a field once, you'll track it for the lifetime of the observer:

    ```js
    const queryInfo = useQuery(...)

    if (someCondition()) {
        // 🟡 we will track the data field if someCondition was true in any previous render cycle
        return <div>{queryInfo.data}</div>
    }
    ```

Update: Starting with v4, tracked queries are turned on per default in React Query, and you can opt out of the feature with notifyOnChangeProps: 'all'.

### let's understand how Structural sharing works under the hood.

A different, but no less important render optimization that React Query has turned on out of the box is structural sharing. This feature makes sure that we keep referential identity of our data on every level. As an example, suppose you have the following data structure:

```js
[
  { "id": 1, "name": "Learn React", "status": "active" },
  { "id": 2, "name": "Learn React Query", "status": "todo" }
]
```

Now suppose we transition our first todo into the done state, and we make a background refetch. We'll get a completely new json from our backend:

```js
[
-  { "id": 1, "name": "Learn React", "status": "active" },
+  { "id": 1, "name": "Learn React", "status": "done" },
  { "id": 2, "name": "Learn React Query", "status": "todo" }
]
```

Now React Query will attempt to compare the old state and the new and keep as much of the previous state as possible. In our example, the todos array will be new, because we updated a todo. The object with id 1 will also be new, but the object for id 2 will be the same reference as the one in the previous state - React Query will just copy it over to the new result because nothing has changed in it.

This comes in very handy when using selectors for partial subscriptions:

```js
// ✅ will only re-render if _something_ within todo with id:2 changes
// thanks to structural sharing
const { data } = useTodo(2)
```

As I've hinted before, for selectors, structural sharing will be done twice: Once on the result returned from the queryFn to determine if anything changed at all, and then once more on the result of the selector function. In some instances, especially when having very large datasets, structural sharing can be a bottleneck. It also only works on json-serializable data. If you don't need this optimization, you can turn it off by setting structuralSharing: false on any query.

## 4: Status Checks in React Query

React Query refetches quite aggressively per default, and does so without the user actively requesting a refetch. The concepts of refetchOnMount, refetchOnWindowFocus and refetchOnReconnect are great for keeping your data accurate, but they might cause a confusing ux if such an automatic background refetch fails.

### Background errors (refetching error)

Ex:1 The user opens a page, and the initial query loads successfully. They are working on the page for some time, then switch browser tabs to check emails. They come back some minutes later, and React Query will do a background refetch. Now that fetch fails.

Ex:2 Our user is on page with a list view, and they click on one item to drill down to the detail view. This works fine, so they go back to the list view. Once they go to the detail view again, they will see data from the cache. This is great - except if the background refetch fails.

In both situations, our query will be in the following state:

```js
{
  "status": "error",
  "error": { "message": "Something went wrong" },
  "data": [{ ... }]
}
```

As you can see, we will have both an error and the stale data available. This is what makes React Query great - it embraces the stale-while-revalidate caching mechanism, which means it will always give you data if it exists, even if it's stale.

during the refetch we got an error so the state and error message has been changed but we already has stale data so react query show stale data rather then showing no data.

it means React Query will cache data for you and give it to you when you need it, even if that data might not be up-to-date (stale) anymore. The principle is that stale data is better than no data, because no data usually means a loading spinner, and this will be perceived as "slow" by users. At the same time, it will try to perform a background refetch to revalidate that data.

this is one of the advantanges if query has the stale data and refetch fails then it will show stale data. which is better in my oppinion then saying no data.

Now it's up to us to decide what we display. Is it important to show the error? Is it enough to show the stale data only, if we have any? Should we show both, maybe with a little background error indicator?

There is no clear answer to this question - it depends on your exact use-case. However, given the two above examples, I think it would be a somewhat confusing user experience if data would be replaced with an error screen.

This is even more relevant when we take into account that React Query will retry failed queries three times per default with exponential backoff, so it might take a couple of seconds until the stale data is replaced with the error screen. If you also have no background fetching indicator, this can be really perplexing.

This is why I usually check for data-availability first:

```js
const todos = useTodos()

if (todos.data) {
  return <div>{todos.data.map(renderTodo)}</div>
}
if (todos.error) {
  return 'An error has occurred: ' + todos.error.message
}

return 'Loading...'
```

Again, there is no clear principle of what is right, as it is highly dependent on the use-case.

## 5: Testing React Query

### QueryClientProvider

Whenever you use React Query, you need a QueryClientProvider and give it a queryClient - a vessel which holds the QueryCache. The cache will in turn hold the data of your queries.

I prefer to give each test its own QueryClientProvider and create a new QueryClient for each test. That way, tests are completely isolated from each other. A different approach might be to clear the cache after each test, but I like to keep shared state between tests as minimal as possible. Otherwise, you might get unexpected and flaky results if you run your tests in parallel.

For custom hooks
If you are testing custom hooks, I'm quite certain you're using react-hooks-testing-library. It's the easiest thing there is to test hooks. With that library, we can wrap our hook in a wrapper, which is a React component to wrap the test component in when rendering. I think this is the perfect place to create the QueryClient, because it will be executed once per test:

```js
const createWrapper = () => {
  // ✅ creates a new QueryClient for each test
  const queryClient = new QueryClient()
  return ({ children }) => (
    <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
  )
}

test("my first test", async () => {
  const { result } = renderHook(() => useCustomHook(), {
    wrapper: createWrapper()
  })
})
```

If you want to test a Component that uses a useQuery hook, you also need to wrap that Component in QueryClientProvider. A small wrapper around render from react-testing-library seems like a good choice.

### Turn off retries

It's one of the most common "gotchas" with React Query and testing: The library defaults to three retries with exponential backoff, which means that your tests are likely to timeout if you want to test an erroneous query. The easiest way to turn retries off is, again, via the QueryClientProvider. Let's extend the above example:

```js
const createWrapper = () => {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: {
        // ✅ turns retries off
        retry: false,
      },
    },
  })

  return ({ children }) => (
    <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
  )
}

test("my first test", async () => {
  const { result } = renderHook(() => useCustomHook(), {
    wrapper: createWrapper()
  })
}
```

This will set the defaults for all queries in the component tree to "no retries".

Note: It is important to know that this will only work if your actual useQuery has no explicit retries set. If you have a query that wants 5 retries, this will still take precedence, because defaults are only taken as a fallback.

### How to overWrite useQuery methods (setQueryDefaults)

you can overWrite anyQuery methods by using setQueryDefaults.
you can only set Default to those query who's key you know.

```js
const queryClient = new QueryClient()

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  )
}

function Example() {
  // 🚨 you cannot override this setting for tests!
  const queryInfo = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
    retry: 5,
  })
}

// ✅ only todos will retry 5 times
queryClient.setQueryDefaults(['todos'], { retry: 5 })

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  )
}
```

so query with todos key will retry 5 time after overwitting it by setQueryDefaults.

### Always await the query

Since React Query is async by nature, when running the hook, you won't immediately get a result. It usually will be in loading state and without data to check. The async utilities from react-hooks-testing-library offer a lot of ways to solve this problem.

## 8: Effective React Query Keys

### Caching Data

Internally, the Query Cache is just a JavaScript object, where the keys are serialized Query Keys and the values are your Query Data plus meta information. The keys are hashed in a deterministic way, so you can use objects as well (on the top level, keys have to be strings or arrays though).

The most important part is that keys need to be unique for your queries. If React Query finds an entry for a key in the cache, it will use it. Please also be aware that you cannot use the same key for useQuery and useInfiniteQuery. There is, after all, only one Query Cache, and you would share the data between these two. That is not good because infinite queries have a fundamentally different structure than "normal" queries.

```js
useQuery({ queryKey: ['todos'], queryFn: fetchTodos })

// 🚨 this won't work
useInfiniteQuery({ queryKey: ['todos'], queryFn: fetchInfiniteTodos })

// ✅ choose something else instead
useInfiniteQuery({
  queryKey: ['infiniteTodos'],
  queryFn: fetchInfiniteTodos,
})
```

## 9: Placeholder and Initial Data in React Query

### Similarities between Placeholder and Initial Data

As already hinted, they both provide a way to pre-fill the cache with data that we have synchronously available. It further means that if either one of these is supplied, our query will not be in loading state, but will go directly to success state. Also, they can both be either a value or a function that returns a value, for those times when computing that value is expensive:

```js
function Component() {
  // ✅ status will be success even if we have not yet fetched data
  const { data, status } = useQuery({
    queryKey: ['number'],
    queryFn: fetchNumber,
    placeholderData: 23,
  })

  // ✅ same goes for initialData
  const { data, status } = useQuery({
    queryKey: ['number'],
    queryFn: fetchNumber,
    initialData: () => 42,
  })
}
```

Lastly, neither has an effect if you already have data in your cache. So what difference does it make if I use one or the other? To understand that, we have to briefly take a look at how (and on which "level") the options in React Query work:

1. On cache level
   For each Query Key, there is only one cache entry. This is kinda obvious because part of what makes React Query great is the possibility to share the same data "globally" in our application.

    Some options we provide to useQuery will affect that cache entry, prominent examples are staleTime and cacheTime. Since there is only one cache entry, those options specify when that entry is considered stale, or when it can be garbage collected.

2. On observer level
   An observer in React Query is, broadly speaking, a subscription created for one cache entry. The observer watches the cache entry for changes and will be informed every time something changes.

    The basic way to create an observer is to call useQuery. Every time we do that, we create an observer, and our component will re-render when data changes. This of course means we can have multiple observers watching the same cache entry.

By the way, you can see how many observers a query has by the number on the left of the Query Key in the React Query Devtools (3 in this example):

```js
// in stale state
3 ["repoData"]
```

Some options that work on observer level would be select or keepPreviousData. In fact, what makes select so great for data transformations is the ability to watch the same cache entry, but subscribe to different slices of its data in different components.

### Differences

InitialData works on cache level, while placeholderData works on observer level. This has a couple of implications:

### Persistence

First of all, initialData is persisted to the cache. It's one way of telling React Query: I have "good" data for my use-case already, data that is as good as if it were fetched from the backend. Because it works on cache level, there can only be one initialData, and that data will be put into the cache as soon as the cache entry is created (meaning when the first observer mounts). If you try to mount a second observer with different initialData, it won't do anything.

PlaceholderData on the other hand is never persisted to the cache. I like to see it as "fake-it-till-you-make-it" data. It's "not real". React Query gives it to you so that you can show it while the real data is being fetched. it is same as providing defaultvalue to a destructured data. placeholderData will be showen untill fetching data don't get resolve once the data is ready then placeholderdata will be replaced by the fetched data. PlaceholderData works on observer level, and theoretically you can even have different placeholderData for different components.

### Background refetches

With placeholderData, you will always get a background refetch when you mount an observer for the first time. Because the data is "not real", React Query will get the real data for you. While this is happening, you will also get an isPlaceholderData flag returned from useQuery. You can use this flag to visually hint to your users that the data they are seeing is in fact just placeholderData. It will transition back to false as soon as the real data comes in.

InitialData on the other hand, because data is seen as good and valid data that we actually put into our cache, respects staleTime. If you have a staleTime of zero (which is the default), you will still see a background refetch.

But if you've set a staleTime (e.g. 30 seconds) on your query, React Query will see the initialData and be like:

Oh, I'm getting fresh and new data here synchronously, thank you very much, now I don't need to go to the backend because this data is good for 30 seconds.

- React Query when it sees initialData and staleTime

    If that's not what you want, you can provide initialDataUpdatedAt to your query. This will tell React Query when this initialData has been created, and background refetches will be triggered, taking this into account as well. This is extremely helpful when using initialData from an existing cache entry by using the available dataUpdatedAt timestamp:

    ```js
    const useTodo = (id) => {
      const queryClient = useQueryClient()

      return useQuery({
        queryKey: ['todo', id],
        queryFn: () => fetchTodo(id),
        staleTime: 30 * 1000,
        initialData: () =>
          queryClient
            .getQueryData(['todo', 'list'])
            ?.find((todo) => todo.id === id),
        initialDataUpdatedAt: () =>
          // ✅ will refetch in the background if our list query data is older
          // than the provided staleTime (30 seconds)
          queryClient.getQueryState(['todo', 'list'])?.dataUpdatedAt,
      })
    }
    ```

### Error transitions

Suppose you provide initialData or placeholderData, and a background refetch is triggered, which then fails. What do you think will happen in each situation? I've hidden the answers so that you can try to come up with them for yourselves if you want before expanding them.

- InitialData
  Since initialData is persisted in the cache, the refetch error is treated like any other background error. Our query will be in error state, but your data will still be there.

- PlaceholderData
  Since placeholderData is "fake-it-till-you-make-it" data, and we didn't make it, we won't see that data anymore. Our query will be in error state, and our data will be undefined.

### When to use what

As always, that is totally up to you. I personally like to use initialData when pre-filling a query from another query, and I use placeholderData for everything else.

## 11: React Query Error Handling

### Prerequisites

React Query needs a rejected Promise in order to handle errors correctly. Luckily, this is exactly what you'll get when you work with libraries like axios.

If you are working with the fetch API or other libraries that do not give you a rejected Promise on erroneous status codes like 4xx or 5xx, you'll have to do the transformation yourself in the queryFn.

### The standard Error handling

```js
function TodoList() {
  const todos = useQuery({ queryKey: ['todos'], queryFn: fetchTodos })

  if (todos.isLoading) {
    return 'Loading...'
  }

  // ✅ standard error handling
  // could also check for: todos.status === 'error'
  if (todos.isError) {
    return 'An error occurred'
  }

  return (
    <div>
      {todos.data.map((todo) => (
        <Todo key={todo.id} {...todo} />
      ))}
    </div>
  )
}
```

Here, we're handling error situations by checking for the isError boolean flag (which is derived from the status enum) given to us by React Query.

This is certainly okay for some scenarios, but has a couple of drawbacks, too:

- bad for UI: Would we really want to remove the entire page (UI) when error occers and show them just an error message.

- It doesn't handle background errors coz be background refetching very well: Would we really want to unmount our complete Todo List just because a background refetch failed? Maybe the api is temporarily down, or we reached a rate limit, in which case it might work again in a few minutes. You can have a look at #4: Status Checks in React Query to find out how to improve that situation.

- It can become quite boilerplate-y if you have to do this in every component that wants to use a query.

To solve the second issue, we can use a great feature provided directly by React called Error Boundaries

### Error Boundaries

Error Boundaries are a general concept in React to catch runtime errors that happen during rendering, which allows us to react (pun intended) properly to them and display a fallback UI instead.

This is nice because we can wrap our components in Error Boundaries at any granularity we want, so that the rest of the UI will be unaffected by that error.

One thing that Error Boundaries cannot do is catch asynchronous errors, because those do not occur during rendering. So to make Error Boundaries work in React Query, the library internally catches the error for you and re-throws it in the next render cycle so that the Error Boundary can pick it up.

I think this is a pretty genius yet simple approach to error handling, and all you need to do to make that work is pass the useErrorBoundary flag to your query (or provide it via a default config):

```js
function TodoList() {
  // ✅ will propagate all fetching errors to the nearest Error Boundary
  const todos = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
    useErrorBoundary: true,
  })

  if (todos.data) {
    return (
      <div>
        {todos.data.map((todo) => (
          <Todo key={todo.id} {...todo} />
        ))}
      </div>
    )
  }

  return 'Loading...'
}
```

### filter Error for ErrorBounadry

Starting with v3.23.0, you can even customize which errors should go towards an Error Boundary, and which ones you'd rather handle locally by providing a function to useErrorBoundary:

```js
useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
  // 🚀 only server errors will go to the Error Boundary
  useErrorBoundary: (error) => error.response?.status >= 500,
})
```

This also works for mutations, and is quite helpful for when you're doing form submissions. Errors in the 4xx range can be handled locally (e.g. if some backend validation failed), while all 5xx server errors can be propagated to the Error Boundary.

### Showing error notifications

For some use-cases, it might be better to show error toast notifications that pop up somewhere (and disappear automatically) instead of rendering Alert banners on the screen. These are usually opened with an imperative api, like the one offered by react-hot-toast:

```
const useTodos = () =>
  useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
    // ⚠️ looks good, but is maybe _not_ what you want
    onError: (error) =>
      toast.error(`Something went wrong: ${error.message}`),
  })
```

At first glance, it looks like the onError callback is exactly what we need to perform a side effect if a fetch fails, and it will also work - for as long as we only use the custom hook once!

You see, the onError callback on useQuery is called for every Observer, which means if you call useTodos twice in your application, you will get two error toasts, even though only one network request fails.

if we don't really want to notify all Observers that our fetch failed, but just notify the user once that the underlying fetch failed? For that, React Query has callbacks on Global level:

### The global callbacks

The global callbacks need to be provided when you create the QueryCache, which happens implicitly when you create a new QueryClient, but you can also customize that:

```
const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error) =>
      toast.error(`Something went wrong: ${error.message}`),
  }),
})
```

This will now only show an error toast once for each query, which exactly what we want.🥳 It is also likely the best place to put any sort of error tracking or monitoring that you want to perform, because it's guaranteed to run only once per request and cannot be overwritten like e.g. the defaultOptions.

### Putting it all together

The three main ways to handle errors in React Query are:

- the error property returned from useQuery
- the onError callback (on the query itself or the global QueryCache / MutationCache)
- using Error Boundaries

You can mix and match them however you want, and what I personally like to do is show error toasts for background refetches (to keep the stale UI intact) and handle everything else locally or with Error Boundaries:

```
const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error, query) => {
      // 🎉 only show error toasts if we already have data in the cache
      // which indicates a failed background update
      if (query.state.data !== undefined) {
        toast.error(`Something went wrong: ${error.message}`)
      }
    },
  }),
})
```

## 12: Mastering Mutations in React Query

We've covered a lot of ground already when it comes to the features and concepts React Query provides. Most of them are about retrieving data - via the useQuery hook. There is however a second, integral part to working with data: updating it.

For this use-case, React Query offers the useMutation hook.

### What are mutations?

Generally speaking, mutations are functions that have a side effect. As an example, have a look at the push method of Arrays: It has the side effect of changing the array (Orginal) in place where you're pushing a value to:

```js
const myArray = [1]
myArray.push(2)

console.log(myArray) // [1, 2]
```

The immutable counterpart would be concat, which can also add values to an array (New), but it will return a new Array instead of directly manipulating the Array you operate on:

```js
const myArray = [1]
const newArray = myArray.concat(2)

console.log(myArray) //  [1]
console.log(newArray) // [1, 2]
```

As the name indicates, useMutation also has some sort of side effect. Since we are in the context of managing server state with React Query, mutations describe a function that performs such a side effect on the server. Creating a todo in your database would be a mutation.

In some aspects, useMutation is very similar to useQuery. In others, it is quite different.

### Similarities between useQuery and useMutation

useMutation will track the state of a mutation, just like useQuery does for queries. It'll give you loading, error and status fields to make it easy for you to display what's going on to your users.

You'll also get the same nice callbacks that useQuery has: onSuccess, onError and onSettled. But that's about where the similarities end.

### Differences to useQuery

`useQuery is declarative, useMutation is imperative`.

By that, I mean that queries mostly run automatically. You define the dependencies, but React Query takes care of running the query immediately, and then also performs smart background updates when deemed necessary. That works great for queries because we want to keep what we see on the screen in sync with the actual data on the backend.

For mutations, that wouldn't work well. Imagine a new todo would be created every time you focus your browser window 🤨. So instead of running the mutation instantly, React Query gives you a function that you can invoke whenever you want to make the mutation:

```js
function AddComment({ id }) {
  // this doesn't really do anything yet
  const addComment = useMutation({
    mutationFn: (newComment) =>
      axios.post(`/posts/${id}/comments`, newComment),
  })

  return (
    <form
      onSubmit={(event) => {
        event.preventDefault()
        // ✅ mutation is invoked when the form is submitted
        addComment.mutate(new FormData(event.currentTarget).get('comment'))
      }}
    >
      <textarea name="comment" />
      <button type="submit">Comment</button>
    </form>
  )
}
```

Another difference is that mutations don't share state like useQuery does. You can invoke the same useQuery call multiple times in different components and will get the same, cached result returned to you - but this won't work for mutations.

### My Observation

- useMutation don't get background fetching.
- useMutation also don't get cached.

### how to update the queue after mutation sucessed

Mutations are, per design, not directly coupled to queries. A mutation that likes a blog post has no ties towards the query that fetches that blog post. For that to work, you would need some sort of underlying schema, which React Query doesn't have.

To have a mutation reflect the changes it made on our queries, React Query primarily offers two ways:

1. Invalidation

    This is conceptually the simplest way to get your screen up-to-date. Remember, with server state, you're only ever displaying a snapshot of data from a given point in time. React Query tries to keep that up-to-date of course, but if you're deliberately changing server state with a mutation, this is a great point in time to tell React Query that some data you have cached is now "invalid". React Query will then go and refetch that data if it's currently in use, and your screen will update automatically for you once the fetch is completed. The only thing you have to tell the library is which queries you want to invalidate:

    ```js
    const useAddComment = (id) => {
      const queryClient = useQueryClient()

      return useMutation({
        mutationFn: (newComment) =>
          axios.post(`/posts/${id}/comments`, newComment),
        onSuccess: () => {
          // ✅ refetch the comments list for our blog post
          queryClient.invalidateQueries({ queryKey: ['posts', id, 'comments'] })
        },
      })
    }
    ```

    Query invalidation is pretty smart. Like all Query Filters, it uses fuzzy matching on the query key. So if you have multiple keys for your comments list, they will all be invalidated. However, only the ones that are currently active will be refetched. The rest will be marked as stale, which will cause them to be refetched the next time they are used.

    As an example, let's assume we have the option to sort our comments, and at the time the new comment was added, we have two queries with comments in our cache:

    ```js
    ['posts', 5, 'comments', { sortBy: ['date', 'asc'] }]
    ['posts', 5, 'comments', { sortBy: ['author', 'desc'] }]
    ```

    Since we're only displaying one of them on the screen, invalidateQueries will refetch that one and mark the other one as stale.

2. Direct updates (from mutation)

    Sometimes, you don't want to refetch data, especially if the mutation already returns everything you need to know. If you have a mutation that updates the title of your blog post, and the backend returns the complete blog post as a response, you can update the query cache directly via setQueryData:

    ```js
    const useUpdateTitle = (id) => {
      const queryClient = useQueryClient()

      return useMutation({
        mutationFn: (newTitle) =>
          axios
            .patch(`/posts/${id}`, { title: newTitle })
            .then((response) => response.data),
        // 💡 response of the mutation is passed to onSuccess
        onSuccess: (newPost) => {
          // ✅ update detail view directly
          queryClient.setQueryData(['posts', id], newPost)
        },
      })
    }
    ```

    Putting data into the cache directly via setQueryData will act as if this data was returned from the backend, which means that all components using that query will re-render accordingly.

    I personally think that most of the time, invalidation should be preferred. Of course, it depends on the use-case, but for direct updates to work reliably, you need more code on the frontend, and to some extent duplicate logic from the backend. Sorted lists are for example pretty hard to update directly, as the position of my entry could've potentially changed because of the update. Invalidating the whole list is the "safer" approach.

### Optimistic updates

Optimistic updates are one of the key selling points for using React Query mutations. The useQuery cache give us data instantly when switching between queries, especially when combined with prefetching. Our whole UI feels very snappy because of it, so why not get the same advantage for mutations as well?

A lot of the time, we are quite certain that an update will go through. Why should the user wait for a couple of seconds until we get the okay from the backend to show the result in the UI? The idea of optimistic updates is to fake the success of a mutation even before we have sent it to the server. Once we get a successful response back, all we have to do is invalidate our view again to see the real data. In case the request fails, we're going to roll back our UI to the state from before the mutation.

This works great for small mutations where instant user feedback is actually required. There is nothing worse than having a toggle button that performs a request, and it doesn't react at all until the request has completed. Users will double or even triple click that button, and it will just feel "laggy" all over the place.

### when not to use Optimistic updates

I further think that optimistic updates are a bit over-used. Not every mutation needs to be done optimistically. You should really be sure that it rarely fails, because the UX for a rollback is not great. Imagine a Form in a Dialog that closes when you submit it, or a redirect from a detail view to a list view after an update. If those are done prematurely, they are hard to undo.

Also, be sure that the instant feedback is really required (like in the toggle button example above). The code needed to make optimistic updates work is non-trivial, especially compared to "standard" mutations. You need to mimic what the backend is doing when you're faking the result, which can be as easy as flipping a Boolean or adding an item to an Array, but it might also get more complex really fast:

- If the todo you're adding needs an id, where do you get it from?
- If the list you're currently viewing is sorted, will you insert the new entry at the right position?
- What if another user has added something else in the meantime - will our optimistically added entry switch positions after a refetch?

All these edge cases might make the UX actually worse in some situations, where it might be enough to disable the button and show a loading animation while the mutation is in-flight. As always, choose the right tool for the right job.

### Common mistakes people do while using useMutation

`awaited Promises`

Promises returned from the mutation callbacks are awaited by React Query, and as it so happens, invalidateQueries returns a Promise. If you want your mutation to stay in loading state while your related queries update, you have to return the result of invalidateQueries from the callback:

```js
{
  // 🎉 will wait for query invalidation to finish
  onSuccess: () => {
    return queryClient.invalidateQueries({
      queryKey: ['posts', id, 'comments'],
    })
  }
}
{
  // 🚀 fire and forget - will not wait
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['posts', id, 'comments'] })
  }
}
```

Returning the invalidateQueries function from the onSuccess callback is important if you want the mutation to wait for the queries to be invalidated and stay in the loading state until the updates are completed. This ensures that the mutation and the related queries are synchronized in terms of their state transitions.

### difference between Mutate or MutateAsync

useMutation gives you two functions - mutate and mutateAsync. What's the difference, and when should you use which one?

mutate doesn't return anything, while mutateAsync returns a Promise containing the result of the mutation. So you might be tempted to use mutateAsync when you need access to the mutation response, but I would still argue that you should almost always use mutate.

You can still get access to the data or the error via the callbacks, and you don't have to worry about error handling: Since mutateAsync gives you control over the Promise, you also have to catch errors manually, or you might get an unhandled promise rejection.

```js
const onSubmit = () => {
  // ✅ accessing the response via onSuccess
  myMutation.mutate(someData, {
    onSuccess: (data) => history.push(data.url),
  })
}

const onSubmit = async () => {
  // 🚨 works, but is missing error handling
  const data = await myMutation.mutateAsync(someData)
  history.push(data.url)
}

const onSubmit = async () => {
  // 😕 this is okay, but look at the verbosity
  try {
    const data = await myMutation.mutateAsync(someData)
    history.push(data.url)
  } catch (error) {
    // do nothing
  }
}
```

Handling errors is not necessary with mutate, because React Query catches (and discards) the error for you internally. It is literally implemented with: mutateAsync().catch(noop)😎

The only situations where I've found mutateAsync to be superior is when you really need the Promise for the sake of having a Promise. This can be necessary if you want to fire off multiple mutations concurrently and want to wait for them all to be finished, or if you have dependent mutations where you'd get into callback hell with the callbacks.

### Mutations only take one argument for variables

Since the last argument to mutate is the options object, useMutation can currently only take one argument for variables. This is certainly a limitation, but it can be easily worked around by using an object:

```js
// 🚨 this is invalid syntax and will NOT work
const mutation = useMutation({
  mutationFn: (title, body) => updateTodo(title, body),
})
mutation.mutate('hello', 'world')

// ✅ use an object for multiple variables
const mutation = useMutation({
  mutationFn: ({ title, body }) => updateTodo(title, body),
})
mutation.mutate({ title: 'hello', body: 'world' })
```

### Some callbacks might not fire

You can have callbacks on useMutation as well as on mutate itself. It is important to know that the callbacks on useMutation fire before the callbacks on mutate. Further, the callbacks on mutate might not fire at all if the component unmounts before the mutation has finished.

That's why I think it's a good practice to separate concerns in your callbacks:

- Do things that are absolutely necessary and logic related (like query invalidation) in the useMutation callbacks.
- Do UI related things like redirects or showing toast notifications in mutate callbacks. If the user navigated away from the current screen before the mutation finished, those will purposefully not fire.

This separation is especially neat if useMutation comes from a custom hook, as this will keep query related logic in the custom hook while UI related actions are still in the UI. This also makes the custom hook more reusable, because how you interact with the UI might vary on a case by case basis - but the invalidation logic will likely always be the same:

```js
const useUpdateTodo = () =>
  useMutation({
    mutationFn: updateTodo,
    // ✅ always invalidate the todo list
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['todos', 'list'] })
    },
  })

// in the component

const updateTodo = useUpdateTodo()
updateTodo.mutate(
  { title: 'newTitle' },
  // ✅ only redirect if we're still on the detail page
  // when the mutation finishes
  { onSuccess: () => history.push('/todos') }
)
```

## 13: Offline React Query

### offlineFirst

This mode is very similar to how React Query worked in v3. The first request will always be made, and if that fails, retries will be paused. This mode is useful if you're using an additional caching layer like the browser cache on top of React Query.

Let's take the GitHub repo API as an example. It sends the following response headers:

```js
cache-control: public, max-age=60, s-maxage=60
```

which means that for the next 60 seconds, if you request that resource again, the response will come from the browser cache. The neat thing about this is that it works while you're offline, too! Service workers, e.g. for offline first PWAs, work in a similar way by intercepting the network request and delivering cached responses if they are available.

Now those things wouldn't work if React Query would decide to not fire the request because you have no network connection, like the default online mode does. To intercept a fetch request, it must happen :) So if you have this additional cache layer, make sure to use newtorkMode: 'offlineFirst'.

If the first request goes out, and you hit the cache - great, your query will go to success state, and you'll get that data. And if you have a cache miss, you'll likely get a network error, after which React Query will pause the retries, which will put your query into the paused state. It's the best of both worlds. 🙌

## 15: React Query FAQs

### How can I pass parameters to refetch?

The short answer is still: you cannot. But there's a very good reason for that. Every time you think that's what you want, you usually don't.

Mostly, code that wants to refetch with parameters looks something like this:

```js
const { data, refetch } = useQuery({
  queryKey: ['item'],
  queryFn: () => fetchItem({ id: 1 }),
})

<button onClick={() => {
  // 🚨 this is not how it works
  refetch({ id: 2 })
}}>Show Item 2</button>
```

Parameters or Variables are dependencies to your query. In the above code, we define a QueryKey ['item'], so whatever we fetch will be stored under that key. If we were to refetch with a different id, it would still write to the same place in the cache, because the key stays the same. So id 2 would then overwrite data for id 1. If you were to switch back to id 1, that data would be gone.

Caching different responses under different query keys is one of React Query's greatest strengths. The hypothetical "refetch-with-parameters" api would take that feature away. This is why refetch is only meant to replay the request with the same variables. So in essence, you don't really want a refetch: You want a new fetch for a different id!

To use React Query effectively, you have to embrace the declarative approach: The query key defines all dependencies that the query function needs to fetch data. If you stick to that, all you have to do to get refetches is to update the dependency. A more realistic example would look like this:

```js
const [id, setId] = useState(1)

const { data } = useQuery({
  queryKey: ['item', id],
  queryFn: () => fetchItem({ id }),
})

<button onClick={() => {
  // ✅ set id without explicitly refetching
  setId(2)
}}>Show Item 2</button>
```

setId will re-render the component, React Query will pick up the new key and start fetching for that key. It will also cache it separately from id 1.

The declarative approach also makes sure that no matter where or how you update the id, your query data will always be "in sync" with it. So your thinking goes from: "If I click that button, I want to refetch" towards: "I always want to see data for the current id".

The best part about this approach is that you don't have to manage state, that you get sharable urls and that the browser back button will also just work for your users to navigate between items.

### My Observation

- only use refetch if you want to refetch with the same parameter as defined in the queryFn.

- thhe reson you don't change parameter in refetch is that it will overWrite QueryFn but store at the same QueryKey point in the catch. so queryKey remain same but data has been changed now you can not access prev-QueryFn data.

- better to use the parameter as depenendcy in (queryKey) and also pass it in QueryFn and create a state for that parameter. when state change it will create a new QueryKey and also new QueryFn. so newQueryKey will store the newQueryFn data and now you can access both prev and new data in the catch.

### i don't wan to show loading state?

You might notice that switching query keys will put your query into hard loading state again. That is expected, because we change keys and there is no data for that key yet.

There are a bunch of ways to ease the transition, like setting a placeholderData for that key or prefetching data for the new key ahead of time. A nice approach to tackle this problem is to instruct the query to keep previous data:

```js
const { data, isPreviousData } = useQuery({
  queryKey: ['item', id],
  queryFn: () => fetchItem({ id }),
  // ⬇️ like this️
  keepPreviousData: true
})
```

With this flag on, React Query will still show data for id 1 while data for id 2 is being fetched. Additionally, the isPreviousData flag on the query result will be set to true, so that you can act accordingly in the UI. Maybe you want to show a background loading spinner in addition to the data, or you'd like to add opacity to the shown data, indicating that it's stale. That is totally up to you - React Query just gives you the means to do that. 🙌

### Why are updates not shown?

When interacting with the Query Cache directly, be that because you want to perform an update from a mutation response or because you want to invalidate from mutations, I sometimes get reports that the updates are not reflected on the screen, or that it simply "doesn't work". If that's the case, it mostly boils down to one of two issues:

1. Query Keys are not matching

    Query Keys are hashed deterministically, so you don't have to keep referential stability or object key order in mind. However, when you call queryClient.setQueryData, the key must still match the existing key fully. As an example, those two keys do not match:

    ```js
    ['item', '1']
    ['item', 1]
    ```

    The second value of the key array is a string in the first example and a number in the second. This can happen if you usually work with numbers, but get a string if you read from the URL with useParams.

    The React Query Devtools are your best friend in this case, as you can clearly see which keys exist and which keys are currently fetching. Keep an eye on those pesky details though!

    I recommend using TypeScript and Query Key Factories to help with that problem.

2. The QueryClient is not stable

    In most examples, we create the queryClient outside the App component, which makes it referentially stable:

    ```js
    // ✅ created outside of the App
    const queryClient = new QueryClient()

    export default function App() {
      return (
        <QueryClientProvider client={queryClient}>
          <Example />
        </QueryClientProvider>
      )
    }
    ```

    The QueryClient holds the QueryCache, so if you create a new client, you also get a new cache, which will be empty. If you move the client creation into the App component, and your component re-renders for some other reason (e.g. a route change), your cache will be thrown away:

    ```js
    export default function App() {
      // 🚨 this is not good
      const queryClient = new QueryClient()

      return (
        <QueryClientProvider client={queryClient}>
          <Example />
        </QueryClientProvider>
      )
    }
    ```

    If you have to create your client inside the App, make sure that it is referentially stable by using an instance ref or React state:

    ```js
    export default function App() {
      // ✅ this is stable
      const [queryClient] = React.useState(() => new QueryClient())

      return (
        <QueryClientProvider client={queryClient}>
          <Example />
        </QueryClientProvider>
      )
    }
    ```

### Why should I useQueryClient()? if I can just as well import the client?

The QueryClientProvider puts the created queryClient into React Context to distribute it throughout your app. You can best read it with useQueryClient. This does not create any extra subscriptions and will not cause any additional re-renders (if the client is stable - see above) - it just avoids having to pass the client down as a prop.

Alternatively, you could export the client and just import it wherever you need to:

```js
// ⬇️ exported so that we can import it
export const queryClient = new QueryClient()

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  )
}
```

Here are a couple of reasons why using the hook is preferred:

1. useQuery uses the hook too

    When you call useQuery, we call useQueryClient under the hood. This will look up the nearest client in React Context. Not a big deal, but if you ever get into the situation where the client you import is different from the one in context you'll have a hard to trace bug that could be avoided.

2. It decouples your app from the client

    The client you define in your App is your production client. It might have some default settings that work well in production. However, in testing, it might make sense to use different default values. One example is turning off retries during testing, because testing erroneous queries might time out the test otherwise.

    A big advantage of React Context when used as a dependency injection mechanism is that it decouples your app from its dependencies. useQueryClient just expects any client to be in the tree above - not a specific client. You'll lose that advantage if you import the production client directly.

3. You sometimes can't export

    It is sometimes necessary to create the queryClient inside the App component (as shown above). for example if you want to use other hooks in the default values of the queryClient, you also need to create it inside the App. Consider a global error handler that wants to show a toast for every failing mutation:

    ```js
    export default function App() {
      // ✅ we couldn't useToast outside of the App
      const toast = useToast()
      const [queryClient] = React.useState(
        () =>
          new QueryClient({
            mutationCache: new MutationCache({
              // ⬇️ but we need it here
              onError: (error) => toast.show({ type: 'error', error }),
            }),
          })
      )

      return (
        <QueryClientProvider client={queryClient}>
          <Example />
        </QueryClientProvider>
      )
    }
    ```

    So if you create your queryClient like that, there is no way that you can just export it and import it in your App.

### Why do I not get errors ?

If your network request fails, you'd ideally want your query to go to the error state. If that doesn't happen, and you still see a successful query instead, that means that your queryFn did not return a failed Promise.

Remember: React Query doesn't know (or care) about status codes or network requests at all. It needs a resolved or rejected Promise that the queryFn needs to provide.

If React Query sees a rejected Promise, it can potentially start retries, pause queries if you are offline and eventually put the query into the error state, so it's quite an important thing to get right.

1. The fetch API

    Luckily, many data fetching libraries like axios or ky transform erroneous status codes like 4xx or 5xx into failed Promises, so if your network request fails, your query fails too. The notable exception is the built-in fetch API, which will only give you a failed Promise if the request failed due to a network error. it doesn't give failed promise for response fail.

2. Logging

    The second reason I've seen a lot is that errors are caught inside the queryFn for logging purposes. If you do that without re-throwing the error, you will again return a successful Promise implicitly:

    ```js
    useQuery({
      queryKey: ['todos', todoId],
      queryFn: async () => {
        try {
          const { data } = await axios.get('/todos/' + todoId)
          return data
        } catch (error) {
          console.error(error)
          // 🚨 here, an "empty" Promise<void> is returned
        }
      },
    })
    ```

    If you want to do this, remember to re-throw the error:

    ```js
    useQuery({
      queryKey: ['todos', todoId],
      queryFn: async () => {
        try {
          const { data } = await axios.get('/todos/' + todoId)
          return data
        } catch (error) {
          console.error(error)
          // ✅ here, a failed Promise is returned
          throw error
        }
      },
    })
    ```

    if you still want to handle errors then use the onError callback of useQuery:

    ```js
    useQuery({
      queryKey: ['todos', todoId],
      queryFn: async () => {
        const { data } = await axios.get('/todos/' + todoId)
        return data
      },
      onError: (error) => console.error(error)
    })
    ```

## 17: Seeding the Query Cache

this section talks about how to show data get fetched from the server.

### prefetch

The earlier you initiate a fetch, the better, because the sooner it starts, the sooner it can finish. 🤓

- If your architecture supports server side rendering - consider fetching on the server.
- If you have a router that supports loaders, consider prefetching there.

But even if that's not the case, you can still use prefetchQuery to initiate a fetch before the component is rendered:

```js
const issuesQuery = { queryKey: ['issues'], queryFn: fetchIssues }

// ⬇️ initiate a fetch before the component renders
queryClient.prefetchQuery(issuesQuery)

function Issues() {
  const issues = useQuery(issuesQuery)
}
```

The call to prefetchQuery is executed as soon as your JavaScript bundle is evaluated. This works very well if you do route base code splitting, because it means the code for a certain page will be lazily loaded and evaluated as soon as the user navigates to that page. This means it will still be kicked off before the component renders.

### Seeding details from lists

Another nice way to make sure that your cache is filled by the time it is read is to seed it from other parts of the cache. Oftentimes, if you render a detail view of an item, you will have data for that item readily available if you've previously been on a list view that shows a list of items.

There are two common approaches to fill a detail cache with data from a list cache:

#### `Pull approach (with initialData)`

use initialData to provide data from already fetched data (list). pulling out data from other source to provide data to another. if a query have a initalData option then it will look at the initalData option before executing the queryFn.

```js
const useTodo = (id: number) => {
  const queryClient = useQueryClient()
  return useQuery({
    queryKey: ['todos', 'detail', id],
    queryFn: () => fetchTodo(id),
    initialData: () => {
      // ⬇️ look up the list cache for the item
      return queryClient
        .getQueryData(['todos', 'list'])
        ?.find((todo) => todo.id === id)
    },
  })
}
```

If the initialData function returns undefined, the query will proceed as normal and fetch the data from the server. And if something is found, it will be put into the cache directly.

Be advised that if you have staleTime set, no further background refetch will occur, as initialData is seen as fresh. This might not be what you want if your list was last fetched twenty minutes ago.

As shown in the docs, we can additionally specify initialDataUpdatedAt on our detail query. It will tell React Query when the data we are passing in as initialData was originally fetched, so it can determine staleness correctly. Conveniently, React Query also knows when the list was last fetched, so we can just pass that in:

```js
const useTodo = (id: number) => {
  const queryClient = useQueryClient()
  return useQuery({
    queryKey: ['todos', 'detail', id],
    queryFn: () => fetchTodo(id),
    initialData: () => {
      return queryClient
        .getQueryData(['todos', 'list'])
        ?.find((todo) => todo.id === id)
    },
    initialDataUpdatedAt: () =>
      // ⬇️ get the last fetch time of the list
      queryClient.getQueryState(['todos', 'list'])?.dataUpdatedAt,
  })
}
```

🟢 seeds the cache "just in time"
🔴 needs more work to account for staleness

### Push approch

the push approach is to do it directly in the queryFn, after data has been fetched:

```js
const useTodos = () => {
  const queryClient = useQueryClient()
  return useQuery({
    queryKey: ['todos', 'list'],
    queryFn: async () => {
      const todos = await fetchTodos()
      todos.forEach((todo) => {
        // ⬇️ create a detail cache for each item
        queryClient.setQueryData(['todos', 'detail', todo.id], todo)
      })
      return todos
    },
  })
}
```

This would create a detail entry for each item in the list immediately. Since there is no one interested in those queries at the moment, those would be seen as inactive, which means they might be garbage collected after cacheTime has elapsed (default: 15 minutes).

So if you use the push approach, the detail entries you've created here might no longer be available once the user actually navigates to the detail view. Also, if your list is long, you might be creating way too many entries that will never be needed.

🟢 staleTime is automatically respected
🟡 there is no good callback
🟡 might create unnecessary cache entries
🔴 pushed data might be garbage collected too early

Keep in mind that both approaches only work well if the structure of your detail query is exactly the same (or at least assignable to) the structure of the list query. If the detail view has a mandatory field that doesn't exist in the list, seeding via initialData is not a good idea. This is where placeholderData comes in.

### 18: Inside React Query

Note: read the blog post for this one which contain pictures and code for better understand the working of react query : [TK DODO Blogs](https://tkdodo.eu/blog/inside-react-query)

---

## Extra

### what is de-duplication in react query?

In React Query, deduplication refers to a feature that helps avoid redundant network requests for the same data. When multiple components or parts of an application need the same data, React Query can automatically detect if a request for that data is already in progress or if the data has been fetched previously. It then ensures that only a single network request is made for that data, and the result is shared among all the components that require it.

React Query achieves deduplication by maintaining a cache of previously fetched data. When a component or a query function requests data, React Query first checks its cache to see if the data is already available. If the data exists in the cache, it is immediately returned, preventing an unnecessary network request. If the data is not available in the cache, React Query sends a network request to fetch the data, stores it in the cache, and provides it to the requesting component.

This deduplication mechanism is especially useful in scenarios where multiple components in an application require the same data, such as a shared data table or a form. It helps optimize the performance of the application by reducing network traffic and avoiding redundant data fetching.

Overall, deduplication in React Query helps improve the efficiency and responsiveness of an application by minimizing unnecessary network requests and maximizing data reuse.

### get back to blog

It all starts with a QueryClient. That's the class you create an instance of, likely at the start of your application, and then make available everywhere via the QueryClientProvider:

```js
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

// ⬇️ this creates the client
const queryClient = new QueryClient()

function App() {
  return (
    // ⬇️ this distributes the client
    <QueryClientProvider client={queryClient}>
      <RestOfYourApp />
    </QueryClientProvider>
  )
}
```

The QueryClientProvider uses React Context to distribute the QueryClient throughout the entire application. The client itself is a stable value - it's created once (make sure you don't inadvertently re-create it too often), so this is a perfect case for using Context. It will not make your app re-render it just gives you access to this client via useQueryClient.

### A vessel that holds the cache

It might not be well known, but the QueryClient itself doesn't really do much. It's a container for the QueryCache and the MutationCache, which are automatically created when you create a new QueryClient.

It also holds some defaults that you can set for all your queries and mutations, and it provides convenience methods to work with the caches. In most situations, you will not interact with the cache directly - you will access it through the QueryClient.

### QueryCache

Simply put - the QueryCache is an in-memory object where the keys are a stably serialized version of your queryKeys (called a queryKeyHash) and the values are an instance of the Query class.

I think it's important to understand that React Query, per default, only stores data in-memory and nowhere else. If you reload your browser page, the cache is gone. Have a look at the persisters if you want to write the cache to an external storage like localstorage.

### Query

The cache has queries, and a Query is where most of the logic is happening. It not only contains all the information about a query (its data, status field or meta information like when the last fetch happened), it also executes the query function and contains the retry, cancellation and de-duplication logic.

It has an internal state machine to make sure we don't wind up in impossible states. For example, if a query function should be triggered while we are already fetching, that fetch can be de-duplicated. If a query is cancelled, it goes back to its previous state.

Most importantly, the query knows who's interested in the query data, and it can inform those Observers about all changes.

### QueryObserver

Observers are the glue between the Query and the components that want to use it. An Observer is created when you call useQuery, and it is always subscribed to exactly one query. That's why you have to pass a queryKey to useQuery. 😉

The Observer does a bit more though - it's where most of the optimizations happen. The Observer knows which properties of the Query a component is using, so it doesn't have to notify it of unrelated changes. As an example, if you only use the data field, the component doesn't have to re-render if isFetching is changing on a background refetch.

Even more - each Observer can have a select option, where you can decide which parts of the data field you are interested in. I've written about this optimization before in #2: React Query Data Transformations. Most of the timers, like ones for staleTime or interval fetching, also happen on the observer-level.

### Active and inactive Queries

A Query without an Observer is called an inactive query. It's still in the cache, but it's not being used by any component. If you take a look at the React Query Devtools, you will see that inactive queries are greyed out. The number on the left side indicates the number of Observers that are subscribed to the query.

### From a component perspective

- the component mounts, it calls useQuery, which creates an Observer.
- that Observer subscribes to the Query, which lives in the QueryCache.
- that subscription might trigger the creation of the Query (if it doesn't yet exist), or it might
  trigger a background refetch if data is deemed stale.
- starting a fetch changes the state of the Query, so the Observer will be informed about that.
- The Observer will then run some optimizations and potentially notify the component about the update, which can then render the new state.
- after the Query has finished running, it will inform the Observer about that as well.

Please note that this is just one of many potential flows. Ideally, data would be in the cache already when the component mounts - you can read more about that in #17: Seeding the Query Cache.

What's the same for all flows is that most of the logic happens outside of React (or Solid or Vue), and that every update from the state machine is propagated to the Observer, who then decides if the component should also be informed.

---