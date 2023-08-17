> ## React Router version: 6.15.0

# Chapter:1 Introduction to React Router

## 1.BrowserRouter

BrowserRouter is a component provided by the React Router library that enables client-side routing in a React application. It uses HTML5 history API to synchronize the UI with the current URL in the browser.

When you wrap your application with the BrowserRouter component, it listens for changes in the URL and renders the appropriate component based on the defined routes. It allows you to create multiple routes within your application and navigate between them without reloading the entire page.

```js
<BrowserRouter>
  <App />
</BrowserRouter>
```

---

### Extra History API

### Before History API, multi-page application era, if you don't use History API , Before AJAX Request

when a user click on any Link (internal or External) a request will be made to the server which return a response. that response is New HTML page which have it's own URL and everything. so your entire Page get's refreshed or replaced with the new HTML page. re-paint of the entire page

### after History API, Single page application era, after AJAX Request.

The HTML5 History API gives developers the ability to modify a website’s URL without a full page refresh.

History API allow the developer to intereact with Browser History Stack and Current Location / URL with the cozing the page to refresh.

which helps us to make single page application which don't get refreshed. now we handle links (external or internal)

internal links don't need to go to the server. so there is no need of refresh everything is handle by javascript on the frontend.

external Links do make a AJAX request but now days we don't request the entire HTML page. we request small Json packages which only return the related data and entire UI is handle by the Javascript on the frontend of the user client. in single page application we only update/replace a portion of the Page. so there is no need to refresh the full page which will repaint the entire page again from scratch.

## How to use History API

The history is a JavaScript object available in window object, which contains details about the browser session history of the tab. The list of URL's (location) you have visited will be stored, top on each other(like a stack) becouse Browser use stack data structure to store History data. Based on the history the back and forward button on the browser are displayed.

Note: this stack not just store the URL text it stores Location object (Location API)

history object contains length properties, which has a number of URL’s in the session history stack,

For example: If the user opens a tab in the browser the length of history will be 1 , because the new tab is also a webpage. Then the user enters a URL digitalocean.com and hit enter now the length of history object will be 2 , then users go to another page medium.com, now the length of the history object will be 3 .

```
3.  medium.com   //currentPage // location // this will be visible on the URL
2.  digitalocean.com
1.  newTab

const stack = ['','digitalocean.com','medium.com']

Note: Location API shows the currentPage URL used from the stack.
```

## My observation

History API has two importent part

1. History stack.
2. properties and method for intereacting with the History stack

`History Stack`: it is a stack of location object of the tab (session). it uses stack data structure to store location. every time a tab is open new History satck is created.

**Browser has**:

1. forward button to show the next page from the History stack.
2. Backword button to show the prev page from the History stack.
3. URL bar which show the current page from the History stack.

current page is what is currently being displayed by the browser.
when we say location/ URL which also means the current page.

location API allow us to interact with the current page.

## History API Methods to intereact with the History stack

1. `history.back()` will go back in the stack by one page

   ```
   3.  medium.com
   2.  digitalocean.com //currentPage // location // this will be visible on the URL
   1.  newTab
   ```

2. `history.forward()` will go forward in the stack by one page

   ```
   3.  medium.com   //currentPage // location // this will be visible on the URL
   2.  digitalocean.com
   1.  newTab
   ```

3. `history.go(num)` : will allow you to go n number of page forward and backword in the stack

   ```
   history.go(2) = history.forward() + history.forward()
   history.go(-2) = history.back() + history.back()
   history.go(1) = history.forward()
   history.go(-1) = history.back()

   history.go(0) = The default value of n is 0. If you don’t pass any number, then current page is refreshed
   ```

4. `PushState` : The pushstate will change the URL of the page and add the changed URL to the top of the URL stack of history object.

   ```
   history.pushState(state , title, Relative URL)

   state = data that you want to attached with the URL. and can be access throuh Location API
   title = This parameter exists for historical reasons, and cannot be omitted; passing an empty string is safe against future changes to the method.
   Relative URL = use to add path only. URL origin is not to be tempered with.
   ```

   ```js
   const stack = ["", "digitalocean.com", "medium.com"];
   history.length; // 3

   history.pushState("", "", "/how-to-use-HistoryAPI");

   const stack = [
     "",
     "digitalocean.com",
     "medium.com",
     "medium.com/how-to-use-HistoryAPI",
   ];
   history.length; // 4
   ```

5. The `replaceState` only changes the Current URL (location). It doesn’t add new URL to the history of URL the stack.

   ```
   history.replaceState(state , title, Relative URL)

   state = data that you want to attached with the URL. and can be access throuh Location API
   title = This parameter exists for historical reasons, and cannot be omitted; passing an empty string is safe against future changes to the method.
   Relative URL = use to add path only. URL origin is not to be tempered with.
   ```

   ```js
   const stack = ["", "digitalocean.com", "medium.com"];
   history.length; // 3

   history.replace("", "", "/how-to-use-HistoryAPI");

   const stack = ["", "digitalocean.com", "medium.com/how-to-use-HistoryAPI"];
   history.length; // 3
   ```

6. `History.scrollRestoration` : allow you to set default scroll restoration behavior. when a user come back from forward page they will be at the same scroll place when they left.

   - `auto` : The location on the page to which the user has scrolled will be restored.

   - `manual` : The location on the page is not restored. The user will have to scroll to the location manually.

7. `History.length` : tell the stack length of the history stack. How many location object does history stack have.

---

## 2.Routes

the <Routes> component serves as a container for defining routes and rendering the appropriate components based on the current URL (Matching and Rendering). It provides a powerful mechanism for creating nested routes and managing the UI hierarchy in a React application.

Whenever the location (URL) changes, the <Routes> component scans through its child routes to find the best match based on the current URL. The matched route's branch of the UI is rendered. This means that the components specified by the matched routes will be rendered.

## 3.Route

route refers to different path/direction/Road that lead to different part of your website. in web development route refers to a piece of our URL which tell us which path/page currently you are on.

```js
Ex: www.example.com //this is Domain
Ex: Domain/about // about is Path
so, Domain/path

EX: www.example.com  //Home page
Ex: www.example.com/about // about page
Ex: www.example.com/context // contact page

/about and /contact are two different path/direction/Road which are routes
```

Routes are children of Routes which define path (URL) and element (React component) that you want to render when you are on this path.

In React Router, a "route" refers to a configuration that associates a specific path with a component to render when that path is matched. Routes define how different URL paths within a React application should be handled and which components should be displayed.

The Route component is used to define routes in React Router. It is typically used within the <Routes> component to specify the individual routes and their corresponding components.

```js
import { BrowserRouter} from "react-router-dom"

<BrowserRoute>
<App/>
</BrowserRoute>

//------------------------------------------
import { Routes, Route } from "react-router-dom"

const App = () => {
    return (
      <Routes>
            <Route path="/" element={<App />} />
            <Route path="/about" element={<About />} />
      </Routes>
    )
}

Note: www.example.com and www.example.com/ are same thing so path ="/" means the HomePage
      the main/Root directory.
```

## Ranking route

```js
 URL = "/teams/new"
 Route path = "/",
 Route path = "/teams",
 Route path = "/teams/:teamId",
 Route path = "/teams/:teamId/edit",
 Route path = "/teams/new",
```

when we do Route path matching with the URL there are two path that fits the description.

1.  Route path = "/teams/:teamId"
2.  Route path = "/teams/new",

both of them matches with the URL? Now which one to render or both will be render?

here whats Ranking system comes in Route. if there are multiple match to the URL then priority will be based on

1. static segments.
2. dynamic segments.
3. star patterns

so above we have a static path too. which will win (Route path = "/teams/new")

## multiple Route match

Note: when ever URL changes routes will check all the children route and try to match/compare which path are matching and display those component that matches the URL. the thing is it is also possible that there are multiple matching element that matches with the URL in that case all the match route will be render.

```js
URL = www.example.com/host/income
route1 = <route path='www.example.com' element={<Home/>}/>
route2 = <route path='www.example.com/host' element={<Host/>}/>
route3 = <route path='www.example.com/host/income' element={<Income/>}/>

route 1,2,3 all of the route element will be render on the Screen.
```

Route matching start matching from the left-right and match those who matches the Sequence.

## Index Route

In React Router, an index route is a special type of route that represents the default content to be rendered when the parent route is matched. It is typically used when you want to render a specific component as the default content for a particular route.

When a route configuration (composition) includes an index route, it will be matched and rendered if the parent route's path exactly matches the current URL. In other words, when the parent route is the active route and there is no additional path segment in the URL beyond the parent route's path, the index route's component will be rendered.

```js
URL =  /teams
-------------------------------------------------
<Route path="teams" element={<Teams />}>
  <Route path=":teamId" element={<Team />} />
  <Route path="new" element={<NewTeamForm />} />
  <Route index element={<LeagueStandings />} />
</Route>
--------------------------------------------------
// render output

<Teams> // URL === Path  (/teams)
  <LeagueStandings />  // URL === Path === Index
</Teams>
```

one way to think of an index route is that it's the default child route when the parent matches but none of its children do.

you can also think of index Route as a default UI you want to show when parent (layout Route) matches. layout route wrap multiple children route which are actually just navigation links so layout route show the navigation links on the page when URL matches but navigaion links are just buttons untill someone (user) click on them we will not show anyrelated UI to that so till user click at any navigation link we want to show some UI to the user. and that is where index route (default UI) is helpful. to show some UI untill user user anyof the navigation link shown to them.

Depending on the user interface, you might not need an index route, but if there is any sort of persistent navigation in the parent route you'll most likely want an index route to fill the space when the user hasn't clicked one of the items yet.

## relative Route path

Links that don't start with "/" will inherit the closest Parent route in which they are rendered. This makes it easy to link to deeper URLs without having to know and build up the entire absolute path.

1. if your Route path starts with "/" then it is an absolute path.
2. if your Route path doesn't start with "/" then it is an relative path and it is relative to the closest parent who have a path property on it.

```js
// absolute Path

 <Routes>
        <Route path="/" element={<Layout />}>
            <Route index element={<Home />} />
            <Route path="/about" element={<About />} />
            <Route path="/vans" element={<Vans />} />
            <Route path="/vans/:id" element={<VanDetail />} />
        </Route>
 </Routes>
//-----------------------------------------------------------------------
// relative path:1

 <Routes>
        <Route path="/" element={<Layout />}>
            <Route index element={<Home />} />
            <Route path="about" element={<About />} />
            <Route path="vans" element={<Vans />} />
            <Route path="vans/:id" element={<VanDetail />} />
        </Route>
 </Routes>
```

Note: if parent Route is a Layout route and it doesn't have any path (pathless Route) then all of the Children will look at layout parent if layout parent has path then all the chidlren will be relative to that parent.

Note: if layout route has no parent then default path of "/" will be consider as path which is top most level path in react router.

Commonly we setup route composition (layout routes) with a path so all of the children which will inherit the parent path and we don't have to define long path in as we go deeper in the layout composition.

```js
// relative path:2

<Routes>
  <Route element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="about" element={<About />} />
    <Route path="vans" element={<Vans />} />
    <Route path="vans/:id" element={<VanDetail />} />
  </Route>
</Routes>

// relative path:1 === relative path:2
```

## How BrowserRoute & Routes & Route work in harmony

it's like this when URL changes BrowserRouter track the HistoryAPI which get's triggered and which tells Routers that URL have Chaged then Routers go through all of it's children route and compare and match the best path option available in the route children collection. if it finds any match then it will render the apropriate Component defined in the route with that path.

## 4.Link

we talked about BrowserRouter and Router and route which work together to show a component when URL changes but how and when the URL changes? it changes when user intreact with website when user click on a link,button which change the path of the URL section.

In React Router, the Link component is used to create navigation links within your application. It provides a declarative way to navigate between different routes without causing a full page reload.

The Link component is typically used in place of the <a<span></span>> HTML tag for internal navigation within a React application. When a Link is clicked, it prevents the default browser behavior of reloading the page cozed by HTML <a<span></span>> tag and instead uses the React Router's navigation system to update the URL and render the appropriate component.

Note: underneath Link it actually using HTML <a<span></span>> tag with href that points to the resource it's linking to.

```js
<Link to='/'>Home<Link>
<a href='/'>Home</a>
//------------------------------------
<Link to='/about'>About<Link>
<a href to='/about>About'>

// 1. in link we use "to" instead of "href".
// 2. anything between opening and closing bracket (<link>Display Name<link>) of link will be the display name.
```

Note: with Link page don't refresh/re-render when user navigate from one page to another.

## skip client side routing

`<Link reloadDocument>`

if you don't want react router to handle your link rather want the links to be handle by the browser (which is what usually happens) then use reloadDocument which will act as normal HTML <a<span></span>> tag and reload page on every link click by the user.

You can use <Link reloadDocument<span></span>> to skip client side routing (handle by the react router) and let the browser handle the transition normally (as if it were an <a href<span></span>>).

## Relative Link Path (Relative to Parent route)

Links can too have absolute and relative path (to)

1. `absolute path`: if "Link to" starts with "/" then it is an absolute path.
2. `relative path`: if your "Link to" doesn't start with "/" then it is an relative path and it is relative to the parent route path. and parent route will be the route which render that component inWhich the Link is decleared.

By default, links are relative to the route hierarchy.

## when moving forword in by the Link (Deeper in the Route Hiererchy Tree,Down the tree)

```js
URL = `/host/income`

<BrowserRouter>
      <Routes>
            <Route path="/" element={<Layout />}>
                    <Route path="host" element={<HostLayout />}>
                            <Route path="income" element={<Income />} />
                    </Route>
            </Route>
      </Routes>
</BrowserRouter>
//----------------------------------------------------------------
// absolute Link path
function Income() {
  return (
    <Link to="/host/income/gogo">Click Me</Link>
  );
}

// relative Link path
function Income() {
  return (
    <Link to="gogo">Click Me</Link>
  );
}

//when user will click on (Click Me) they will go to (/host/income/gogo)
```

<Income/<span><span>> component route already have the path of "/host/income" which is inherited by all the links defined inside Income component so if we use relatie path in any of the Links of <Income/<span><span>> component then Link will inherit Income Component route path.

## when Moving backwork by the Link (Up in the Route Hiererchy Tree,Up the tree)

generally you Link is relative to Link's component parent route path. but you can define relative to which route by move up in the tree of routes hierarchy. you can use ".." just like you use in Module import or in your Terminal to access directory. to Look up in the tree

Side Knowlage:

1. "." = current Level (parent route)
2. ".." = one LevelUp (parent parent route)
3. "../.." = Two LevelUp (parnet parent parent route)

Note:

```js
//Link1
<Link to=".">click Me</Link>

//Link2
<Link to="">click Me</Link>

// Link1 & Link2 both are same. but "." is Explist but "" is implist
```

```js
URL =   '/host/income'

<BrowserRouter>
      <Routes>
            <Route path="/" element={<Layout />}>
                    <Route path="host" element={<HostLayout />}>
                            <Route path="income" element={<Income />} />
                    </Route>
            </Route>
      </Routes>
    </BrowserRouter>
//----------------------------------------------------------------
// relative Link path
function Income() {
  return (
    <Link to="..">click Me</Link>
  );
}

//when user click on (Click Me) Link user will go from (/host/income) to (/host)
//-------------------------------------------------------------------
```

our Link was relative to (/host/income) but ".." we go one level Up in the Route hiererchy tree and now Link is relative to (/host). if we add more ../.. then we will go 2 level up in the route tree and now Link are relative to (/) the root.

## Relative Link Path (Relative to URL)

By default, links are relative to the route hierarchy, so ".." will go up one Route level. Occasionally, you may find that you have matching URL patterns that do not make sense to be nested, and you'd prefer to use relative path routing but not relative to Route but relative to URL you can use (relative = "path"). to opt out the default Link behaviour.

Side Knowlage: Relative to URL

1. "." = current Level (current URL path segment )
2. ".." = one LevelUp (one previous URL path segment)
3. "../.." = Two LevelUp (two previous URL path segment)

```js
// understand the segment

//ex:
URL = www.exapmle.com/payment/bill/sucess

//a segment is /segment/

//so URL segment division = www.exapmle.com/two prev segment/one prev segment/current segment

//in this URL
sucess = current segment.       // current page
bill = one previous segment.    // one previous page
payment = two previous setment. // two previous page
```

```js
// Contact & EditContact both routes are sibiling not nested

URL = '/contacts/:id/edit'

<Route path="/" element={<Layout />}>
  <Route path="contacts/:id" element={<Contact />} />
  <Route path="contacts/:id/edit" element={<EditContact />}/>
</Route>;

function EditContact() {
  // Since Contact is not a parent of EditContact we need to go up one level
  // in the path, instead of one level in the Route hierarchy
  return (
    <Link to=".." relative="path">
      Cancel
    </Link>
  );
}

// with (relative="path") when user will click on Cancel Link they will go from (/contacts/:id/edit) to (/contacts/:id). which follow relative to the URL not the Route hiererchy.
```

Note: Generally used when routes are in sibling relationship rahter then having parent child relationship.
they don't have parent child relationship becouse of they don't share UI with each other so there is no need for nesting these routes. so routes are not ntested we can't use link relative to route, we have to use relative to URL.

## ScrollRestoration

If you are using ScrollRestoration, this lets you prevent the scroll position from being reset to the top of the window when the link is clicked.

```js
<Link to="?tab=one" preventScrollReset={true} />
```

Note: This does not prevent the scroll position from being restored when the user comes back to the location with the back/forward buttons (Browser history nav), it just prevents the reset when the user clicks the link.

## replace

The replace prop is a boolean that when set to true will cause this link to replace the current page in the browser history. Imagine you have the following browser history.

```js
/
/books
/books/3
```

If you click on a link that goes to the "/books/3/edit" page but it has the replace property set to true your new history will look like this.

```js
// without replace: true

/
/books
/books/3
/books/3/edit
//------------------------------------
// with replace: true

/
/books
/books/3/edit
```

The page your were currently on was replaced with the new page. This means that if you click the back button on the new page it will bring you back to the /books page instead of the /books/3 page.

## state

This prop is used to pass some additional data along with the Link which will not ne seen on the URL when user click on the Link. generally used it in combination with navigation Data (useLocation(),useNavigation()) etc

```js
<Link to="new-path" state={{ some: "value" }} />
```

## NavLink

NavLink is just Link but with some additional features like active and pending state end someOther state like end, caseSensitive, aria-current.

A <NavLink<span></span>> is a special kind of <Link<span></span>> that knows whether or not it is "active" or "pending". This is useful when building a navigation menu, such as a breadcrumb or a set of tabs where you'd like to show which of them is currently selected. It also provides useful context for assistive technology like screen readers.

NavLink comes with 2 boolean states isActive and isPending.
NavLink byDefault is consider active. when NavLink Path matches with the URL path.

```
URL = contact/change
NavLink1 = <NavLink to='contact'>
NavLink2 = <NavLink to='contact/change'>
NavLink3 = <NavLink to='home>
NavLink4 = <NavLink to='home/host>

Navlink1 & NavLink2 Both are active
```

to match the URL path with the NavLink Path they follow the same pattern of matching by sequence from left to write.

but you can define your own logic when to make these NavLink active or pending.

## custom logic for active and pending state activation

you can defien a function inside your className and style. to define your own custom activation rule.
that function take an object which contain both isActive and isPending boolean. you can also destructure it inside your function parameter and use them to define your own logic.

```js
// in ClassName

import { NavLink } from "react-router-dom";

<NavLink
  to="/messages"
  className={({ isActive, isPending }) =>
    isPending ? "pending" : isActive ? "active" : ""
  }
>
  Messages
</NavLink>;
//-----------------------------------------------------
// in stype

<NavLink
  to="/messages"
  style={({ isActive, isPending }) => {
    return {
      fontWeight: isActive ? "bold" : "",
      color: isPending ? "red" : "black",
    };
  }}
>
  Messages
</NavLink>;
```

## Default active class

NavLink Comes with a default Active class attached to it which you can use to add your Css.

By default, an active class is added to a <NavLink> component when it is active so you can use CSS to style it.

```js
a.active {
  color: red;
}
```

when even NavLink is active the color will be changed to red (Css style will be applied).

## end (Change the defualt activation behaviour)

The end prop changes the matching logic for the active and pending states to only match to the "end" of the NavLink's to path. If the URL is longer than to, it will no longer be considered active.

what it means is if you use end in your link then that NavLink will be consider active only when NavLink path matches with the URL path exectly (from the end of NavLink to end of URL)

Which is same in Route,Link,NavLink all of them match multiple Route selecting on the bases of URL sequence if nay of the NavLink matches the sequence it will be selected for active state.

```js
// Without the end prop, this link is always be active because every URL matches has '/'.

<NavLink to="/">Home</NavLink>

// With the end prop, the NavLink will only be active when URL is exectly same URL = / .
<NavLink to="/" end> Home </NavLink>
```

Now this link will only be active at "/". This works for paths with more segments as well:

```
Link	                        URL	        isActive
<NavLink to="/tasks" />	/       tasks	    true
<NavLink to="/tasks" />	/       tasks/123	true
<NavLink to="/tasks" end />	/   tasks	    true
<NavLink to="/tasks" end />	/   tasks/123	false
```

## caseSensitive

Adding the caseSensitive prop changes the matching logic to make it case sensitive.

```
Link	                                        URL	            isActive
<NavLink to="/SpOnGe-bOB" />	            /sponge-bob	        true
<NavLink to="/SpOnGe-bOB" caseSensitive />	/sponge-bob	        false
```

## aria-current

When a NavLink is active it will automatically apply <a aria-current="page" <span></span>>

## reloadDocument

The reloadDocument property can be used to skip client side routing and let the browser handle the transition normally (as if it were an <a href<span></span>>).

## 5. Route parameters (params) | Dynamic Route | useParams | Dynamic Link

at this point for every route we have to define a static path to each and every path URL. but what if you have fetched a list of data then you convert them into tiles so when you click on the tile you will go that that specific tile data.

## problem:1

we have a dog store. which show a list of dogs Breads tile (clickable element) which when clicked leads to diffrenet URL specific to that breads show Dogs available (in that bread) in the shop to adopt.

```js
const dogsBread = [
    {
      id: '01',
      type: Bulldog,
      img: 'www.unsplesh.com/Bulldog'
    },
    {
      id: '02',
      type: German Shepherd,
      img: 'www.unsplesh.com/GermanShepherd'
    },
    {
      id: '03',
      name: 'shiro',
      type: Labrador Retriever,
      img: 'www.unsplesh.com/LabradorRetriever'
    },
    ...
]
```

first of all we have to create <Link<span></span>> for each DogBread so that external links are handle by react router rather then HTML <a<span></span>> tag which re-load the page.

```js
const breadTile = dogsBread.map( dog => {
    return (
        <Link to={`dogs/${dog.id}`}>
        <img src={dog.img}>
        <h2>{dog.type}</h2>
        </Link>
    )
})

```

what will happen now when a user click on the any breadTile the URL path will change
from dog to dog/'anyID'

that anyID can be anything so creating static route will be imposible so react router introduce dynamic route path

```js
path = "dog/01"; // static path nameing
path = "dog/02"; // static path nameing
//---------------------------------------
path = "dog/:id"; // dynamic path nameing

//so here :id can be anything 01,02,03,...
```

if you use ":" it means the name describled after : is just a placeHolder/params which will be replaced by real argunments.

```
argunment are defined at <Link to={`dog/${dog.id}`}>{dog.type}<Link>

parameter/params are defined at <Route path='dog/:id' element={<dogType/>}>
```

## problem:2

now we have dynamic path naming when ever anyone click on anyof the Dog bread tile. URL will change and router with path of dog/:id will be triggered but now we need that id in the dogType component so that we can fetch list of all the dog avaliable in the store related to that bread.

how to acess that ID???

you can use useParams() Hook

The useParams hook returns an object of key/value pairs of the dynamic params from the current URL that were matched by the <Route path<span></span>>. Child routes inherit all params from their parent routes.

```js
<Route path='dog/:id' element={<dogType/>}>
```

which means dogType component inherit all the dynamic path variable defined in the route path. you can access those dynamic path variable keys by useParams() Hook which return an object of key:value pair.
where keys are dynamic path varibale (the parameter/param) and value are argunment passed by Link.

```js
// step:1
<Link to={`dog/${dog.id}`}>${dog.type}<Link>
// step:2
<Route path='dog/:id' element={<dogType/>}/>

//-----------------------------------------------------

import {usePramas} from "react-router-dom"

step:3 const dogtype = ()=>{
    const { id } = useParams()

// fetch the data by using id
// return the JSX
}
```

# Chapter:2 Nested Routes

## 1.Layout Route (for Nesting)

a "layout route" refers to a route configuration that defines a common layout or structure for a group of related routes within your application. It allows you to wrap multiple routes with a common layout component, such as a header, footer, or sidebar, while keeping the nested routes separate and independent.

By using a layout route, you can establish a consistent visual structure or design for a specific section or area of your application, while still allowing individual routes within that section to have their own unique components and functionality.

Using layout routes helps to modularize and organize your application's routing structure. It allows you to apply a consistent layout or structure to a group of related routes while maintaining the flexibility to define unique components and functionality for each individual route.

Note: you should add <Outlet<span></span>> component with in the layout component where you want the child route elements to be rendered. Using {children} directly within the layout component will not work as expected.

## MyObservation

Layout Route is a route composition or structuring. where one route act as parent route and wrap multiple children route.

parent route is used to show a common Shared-UI that you want to share with all the Children routes and each child route has there own Unique-UI.

```js
//step:1 setup the structure/composition

<Routes>
  <Route path= '/'  element={<Shared-UI/>} >                // Parent route
     <Route path= '/Home'  element={<Unique-Ui3>}/>         // child route
     <Route path= '/About'  element={<Unique-Ui1>}/>        // child route
     <Route path= '/Contact'  element={<Unique-Ui2>}/>      // child route
  </Route>
</Routes>

```

Note: Layout route can also work without path. it is not mandatory to use path in parent route when ever URL matches with any of the child route path parent route will be automatically be render with the matched child route.

Note: Common rule of thumb is you should only use layout route when you want to share a common UI with a group of routes.

```js
//step:2 add outlet component

import { Outlet } from "react-router-dom"

export default function Shared-UI() {
    return (
        <>
        <header>
            <Link className="site-logo" to="/">#VanLife</Link>
            <nav>
                <Link to="/about">About</Link>
                <Link to="/vans">Vans</Link>
            </nav>
        </header>
//-------------------------------------------------------------------------
        <Outlet />   // all the child route will be render here
//-------------------------------------------------------------------------
        </>
    )
}

```

## Outlet

```js
// react ex:
<Shared-UI>                                                 // Parent route
     <Route path= '/Home'  element={<Unique-Ui3/>}/>         // child route
     <Route path= '/About'  element={<Unique-Ui1/>}/>        // child route
     <Route path= '/Contact'  element={<Unique-Ui2/>}/>      // child route
<Shared-UI/>
//------------------------------------------------
const Shared-UI = ({Children}){
    const [home,about,contact] = Children
    // use it as you please
}
//-------------------------------------------------------------------------------------
// step:1
// step:2
```

if we use this structure with is pretty common in React. if we want to use all of the child route we need to use children prop inside Shared-UI component and you can use there children route inside shared-UI component.

then itr over or place them as you like

Outlet is pretty similar to Children prop that we use in react. in react router we use outlet component inside layout route to tell where want to place all of the route children.

In React Router, the <Outlet<span></span>> component is used to specify the location where child route components should be rendered within a layout or parent component. It acts as a placeholder for the nested components that correspond to the matched routes.

When a route is matched and rendered by React Router, the component specified in the corresponding <Route<span></span>> element is rendered. If the <Route<span></span>> component has child routes, the <Outlet<span></span>> component is used within the parent component or layout to determine where the child route components should be inserted.

Note: the <Outlet<span></span>> component should be used within a parent component or layout, and it cannot be used outside the context of a route configuration.

## useOutletContext (share state between parent and child route)

Often parent routes manage state or other values you want shared with child routes. You can create your own context provider if you like, but this is such a common situation that it's built-into <Outlet <span></span> />:

useOutletContext is just a useContext but specificly for Outlet.

there are two steps:

1. in `Parent Route` = <Outlet context={..here..}<span></span>> provide the context which will be consuable by all the child route.

2. in `Child route` = useOutletContext() Hook to consume the data passed from the parent outlet.

```js
// Parent Route (layoutRoute) that handle state for all Children
// share them with context

function Parent() {
  const [count, setCount] = React.useState(0);
  return <Outlet context={[count, setCount]} />;
}
----------------------------------------------------------------------
// Child Route that consume the context

import { useOutletContext } from "react-router-dom";

function Child() {
  const [count, setCount] = useOutletContext();
  const increment = () => setCount((c) => c + 1);
  return <button onClick={increment}>{count}</button>;
}

```

# Chapter:3 Search Params

## Search / Query Parameters

- Query Parameter represent some kind of change in your UI
  some common changes are Sorting, filtering, pagination.

- Think of query parameters as a single source of truth for certain application state.

`Ask Yourself`: if a user share your website URL after applying some change (like filtering a list of item) with there friends. will friends will be able to see the website with appied filter options or not.

if you don't use query parameter in your URL then there is no records of changes in the URL due to that if a user filter some list on the website. that filtering information is only in the memoery of the browser of the user (when you use state to manage filtering, all of the logic of react is only in the memory of the clint browser) if user share the URL with someone else then the new user have No added filter in the web. becouse browser store the filtering info in the memory in the RAM of the user PC. due to that new user will have the website but with the initial setup of the website (just like react app is loading for the first time).

To share the filter info we use query parameter in the URL so that when someone share URL with any change in the website that change information is included on the URL and the new user will have the same changes on when he visit the website.

- QueryParameters in the URL starts with a question Marks "?".

Ex:

```
URL = www.example.com/home?Key=Value
```

- for adding multiple Query parameter in a single URL you can seprate query parameters by "&" symbol.

Ex:

```
URL = www.example.com/home?key=value&key2=value2
```

---

Query parameters, also known as query string parameters or URL parameters, are a way to pass information to a web server / clint Browser as part of a URL. They are typically used in the address bar of a web browser or in an HTTP request to specify additional data that the server / Browser can use to process the request or provide customized responses.

Query parameters consist of key-value pairs appended to the end of a URL, separated by the question mark (?) and separated from each other by an ampersand (&).

Ex:

```
https://example.com/search?q=chatbot&category=technology
```

In this example, the URL has two query parameters: q and category. The values assigned to these parameters are chatbot and technology, respectively. The URL parameters provide additional information to the web server / browser about the user's search query and the desired category.

Query parameters can be used for various purposes, such as filtering data, specifying search terms, passing authentication tokens, and more. Web applications and APIs often use query parameters to enable dynamic and personalized content delivery based on user input.

When a web server receives a URL with query parameters, it can extract and process those parameters to generate an appropriate response. The server-side application or script can access the query parameters and use them to perform specific actions or retrieve relevant data to return to the client.

---

## useSearchParams()

---

The useSearchParams hook is used to read and modify the query params from the Current URL Location. Like React's own useState hook, useSearchParams returns an array of two values: the current URL location search params and a function that may be used to update them. Just as React's useState hook, setSearchParams also supports functional updates. Therefore, you may provide a function that takes a searchParams and returns an updated version.

Ex:

```
let [searchParams, setSearchParams] = useSearchParams();
```

---

In React Query, the useSearchParams hook is used to access and manipulate the query parameters in the URL of the current page. It provides an easy way to read and update the query parameters without directly manipulating the URL.

Here's an example of how you can use useSearchParams in React Query:

```js
import { useSearchParams } from "react-router-dom";

function MyComponent() {
  const [searchParams, setSearchParams] = useSearchParams();

  // Accessing query parameters
  const searchTerm = searchParams.get("q");
  const category = searchParams.get("category");

  // Updating query parameters
  const updateSearchParams = () => {
    setSearchParams({ q: "chatbot", category: "technology" });
  };

  return (
    <div>
      <p>Search Term: {searchTerm}</p>
      <p>Category: {category}</p>
      <button onClick={updateSearchParams}>Update Search Params</button>
    </div>
  );
}
```

In the example above, the useSearchParams hook is used to initialize the searchParams and setSearchParams variables. searchParams represents the current query parameters, and setSearchParams is a function used to update the query parameters.

You can access specific query parameters using the get method on the searchParams object. In the example, we retrieve the values of the q and category query parameters.

To update the query parameters, you can call the setSearchParams function with an object containing the key-value pairs you want to update or add. In this case, we update the q and category parameters with new values.

Finally, in the JSX, we display the current values of the query parameters and provide a button that, when clicked, updates the query parameters to the specified values.

---

## Extra:1 URL Interface

The URL interface provides methods and properties for working with URLs in the browser's JavaScript environment. It allows developers to parse, manipulate, and extract information from URLs.

1. Constructor: The URL interface provides a constructor that allows you to create a new URL object by passing in a string representing the URL.

   ```js
   const myURL = new URL("https://www.example.com/path?query=string");
   ```

2. Properties: The URL object exposes several properties that allow you to access different parts of the URL:

   - href: The complete URL string.(everything)
   - pathname: The path of the URL (e.g., "/path").
   - search: The query string of the URL (e.g., "?query=string").
     Note: use new URL().search or use new URLSearchParams(searchParams)

   - hash: The fragment identifier of the URL (e.g., "#section").
   - origin: The origin of the URL (e.g., "www.example.com").
   - protocol: The protocol (e.g., "https:", "http:").
   - hostname: The hostname of the URL (e.g., "www.example.com").
   - port: The port number of the URL.( port Number )

## Extra:2 URLSearchParams Interface

useQueryParams() use URLSearchParams Browser API under the hood.

Note: URLSearchParams only take queryString as parameter and provide methods to intereact with the queryString. you can't provide URL string.

if you want to provide URL string then

```js
const urlString = new URL("https://example.com/?name=Jonathan&age=18");
const queryString = urlString.Search;
const searchParams = new URLSearchParams(queryString);
or;
const searchParams = queryString.searchParams;
```

```js
const paramsString = "q=URLUtils.searchParams&topic=api";
const searchParams = new URLSearchParams(paramsString);
```

this searchParams and searchParam return by the useSearchParams() hook array are same. you can use same methods used in URLSearchParams API searchParams instance on the useSearchParams hooks searchPrams.

searchParams is an object which contain lots of methods and propeties.

Note: searchParams object can also be directly itr-able by for...of method which will itr over each and every key/value pairs in the same order as they appear in the URL query string.

```js
// both are same
for (const [key, value] of mySearchParams) {
}
for (const [key, value] of mySearchParams.entries()) {
}
```

## Propeties:

1. `size`: Indicates the total number of search parameter entries.

```
const searchParams = new URLSearchParams("c=4&a=2&b=3&a=1");
searchParams.size; // 4
```

## Methods:

1. `URLSearchParams.get()`: Returns the first value associated with the given search parameter.

   ```js
   URL = https://example.com/?name=Jonathan&age=18
   let params = new URLSearchParams(document.location.search);
   let name = params.get("name"); // is the string "Jonathan"
   let address = params.get("address"); // null // if key doesn't exist
   ```

2. `URLSearchParams.getAll()`: Returns an arrar of all the values associated with a given search parameter.

   ```js
   let url = new URL("https://example.com?foo=1&bar=2");
   let params = new URLSearchParams(url.search);

   //Add a second foo parameter.
   params.append("foo", 4);

   console.log(params.getAll("foo")); //Prints ["1","4"].
   ```

3. `URLSearchParams.has()`: Returns a boolean value indicating if such a given parameter exists.

   ```js
   let url = new URL("https://example.com?foo=1&bar=2");
   let params = new URLSearchParams(url.search);

   console.log(params.has("bar")); //true
   ```

4. `URLSearchParams.set()`: Sets the value associated with a given search parameter to the given value.

   - if there already exist a value to speccify key , the value will be replaced.
   - If there are several values, the others are deleted.

   but in react route there are two ways to set search Params

   - useSearchParam Hooks setUseParams() fn which can take fn with previous value as parameter or you can set directly too. (prefered method)

   when you use directly setting params with setUseParams you can specify value two ways.

   - "type=jedi" (string type)
   - {type:"jedi"} (Object type)

   Note: setUseParams() don't need ? at the start for adding params. it is smart enough to figure that out of its own accords.

   - you can use <Link> to add Query too. (works great but not a prefered method)

   ```js
   // with useSearchParams()

   URL = www.example.com/info?

   const [searchParams, setSearchParams] = useSearchParams();

   return (
   //--- string type
       <Button onClick={setSearchParams("type=jedi")}>Jedi </Button>
       <Button onC lick={setSearchParams("")}> Clear </Button> // remove all the params, default back to show full list

   //--- Object type
       <Button onClick={setSearchParams({type:"sedi"})}> Sedi </Button>
       <Button onClick={setSearchParams({})}> Clear </Button> //remove all the params, default back to show full list
   );

   //updated URL with Jedi Button Click = www.example.com/info?type=jedi

   //updated URL with Sedi Button Click = www.example.com/info?type=sedi
   //-----------------------------------------------------------------
   // With Link

   <Link to="?type=jedi">Jedi</Link>
   <Link to="?type=seth">Sedi</Link>
   ------------------------------------------------------------
   // in URLSearchParams Browser API

   let url = new URL("https://example.com?foo=1&bar=2");
   let params = new URLSearchParams(url.search);

   // Add a third parameter.
   params.set("baz", 3);
   params.toString(); // "foo=1&bar=2&baz=3"

   ```

5. `URLSearchParams.append()`: Appends a specified key/value pair as a new search parameter.

   ```js
   let url = new URL("https://example.com?foo=1&bar=2");
   let params = new URLSearchParams(url.search);

   //Add a second foo parameter.
   params.append("foo", 4);
   //Query string is now: 'foo=1&bar=2&foo=4'
   ```

6. `URLSearchParams.delete()`: Deletes the given search parameter, and its associated value, from the list of all search parameters.

   ```js
   let url = new URL("https://example.com?foo=1&bar=2&foo=3");
   let params = new URLSearchParams(url.search);

   // Delete the foo parameter.
   params.delete("foo"); //Query string is now: 'bar=2'
   ```

7. `URLSearchParams.entries()`: Returns an iterator allowing iteration through all key/value pairs contained in this object in the same order as they appear in the query string.

   ```js
   // Create a test URLSearchParams object
   const searchParams = new URLSearchParams("key1=value1&key2=value2");

   // Display the key/value pairs
   for (const [key, value] of searchParams.entries()) {
     console.log(`${key}, ${value}`);
   }
   ```

8. `URLSearchParams.forEach()`: Allows iteration through all values contained in this object via a callback function.

   ```js
   // Create a test URLSearchParams object
   const searchParams = new URLSearchParams("key1=value1&key2=value2");

   // Log the values
   searchParams.forEach((value, key) => {
     console.log(value, key);
   });
   ```

9. `URLSearchParams.keys()`: Returns an iterator allowing iteration through all keys of the key/value pairs contained in this object.

   ```js
   // Create a test URLSearchParams object
   const searchParams = new URLSearchParams("key1=value1&key2=value2");

   // Display the keys
   for (const key of searchParams.keys()) {
     console.log(key);
   }
   ```

10. `URLSearchParams.values()`: Returns an iterator allowing iteration through all values of the key/value pairs contained in this object.

    ```js
    const searchParams = new URLSearchParams("key1=value1&key2=value2");

    for (const value of searchParams.values()) {
      console.log(value);
    }
    ```

11. `URLSearchParams.sort()`: Sorts all key/value pairs, if any, by their keys unicode code.

    ```js
    // Create a test URLSearchParams object
    const searchParams = new URLSearchParams("c=4&a=2&b=3&a=1");

    // Sort the key/value pairs
    searchParams.sort(); //a=2&a=1&b=3&c=4
    ```

12. `URLSearchParams.toString()`: Returns string version of URL query.

    ```js
    const url = new URL("https://example.com?foo=1&bar=2");
    const params = new URLSearchParams(url.search);

    // Add a second foo parameter.
    params.append("foo", 4); // 'foo=1&bar=2&foo=4'
    ```

---

## use Links to add Query in URL

you can use Links not just to add paths in URL but you can use Links to add Query too.

```js
URL =   /host/income

<BrowserRouter>
      <Routes>
            <Route path="/" element={<Layout />}>
                    <Route path="host" element={<HostLayout />}>
                            <Route path="income" element={<Income />} />
                    </Route>
            </Route>
      </Routes>
</BrowserRouter>
//------------------------------------------------
// relative path + query in links

function Income() {
  return (
    <Link to="?type=jedi">Jedi</Link> ///host/income?jedi
    <Link to="?type=seth">Sedi</Link> ///host/income?sedi
  )
}
//------
function Income() {
  return (
    <Link to=".?type=jedi">Jedi</Link> ///host/income?jedi
    <Link to=".?type=seth">Sedi</Link> ///host/income?sedi
  )
}

both are same
```

**Note**:

1. to add a filter query <Link to=".?type=jedi">Jedi</Link>
2. to remove filter (means displayBack the full list) <Link to=".">Clear</Link>

## preserve prev query

when you use setSearchParams() Fn to add new param or use Link to add new param both of these methods will replace the previous queryString with the new Query string.

which means if you have some prev param set on your URL and you use setSearchParams() or Link then all of the prev params will be removed.

then how to presever prev params??

- you have to write some custom logic by using URLSearchParams API Methods

```js
// for Link
// with Links we have to use new URLSearchParams() becouse searchParams from useSearchParams is readonly property. and we are not using setSeachParams to modify the seachParams value so we need to create a new instance with new URLSearchParams().

function genNewSearchParamString(key, value) {
    const sp = new URLSearchParams(searchParams)
    if (value === null) {
      sp.delete(key)
    } else {
      sp.set(key, value)
    }
    return `?${sp.toString()}`
  }

<Link to={genNewSearchParamString("type", "jedi")}>Jedi</Link>
<Link to={genNewSearchParamString("type", "sith")}>Sith</Link>
<Link to={genNewSearchParamString("type", null)}>Clear</Link>
//--------------------------------------------------------------
//for setSearchParams()
// searchParam of useSearchParams() === searchParams of URLSearchParams API
// we can use prevParams with in setSearchParams fn to update query

function handleFilterChange(key, value) {
    setSearchParams(prevParams => {
      if (value === null) {
        prevParams.delete(key)
      } else {
        prevParams.set(key, value)
      }
      return prevParams
    })
  }

<button onClick={() => handleFilterChange("type", "jedi")}>Jedi</button>
<button onClick={() => handleFilterChange("type", "sith")}>Sith</button>
<button onClick={() => handleFilterChange("type", null)}>Clear</button>
```

## conditional apply css based on QueryParameters

Note: <NavLink> className fn with isActive will not work becouse navLink isActive is made to work with URL path not the URL qury params.

```JS
// create a selected className and add your filter css

 const [searchParams, setSearchParams] = useSearchParams()

 const typeFilter = searchParams.get("type")

 <button
    onClick={() => handleFilterChange("type", "rugged")}
    className={ `van-type rugged ${typeFilter === "rugged" ? "selected" : ""}` }
    >
    Rugged
    </button>

```

## useLocation Hook?

before understanding useLocation Hook let's understand what Location means?

### Location

in simple term Location means current Location or URL (Uniform Resource Locator).

In React Router, the location object store info realted to the current location or URL of the application. It is used to provide information about the current URL and can be accessed within components rendered by the router.

---

## Extra Location API

Note: the real Location object in JavaScript which can be access by location or window.location contain many properties and methods related to URL. which are not all that usefull. react router useLocation object use javascript location APi under the hood to provide a simpler interface to intereact witht the location API.

Read only Location properties:

```
Location.href, 
Location.protocol, 
Location.host, 
Location.port, 
Location.pathname, 
Location.search ,
Location.hash ,
Location.origin ,
Location.username, 
Location.password,
```

To Manipulating the location:

```
Location.assign(), 
Location.reload(), 
Location.replace().
```

---

The location object provided by react roter useLocation() hook typically contains the following properties:

1. `pathname`: it is a string that contains an initial / followed by the remainder of the URL up to the ?.

   Ex:

   ```
   URL = "https://example.com/products", then pathname would be "/products".
   ```

2. `search`: It is a string that contains an initial ? followed by the key=value pairs in the query string. If there are no parameters, this value may be the empty string (i.e. '').

   Ex:

   ```
   URL = "https://example.com/products?category=shoes&color=blue", then search would be "?category=shoes&color=blue".
   ```

3. `hash`: The location.hash property is a string that contains an initial # followed by fragment identifier of the URL. If there is no fragment identifier, this value may be the empty string (i.e. '').

   Ex:

   ```
   URL = "https://example.com/products#top", then hash would be "#top".
   ```

---

### What is Hash?

The hash property of a URL represents the fragment identifier of the URL. It is a string that starts with the # character and is typically used to identify a specific section or location within a web page.

When a URL contains a hash fragment, the browser will scroll the page to the element with a corresponding id attribute matching the fragment identifier. This allows users to easily navigate to specific sections of a long web page by simply appending the desired section's id to the URL.

---

4. `state`: state object representing the state data associated with the current location. This is typically used to pass data between different routes or components. The state object can contain any serializable data, such as strings, numbers, arrays, or objects.

   some key point's related to state object:

   - use State object only for sharing non-presistant data.

   state object is stored in the History stack of the browser tab of the user and it is stored in the Memory of the user browser and it will not be preserved state if you copy that URL and paste in another tab. the state will be removed (becouse new tab will not have the same History stack, every time a tab is open new History satck is created)

   - state only presist if you use the same tab

   you can refresh the page, you can go back and forword state will be presisted. but keep in mind each URL has there own state.

   - state object store state related to current Location / URL.

   the Location.state property only represents the state associated with the current location in the history stack. If you navigate backward or forward in the history stack, the state object will change accordingly to reflect the state of the corresponding location.

   we use state to share some additional data that we don't want to include in the URL.

---

### What is Location.state?

In React Router, the history.state property is used to access the state object associated with the current location in the history stack. It provides a way to store and retrieve additional data related to a specific location, allowing you to pass information between different routes (URL) or navigation action buttons.

When you navigate to a new route using React Router's <Link<span></span>>, history.pushStack, or history.replaceStack methods, you can include a state object as part of the navigation. This state object can contain any serializable data, such as JSON objects or primitive values.

Here's an example that demonstrates how to pass state while navigating to a new route:

```js
//current URL
let URL = 'www.example.com'

<Link to='/dashboard'
  state = { user: 'John', loggedIn: true }>
  Go to Dashboard
</Link>
```

In this example, when user click on the Link. the user will be directed from 'www.example.com' to "www.example.com/deshboard". and URL/Location will also contain additional state object which contain some extra info about the user.

To access the state object in the component corresponding to the "/deshboard", you can use the useLocation hook provided by React Router:

```js
import { useLocation } from 'react-router-dom';

const NewRouteComponent = () => {
  const location = useLocation();
  const { user, loggedIn } = location;

  return (...);
};
```

In this example, the useLocation hook is used to get the current location object from the History stack, and the state property is extracted from it. If a state object was passed during the navigation, it will be available in location.state. You can then access the specific properties of the state object and use them within your component.

Note: It's important to note that the history.state property only represents the state associated with the current location in the history stack. If you navigate backward or forward in the history stack, the state object will change accordingly to reflect the state of the corresponding location.

However, please be aware that the state object should be used with caution, as it can consume memory and may not be preserved if you copy URL and paste it in another tab. becouse new tab will not have the same history stack Therefore, it's generally recommended to use the state object for temporary or non-persistent data, and consider other means of data management (such as Redux or context) for more complex scenarios.

---

5. `key`: the location.key is a unique identifier associated with a particular location object. It is automatically generated by React Router and assigned to each location when it is added to the history stack. The location.key is useful for tracking and differentiating between different locations/ URL, especially when performing navigation actions such as pushing or replacing a location.

---

In React Router, the location.key is generated and managed internally by the router itself. You cannot define or assign a specific value to the location.key property manually.

When a new location is pushed onto the history stack using React Router's navigation methods (e.g., history.push() or Link component), React Router automatically generates a unique key for that location. This key is used to differentiate between different locations in the history stack.

The purpose of having an automatically generated key is to ensure that each location has a unique identifier. It helps React Router track and manage the state of each location accurately, handle navigation transitions, and perform efficient rendering optimizations.

---

## When to use state?

as we all know URL / Location is a single source of truth and this is the only info that is presisted between two different user.

when a user use a website perform some actions like filtering. that data get stored in the URL. when this user share his URL (which contian query that he performed) with other. when other user open the URL. all the action will be presisted from the previous user meaning they will see the same website with all the filteration appied by the previous user becouse shared URL contain queryFilter too which prev user performed.

state is used when we don't want to presist one user action (filter query) to otherUsers when user share his URL with others.

state is not a source of truth it is just a tempreary data that stays in the user Browser's tab history stack. so it is not sharedable. when user share his URL. tabs history stack data don't get shared with it.

we use state when we want the user to share some data when user go from one URL to another URL of the App. but we don't wan't to presist this data to otherUser (when user share the URL with others).

this data can be filter queries that user had on prev URL when user leave from prev to next URL so when he go back to prev URL he still have those filter setting on.

Now how to use state?

## useLocation Hook?

This hook returns the current location object. this object contains info related to current URL/location.

info like:

1. pathname: start after / end before ?
2. search: start after ?
3. state: data
4. key: key is unique key automatically created by react router when a new location has pushed on th History stack. react router use key to manange location state.(for internal use only)

```js
// page1
let url = 'http://example.com/vans?type=luxury'

// if we use uselocation here location object contain

useLocation = {
  pathname: "/vans",
  search: "?type=luxury",
  hash: null ,
  key: "2JH3G3S",
  state: null
}

<Link to="2" state={{ name: "Kyle" }}>

//user click on the link and directed to "/books" page
//--------------------------------------------------
// page2
// url changes

let url = 'http://example.com/vans/2'

const location = useLocation()

// useLocation return object which contain info about current location.

useLocation object = {
      pathname: "/vans/2",
      search: "",
      hash: "",
      state: {name: "Kyle"},
      key: "2JH3G3S",
}

// Note: here we have state which was passed from page1 <Link>

const search = location.state?.search || ""


<Link
    to={`..${search}`}
    relative="path"
    >
Back to all vans
</Link>

// user clicks this link to go back to previous page
---------------------------------------------------------------------
// page1

URL =  url = http://example.com/vans?type=luxury'

// where to={`..` + ${search}}
// .. = http://example.com/vans
// search = ?type=luxury

```

## Page Not Found

for page not found you can create a route who's path is _ it is like css selector . it select all route but it has the lowest specificity so it will not get selected if a route exisit in the routes list who ultimatly has higher presedence then select all _.

so this is a great tool so creating a 404 page which means if in the routes list of route. any of the route doesn't match with the route avilible then use this route \\\* and show this component.

```js
<route path="*' element={<PageNotFound />}>
```

## Migrate to react router v6.4

react router now support data API layer on top of prev react router. this Data API layer allow you to optimise data fetching from server.

1. `createBrowserRouter,RouterProvider,createRoutesFromElements (for Web)`

   in previous Version we used: BrowserRouter to wrap our <app/> which was use to track history API of the entire APP.

   now, we need to use createBrowserRouter to take advantages of DATA layer API on react router API.

   createBrowserRouter is same as BrowserRouter .It also uses the DOM History API to update the URL and manage the history stack.

   BrowserRouter was a component but createBrowserRouter is a function which take an array of route objects and returns a router object which is same as previous router. but now we need RoterProvider which act as context provider. which has a router prop to attach your router object.

   Note: if you want to provide JSX insted of array of objects to createBrowserRouter function then use createRoutesFromElements

   createRoutesFromElements is a helper that creates route objects from <Route> elements. It's useful if you prefer to create your routes as JSX instead of objects.

   Note: under the Hood in prev <Router/> component when we use to define a list of <Route>. Router component use to convert them into array of route objects. so originally it is just an array of route object. JSX help us to just beutify it.

   ```js
   const router = [{
                       path:"/",
                       element:</component>,
                       children:[
                           {
                               route,
                               route
                           }
                       ]
                   }]
   ```

   ```js
   // New createBrowserRouter with route as object

   import { createBrowserRouter, RouterProvider} from "react-router-dom";

   const router = createBrowserRouter([
     {
       path: "/",
       element: <Root />,
       loader: rootLoader,
       children: [
         {
           path: "team",
           element: <Team />,
           loader: teamLoader,
         }
       ]
     }
   ]);


   return (
       <RouterProvider router={router}>
       <App/>
       <RouterProvider />
   )

   //---------------------------------------------------

   // New createBrowserRouter with route as JSX

   import { createBrowserRouter, RouterProvider, createRoutesFromElements} from "react-router-dom";

   const router = createBrowserRouter(
                       createRoutesFromElements(
                               <Route path="/" element={<Root />}>
                                 <Route path="dashboard" element={<Dashboard />} />
                                 <Route path="about" element={<About />} />
                               </Route>
                       )
                   );


   return (
       <RouterProvider router={router}>
       <App/>
       <RouterProvider />
   )

   //---------------------------------------------------

   // old BrowserRoute

   import { BrowserRouter } from "react-router-dom";

   return(
       <BrowserRouter>
       <App/>
       </BrowserRouter>
   )


   const App = () => {
       return (
           <Router>
           <Route path='' element={}>
           <Route path='' element={}>
           <Route path='' element={}>
           <Route path='' element={}>
           </Router>
       )
   }

   ```

2. `createMemoryRouter (for testing)`

   createBrowserRouter use History API which is a Browser API. during testing Browser API's are not aviliable for use. for that use createMemoryRouter.

   Instead of using the browser's history, a memory router manages its own history stack in memory. It's primarily useful for testing and component development tools like Storybook, but can also be used for running React Router in any non-browser environment.

   createMemoryRouter take 2 argunemnts.

   1. routes (object version or createRoutesFromElements JSX version)
   2. config object

   this config object take two keys

   1. initialEntries: The initial entries in the history stack.
   2. initialIndex: it is a current location. default to the last index

   ```js
   import { RouterProvider, createMemoryRouter } from "react-router-dom";

   const routes = [
     {
       path: "/events/:id",
       element: <CalendarEvent />,
       loader: () => FAKE_EVENT,
     },
   ];

   const router = createMemoryRouter(routes, {
     initialEntries: ["/", "/events/123"],
     initialIndex: 1,
   });
   ```

## Loaders

The purpose of loaders is to provide a way to fetch data or perform necessary operations before rendering route components. This helps ensure that the required data is available before rendering the component, improving the user experience and enabling dynamic content in your React Router application.

Note: loader is for getting Data similer as useQuery() from react query.

- when a user Visit to an specific Rotue path and that path has loader fn attached to it. it will call that fn first and when the data is aviliable with in the loader fn. the component attached to that route will be render. so that render componet will have data aviliable in advance so there is no loading state.

- loader define when the fetch occers not the how the fetch has occered.

```js
function vansData = () => {
    return fetch("URL").then( res => res.json())
}


<route path="/vans" element={<Component/>} loader={vansData}>
```

Note: you can define loader fn directly inside loader prop or create a loaderFn (vansData) and attach it.

- also if there are multiple routes that matches THe URL path means multiple loaders triggering. all of the loaders will run in parallel. Not in sequence one after another save time and faster loading of data to the component better user exprience.

```js
const URL = /vans/protected

<Route path="/vans" loader={} element={}>
<Route path="protected" loader={} element={}/>
</Route>

//both route loader fn will run in parallel.

```

- react router path matching follow URL sequence left to right. if the route sequence match the router loader will execute.

# params

Loader Function can take params object which is same as useParams() Hook. this object contain all the dynamic parameter pass to the current URL path attached to the router. you can use params to fetch different request depending on different dynamic params.

```js
function vansData = ({params}) => {
    return fetch("URL/params.vansid").then( res => res.json())
}


<route path="/vans/:vansid" element={<Component/>} loader={vansData}>
```

## request

Loader function can also take request object (The Request interface of the Fetch API represents a resource request) you can use that to get all the info related to request object when you send a request to the server. but generally you don't need all the info. generally you will use request object to get the URL of the request (current Location) which you can use for making request based of queryParams:

- getting the QueryParams by using URLSearchParams().

```js
function vansData = ({request}) => {
    const urlString = new URL(request.url)
    const searchParams = urlString.searchParams
    const typeFilter = searchParams.get("type")

    return fetch("URL/params.vansid").then( res => res.json())
}
```

# useLoaderData

- useLoaderData gets what loader function returns

Loaders can return any value or object. The data returned by the loader function is then made available to the route component through the useLoaderData hook. Additionally, loaders can return a Fetch Response object directly, allowing you to make requests to APIs or construct responses yourself.

```js
<route path="/vans" loader={vansData} element={<Component />} />;
//-----------------------------------------------------------------
import { useLoaderData } from "react-router-dom";

const Component = () => {
  const data = useLoaderData();

  // return JSX
};
```

---

## Response API

Response API Interface allow you to inteact with the Response Object Returned by the Server For the Request that you made with fetch request.

The Response interface of the Fetch API represents the response to a request.

You can create a new Response object using the Response() constructor, but you are more likely to encounter a Response object being returned as the result of Fetch request.

Response Constructor: new Response(body = (FormData, string) , options = {status, statusText, headers})

Here are some key aspects of the Response interface:

Properties:

- Response.ok: A Boolean indicating whether the response was successful (status in the range of 200-299) or not.
- Response.status: The HTTP status code of the response (e.g., 200 for "OK", 404 for "Not Found").
- Response.statusText: A status message associated with the status code (e.g., "OK" for status 200).
- Response.url: The URL of the response.
- Response.headers: An object representing the response headers, allowing access to specific headers.

Methods:

- Response.formData(): Returns a promise that resolves with the response body as a FormData object.
- Response.error(): Returns a new Response object associated with a network error.
- Response.text(): Returns a promise that resolves with the response body as text.
- Response.json(): Returns a promise that resolves with the response body parsed as JSON.

---

## Loader error handling

when Loader function fetch returns an error or throw an error the element attached to the router will not be render. that error will be handle by errorElement and component attached to errorElement will be render and that component gets the Error data through through useRouteError hook.

```js
function vansData = () => {
    return throw new Error("Error 404")
}


<route path="/vans" loader={vansData} element={<Component/>} errorElement={<Error/>} />
```

## how loader is different then useEffects

loader allow us to extract out the fetching logic out of the React Component so that it can delay the execution of the component untill the fetching is done. useEffects happen inside the react component which means it loads the component first then it make a fetch request (useEffect execute imidiatly after page mount). which leads to bad user exprience become some part of app are visible but others are not. user see the loading state. on the other hand eventhought user have to wait a little. user get the full page not in chunks little by little which makes snappy User exprience for the user. there is no loading time after the page has been loaded. loader eleminate the need of Loading state. and if you attach errorElement (which you should) there is no need to have error state in the page either.

with that there is no need to mantain a state with useState (react) which we needed to becouse page get's render first at that time we don't have the data then fetching happen which gives the data. so we need to mantain the state for fetching data. so when the data comes we re-render the component on the browser so that we can display the data becouse the data was changed. But now due to loader data will always be present in the component. becouse loader makesure of that.

## Error Handleing

### Error Elements

the errorElement refers to a React Component that is rendered when exceptions (Error) are thrown in loaders, actions, or component rendering. When an exception (Error) occurs in any of these areas, instead of following the normal render path defined by the <Route element>, the error path defined by the <Route errorElement> will be rendered.

### How to access Error Data in Error Element Component

The ErrorBoundary component, in this case, utilizes the useRouteError hook to access the error information. It can handle the error and display an appropriate message or perform any necessary error handling logic. The error object obtained from useRouteError contains information about the error that occurred.

### Error Gets BubbleUp in The Route Tree

It's worth noting that if you don't specify an errorElement, the errors will bubble up through parent routes. This means you can handle errors at different levels of granularity. You can have a top-level error element to handle most errors in one place, or you can have error elements on individual routes to handle errors specific to those routes. This provides flexibility in error handling and allows the unaffected parts of the application to continue rendering normally.

Note: It is recommended to always provide at least a root-level errorElement before deploying your application to production. If a specific error is not handled by any errorElement, a default error element will be used, which will print the error message and stack trace which may have an ugly UI and is not intended for end-user consumption.

### Handle Expected Errors

you can manually throw exceptions (Error) in loaders or actions when you encounter specific scenarios. This allows you to handle expected exceptions (Error) and control the error path. By throwing exceptions, you can break the call stack and prevent further execution of the loader or action code. The corresponding errorElement will then be rendered.

```
<Route
  path="/properties/:id"
  element={<PropertyForSale />}
  errorElement={<PropertyError />}
  loader={async ({ params }) => {
    const res = await fetch(`/api/properties/${params.id}`);
    if (res.status === 404) {
      throw new Response("Not Found", { status: 404 });
    }
// if error occers then this code will never get's to read
// and Error will be handle by ErrorElement
    const home = await res.json();
    const descriptionHtml = parseMarkdown(
      data.descriptionMarkdown
    );
    return { home, descriptionHtml };
  }}
/>
```

### Handling Response Error

if you throw any value when an error occurs, it will be provided back to you through the useRouteError hook. However, if you throw a Response object, React Router automatically parses the response data before returning it to your components.

you can create your custom Response Object with json function provided by react-router-dom. which allow you to throw custom response Error and React router provide isRouteErrorResponse (boolean) which you can check that is it a response Error or not.

This allow you to have greater control over showing differrent Error on different Response Error.

## How to make custom Response Object with json Function?

json function takes an object representing the response data as its first argument and an optional object specifying the response status as the second argument. This allows you to throw responses with specific data and status codes.

```js
throw json(response data object, response status object)

---------------------------------------------------------------
import { json } from "react-router-dom";

function loader() {
  const stillWorksHere = await userStillWorksHere();
  if (!stillWorksHere) {
   throw json(
      { error: response.error.data },
      { status: response.status }
    );
  }
}
```

## handle different response errors

after providing json error in your Error element you can check that is it a response Error or not with the help of isRouteErrorResponse (boolean).

you can use response error handling at two level.

1. at errorElement level showing error related to the component. showing Error UI based on the component.
2. at Root level showing general Error with a common UI for all if any error element missses to catch the error.

```js
// Handling Specific Error Cases (at errorElement level)

function ErrorBoundary() {
  const error = useRouteError();

  if (isRouteErrorResponse(error) && error.status === 401) {
    // the response json is automatically parsed to
    // `error.data`, you also have access to the status
    return (
      <div>
        <h1>{error.status}</h1>
        <h2>{error.data.sorry}</h2>
        <p>
          Go ahead and email {error.data.hrEmail} if you feel like this is a
          mistake.
        </p>
      </div>
    );
  }

  // rethrow to let the parent error boundary handle it
  // when it's not a special case for this route
  throw error;
}

//------------------------------------------------------------------------------------
// general Error boundary, usually on your root route, that handles many cases:

import { isRouteErrorResponse } from "react-router-dom";

function RootBoundary() {
  const error = useRouteError();

  if (isRouteErrorResponse(error)) {
    if (error.status === 404) {
      return <div>This page doesn't exist!</div>;
    }

    if (error.status === 401) {
      return <div>You aren't authorized to see this</div>;
    }

    if (error.status === 503) {
      return <div>Looks like our API is down</div>;
    }

    if (error.status === 418) {
      return <div>🫖</div>;
    }
  }

  return <div>Something went wrong</div>;
}
```

## Json

json is generally used inside loader it. it is just a wrapper over data.json() to json(data)

Purpose of the json function: The json function simplifies the creation of a Response object with a JSON payload. It takes an input value, converts it to a JSON string using JSON.stringify, and sets the appropriate headers to indicate that the content is in JSON format.

you can use json directly rather then then(res => res.json()) in fetch request.

```js
new Response(JSON.stringify(someValue), {
  headers: {
    "Content-Type": "application/json; utf-8",
  },
});
------------------------------------------------
import { json } from "react-router-dom";

const loader = async () => {
  const data = await getSomeData();
  return json(data);
};

```

# Chapter:3 Actions and Protected Routes

## Protected Route (pattern)

Protected routes are a common concept in web development and refer to certain routes or endpoints in a web application that require authentication or authorization before access is granted. These routes are typically used to protect sensitive or private information, restrict certain actions to authorized users, or ensure that only authenticated users can access certain functionalities/ Routes.

The implementation of protected routes depends on the web framework or technology being used. Here's a general overview of how protected routes are typically implemented:

Authentication: Before accessing any protected route, users are required to authenticate themselves. This authentication process usually involves providing credentials such as a username and password. Common authentication mechanisms include session-based authentication, token-based authentication (e.g., JSON Web Tokens), or OAuth.

Authorization: Once a user is authenticated, the application needs to determine if the authenticated user has the necessary permissions to access the requested route. Authorization can be role-based, where users are assigned specific roles or privileges, or it can be based on custom access control rules.

Middleware/Route Guards: Web frameworks often provide middleware or route guard mechanisms to enforce authentication and authorization for protected routes. Middleware or route guards are code snippets that intercept incoming requests and perform the necessary checks before allowing the request to proceed. If a user is not authenticated or authorized, the middleware or route guard can redirect them to a login page or return an error response.

Access Control Lists (ACL): In some cases, an Access Control List (ACL) is used to define granular permissions for different users or user roles. An ACL specifies the authorized actions (e.g., read, write, delete) that a user or role can perform on specific resources (e.g., data entities or API endpoints).

By implementing protected routes, web applications can ensure that only authenticated and authorized users can access certain parts of the system. This helps protect sensitive data, maintain the integrity of the application, and enforce user-specific permissions and restrictions.

## Protected Route in context of React router.

Purpose of Protected Routes: Stop data fetching of sensitive information for pretected routes. only allow Logged-In user to access / fetching data from protected Routes.

to protect certain routes it's the React router job to not allow user action to trigger fetching data for a protected route. becouse even if you re-direct the user from a protected route to Login page when user try to open a protected route. the fetching data is available in the browser of the user and can be miss-used by the user with simple code-breaching technique.

in react route when user click on the <Link/> they are directed to a route component.

## How to use react router to Protect sensitive information

### in react router (old) (use LayoutRoute)

<Link<span></span>> direct to a route and your protected component get's render. so we need a middleWare which can deside that this user is authorized to see this route or not.

we can take advantage of Layout routes Parent which has the authentication Logic to check that this user has the acess to visit these routes or not.

Note: in old react router This LayoutRoute apporch works becouse fetching of protected data happens after the Component renders.
if the component don't get's render then No fetching triggers to begain with win win.

if user have the permisition then we will render Child Route by using <Outlet<span></span>/>.
if user don't have permision then we will redirect the user to Login page to get autherization.

```js
// how routes looks generally
/
/host          //Protected Route
/host/vans     //Protected Route
/host/vans/1   //Protected Route
--------------------------------------------------
/
Authrization checker (Layout Wrapper)
    /host          //Protected Route
    /host/vans     //Protected Route
    /host/vans/1   //Protected Route

```

```js
// a real example
// index.jsx

 <Route path="/" element={<h1>Home page</h1>} >

 <Route element={<AuthRequired />}>
      <Route path="protected" element={<h1>Super secret info here</h1>} />
 </Route>

 <Route path="login" element={<h1>The Login page</h1>} />
 </Route>
-------------------------------------------------------------------------------
// AuthRequest.jsx
// conditional render child component

import React from "react"
import { Outlet, Navigate } from "react-router-dom"

export default function AuthRequired() {
    const isLoggedIn = false
    if (!isLoggedIn) {
        return <Navigate to="/login" />
    }
    return <Outlet />
}
```

## How to redirect in React Rotuer (old)

Note: It's usually better to use redirect Function in (loaders and actions) than this useNavigate hook

1. `useNavigate`: this is a hook that returns a Function when ever the function get called the user get's redirected to different rotue.

   ```js
   // Navigate Function can take these argunments:

   NavigateFunction
     (
       to: To,                                     // To is similar to Link to
       options?: {                                 // it can also take an optional Object which contain there properties
         replace?: boolean;                        // replace current location with provided "To" in the History Stack
         state?: any;                              // some extra data you want to pass
       }
     )
     or
     (
       delta: Number               // same as History API History.go(num) + num forward, - num backword in the stack
     )
   ```

   there are two ways you can use navigate fn()

   1. `navigate(To, optional {replace : (Boolean), state: data})`
   2. `navigate(Num)`

   ```js
   import { useNavigate } from "react-router-dom";

   function useLogoutTimer() {
     const userIsInactive = useFakeInactiveUser();
     const navigate = useNavigate();

     useEffect(() => {
       if (userIsInactive) {
         fake.logout();
         navigate("/session-timed-out");
       }
     }, [userIsInactive]);
   }
   ```

2. `<Navigate>` : this is same as useNavigate Hook but this is a component . it can take all the params that useNavigate Hook can take.

when the Navigate Component get's render. Route get's re-directed to different provided route.

Note: delta don't work

```js
<Navigate to="url" />
<Navigate to="url" replace="true" state={data} />
```

## in react router (New) (use redirect in Loader)

first why layoutRoute solution don't work with Loaders.

layoutRoute conditionally render the Child routes and fetching happen after the child Route renders. but in Loader fetching happen when route path matches with the current location. which means we can't prevent fetching of data with layoutRoutes becouse fetching happen before the component mount. not after the component componet.

and Router can run multiple loaders in parallel for fetching data for all the routes that matchs with the location URL.

## How to redirect in React Rotuer (new way) use redirect Fn

we place the authentication logic in all those Route loaders that show protected Component before fetching Logic. and check inside loader and if the user is autherized or not. if the user is not authorized then we will redirect them directly from the loaders which will stop the loader execution in the middle before even reaching to the fetching realted code. which will terminate the loader in middle of the execution and we will never reach to component so component never get's any change to render to begain with.

Current Downside with this approch: we have to apply authentication logic in all of the protected component's loaders.

```JS
const loader = ()=>{
    // authentication Logic
    // if autherize read next line of code
    // if not use redirect Fn to redirect the user to Login page

    // fetching logic
    // return data
}
```

```js
// structure

    /
    /host           + loader with redirect //Protected Route
    /host/vans      + loader with redirect //Protected Route
    /host/vans/1    + loader with redirect //Protected Route

```

```js
// real example

<Route path="/" element={<h1>Home Page</h1>}>
  <Route
    path="protected"
    element={<h1>Super secret info here</h1>}
    loader={async () => {
      const isLoggedIn = false;
      if (!isLoggedIn) {
        throw redirect("/login");
      }
      return null;
    }}
  />
  <Route path="login" element={<h1>Login page goes here</h1>} />
</Route>
```

Note: when user click on <Link> that take the user to protected route. before changing the location of the URL Loader function attached to the Protected Route will be executed. and due to our logic thowing redirect("/login") the URL will change to
("/login") URL we never visited to ("/protected") so there is no stack entrie in the history stack about ("/protected") so when ever we click on back Button on Login Page will be go back to Home page. rather then going to ("/protected") page. it's like we use (replace = True) which have replaced the ("/protected") with ("/login"). but in reality that was happened automatically due to our logic and loader Fn natural behaviour.

## redirect

we can't use <Navigate<span></span>/> or useNavigate() Hook becouse Loader is not a React Component it is just a Function. so for that react router Data API provide use redirect function to do that same. when ever redirect function get called page gets redirected to different page.

Because you can return or throw responses in loaders and actions, you can use redirect to redirect to another route.

redirect Function can take two params (URL, response object)

```
redirect("URL",response object Optional)
redirect("login")
```

you can define Response object too. but generally you don't need it.

```
redirect("/login", {
      status: 301,
      headers: {
        "Content-Type": "text/html",
        "X-Custom-Header": "Some value",
      },
      body: "Redirecting to login...",
    })
```

## Forms in react router v6.4

Forms in react are pretty difficult to work with.

React difficulties:

1. you have to mantain state for every inputfield so when change happen you can re-render the UI to update the Value.
2. you need to check for what user is typing by onChange handler so that you can sync userInput with the react state.
3. you need to sync the update value in the form by providing value = {state} so user can see the changed value.

form difficulties:

1. when you want to submit the form in Javascript/React you have to intercept the request by (preventDefault()). which prevent the Natural behviour of the HTML Form. we then collect all the form name and value by ourSelf and create a request by ourself and send that data to sever with AJAX or Fetch call.

---

### Natural flow of Form Submition in HTML:

Normally in HTML form. form has a action="server URL" and a method="post" attached to it. so when a user submit the Form. The Browser will serealize the Form data automatically and then create a request object add formData in body of the response.
and reads the method and server address and send that request object to the server and then later server send back the response data due to that your page gets updated on the page (Refreshed)

Note: The value of the action attribute can be a relative or an absolute URL. If it is a relative URL, it is resolved relative to the current page's URL. if you don't define action attribute in HTML. the form data will be submitted to the same URL that served the page containing the form. This is known as a "self-submitting" form.

### Javascript/React way of Form submition.

there is no action and method attached to the HTML Form element. when a user submit the Form. we interecpt the natural flow Browser behaviour by preventDefault() and extract out the form data by selection all the input (name:value) and create a request object by ourself which in natural flow handle by the browser and send that request by fetch or AJAX requst.

### React Router way best of Both word.

in React Router we don't use native HTML form we use React router Form component. React Router <Form<span></span>> prevents the browser from sending that request to the server and instead sends the request to your route action fn.

when define method on the React router Form and when a user submit the Form. then browser serealize the request object by reading the method and just before the form is trying to make a server request react router <Form<span></span>> intercpet the request (just like preventDefault()) and provide that request object to the action function. which allow us to take advantage of automatically generated request object by the browser and manipulate that request object and make fetch or AJAX request. without a refresh of the page.

---

in react Router:

React Router allow us to Not follow the React paradigm and follow native Form API to intereact with the form with some additional javascript on top to make it more simpler.

1. No need to maintain state. for re-rendering.
2. No need onChange to sync (becouse there is no state).
3. No need of from sycn with react app with value={state}
4. No need handleSubmit to prevent Natural behavior of form. we will use React Router <Form<span></span>> Componet
   and action fn to handle that.

Compare the Code difference between React vs rect router.

```js
// React version form

import React from "react"
import { useNavigate } from "react-router-dom"

export default function Login() {
const [loginFormData, setLoginFormData] = React.useState({ email: "", password: "" })

const navigate = useNavigate()

function handleSubmit(e) {
    e.preventDefault()
    console.log(loginFormData)
}

function handleChange(e) {
    const { name, value } = e.target
    setLoginFormData(prev => ({
        ...prev,
        [name]: value
    }))
}

return (
    <form onSubmit={handleSubmit}>
        <input
            name="email"
            onChange={handleChange}
            type="email"
            placeholder="Email address"
            value={loginFormData.email}
        />
        <input
            name="password"
            onChange={handleChange}
            type="password"
            placeholder="Password"
            value={loginFormData.password}
        />
        <button>Log in</button>
    </form>
    )
}
//-----------------------------------------------------------------
// React router v6.4 Form

import React from "react"
import { useNavigate, Form } from "react-router-dom"

export default function Login() {
    return (
        <Form>
            <input
                type="email"
                name="email"
                placeholder="Email address"
            />
            <br />
            <input
                type="password"
                name="password"
                placeholder="Password"
            />
            <br />
            <button>Log in</button>
        </Form>
    )
}

```

## Action

Actions in React Router, which provide a way to perform data mutations or "writes" in response to non-GET submissions (e.g., POST, PUT, PATCH, DELETE) made to a specific route. Actions are designed to simplify the process of performing data mutations while abstracting away the complexity of asynchronous UI and revalidation.

Actions are the "writes" (useMutation) to route loader "reads" (useQuery). They are triggered when the app sends a non-GET submission (e.g., POST, PUT, PATCH, DELETE) to a specific route. Actions allow you to define custom logic to handle these submissions and perform data mutations accordingly.

Action are triggered when App sends request to the server to update the server data (POST, PUT, PATCH, DELETE)

Note: Loader are triggered before the Component renders and Action are triggered after the Component render, when component send non-get request.

Note: when action returns you can acess that data inside your component with useActionData() Hook.

similer to Loader, Action take a function which also take object as a parameter which contain params and request object.

```js
<Route
  path="/song/:songId/edit"
  element={<EditSong />}

  action={async ({ params, request }) => {
    let formData = await request.formData();
    return fakeUpdateSong(params.songId, formData);
  }}

  loader={({ params, request }) => {
    return fakeGetSong(params.songId);
  }}
/>
//---------------------------------------------------------------------

<Form method="post">
  <input name="songTitle" />
  <textarea name="lyrics" />
  <button type="submit">Save</button>
</Form>;

// you don't need to pass the action attribute in Form. action fn will automatically get the request object becouse component
// and action fn are defined on the same route. also you need to specify the method to be non-getter method.
```

in loader we generally use Request object to get url and use that url to extract out queryParams.

similarly you can use request in action to extract away the formData from the request object with request.formData() Method. The formData() method reads the request body and returns it as a promise that resolves with a FormData object. which is a webAPI. FormData API interface allow us to intereact with the serealized version of the Form data that was generated by the Browser when user submited the form.

Note: request.formData() returns an promise.

---

## FormData API

The FormData object provides several methods to access and manipulate the form data:

1. `get(name)`: Retrieves the value of a specific form field with the given name.
   Ex: formData.get("username"); // Returns "Chris"

2. `getAll(name)`: Returns an array of all the values associated with a given name.
   Ex: formData.getAll("username"); // Returns ["Chris", "Bob"]

3. `has(name)`: Returns a boolean indicating whether a field with the given name exists in the FormData object.
   Ex: formData.has("username"); // Returns true

4. `append(name, value)`: Appends a new field with the specified name and value to the FormData object.
   Ex: formData.append("username", "Chris");

5. `set(name, value)`: Sets a new value for an existing name inside a FormData object, or adds the name/value if it does not already exist.

   ```js
   formData.set("name", 72);
   formData.get("name"); // "72"
   ```

6. `delete(name)`: Removes the field with the given name from the FormData object.
   Ex: formData.delete("username");

7. `entries()`: Returns an iterator over the form data fields, yielding an array [name, value] for each field.

   ```js
   formData.append("key1", "value1");
   formData.append("key2", "value2");

   // Display the key/value pairs
   for (const pair of formData.entries()) {
     console.log(`${pair[0]}, ${pair[1]}`);
   }
   ```

8. `forEach(callback)`: Executes a provided callback function for each form data field, passing the field's value and name as arguments.

   ```js
   formData.append("key1", "value1");
   formData.append("key2", "value2");

   // Display the key/value pairs
   formData.forEach((value,key)=> ...)

   ```

9. `keys()`: Returns an iterator over the names of the form data fields.

   ```js
   const formData = new FormData();
   formData.append("key1", "value1");
   formData.append("key2", "value2");

   // Display the keys
   for (const key of formData.keys()) {
     console.log(key);
   }
   ```

10. `values()`: Returns an iterator over the values of the form data fields.

    ```js
    const formData = new FormData();
    formData.append("key1", "value1");
    formData.append("key2", "value2");

    // Display the values
    for (const value of formData.values()) {
      console.log(value);
    }
    ```

---

## How to retrive Action data in component.

- use useActionData Hook to access data return by the Action Fn.

you can return anything you want from an action and get access to it from useActionData, you can also return a Response. which you can use to do optimistic update to your UI.

## Action Error handling

if error occers during action request making or response getting. then react router will handle that Error automatically by showing errorElement component rather then showing element componet.

you can allow throw Error manualy my setting condtions. that will be also handle by errorElement.

if an error is found then your action will be break out of the current call stack (stop running the current code) and React Router will start over down the "error path".

## How to handle multiple forms in a single componet action.

simple solution of this is that you need to add extra query of every form submit button which will hep you to filter out which form request we are handling inside action. you can retrive that query from request.formData() object and do the appropriate action depending on the right form.

```js
// component

function Component() {
  let song = useLoaderData();

  // When the song exists, show an edit form
  if (song) {
    return (
      <Form method="post">
        <p>Edit song lyrics:</p>
        {/* Edit song inputs */}
        <button type="submit" name="intent" value="edit">
          Edit
        </button>
      </Form>
    );
  }

  // Otherwise show a form to add a new song
  return (
    <Form method="post">
      <p>Add new lyrics:</p>
      {/* Add song inputs */}
      <button type="submit" name="intent" value="add">
        Add
      </button>
    </Form>
  );
}
//---------------------------------------------------------
// action of the same component

async function action({ request }) {
  let formData = await request.formData();
  let intent = formData.get("intent");

  if (intent === "edit") {
    await editSong(formData);
    return { ok: true };
  }

  if (intent === "add") {
    await addSong(formData);
    return { ok: true };
  }

  throw json({ message: "Invalid intent" }, { status: 400 });
}
```

## React router Form

The <Form> component is a wrapper around a plain HTML form that emulates browser behavior for client-side routing and data mutations. It should not be confused with form validation or state management libraries.

```js
import { Form } from "react-router-dom";

function NewEvent() {
  return (
    <Form method="post" action="/events">
      <input type="text" name="title" />
      <input type="text" name="description" />
      <button type="submit">Create</button>
    </Form>
  );
}
```

Note: Make sure your inputs have names or else the FormData will not include that field's value.

when user submit the Form your Action fn get's kickedIn which will do a lot of async task like making request object (await request.formData()), then fetching that request to the server(await credentialValidation({ email, password })).

these async task will take time depending on the network. during this waiting. you need to tell the user some indication that your request is in Process, disable submit button while request is being process, show some loading state.

in react we create loading state:

```js
const [loading,setLoading] = React.useState("idle")

Try{
   // during fetching
   // setLoading("loading")
}catch{

}Finally{
    // after fetching is resolved
    // setLoading("idle")
}
```

**In react router v6.4**:

if you are using single form in a component + also doing navigation leaving from page to another page when form get submitted then use: useNavigation() hook to manage page navigation and form submission state. this is singleton global navigation hook.

if you are handlling multiple Form in a single component and not navigating away to to other page once the form get submitted then you should use: useFetcher() hook to manage multiple state for each form and also revalidation of the response data automatically + it also handle race-conditon , cancelation of the request of one request is on the fly. etc

## From action

The action prop in the <Form<span></span>> component determines the URL path of the action fn which will be called once the form is submmited.

if you don't specify the action prop on the Form component then action URL path will be default to the closest relative matching route in the context of the <Form<span></span>> component. generally it's the same route in which the form component is rendering as an element.

if you are specifying action path then provide the absolute path.

```js
<Form action="/projects/new" method="post" />
```

Note: when submitting a form we only want to trigger one action fn so action fn path are needed to be exect to be matched. sequence matching does not work.

you also might face action path matching problem: when your child route is index path and parent route has a path. in routing sence both child and parent has the same path so to not trigger wrong action fn use url?index to tell specificaly that you want to trigger child route action fn to trigger not the parent rotue action fn.

## Form method

method determines are you using form for mutation (post,put,patch,delete) updating data on the server or for navigation (get) using it for changing page like a search bar. The default is "get".

if you use method for mutation then after from submission action fn will be called.
if you use form for navigation action function will not be called.

## method get (navigation)

The default method is "get". Get submissions will not call an action. Get submissions are the same as a normal navigation (user clicks a link) except the user gets to supply the search params (Query params) that go to the URL from the form.

when user fill the get method form and submit it. the user input will be serialize by the browser and Current URL will be updated with added query params. that the user has provided.

```js
// example

<Form method="get" action="/products">
  <input aria-label="search products" type="text" name="name" />
  <button type="submit">Search</button>
</Form>
```

user typed in the search bar (Form) = "mobile" and press search btn (submit btn) the current URL will be then updated.
before subbmission URL = www.amazon.com/products
after subbmission URL = www.amazon.com/products?name=mobile
then react router router will look at the routes aviliable are show those component that matches the URL sequence.
it's same as if you have used <Link to="products?name=mobile" <span></span>/> to navigate to products?name=mobile page

## method POST, PUT, PATCH, or DELETE (Mutation)

All other methods are "mutation submissions", meaning you intend to change something about your data with POST, PUT, PATCH, or DELETE.

When the user submits the form, action fn will be called which recive the serialized FormData which you can get from (request.formData). When the action completes, all of the loader data on the page will automatically revalidate (called again) to keep your UI in sync with your server data (loader will load the latest data from the server and stale Data on your app will be replaced automatically).

The method will be available on request.method . you can create logic for differenet methods if you have multiple form on the same page.

```js
<Route
  path="/projects/:id"
  element={<Project />}
  loader={async ({ params }) => {
    return fakeLoadProject(params.id);
  }}
  action={async ({ request, params }) => {
    switch (request.method) {
      case "PUT": {
        let formData = await request.formData();
        let name = formData.get("projectName");
        return fakeUpdateProject(name);
      }
      case "DELETE": {
        return fakeDeleteProject(params.id);
      }
      default: {
        throw new Response("", { status: 405 });
      }
    }
  }}
/>;
//-------------------------------------------------------------------
function Project() {
  let project = useLoaderData();

  return (
    <>
      //---------------------------------Form-1------------------------------
      <Form method="put">
        <input type="text" name="projectName" defaultValue={project.name} />
        <button type="submit">Update Project</button>
      </Form>
      //---------------------------------Form-1------------------------------
      //---------------------------------Form-2------------------------------
      <Form method="delete">
        <button type="submit">Delete Project</button>
      </Form>
      //---------------------------------Form-2------------------------------
    </>
  );
}
```

As you can see, both forms submit to the same route but you can use the request.method to branch on what you intend to do. After the actions completes, the loader will be revalidated and the UI will automatically synchronize with the new data.

## Form replace

replace will replace the current location with the next location in the History stack.

```js
<Form replace />
```

The default behavior replace value is conditional on the form behavior:

**method = get forms default to replace= {false}**

**submission methods depend on the formAction and action behavior**:

- if your action throws Error, then it will default to replace= {false}
- if your action redirects to the current location, it defaults to replace= {true}
- if your action redirects elsewhere, it defaults to replace= {false}
- if your formAction is the current location, it defaults to replace= {true}
- otherwise it defaults to replace= {false}

We've found with get you often want the user to be able to click "back" to see the previous search results/filters, etc.

But with the other methods the default is true to avoid the "are you sure you want to resubmit the form?" prompt. Note that even if replace={false} React Router will not resubmit the form when the back button is clicked and the method is post, put, patch, or delete.

In other words, this is really only useful for GET submissions and you want to avoid the back button showing the previous results.

## Form reloadDocument

When you want to handle form submmision by the default behaviour (reload of page) rather then handleing Form with react router.

```js
<Form reloadDocument />
```

## Form preventScrollReset

If you are using <ScrollRestoration>, this lets you prevent the scroll position from being reset to the top of the window when the form action redirects to a new location.

```js
<Form method="post" preventScrollReset={true} />
```

## Form for Large List Filtering

A common use case for GET submissions is filtering a large list, like ecommerce and travel booking sites. where you provide input field to filter out the sertain type of product.

```js
function FilterForm() {
  return (
    <Form method="get" action="/slc/hotels">
      //---------------------------------- select option
      filter----------------------------
      <select name="sort">
        <option value="price">Price</option>
        <option value="stars">Stars</option>
        <option value="distance">Distance</option>
      </select>
      //---------------------------------- select option
      filter---------------------------- //----------------------------------
      radio btn filter (rating)----------------------------
      <fieldset>
        <legend>Star Rating</legend>
        <label>
          <input type="radio" name="stars" value="5" /> ★★★★★
        </label>
        <label>
          <input type="radio" name="stars" value="4" /> ★★★★
        </label>
        <label>
          <input type="radio" name="stars" value="3" /> ★★★
        </label>
        <label>
          <input type="radio" name="stars" value="2" /> ★★
        </label>
        <label>
          <input type="radio" name="stars" value="1" /> ★
        </label>
      </fieldset>
      //---------------------------------- radio btn filter
      (rating)---------------------------- //----------------------------------
      checkbox btn filter (type of product you want to
      include)----------------------------
      <fieldset>
        <legend>Amenities</legend>
        <label>
          <input type="checkbox" name="amenities" value="pool" /> Pool
        </label>
        <label>
          <input type="checkbox" name="amenities" value="exercise" /> Exercise
          Room
        </label>
      </fieldset>
      //---------------------------------- checkbox btn filter (type of product
      you want to include)----------------------------
      <button type="submit">Search</button>
    </Form>
  );
}
```

When the user submits this form, the form will be serialized to the URL with something like this, depending on the user's selections:

```js
/slc/hotels?sort=price&stars=4&amenities=pool&amenities=exercise
```

you can select these filter option from request object show the component depending on it

```js
<Route
  path="www.trivago.com/hotels"
  loader={async ({ request }) => {
    let url = new URL(request.url);
    let sort = url.searchParams.get("sort");
    let stars = url.searchParams.get("stars");
    let amenities = url.searchParams.getAll("amenities");
    return { sort, stars, amenities };
  }}
  element={<Hotel/>}
/>
-----------------------------------
const Hotel = (){
    const {sort,starts,amenities} = useLoaderData()
    // return ...
}
```

## useNavigation

This hook tells you everything you need to know about a page navigation to build pending navigation indicators and optimistic UI on data mutations.

useNavigation is a Global loading indicator which get's trigger when ever any Navigation happens in the Entire App. it is singleton it means that useNavigation can only handle single navigation state at any time.

Things like:

- Global loading indicators
- Disabling forms while a mutation is happening
- Adding busy indicators to submit buttons
- Optimistically showing a new record while it's being created on the server
- Optimistically showing the new state of a record while it's being updated

useNavigation return an object which has multiple methods on it which helps to

```js
import { useNavigation } from "react-router-dom";

function SomeComponent() {
  const navigation = useNavigation();
  navigation.state;
  navigation.location;
  navigation.formData;
  navigation.formAction;
  navigation.formMethod;
}
```

1. `navigation.state`

For page transition:

- idle - No navigation happeining.
- loading - when going from one page to another page (transition)

```
idle → loading → idle
```

For Form submmition with POST, PUT, PATCH, or DELETE:

- idle - No navigation happeining.
- submmiting - route action is being called.
- loading - when going from one page to another page (transition)

```
idle → submitting → loading → idle
```

2. `navigation.location` = the location that will be navigated to after action fn

3. `navigation.formMethod` = method used on the <Form<span></span>>

4. `navigation.formAction` = action path address.

5. `navigation.formData` = request.formData(), data that form has submitted useful for optimistic updates.

- if Form is using get method then formdata will be undefined.

- Any POST, PUT, PATCH, or DELETE navigation that started from a <Form<span></span>> or useSubmit will have your form's submission data attached to it.

```js
const navigation = useNavigation();

const data = navigation.formData;

const { email, password } = object.fromEntries(data);
```

what if there are multiple navigation happening in the app.(ex: a component with munltiple Form element and some one submit all of them one after the other when one form is on the fly) use navigation will not be able to handle multiple navigaion of the form stata. for that use useFetcher hook. which can handle multiple navigation in concurently made for specially these kind of situations.

## useFetcher

<Link<span></span>> and <Form<span></span>> are navigation event. when we click we go from one page to another similarly when we use <Form<span></span>> with action once the form page get submitted you navigate the user from the form page to another page in actions by redirect.

but in some cases we don't use form as navigation event.

For ex: list app where you submit an input which get added to the list. you don't navigate user to other pages when the form get submitted becouse form and list both are on the same page.

useFetcher is useful when you don't want to navigate away from the currect page, handle multiple concurrent submissions, it also have some great features like calling loader fn on demand (after submit the form you want to show the fresh data on the page for that you can call loader fn) or calling action fn on demand (after submiting the form. most of the time server send response object back which most of the time contain latest data. you can use that by calling action after form submitions)

Fetchers have a lot of built-in behavior:

- Automatically handles cancellation on interruptions of the fetch.

when you are making the same request again while the first one is in the fly useFetcher will automatically cancel rest of the same request. it will keep on cancelling untill currenctly on the fly request get's completed.

Note: Browser also do the same thing by default (cancelling all the request if the same request is on the fly)

but in react/react query you need to add abortContorller signal to handle that same thing.

- When submitting with POST, PUT, PATCH, DELETE, the action is called first
  - After the action completes, the data on the page is revalidated to capture any mutations that may have happened, automatically keeping your UI in sync with your server state

useFetcher comes with useFetcher.data property which is the data recived from the server (The response Data). after form submition happens action fn get called first and action fn get executed and recived data from the server. later that data can be access by userfetcher.data to update the data on the UI. all of this happens automatically which makes your page data fresh/up-to-date with the server.

Revalidation means making the currect data stale and making a request on the server to get the updated data. revalidation means invalidation for certain query who's data is out-of-date/stale/not-fresh. in react router revalidation happens automatically.

- When multiple fetchers are inflight at once, it will
  - commit the freshest available data as they each land
  - ensure no stale loads override fresher data, no matter which order the responses return

with useFetcher you can call loader Fn of that component to get the freshest data so that you can revalidate it. but there can be a problem of race conditions.

useFetcher when you do multiple submission at the same time. it can cause reace conditions. for revalidation of the loader fn. becouse there are multiple request are being made to the server to update the server data. but there are multiple revalidation happening meaning loader keep on sending request to send the updated fresh data.

so.. let's say you made 3 consicutive submision. which will coz

1. Action-1 updates the server then loader-1 it take some time
2. Action-2 updates the server then loader-2 it take some time
3. Action-3 updates the server then loader-3 it take some time

loader-1 and loader-2 get request will be canceled by the react router becouse action-3 request to update the server is the last one so loader-3 will bring the latest data from the server. there is no need to wait for loader-1, loader-2. this will avoid the race conditon.

beocuse what if loader-1, loader-2, loader-3 start to make a request to the server but loader-3 will come first then loader-2 come second and loader-1 will be the last which has outdated data from the server cozing our app to go out of sync from the server.

- Handles uncaught errors by rendering the nearest errorElement (just like a normal navigation from <Link<span></span>> or <Form<span></span>>)

- Will redirect the app if your action/loader being called returns a redirect (just like a normal navigation from <Link<span></span>> or <Form<span></span>>)

let's Learn about different property that useFetcher comes with:

1. `fetcher.state`

   - `idle` - nothing is being fetched.
   - `submitting` - A route action is being called due to a fetcher submission using POST, PUT, PATCH, or DELETE
   - `loading` - The fetcher is calling a loader (from a fetcher.load) or is being revalidated after a separate submission or useRevalidator call

2. `fetcher.Form`

   Just like <Form<span></span>> except it doesn't cause a navigation. use it when you don't want to coz navigation event to trigger. you can handle all the Form navigation with fetcher.state.

   ```js
   function SomeComponent() {
     const fetcher = useFetcher();
     return (
       <fetcher.Form method="post" action="/some/route">
         <input type="text" />
       </fetcher.Form>
     );
   }
   ```

3. `fetcher.load()`

   you can call loader fn by specifing loader router URL path. fetcher.load(absolueURL). and then inside component you can use fetcher.data to recive the return value send by the loader Fn of that router.

   Note: your provided URL will only trigger loader that match the exect path. it will not trigger loader that partically match the URL. also your router is nested and has index path then add URL?index to match the index path router.

   ```js
   <Route
         path="login"
         element={<Login />}
         action={loginAction}
         loader={loginLoader}
       />
   //--------------------------------------------------------
   export function loader(){
       return "loader"
   }
   export function action(){
       return "action"
   }

   function Login() {
       const fetcher = useFetcher()

       if(fetcher.state === "idle" && !fetcher.data){
           fetcher.load("/login")
       }

     return (
           <fetcher.Form method="post" replace>
               <h2>ListName</h2>
               <input
                   type="name"
                   name="name"
               />
               <br />

               <button>
               submit
               </button>
           </fetcher.Form>
           {fetcher.state === "idle" && fetcher.data && <listItem name={fetcher.data.name}>}
     )
   }


   ```

4. `fetcher.submit()`

   same as useSubmit allow programmer to submit the form programatically rahter then relying on the user submiting the fetcher.Form.

   take same paramaters as useSubmit do.

5. `fetcher.data`

   - fetcher.data return the response data that you recive from the server.
   - fetcher.data is the return value from the action fn or the loader fn (if you initiate fetcher.load(url))
   - fetcher.data presist the data Once the data is set, it persists on the fetcher even through reloads and resubmissions.

   it means data don't get blank when state is changing.

   with non-navigation form use fetcher.data otherwise you need to use useLoaderData or useActionData

   fetcher.data get updated automatically once the action response has returned and fetcher.data get's updated with it.

   ```js
   // revalidation cycle automatically
   Form submitted > action get called > action return > fetcher.data updated.
   ```

   if you call fetcher.load then first fetcher.data data get updated from the action return then loader return.

   ```js
   // revalidation cycle with fetcher.load()
   Form submitted > action get called > action return > fetcher.data updated > loader return > fetcher.data upadated
   ```

6. `fetcher.formData`

   - fetcher.formData === navigation.formData

   - the form data that you send in the request object. you can use it to do optimistic updates on the page before revalidation occers.

   - When using <fetcher.Form<span></span>> or fetcher.submit(), the form data is available to build optimistic UI.

7. `fetcher.formAction` : Tells you the action url the form is being submitted to.

8. `fetcher.formMethod` : Tells you the method of the form being submitted: get, post, put, patch, or delete.

## useFetchers (array of useFetcher)

useFetchers hook return array of useFetcher which are in the fly at this movement.

each instance of useFetcher will have these porperties attached to them:

- fetcher.state
- fetcher.data
- fetcher.formData
- fetcher.formAction
- fetcher.formMethod

these properties are not included in useFetcher instances:

- fetcher.load
- fetcher.submit
- fetcher.Form

useFetchers hook is provided to sync your other component optimistic update with the Fom component.

The purpose of useFetchers in the given example is to allow components that did not create the fetchers themselves to access and utilize the information from those fetchers. This is useful for implementing optimistic UI, where the user interface immediately updates based on the assumption that the server will eventually process the submitted data successfully.

In the example, there are two components: Task and ProjectTaskCount. The Task component represents a checkbox for a specific task, and it uses useFetcher to create a fetcher for updating the task's state. The ProjectTaskCount component represents the sidebar that displays the number of completed tasks for a project.

By utilizing useFetchers in the ProjectTaskCount component, it can access the inflight fetcher states from the checkboxes, even though it did not create those fetchers itself. This allows the ProjectTaskCount component to immediately update the count of completed tasks based on the optimistic versions of the task states.

The ProjectTaskCount component performs the following steps:

- It iterates through the fetchers obtained from useFetchers and identifies the fetchers associated with tasks in the specific project.
- It uses the fetcher.formData to determine the optimistic version of the task state and increments the count of completed tasks accordingly.
- If a task does not have an inflight fetcher, it falls back to using the normal version of the task's state.

Note: ProjectTaskCount component and the sidebar component are both rendered on the same URL or route, any updates or changes made by the ProjectTaskCount component will reflect immediately in the sidebar. useFetchers hook is only useful for doing optimistic updates.

In summary, useFetchers allows components to access and participate in the optimistic UI behavior by providing access to the inflight fetcher states created by other components. This enables a smoother and more responsive user experience by updating the UI immediately, even before the server processes the submitted data.

## useSubmit

useSubmit hook allow programmer to submit the form programatically rahter then relying on the user submiting the form.

Note: useSubmit hook will automatically be attached to the form of the component in which we are useing it.

useSubmit hook take two paramters.

1. dependency event, you can define an event when which occers will submit the form or formData can also be added to submit it to the form.

2. form options object, any options that we can define one <Form ...here...> can be added

   ```js
   submit(null, {
     action: "/logout",
     method: "post",
   });

   // same as
   <Form action="/logout" method="post" />;
   ```

### Use Cases of useSubmit Hook.

1. when ever user type inside input field submit the Form.

   ```js
   // input element events
   <input onChange={(event) => submit(event.currentTarget)} />;

   // with React ref
   let ref = useRef();
   <button ref={ref} />;
   submit(ref.current);
   ```

2. add some data in the form programatically

   ```js
   let formData = new FormData();
   formData.append("cheese", "gouda");
   submit(formData);
   ```

3. automatically sign someone out of your website after a period of inactivity

   ```js
   submit(null, { method: "post", action: "/logout" });
   ```

---

Form are just navigation Event. when user sucessfully submited the form and they are logedIn then they should be re-directed to the protected Page. you can diffine the logic of redirecting in the action after validating the Form data (user:password). if the password is currect then you can use redirect to take them to the currect Protected page. if the credential are not currect then return a error message to the Form from action.

you should conditionally redtect the user: if the user credential are currect then redirect if not then show the Error message on the form page.

Note: you can use Form replace (<Form replace<span></span>>) machanism so when re-direct happen the form page is will be replaced by the redirected page in the history stack.

Note: replace will replace the currect location with the next (redirect) Location. after the redirect happens. if you don't leave the login page then No replace will happen

## Index Query Param (?index)

when navigating (page to page) it's ok to execute all the loader that match the URL path sequence but when submiting form calling the action of the form and executing a loader of the form we want to execute sepecific route path not the relavtive path becouse we want to execute the one action or one loader of the form not all the actions that match the path.

1. when you provide action path in form

   ```js
   <Route path="/projects" element={<Projectlayout/>} action ={actionProjectsLayout}> // action-1
   <Route index element={<ProjectsPage/>} action ={actionProjectsPage}> // action-2
   <Route />
   //------------------------------------------------------------------------
   <Form method="post" action="/projects" />; this will trigget action-1
   <Form method="post" action="/projects?index" />; this will trigger action-2
   ```

   The ?index param will submit to the index route, the action without the index param will submit to the parent route.

2. when action path of the form is not defined (intrensic)

   When a <Form<span></span>> is rendered in an index route without an action, the ?index param will automatically be appended so that the form posts to the index route. The following form, when submitted, will post to /projects?index because it is rendered in the context of the projects index route:

   ```js
   function ProjectsPage() {
     return <Form method="post" />;
   }
   ```

## useRevalidator

in react router revalidation happens automatically when you use <Form<span></span>>, useSubmit, or useFetcher. but in some case you might need to revalidate programmatically. for that useRevalidator is used.

While you can render multiple occurrences of useRevalidator at the same time, underneath it is a singleton. This means when one revalidator.revalidate() is called, all instances go into the "loading" state together (or rather, they all update to report the singleton state).

Race conditions are automatically handled when calling revalidate() when a revalidation is already in progress.

If a navigation happens while a revalidation is in flight, the revalidation will be cancelled and fresh data will be requested from all loaders for the next page.

```js
import { useRevalidator } from "react-router-dom";

let revalidator = useRevalidator();
```

useRevalidator comes with two fetures.

1. `revalidator.state`:

   - idle
   - loading

   This is useful for creating loading indicators and spinners to let the user know the app is thinking.

2. `revalidator.revalidate()`: This initiates a revalidation.

   ```js
   function useLivePageData() {
     let revalidator = useRevalidator();
     let interval = useInterval(5000);

     useEffect(() => {
       if (revalidator.state === "idle") {
         revalidator.revalidate();
       }
     }, [interval]);
   }
   ```

   data will be fetched in every 5 sec. it more like staleTime: 5000.

## shouldRevalidate

shouldRevalidate is a function which is used to opt out of revalidation behaviour if you don't want it.

shouldRevalidate fn take currentURL as params and if it return false then loader fn will not be called no revalidation will happen on that route

There are several instances where data is revalidated, keeping your UI in sync with your data automatically:

- After an action is called from a <Form<span></span>>.
- After an action is called from a <fetcher.Form<span></span>>
- After an action is called from useSubmit
- After an action is called from a fetcher.submit
- When the URL params change for an already rendered route
- When the URL Search params change
- When navigating to the same URL as the current URL

If you define shouldRevalidate on a route, it will first check the shouldRevalidate function before calling the route loader for new data. If the function returns false, then the loader will not be called and the existing data for that loader will persist on the page.

```js
<Route
  path="meals-plans"
  element={<MealPlans />}
  loader={loadMealPlans}
  shouldRevalidate={({ currentUrl }) => {
    // only revalidate if the submission originates from
    // the `/meal-plans/new` route.
    return currentUrl.pathname === "/meal-plans/new";
  }}
/>
```

Note: only already loaded data get's revalidated. if the loader fn getting executed for the first time then shouldRevalidate fn has no affect on that.

## <ScrollRestoration />

This component will emulate the browser's scroll restoration on location changes after loaders have completed to ensure the scroll position is restored to the right spot, even across domains.

You should only render one of these and it's recommended you render it in the root route of your app:

```js
function App() {
  return (
    <>
      <ScrollRestoration />
      other components
    </>
  );
}
```

ScrollRestoration comes with one optional prop option.

1. `getKey`:

   ```js
   // default behaviour

   <ScrollRestoration
     getKey={(location, matches) => {
       // default behavior
       return location.key;
     }}
   />
   ```

   scrollRestoration will store scroll position of the page with refrence to the location key of the page.

   in history stack every time an entry (location) is added to the stack. for tracking that entry (location) browser assign a key to it. so it can track that location.

   by default scrollRestoration store it's scroll position in refrence to the location key means. if the user go back and forth in the history stack user will be scrolled down to that palce where they were in that location page.

   but the thing is every entry in the histry stack generalte unique key eventthough location can be same. you can chane this behaviour by defining the getkey to take refrence to location.path not the key in the histry stack.

   ```js
   // location.key

   1 / home; // user visit home scroll go down
   2 / messages; // user visit messages scroll go down. if user go back by back button he will be placed at home down.
   3 / home; // but user visit home page by a link then user will be place at up.
   //-----------------------------------------------------------------------------------------
   // location.pathname

   1 / home; // user visit home scroll go down
   2 / messages; // user visit messages scroll go down. if user go back by back button he will be placed at home down.
   3 / home; // but user visit home page by a link then still user will be place at down posotion.
   ```

   ```js
   <ScrollRestoration
     getKey={(location, matches) => {
       return location.pathname;
     }}
   />
   ```

   you may want to only use the pathname for some paths, and use the normal behavior for everything else:

   ```js
   <ScrollRestoration
     getKey={(location, matches) => {
       const paths = ["/home", "/notifications"];
       return paths.includes(location.pathname)
         ? // home and notifications restore by pathname
           location.pathname
         : // everything else by location like the browser
           location.key;
     }}
   />
   ```

### Preventing Scroll Reset

ScrollRestoration component provides a way to prevent scroll reset when navigating using links or forms. You can use the preventScrollReset prop with the <Link<span></span>> and <Form<span></span>> components

```js
<Link preventScrollReset={true} />
<Form preventScrollReset={true} />
```

This prevents the scroll position from being reset to the top of the page when those links or forms are clicked.

### Scroll Flashing

Note: without server-side rendering frameworks like Remix, there may be some scroll flashing on initial page loads. This is because scroll restoration cannot occur until the JavaScript bundles are downloaded, data is loaded, and the full page has rendered. Server rendering frameworks can handle this more effectively by sending a fully formed document on the initial load, allowing scroll restoration when the page first renders.

## deferred Data, defer Await and React.Suspance

generally we use loader fn which eleminate the need of loading state becouse loader fn fetch data from the sever as user navigate to a page(component) all the loader related to that page will be triggered and fetch data once the data has arived in the loader fn the component get's render on the screen.

as you can notice that before rendering the component we wait some Time and how much time we wait it depends on loader fn. how much time it take for loader fetcher fn to recive data from the server.

what if it take 3 sec or above time to retrive data from the server. the user who has clicked on a link to navigate to that page will have to wait 3 sec before seeing any change happening on the screen.

till those 3 sec these no indication. user can be confused that did he click on the button to navigate to other page or not, due to loader there is no loading state. No indication from the user that data is being loaded plz wait.

so we can simply load data inside component as we used to do before loaders show loading state or skeleton UI before showing that data. once the data arives we can show the data. this works right!!!

what if there are multiple component on that page which fatches to multiple server? you will face water fall effect problem?
fetching happening one after another in sequence. we can use promise.all to fetch all the request in parallel!!!

but promise.all will wait untill all the request are completed. which means if we have 3 request:

1. request 1 take 30 sec
2. request 2 take 20 sec
3. request 3 take 1 min

total time promiss.all will take 1 min. untill then all the component will be in the loading state.

Solution:

## defer with loader fn

defer is advancement over promise.all. where in promise.all run all the promises in parallel but it locks in the use of these promise untill all the promises are resolved. here is what defer is helful . defer + loader fn give us some freedom.

when user visit a page all the route that match the sequence will run there loader fn. it means if there are multiple route match there will be multiple loader fn running in the parallel (which replace the need of promise.all) but with defer eventhough all the promise are running in the parallel what if these promise take too much time to load data. in that case we can use defer.

if we know that this promise take too much time (like 1 min) and this promise take less time (10 sec) then we can defer the 10 sec loader promise and after 10 sec user has some UI they can see while they can wait for main content to arive.

so if we have same request sinerio:

1. loader-1 request 1 take 30 sec (defer) -
2. loader-2 request 2 take 20 sec (defer) | all the loader are running in the parallel.
3. loader-3 await request 3 take 1 min -

as soon as data has arived in the loader fn (1,2,3) that will be transfered to the component. so request 2 will be shown on page after 20 sec and request 1 will be shown 10 sec after the request 2 and request 3 will be shown 30 sec later to the request 1. due to that user have something on the UI to see untill all the data is being fetch.

React router deferred machanism solve both problem:

1. Your data is no longer on a waterfall: document -> JavaScript -> Lazy Loaded Route & data (in parallel)
2. Your code can easily switch between rendering the fallback and waiting for the data

## <Await> (before we understand how defer works)

Await component is used to render deferred values with automatic error handling.

```js
import { Await, useLoaderData } from "react-router-dom";

function Book() {
  const { book, reviews } = useLoaderData();
  return (
    <div>
      <h1>{book.title}</h1>
      <p>{book.description}</p>
      //------------------------------------------------------------
      <React.Suspense fallback={<ReviewsSkeleton />}>
        <Await
          resolve={reviews}
          errorElement={<div>Could not load reviews 😬</div>}
          children={(resolvedReviews) => <Reviews items={resolvedReviews} />}
        />
      </React.Suspense>
      //------------------------------------------------------------
    </div>
  );
}
```

Note: <Await> expects to be rendered inside of a <React.Suspense> or <React.SuspenseList> parent to enable the fallback UI.

Await act as seprate component. you can provide errorElement,resolve,children.

## children

similar to react component it can also take children. it's child can be:

- React elements
- function

once the deferred unresolvedPromse get resolve that resolved promise value
is given to childrens

if you use function as a child then that function only parameter is resolvedPromise. that function recive that value from the resolve

```js
<Await resolve={reviewsPromise}>
  {(resolvedReviews) => <Reviews items={resolvedReviews} />}
</Await>
```

if you use react component as a child then you can use useAsyncValue hook to recive the resolvedPromise value inside your react component.

```js
<Await resolve={reviewsPromise}>
  <Reviews />
</Await>;
---------------------------------------------
function Reviews() {
  const resolvedReviews = useAsyncValue();
  return <div>{/* ... */}</div>;
}
```

## errorElement

you can provide optional errorElement which get triggers when Promise Provided get's rejected.

you can use useAsyncError() hook to recive error object inside your errorElement which will hep you render error when error occers in the deferred UnresolvedPromise.

```js
<Await resolve={reviewsPromise} errorElement={<ReviewsError />}>
  <Reviews />
</Await>;

function ReviewsError() {
  const error = useAsyncError();
  return <div>{error.message}</div>;
}
```

Note: If you do not provide an errorElement, the rejected value will bubble up to the nearest route-level errorElement and be accessible via the useRouteError hook.

## resolve

Takes a unresolvedPromise returned from a deferred loader value. it will wait untill the unresolvedPromise get resolved once it get resolved. if Promise is fulfilled (recived the data) then that promise is given to the Childrens. if the promise is reject then it is handle by the errorElement.

```js
<React.Suspense fallback={<ReviewsSkeleton />}>
  <Await
    resolve={reviews}
    errorElement={<div>Could not load reviews 😬</div>}
    children={(resolvedReviews) => <Reviews items={resolvedReviews} />}
  />
</React.Suspense>
```

## how to use defer

```js
import {defer, useLoaderData, Await} from "react-router-dom";


//------------------------------------------------------------------------
// here we are defering regularContent only
// why regularContent becouse regularContent is not so importent (some text) we want to show this as soon as it's done retuning the promise
// mainContent will be provided after it is resolved.

// return defer({}) we are returning both of the promises (at the time useLoaderData recive this object both are unresolved promises)

export function loader (){
    const mainContent = await fetch(URL)          // regular promise take 3 sec to load
    const regularContent = fetch(URL)             // defer promise

    return defer({
        mainContent,
        regularContent
    })
}

//--------------------------------------------------------------------------
//useLoaderData return both unresolved promises in the component. these unresolved promise act as a placeholder for when they
//get resolved.

//<React.Suspence> will show the fallback UI untill unresolved Promises get resolved.

//<Await> take resolve prop which take an un-resolved promise and wait and track the unresolve promise untill get resolved
//once the unresolved promise get resolved Await component provide the resolved Promise to it's Children Component. here we are
//using Function.

//once the unresolve promise get resolved the child Component UI get's render and <Suspence> fallback will be removed.
//Note: suspence fallback UI will only work for the inital render of the component.


export Default function Component(){
    const {mainContent,regularContext} = useLoaderData()

return(
    <>
     <section className="weather-container">
        <h1>Weather in Salt Lake City</h1>
//-------------------------- Main Content ------------------------------------
                <React.Suspense fallback={<p>Loading...</p>}>
                    <Await resolve={mainContent.data}>
                        {
                            (mainData) => {
                            return <>
                                    {mainData.map( item => return <H2> item.name  </H2>)}
                                </>
                                }
                        }
                    </Await>
                </React.Suspence>
//-------------------------- Main Content ------------------------------------
//-------------------------- reguler Content ------------------------------------
                <React.Suspense fallback={<p>Loading...</p>}>
                    <Await resolve={regularContent.data}>
                        {
                            (regData) =>{
                                return <>
                                        {regData.map( location => return <H2> location.steet </H2>)}
                                    </>
                            }
                        }
                    </Await>
                </React.Suspence>
//-------------------------- reguler Content ------------------------------------
        </section>
    </>
)

}
```

you can use await keyword to define which promise you want to handle as regular promise (non-defered) or which promise you want to define as defer one which is without await keyword.

loader will provide await promise once they are resolved and loader will provide non-await (defer) as soon as they are ready.

## Why Not defer every promise By default?

we use defer to show some content to the user untill main content is on the fly.
if we have two promise (1,2) and we know that promise (1) take longer then promise (2). then we can defer promise (2) can show that promise data to the user. so that these is something for using to see on the browser untill the main content load.

you should Not defer every promise becouse if you do then you will see alot of layout shift on the page which is bad for the SEO.
becouse every defer promise will be resolved one after an other and your UI will jump alot which is bad for seo reason.

it's better to be selective or it's depends on what type of site it is.

By default, React Router does not defer everything because different applications have different priorities and performance considerations. The defer API gives you the ability to make choices based on your specific needs and goals.

Here are some scenarios mentioned in the documentation:

- Rendering Speed: If you prioritize faster initial rendering of your page, you can choose to defer certain components or data loading processes. This means that these elements will be loaded and rendered asynchronously, allowing the rest of the page to be displayed more quickly.

- CLS (Content Layout Shift): CLS is a metric that measures the visual stability of a page. If you want to minimize CLS and ensure a smoother user experience, you may choose not to defer certain components or content that could cause layout shifts. This ensures that the page layout remains stable during the loading process.

- Finding the Right Balance: You may have a goal of achieving both faster rendering and a lower CLS. In such cases, you can selectively defer only the slow and less important components or data loading processes, while ensuring that critical or layout-sensitive elements are rendered synchronously.

## When does the <Suspense/> fallback render?

The <Await<span></span>> component, when used with an unresolved promise, will throw the promise up to the nearest <Suspense<span></span>> boundary. This means that the fallback content specified within the <Suspense<span></span>> component will be displayed while the promise is being resolved. However, it's important to note that the fallback will only be rendered during the initial render of the <Await<span></span>> component with an unresolved promise. It will not re-render the fallback if the props of the <Await<span></span>> component change.

The documentation provides an example to clarify this behavior. Suppose you have a form submission and data loading process happening within the same route. In this case, if the user submits the form and the data is revalidated, the fallback content will not be rendered. The fallback is only rendered when the user navigates to the same route with different parameters. For example, if the user selects different options from a list and the content on the page needs to be updated based on those options.

This behavior may seem counter-intuitive at first, but the reason behind it is to ensure compatibility with existing optimizations, such as Optimistic UI for form submissions and revalidation. The deferred API is designed to allow you to easily switch between deferring certain data loading processes and not deferring them. By maintaining the existing behavior, it prevents situations where form submissions trigger the fallback loading state instead of the expected optimistic UI.

## Why don't Response objects returned by the loader work anymore?

When you use the defer API, you're indicating that the page should be loaded immediately without waiting for the deferred data to be resolved. However, this means that the Response objects returned by the loader are not automatically processed in the same way as if you had used return fetch(url) directly.

Instead, you need to handle the Response processing yourself and resolve the deferred Promise with the appropriate data:

1.  if the Response contains JSON data, you can use the json() method to extract the data and resolve the Promise with it:

    ```js
    async function loader({ request, params }) {
      return defer({
        // Resolve with the response data
        data: fetch(url).then((res) => res.json()),
      });
    }
    ```

2.  if the deferred data could potentially return a redirect Response, you can detect the redirect, extract the necessary information (such as the status code and location), and resolve the Promise with that data.

    ```js
    async function loader({ request, params }) {
      let data = fetch(url).then((res) => {
        if (res.status == 301) {
          return {
            isRedirect: true,
            status: res.status,
            location: res.headers.get("Location"),
          };
        }
        return res.json();
      });

      return defer({ data });
    }
    ```

## understand render + fetch chain (don't read it)

We've learned that fetching in components is the quickest way to the slowest UX (not to mention all the content layout shift that usually follows).

When you fetch inside of components, you create what we call render+fetch chains that artificially slow down your page loads and transitions by fetching several data dependencies in sequence instead of in parallel.

```js
// URL = /projects/123

<Routes>
  <Route element={<Root />}>
    <Route path="projects" element={<ProjectsList />}>
      <Route path=":projectId" element={<ProjectPage />} />
    </Route>
  </Route>
</Routes>
//----------------------------
function Root() {
  let data = useFetch("/api/user.json");

  if (!data) {
    return <BigSpinner />;
  }

  if (data.error) {
    return <ErrorPage />;
  }

  return (
    <div>
      <Header user={data.user} />
      <Outlet />
      <Footer />
    </div>
  );
}
//----------------------------
function ProjectsList() {
  let data = useFetch("/api/projects.json");

  if (!data) {
    return <MediumSpinner />;
  }

  return (
    <div style={{ display: "flex" }}>
      <ProjectsSidebar project={data.projects}>
      <ProjectsContent>
        <Outlet />
      </ProjectContent>
    </div>
  );
}
//----------------------------
function ProjectPage() {
  let params = useParams();
  let data = useFetch(`/api/projects/${params.projectId}.json`);

  if (!data) {
    return <div>Loading...</div>;
  }

  if (data.notFound) {
    return <NotFound />;
  }

  if (data.error) {
    return <ErrorPage />;
  }

  return (
    <div>
      <h3>{project.title}</h3>
      {/* ... */}
    </div>
  );
}


```

Note: all the fetching request are sync (which will block the UI)

Component fetching like this makes your app ridiculously slower than it could be. Components initiate fetches when they mount, but the parent component's own pending state blocks the child from rendering and therefore from fetching!

what it is saying due to fetching request are sync fetching is a part of rendering (sync task) so untill parant component get fully render . rendering of child component can not start. this create a water fall-effecct.

This is a render+fetch chain. All three fetches in our sample app could logically go out in parallel, but they can't because they're coupled to UI hierarchy and blocked by parent loading states.

If each fetch takes one second to resolve, the whole page takes at least three seconds to render! This is why so many React apps have slow loads and slow transitions.

```
// Page render time
grandParant          ===================
Parant                                   ===================
child                                                         ===================

// total time = (grandParant + parent + child) render time
// if each component takes fetching time 1 sec then total render time will be atleast 3 sec.
```

But introducing a Data Router allows you to parallelize your fetches and render everything all at once by loader fn (promise.all)

where is loader fetching happen in parallel and then component rendering start. you save alot of time + No layout shift.

```
// Page render time
grandParant          ===================
Parant               ===================
child                ===================

// if each component takes fetching time 1 sec are fethcing are happening in parallel then total render
// time will be atleast 1 sec.
```

---

## Extra: from Chat GPT

The term "render + fetch chain" refers to a sequence of events that occur when fetching data within a component during the rendering process. This chain involves the rendering of a component followed by the initiation of a fetching request to retrieve data.

Here's a breakdown of the render + fetch chain:

Rendering: When a component is mounted or updated, React triggers a rendering process. This process involves creating a virtual representation of the component's user interface, known as the virtual DOM.

Fetching: During the rendering process, a component may initiate a fetching request to retrieve data from an API or another data source. This fetching request is usually asynchronous, meaning it occurs in the background while the rendering process continues.

Rendering Blocking: If the fetching request initiated within the component is synchronous or if the component is waiting for the fetched data before it can complete its rendering, it can cause blocking. This means that the rendering process of the component, as well as any child components, will be delayed until the fetching request is resolved.

Data Availability: Once the fetching request is completed and the data is available, the component can access and use that data within its rendering process. This allows the component to display the fetched data as part of its user interface.

Rendering Continuation: After the fetched data is obtained, the rendering process continues, updating the virtual DOM and potentially triggering further rendering updates in child components.

The render + fetch chain represents the sequential flow of rendering and fetching within a component hierarchy. It's important to note that the rendering process is usually asynchronous and optimized for efficiency, but the presence of synchronous fetching or blocking can introduce delays and impact the overall performance of the application.

To optimize the rendering + fetching chain, it's recommended to handle fetching asynchronously, allowing components to render without being blocked by data retrieval.

---

## lazy loading with react

react lazy loading also suffers from waterFall effect.

let's say you have 3 component grandParent, parent, child. they all are nested.

```js
// grandParent
const LazyParentComponent = React.lazy(() => import("./ParentComponent"));

function App() {
  return (
    <React.Suspense fallback={<p>Loading lazy chunk...</p>}>
      <LazyParentComponent />
    </React.Suspense>
  );
}
//-------------------------------------------------------------------
// parent
const LazyChildComponent = React.lazy(() => import("./ChildComponent"));

function App() {
  return (
    <React.Suspense fallback={<p>Loading lazy chunk...</p>}>
      <LazyChildComponent />
    </React.Suspense>
  );
}
//--------------------------------------------------------------------
// child

function App() {
  return <childComponent />;
}
```

even though these are async task. untill grandParent component get's render. we can't initiate lazy Loading of parentComponent. untill we render parentComponent we can't initiate lazy loading of childComponent.

they all are dependent of each other. follow certian sequence. coz a waterfall effect

```
// Page render time
grandParant          ===================
Parant                                   ===================
child                                                         ===================

// total time = (grandParant + parent + child) render time
// if each component takes 1 sec to lazyload then total render time will be atleast 3 sec.
```

## Route Lazy soution

If we want lazy-loading to play nicely with data routers, we need to be able to introduce laziness outside of the render cycle. Just like we lifted data fetching out from the render cycle, we want to lift route fetching out of the render cycle as well.

If you step back and look at a route definition, it can be split into 3 sections:

- Path matching fields such as (path, index, and children)
- Data loading/submitting fields such (loader and action)
- Rendering fields such as (element and errorElement)

The only thing a data router truly needs on the critical path is the path matching fields, as it needs to be able to identify all of the routes matched for a given URL.

```js
// app.jsx
import Layout, { getUser } from `./layout`;
import Home from `./home`;

<Route path="/" loader={getUser()} element={<Layout/>}>
    <Route path="projects" lazy={() => import("./projects")} />  // 💤 Lazy load!
        <Route path=":projectId" lazy={() => import("./project")} /> // 💤 Lazy load!
    </ Route>
</ Route>

//----------------------------------------------------------------------
// projects.jsx
export function loader = () => { ... }; // formerly named getProjects

export function Component() { ... } // formerly named Projects

//----------------------------------------------------------------------
// project.jsx
export function loader = () => { ... }; // formerly named getProject

export function Component() { ... } // formerly named Project
```

The properties exported from this lazy module are added to the route definition verbatim (in exactly the same words as were used originally). Because it's odd to export an element, we've added support for defining Component on a route object instead of element (but don't worry element still works!).

In this case we've opted to leave the layout and home routes in the primary bundle as that's the most common entry-point for our users. But we've moved the imports of our projects and :projectId routes into their own dynamic imports that won't be loaded unless we navigate to those routes.

This gives us a bit of the best of both worlds in that we're able to trim our critical-path bundle to the relevant homepage routes. And then on navigations, we can match paths and fetch the new route definitions we need.

to be fair this is exectly doing what react lazy do and have the same problem of waterfall effect. sure we have seprated the two component by link but same thing can be done in react with the help of button.

## water fall problem with current react router approch?

when use click on the <Link to="/Projects"/> then we request the Projects component from the server when we recive it we start to render it and starts the loader fn as soon as we start rendering the component.

are fetching (loader Fn) after we recive the Projects component. our fetching login is inside the component. face same sequence problem.

```
// URL = /Projects/123
                           (loader fn + Component) render   loader Fn execute(fetching)
projects Component:        ==============================
                                                          =================================
-------------------------------------------------------------------------------------------
                           (loader fn + Component) render   loader Fn execute(fetching)
project123 Component:        ==============================
                                                          =================================
```

# react router lazy optimization (partial parallism)

```
// URL = /Projects/123
                           (loader fn + Component) render but with partial parallism
projects Component:        ======== loader Fn
                           ================= component
                                    =============== fetching
```

```
// app.jsx
import Layout, { getUser } from `./layout`;
import Home from `./home`;

<Route path="/" loader={getUser()} element={<Layout/>}>
    <Route  path="projects"
            loader={async ()=>{
                let { loader } = await import("./projects-loader");
                return loader({ request, params });
            }}
            lazy={() => import("./projects")} />  // 💤 Lazy load!
</ Route>
-----------------------------------------------------------
// /projects-loader
export function loader = () => { ... }; // formerly named getProjects
----------------------------------------------------------------------
// projects.jsx
export function Component() { ... } // formerly named Projects

```

we have created seprate file for loader and imported that loader component.

Any fields defined statically on the route will always take precedence over anything returned from lazy. So while you should not be defining a static loader and also returning a loader from lazy, the lazy version will be ignored and you'll get a console warning if you do.

## react router lazy (Complete parallism)

```
// URL = /Projects/123
                           Component render and fetching  with complete parallism
projects Component:        ================ fetching
                           ================= component

```

```js
// app.jsx
import Layout, { getUser } from `./layout`;
import Home from `./home`;

<Route path="/" loader={getUser()} element={<Layout/>}>
    <Route  path="projects"
            loader={async()=> await fetch(URL).then(res => res.jeson())}
            lazy={() => import("./projects")} />  // 💤 Lazy load!
</ Route>
-----------------------------------------------------------
// projects.jsx
export function Component() { ... } // formerly named Projects
```

we are providing loader Fn directly on the roter which generally not that big of a code. due to that we can fetch the data and fetch the component at the same time in parallel.

this is also how remix solve this problem.

## Extra helpful react router features

### useBeforeUnload

useBeforeUnload is just a wrapper around window.useBeforeUnload and useful when working with forms.

---

### window.useBeforeUnload

window.onbeforeunload is an event handler in JavaScript that is triggered when the user is about to leave or reload a webpage. It provides a way to execute code or display a prompt before the user navigates away from the current page.

---

when working with Form it is better to store all the input related data inside localstoage when user either acidently reload the page or leave the page or close tha page. you can use useBeforeUnload function which get's triggered when ever user either leaves,refresh the page. and use localstorage to store those input value so when he comes back he already has his form filled. he don't have to fill the form data again. from scratch.

```js
import { useBeforeUnload } from "react-router-dom";

function SomeForm() {
  const [state, setState] = React.useState(null);

  // save it off before users navigate away
  useBeforeUnload(
    React.useCallback(() => {
      localStorage.stuff = state;
    }, [state])
  );

  // read it in when they return
  React.useEffect(() => {
    if (state === null && localStorage.stuff != null) {
      setState(localStorage.stuff);
    }
  }, [state]);

  return <>{/*... */}</>;
}
```

## useFormAction

the useFormAction hook allows for dynamically resolving form actions based on the current route in the context. It is primarily used internally by the <Form<span></span>> component but can also be used directly in other components to compute the correct action for specific elements, such as <button<span></span>> elements with the formAction attribute.

```js
import { useFormAction } from "react-router-dom";

function DeleteButton() {
  return (
    <button formAction={useFormAction("destroy")} formMethod="post">
      Delete
    </button>
  );
}
```

when this button will be click action fn attached to it will be called and you can do something with it.

## useMatches

The useMatches hook returns an array of current route matches on the page, which is useful for creating abstractions in parent layouts to access the data of child routes.

useMatches will return an object which contain Route related info.

```
// earch matching instance of route array will be represented by this type of object.
{
  id: The route ID.
  pathname: The portion of the URL that the route matched.
  data: The data obtained from the loader associated with the route.
  params: The parsed parameters extracted from the URL.
  handle: is an object that can be attached to any route and you can access handle by useMatcher hook to extract data out of it.
}
```

```js
// useMatcher will be able to acess the handle object if route path matches with the current URL.

  <Route
    path="messages"
    element={<Messages />}
    loader={loadMessages}
    id = "root"
    handle={{
      // you can put whatever you want on a route handle
      // here we use "crumb" and return some elements,
      // this is what we'll render in the breadcrumbs
      // for this route
      crumb: () => <Link to="/messages">Messages</Link>,
    }}
js---------------------------------------------------------------------

function Breadcrumbs() {
  let matches = useMatches();
  let crumbs = matches.filter( routeObj => routeObj.?crumb)
  return (...);
}
```

useMatcher is a great Hook for making Breadcrumbs (show list of which page has you come here) on your rootPage.

Returns the current route matches on the page. This is most useful for creating abstractions in parent layouts to get access to their child route's data.

## useRouteLoaderData

useRouteLoaderData hook allows you to access the loader fn returned data of any currently rendered route anywhere in your component tree.

useRouteLoaderData hook take route Id and return loader data attached to that route.

Purpose of useRouteLoaderData Hook: The useRouteLoaderData hook provides a way to access the data associated with the currently rendered route. It allows components deep within the component tree to access data from routes higher up in the hierarchy, as well as parent routes to access data from child routes deeper in the tree.

```js
createBrowserRouter([
  {
    path: "/",
    loader: () => fetchUser(),
    element: <Root />,
    id: "root",
    children: [
      {
        path: "jobs/:jobId",
        loader: loadJob,
        element: <JobListing />,
      },
    ],
  },
]);
//--------------------------------------------------
import { useRouteLoaderData } from "react-router-dom";

function SomeComp() {
  const user = useRouteLoaderData("root");
  // ...
}
```

Note: you can access loader fn Data only for those route which are currently rendering on the page otherwise it will return undefined.

## when reactQuery meets react router

react router does not have catch to store already fetched data so that we don't make request everytime loader fn runs.

here is where reacct query jumps in to help us which has catching caching.

```js
// without react query only react router
import { useLoaderData } from "react-router-dom";
import { getContact } from "../contacts";

// ⬇️ this is the loader for the detail route
export async function loader({ params }) {
  return getContact(params.contactId);
}

export default function Contact() {
  // ⬇️ this gives you data from the loader
  const contact = useLoaderData();
  // render some jsx
}
//----------
import Contact, { loader as contactLoader } from "./routes/contact";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      {
        path: "contacts",
        element: <Contacts />,
        children: [
          {
            path: "contacts/:contactId",
            element: <Contact />,
            // ⬇️ this is the loader for the detail route
            loader: contactLoader,
          },
        ],
      },
    ],
  },
]);
```

```js
//with react query + react router

import { useQuery } from '@tanstack/react-query'
import { getContact } from '../contacts'

// ⬇️ define your query
const contactDetailQuery = (id) => ({
  queryKey: ['contacts', 'detail', id],
  queryFn: async () => getContact(id),
})

// ⬇️ needs access to queryClient
export const loader =
  (queryClient) =>
  async ({ params }) => {
    const query = contactDetailQuery(params.contactId)
    // ⬇️ return data or fetch it
    return (                                      || you can replace this code with
      queryClient.getQueryData(query.queryKey) ?? || return queryClient.ensureQuery(query.queryKey)
      (await queryClient.fetchQuery(query))       ||
    )
  }

export default function Contact() {
  const params = useParams()
  // ⬇️ useQuery as per usual
  const { data: contact } = useQuery(contactDetailQuery(params.contactId))
  // render some jsx
}

//
//-------------------
const queryClient = new QueryClient()

const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    children: [
      {
        path: 'contacts',
        element: <Contacts />,
        children: [
          {
            path: 'contacts/:contactId',
            element: <Contact />,
            // ⬇️ pass the queryClient to the route
            loader: contactLoader(queryClient),
          },
        ],
      },
    ],
  },
])
```

let me explain this step by step:

1. we first need to provide queryClinet to our loader fn becouse queryClient only work with components and loader is a function so that's why we need to pass queryClient to every loader fn. (which is the bad part)

2. after that we use contactDetailQuery(params.contactId) which is just an abstraction to create queryKey and queryfn as an object so that we can use it later in useQuery({queryKey,queryObject})

3. Note: we are returning from Loader Fn to not to get data in the Component (Contect) but to throw Error if we get some error during fetching so that errorElement can get take over.

   ```js
   return (
     queryClient.getQueryData(query.queryKey) ??
     (await queryClient.fetchQuery(query))
   );
   ```

   `queryClient.getQueryData(query.queryKey)` will check if any value is stored with this key and return it. if no value is present then return undefined.

   `queryClient.fetchQuery(query)` it will fetch for query and store it in the queryCatch. if something went wrong during fetching it will throw an error.

   we are using fetchQuery over prefetch becouse prefetch does not return anything. it just catch it. we need to return in case error happen and we can show errorElement.

   you can also use queryClient.ensureQuery(query.queryKey) which do both the job. it first check that queryCatch has the data for this key and if not then do fetching.

4. later inside your Component (Contect) you can use the same key to use the catched value from the queryCatch. inside component it will also create an observer so all the features of the query will work like refetching in background when data get stale.

## Invalidating in actions

after action runs it will revalidate the data by running the loader fn. but now we can use react query inside loader fn. which means ut's possible that loader fn query return stale data so now to solve that we need to add invalidation during action. so that we can invalidate loader query data and do refetch.
and provide updated data to our UI

```js
export const action =
  (queryClient) =>
  async ({ request, params }) => {
    const formData = await request.formData();
    const updates = Object.fromEntries(formData);
    await updateContact(params.contactId, updates);
    queryClient.invalidateQueries({ queryKey: ["contacts"] });
    return redirect(`/contacts/${params.contactId}`);
  };
```
