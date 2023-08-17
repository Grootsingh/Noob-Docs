# UI.dev redux

**VS CODE ShortCut**

1. adding indentation : press tab
1. remove indentation: press shfit + tab

---

there are two important part of an APP:

1. UI
2. state

when ever we encounter a bug in a app and that app is miss behaving. why we restart that app??? becouse all the state of that app get reset to it's initalState and most of that time that fixes the bug.

and everyTime a bugs come up in your app is most lickly cozed by state miss-management.

app expected an state to be onething but what we got is something else which is why app crashed or became iresponsive at first place

by increasing the predictability of the state of our app, we can drasticly decrease the amount of bugs that occers in our app.

main philosophy behind using any state management library is to make state more predictable.

## how to make state more predictabale

generally when you make app you have n number of state spreading throughout the application.

if you are using react you have component and all the state are spread out in the component tree. some state are in the same component. some state are lifted, some state are prop-drilled, some state are shared with context API. etc.

state are every where in the entire app.

if our goal is to make state more predictable in our app. the solution will be is to not have state every where in the app (spread out in the component tree). to make is more predictable it is better to manage all the state at a specific place at one location (centerlized)

all of the state insted of being spread across our entire app is going to be at one location and we call that location a state tree.

## why is it benefical to have a state tree (centerlized)?

benefits of state tree?

1. `**shared cache**`

   if two or more component need exect piece of state what you will do?

   move the shared-state up in the component tree (uplifting) and do prop-drilling till you reach to those child-component those actually use them.

   Ex: what if this is a large APP and nearest parent component that can share state to both of the child-component is 7-10 level up in the component tree?

   for that you need to again put the state in that parent (10 levelUp/uplifting) and you need to do alot of plumbing (prop-driling) to all the middle component to share that state to those component that actually need it.

   if all of our state are at single state Tree (at single location/centerlized). outside of our component tree then those component who are in need of the state's can get those state from the state tree directly without going through the hasle of lifting and prop-driling.

2. `**predictable state changes**`

   by having all of our state at single location (state tree). we can set some strict rules for all the states for how the states can be updated? how can you get the state? and all the component that are consuming those states will automatically will be updated when any state get's updated in the state tree. those interensic imposed strict Rules will make your states more predictable becouse those state can only be updated and get access by predefined method which follow some strict rules to intereact with the shared-state. which helps us to avoid bugs cozed by miss-management of state.

   EX: we don't want anyone to update those shared state by providing these own version of state updater methods. state can only be updated by one method (that follow some rule or is predefined) and if anyone want to update the state of the shared state then they need to use that predefined Method. if anyone can update the state by there provided version of updater state. it's more venrnable to make mistakes and due to that entire app state managment suffers.

   so it is importent to only use the predefiend method to update the state of the shared state.

   that predefined method follow some strict rule so when ever we use those predefined method it will updated the state in predictable manner with no sideEffect.(pure function,reducer)

3. `**improve developer Tooling**`

   since all of your state lives at a single location (state tree) which is out of the component structures or component tree or react app. when ever any state get updated. that updated state is aviliable to access in the entire app.

   if the state is located all over the app/component.
   re-rendering of the component can throw away the state, get back to inital state.(user fill the form and excidently refresh the page and user had to fill the form from scratch)

   having state at single location make it eaiser to handle share state,debugging state related problems(impove dev experience).

   for ex: you are shareing state between 2 component which has 5 component in the middle you need to check all the component one by one which one is cozing the bug. on the other hand having single location for state make it easier to handle error.(redux dev tool)

4. `**pure functions**`

   we use pure function to get the states and update the state. pure function have no sideEffect. they are predictable.

5. `**serverside rendering**`

   if all of the state is on one location then it is easier to get the data and place them in the UI for easier HTML page buildUp. which will improve inital loadtime for client side.

# how to intereact with the state tree?

we need few functionallity to work with state tree.

1. be able to get the state from the state tree.
2. update the state of the state tree.
3. listen to anychanges happening in the state tree. so those component who are consuming the state from the state tree can acess the updated state. when ever any state change happen in the state Tree.

`**store**`: is a container/wrapper which keeps/contains the state at single location state tree and have predictable methods to intereact with the states of the state tree like get,update and listen.

if we can do all of these thing in predictable manner (follow some strict rule). we have a state management library.

# build your own state management library.

first we need to create store which contain state, get,update,listen methods.

createStore fn will create a store for us.
a store have four parts to it.

1. `**The state**` : state of our entire applications (called state)
2. `**Get the state**` : a method to consume state inside component.
3. `**Listen to change on the state**` : a listener which listen/keep track of the state and get executed whenever anything changes/update happen to the state. (called listener)
4. `**Update the state**` : a method to update the state.

when ever you execute createStore fn. it will create a store which contain all of these (state,get,listen,update) and return (get,listen,update) method to intreact with the state.

what's happening that we create a private/local state (internal library part) so no one can intereact with the state directly and provide methods publicly (for developer to intereact with the state)

let's talk about internal of our createStore Fn.

1. state

```
let state
```

2. get the state

a method which return the state

```
getState = () => state
------------------------------------
const store = createState()
const state = store.getState()

```

3. listener

listener has two important part:

- subscribe
- listiner array

we allow the component to subscribe to the store which will be executed when ever state change happens.

subscribe take a subFn which get executed when ever state changes in the state tree.

to keep track which component is subscribing to the store we mainatian a listiner Array. everytime anyone try to subscribe to the store listiner add that subFn (the function that needed to execute everytime any state changes) to it's array.

when ever updater fn get executed (meaning state change happen) after update we itr over listiner array and execute each listiner array subscriber.

and subscribe fn also return a fn which allow the user to unsubscribe to the store after subscribing.

which is simple filtering method as you can see.

```
let listiner = []

const subscribe = (subFn) =>{
listiner.push(subFn)

return () => {
        return listener = listiner.filter((allSub)=> allSub !== subFn)
    }
}

---------------------------------------------------------
const store = createState()
const unsubscribe = store.subscribe(()=>{})
unsubscribe()
```

4. update

whole goal here is to increase the predictibility of the state in our application. we just can't allow anyone to update the state. if we did, that will drasticly decrease the predictibilty of our state of the app.

and the only way we can increase predictibility in terms of updating the state of the app. is by stablising strict set of rules for how update can happens.

let's take indian cricket Teams as an example: in order for maximize the predictibility of winning indian team. all the player needed to be on the same page.

they need to operate one cohisive unit. every miss-comunication can/will leads consiquence so in a sence what they do is they create a playbook of strategies or have a meeting where they strategies before match. which player will do what.

that playbook should be known by every player of the team by heart.

so during match if teams decide to play a specific stratergy. all the player know what they exectly have to do at that point. becouse they follow the predetermine rules/tectics/playbook they set before the match for this specific senerio ex: when ever ashwin come to boal we will get closer to the pitch and get inside the inner ring for increasing presser on the batsman.

can we take the same idea and apply to our state . due to having predifined play/stratergies will increase the productivity of the team rather then wasting time during playTime ex: where each player needed to stand during fielding when ashwin will boal.

> Rule number:1 to increase productibility in our app pre-define collections of actions.

just like teams can have collection of strategies for different ceniro.

we too can have collections of strategies/event that can occer inside of our applications which will change the state of our store state tree.

let's say we are building a todo list App:

a todo list can have multiple scenerios/strategies that can occer. like:
adding an item to the store, removing an item to the store

let's create an object which take type property which describe which type of event/stratergy is this

{
type:'ADD_TODO',
todo:{
id:0,
name:'Learn Redux',
complete:false,
}
}

what we are saying here. this is a type of event which will occer in our app. and this event will eventually going to change this state of our store.

this object is called an Action.

action is just an object which represent a sepecific event/strategy/action which will occer in our app and eventually change the state of our store.

in cricket terms this action is a strategies that will occer during match and change the play of the game:

EX: strategies/action in a T20 match.
{
type:'ASHWIN_OVER_18',
filderPosition: {
atSlip: "3 player",
atSlipPlayer:["Rohit sharma","Virat kohli","Hardit pandya"],
}
}

we will create action/strategy for every type of event that can occer in our app that will change the state of our store.

if the store state changes that means one of the defiend action event must had triggered.

just like indian cricket team can look at there strategy book before match to know which strategies they are going to use during the match. before even playing in the match. we too can also look at our collection of actions to know which event are going to occer in our app which are going to change the state of the store.

// More example for actions

```
// Actions:
Action is a object which tell our updater Fn(reducer fn) that what action we need to take to change the state.

action contain two things:
1. what this action will do or what type of action is this.

2. aditional info/data which is required to perform that action.

ex: if we want to add something to the state
then type will be add and payload will be what you are adding the data that you want to add.

{
    type:  'NAME/TYPE OF THE EVENT THAT CHANGES THE STATE',
    payload:'Aditional info that helps us to change the state'
}
-------------------------------------------
// Add todo action
{
    type:'ADD_TODO',
    todo:{
        id:0,
        name:'Learn Redux',
        complete:false,
    }
}

// Remove todo action
{
    type:'REMOVE_TODO',
    id:0
}

// Toggle todo Action
{
    type: 'TOGGLE_TODO',
    id:0
}

// Add Goal action
{
    type:'ADD_GOAL',
    goal:{
        id:0,
        name:'Run a Marathon'
    }
}

// Remove Goal action
{
    type:'REMOVE_GOAL',
    id:0
}
```

now we have state from store and action (events object) we need a way to tie them together. we need a way to update the state based on events/actions which occerd.

# how to update the state based on action

to tie them up together we can use a pure function which takes in current state and action as paramter and return updated new state of our store.

but why we use pure function???

the goal is to update the state with predictibility (follow some strick rules) and without any side effect

benifits/characterstics of pure functions

1. They always return the same result if the same argunments are passed in.

this means if you call a pure function over and over again as long as it has the same argunments it will always returns the same return value.(predictible)

2. They only depend only on the argunments passed into them.

what this means is that they never access values outside the scope of there own scope. meaning the return value can only be determine by the argunment pass into it and nothing else. (improve predictibility)

3. Never produce any sideEffects.

it means pure function don't try to sync with external state meaning they don't do AJAX request or they don't mutate state of outer work.

pure fn take argunment and return a value that value should not have any effect on the state of the app or change anything related to app. pure fn has no intereaction to outer world. it simply return a value.(no side Effect, so improved predictability)

as you can see if any function passess any of these characterstics they are extreamly predictable with no side Effect on the outer world.

# lets take an example to understand:

```
// Add todo Action
{
    type:'ADD_TODO',
    todo:{
        id:0,
        name:'Learn Redux',
        complete:false,
    }
}

// let state;
```

let's write a pure function that take currect state and an action and update the state.

```
function todos(state = [] , action){
    if(action.type === 'ADD_TODO'){
        return state.concat([action.todo])
    }
    return state
}
```

as you can see it take current state and we have initialize state as an empity Array becouse as you can see state is undefined in our store. so when every our todo fn get called it don't thow error becouse state is undefined.

second thing is that we are updating the current state based on which try of action we are passing ('ADD_TODO')

third thing is that we are Not mutating the state of store state as a pure fn third rule is it will not give anyside effect or nothing changing in outer world.

in array array.push() will mutate the orignial state and array.concat() with create a new array and not modify the original state.

then we are returning that updated new state.

if none of the predefined action was passed into it. it will simply return the current state without any update. this will make sure we don't change state on bases of unrecognise action which was not predefined make state more predictable.

this todo fn (reducer fn) is created by the user and later user will use dispatch to release an action.

# dispatch,action and reducer and createStore coming it all of together

so currently we know:

> actions which are just objects contain type:type of event/action and payload: additional data needed to perform the action.

> todo fn which is a pure fn also called reducer fn which take current state of the store and an action to perform update on the state.

> createStore fn which is responsible for 4 things. creating state, and have methods to intereact with the state (getState,subscribe,dispatch)

now let's create a dispatch fn.

```
function createStore(reducer){
const dispatch = (action) =>{
    //update the state
    state = reducer(state, action)

    // execute all the subFn from the listiner array
    listiner.forEach((allSub)=> allSub())
}

return dispatch
}
---------------
// developer will create a reducer fn

function todos(state = [] , action){
    if(action.type === 'ADD_TODO'){
        return state.concat([action.todo])
    }
    return state
}

//perform dispatch action

const store = createStore(todo)

store.dispatch(
{
    type:'ADD_TODO',
    todo:{
        id:0,
        name:'Learn Redux',
        complete:false,
    }
}
)
```

when ever you want to change the state of the store you will dispatch a perticuler action.

dispatch fn takes in action object. dispatch fn then will take the action object and update the state with the reducer fn passing it the current state and the action which is dispatched. after that reducer fn will return the new updated state we will asign it to the store state to update it.

after store state has been updated then now we need to tell all the subscriber that state of the store has been updated so we invoke all the subFn stored inside the listiner array.

Note: as you can see reducer fn is defined by the developer not a part of the library inbuilt code. to stranderdize the reducer fn format we take reducer fn as paramerter inside the createStore fn. which is used by the dispatch fn to update the store state to new update state.

# how to handle multiple reducer

when we create a store by createStore fn we pass it a reducer. this works fine when you only have one reducer to work with.

createStore(reducer)

now we have two reducer fn ??? how to pass both the reducer to createStore fn?

whole motive of todos reducer is to get us to the next state of the todos specificly of the todo array (state=[])

function todos(state = [] , action){
return Next state of the todos array  
}

similary with goals the whole motive of the goal reducer is to get us to the next state of the goals specificly of the goal array (state = [])

function goal(state = [] , action){
return Next state of the goal array
}

Note: what we trying to say here is that reducer job is to return the next updated state.

so if we invoke reducer fn with out any action then it will return us the default condition which is our default state without any action meaning without any update.

but in our params we have decleared that state = [] meaning if we are calling the reducer fn for the first time and without any action then what reducer fn is doing it is asigining the state = [] as the value.

prev: we only have one reducer which maintain one state.

```
todo state = []
```

if there is only one state to maintain then we can use todo state as store main state. so anytime disptach action happens todoReducer Fn get called and state inside our store is undefined and inside todoReducer we have set if state is undefined define it by state = [] as inital state then we update the state with specified action.

Now: we have more then one state to maintain.

```
todo state = []
goal state = []
```

as now we have multiple state to maintain what we can do is: for main store state we can create an object which contain both the state as key as state name and value as state.

```
{
    todo : [],
    goal : []
}
```

this is how our main store state should look like this.

is there any way that we can create a rootReducer which take all the reducers and use them to create an object of child reducer make us a single main state. which later we can pass to createStore() and use that rootReducer as main/parent state for all the childreducer in our store.

we can use a function for that let's call it rootReducer

what we will do is that we define main state to be {} an object as inital state (initalState is just there to not through any error when invoked of undefined propety), then return an object. that object return value will be asignind to mainState inside createStore dispatch fn when any dispatch fn get called. also note that we are invoking all the child reducers. which will return the inital state as state for there respoective reducer.

```
// where rootReducer will be placed
// it will update the state of the store

function createStore(rootReducer){
let state;

const dispatch = (action){
// rootReducer fn ---------------------------------
    state = (state = {} , action) => {
                return {
                    todo: todo(state.todo,action)
                    goal: goal(state.goal,action)
                }
        }
//------------------------------------------------
    listiner.forEach((allSub)=> allSub())
}

return {
    dispatch
}

}

//-------------------------------------------------------------
as you can notice rootReducer return object is asign to the state of the store.

when first dispatch fn get triggered which takes an action rootReducer fn get called for the first time then state is defined as an object then we will invode todoReducer with state.todo (which is undefined) and with provided action if any. and goalReducer in similer ways.

when they childReducer fn get invode and there state is undefined which is true then we will take the defualt inital value for state which is defined inside the childReducer fn and take the action return the updated state. inside the rootReducer fn object and that updated object will be asigned to the state of the store.
```

Note: we are passing state.todo and state.goal as a argunment which are at the time of passing are undefined. before invoking childReducer whats the current main state is :

we have create a state which is an object and that object has two keys todo and goals without any value asigned to them.

```
const state = {
    todo,
    goal
}
```

after this those childReducer will be invoked.

let's take todoReducer as ex:

```
function todos(state = [] , action){

    switch(action.type){

        default :
        return state
    }

}
```

we have created state.todo but it is undefined and inside todoReducer fn we have defined that if state provided is undefined then use the inital value which is an array. and there is no action passed so defualt case will be invoked. so what endup happening is that todoReducer fn will return state which is an array.

Now, inside our store state those undefined state's get defined by those childReducer.

```
const state = {
    todo: []
    goal: []
}
```

anytime dispatch fn get called rootReducer fn get also called which will invoke "all the childreducer fn" and childReducer Fn will take the action object which was passed from dispatch fn. and the current state that we got from the store.

so all the childReducer fn get invoked that's why it is important to define unique action Type for all the childReducer and have a default case, so that only the actual childReducer get's updated (that matches the action.type) other childReducer will return the defualt case which is currentState without any update to them.

after all the childReducer get executed they will return updated state for there respective child state. and placed inside the object of the rootReducer then rootReducer will return updated object of child states. and that object will be asigned to the main Store state.

# how to create a store

```
----------------------------------------

function createStore(reducer){
let state;
let listiner = []

const getState = () => state;

const subscribe = (subFn) =>{

listiner.push(subFn)

return () => {
        return listener = listiner.filter((allSub)=> allSub !== subFn)
    }
}

const dispatch = (action){
    state = reducer(state, action)
    listiner.forEach((allSub)=> allSub())
}


return {
    getState,
    subscribe,
    dispatch
}

}
------------------------------------------------------
// create a store
const store = createState(todo)
------------------------------------------------------
// to get the state
const state = store.getState()
------------------------------------------------------
// subscribe to the store
const unsubscribe = store.subscribe(()=>{})

// unsubscribe to the store
unsubscribe()
----------------------------------------------------
// to update the state
// one reducer and only one action

// create a reducer
function todos(state = [] , action){
    if(action.type === 'ADD_TODO'){
        return state.concat([action.todo])
    }
    return state
}

// pass it to createStore fn
const store = createStore(todo)

// dispatch action to update the state
store.dispatch(
{
    type:'ADD_TODO',
    todo:{
        id:0,
        name:'Learn Redux',
        complete:false,
    }
}
)
----------------------------------------------------
// to update the state
// one reducer and multiple action

// create a reducer
function todos(state = [] , action){

    switch(action.type){
        case 'ADD_TODO':
        return state.concat([action.todo])

        case 'REMOVE_TODO':
        return state.filter( allState => allstate.id !== action.id )

        case 'TOGGLE_TODO':
        return state.map( allState => allState.id === action.id ? Object.assign({},allState,{complete: !allState.complete}) : allState)

        default :
        return state
    }

}

// pass it to createStore fn
const store = createStore(todo)

// dispatch action to update the state

// Add todo action
store.dispatch({
    type:'ADD_TODO',
    todo:{
        id:0,
        name:'Learn Redux',
        complete:false,
    }
})

// Remove todo action
store.dispatch({
    type:'REMOVE_TODO',
    id:0
})


// Toggle todo Action
store.dispatch({
    type: 'TOGGLE_TODO',
    id:0
})
----------------------------------------------------
// to update the state
// multiple reducer and multiple action

// create a reducer for todos
function todos(state = [] , action){

    switch(action.type){
        case 'ADD_TODO':
        return state.concat([action.todo])

        case 'REMOVE_TODO':
        return state.filter( allState => allstate.id !== action.id )

        case 'TOGGLE_TODO':
        return state.map( allState => allState.id === action.id ? Object.assign({},allState,{complete: !allState.complete}) : allState)

        default :
        return state
    }

}

// create a reducer for goals
function goals(state = [] , action){

    switch(action.type){
        case 'ADD_GOAL':
        return state.concat([action.goal])

        case 'REMOVE_GOAL':
        return state.filter( allState => allstate.id !== action.id )

        default :
        return state
    }

}

// create a rootReducer
const rootReducer = (state = {} , action){
    return {
        todos: todos(state.todos,action)
        goals: goals(state.goals,action)
    }
}

// pass it to createStore fn
const store = createStore(rootReducer)

// dispatch action to update the state

// Add todo action
store.dispatch({
    type:'ADD_TODO',
    todo:{
        id:0,
        name:'Learn Redux',
        complete:false,
    }
})

// Remove todo action
store.dispatch({
    type:'REMOVE_TODO',
    id:0
})


// Toggle todo Action
store.dispatch({
    type: 'TOGGLE_TODO',
    id:0
})

>>>

// Add Goal action
store.dispatch({
    type:'ADD_GOAL',
    goal:{
        id:0,
        name:'Run a Marathon'
    }
})

// Remove Goal action
store.dispatch({
    type:'REMOVE_GOAL',
    id:0
})
```

### few additional standered practice with state managemnet library:

1. every time we create a childReducer and do dispatch related to that childReducer we define action.type as string. it is common to make mistakes when comparing string. for standered practice it is better to create variable at one place and asign those variable in childReducer and dispatch for making code less error prons.

2. so making code less error pron you should also stranderdize action object becouse you might define same action type again and again so it is better to make a function which take payload object and return an object which contain both action type (standerdized) and the action payload.

SideNote: a function that returns an action object is known as actionCreater

```
----------------------------------------
// Library Code
function createStore(reducer){
let state;
let listiner = []

const getState = () => state;

const subscribe = (subFn) =>{

listiner.push(subFn)

return () => {
        return listener = listiner.filter((allSub)=> allSub !== subFn)
    }
}

const dispatch = (action){
    state = reducer(state, action)
    listiner.forEach((allSub)=> allSub())
}


return {
    getState,
    subscribe,
    dispatch
}

}
----------------------------------------------------
// Developer code

const ADD_TODO = 'ADD_TODO';
const REMOVE_TODO = 'REMOVE_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';
const ADD_GOAL = 'ADD_GOAL';
const REMOVE_GOAL = 'REMOVE_GOAL';

const addTodoAction = (payload) => {
    return {
        type: 'ADD_TODO',
        payload
    }
}

const removeTodoAction = (payload) => {
    return {
        type: 'REMOVE_TODO',
        payload
    }
}

const toggleTodoAction = (payload) => {
    return {
        type: 'TOGGLE_TODO',
        payload
    }
}

const addGoalAction = (payload) => {
    return {
        type: 'ADD_GOAL',
        payload
    }
}

const removeGoalAction = (payload) => {
    return {
        type: 'REMOVE_GOAL',
        payload
    }
}

// create a reducer for todos
function todos(state = [] , action){

    switch(action.type){
        case ADD_TODO:
        return state.concat([action.todo])

        case REMOVE_TODO:
        return state.filter( allState => allstate.id !== action.id )

        case TOGGLE_TODO:
        return state.map( allState => allState.id === action.id ? Object.assign({},allState,{complete: !allState.complete}) : allState)

        default :
        return state
    }

}

// create a reducer for goals
function goals(state = [] , action){

    switch(action.type){
        case ADD_GOAL:
        return state.concat([action.goal])

        case REMOVE_GOAL:
        return state.filter( allState => allstate.id !== action.id )

        default :
        return state
    }

}

// create a rootReducer
const rootReducer = (state = {} , action){
    return {
        todos: todos(state.todos,action)
        goals: goals(state.goals,action)
    }
}

// pass it to createStore fn
const store = createStore(rootReducer)

// dispatch action to update the state

// Add todo action
store.dispatch(addTodoAction({
        id:0,
        name:'Learn Redux',
        complete:false,
}))

// Remove todo action
store.dispatch(removeTodoAction({id:0}))


// Toggle todo Action
store.dispatch(toggleTodoAction({id:0}))


// Add Goal action
store.dispatch(addGoalAction({
        id:0,
        name:'Run a Marathon'
    }))

// Remove Goal action
store.dispatch(removeGoalAction({id:0}))
```

# when UI met the State

as we previously already talked about that an app has two important part it's UI and it's State.

so when the dispatch happens in a app. when ever state changes inside an app that state change can be a submit btn so you can dispatch an action when user submit new state.

you can define the new Data inside action object payload. and dispatch it with the help of actionCreater Fns.

in general, when we are not using state management library; when we intreact with the UI; any state changes that state lives in the memory of the browser or we can say inside the DOM Tree. we have already talked about disadvantage of keeping state in the memory/DOM tree is that it's easy to losse state changes (ex: refesh of the page) so it is better to keep state inside your store state. so that browser memory refreshing has no effect on the state of the app.

//--------------------------------- XXX own state managment library completes XXX -----------------------//

### similarity between Redux library and our own state managment library

the library code that we have created is similar to the redux library code.

1. createStore()

in our library we also use createStore() which take a rootReducer to create our store.

similarly in redux we also do exect same

```
// our own libaray
const store = createStore(reducer)
----------------------------------
// redux library
import {createStore} from redux

const store = createStore(reducer)
```

2. combineReducer

combineReducer is same as rootReducer fn but it is better and easier to work with.

it take an object which contain all the child reducer and automatically create respective slice for them to get added in the mainState of the store.

when working with rootReducer we need to specify the slice manually

ex:

```
// rootReducer
const rootReducer = (state = {} , action){
    return {
        todos: todos(state.todos,action)
        goals: goals(state.goals,action)
    }
}
```

todo reducer take a slice (state.todo). we have to create that slice by our self.

a slice is a piece/part of the main State.

in rootReducer we have to create that slice by our self. but in combineReducer it is created for us automatically.

```
// redux combineReducer
import {combineReducer,createStore} from redux

const slice = combineReducer({
    todos,
    goals
})
const store = createStore(slice)

------------------------------------
// what happening inside is

const slice = combineReducer({
    todos : todos(state.todos, action),
    goals : goals(state.goals, action)
})

// create slice for childReducer
// store state is an object by default
```

combineReducer is smart enough to know that we want to add todos state on our main Store state. and he will get that state from the todos reducer just as we did in rootReducer. and it is also smart enough to know to create specific slice for todos which is (state.todos) and action argunment to the todos reducer.

### middleware // highjecking the dispatch function

```
dispatch(action) > middleware > reducer
```

Middleware acts as a layer between an action being dispatched and reaching the reducers, allowing you to intercept, modify, or perform additional operations on the action or the state.

When an action is dispatched in a Redux application, it flows through the middleware before it reaches the reducers. The middleware has the ability to inspect the action, perform asynchronous tasks, modify the action or the state, and even dispatch additional actions.

Middleware is useful for handling side effects, such as making asynchronous API calls, logging actions, or handling routing. It provides a clean and centralized way to manage complex logic that goes beyond the basic action-creation and state-management capabilities of Redux.

middleware take advantage of currying to highjeck the dispatch fn and appy some additional logic (like filter or do anything) to the dispatched action (before dispatched action reaches to reducer fn) so that yo don't have to edit the dispatch fn code directly.

---

# Extra knowladge

high order Function > clouser > currying

1. higher order function

a function that takes one or more functions as arguments, or returns a function as its result.

```
// fns that can take other fn as input/argunment
const fn_one = () => "lower order"

const fn_two = (fn) => "higher order" > fn()

fn_two(fn_one)
-------------------------------------------------
// fns that can return other fns

const fn_two = () => function(){}
```

2. clouser

when you are defining a higher order function (fns that return another fns). with in the body of the higher order fn . it has it's own scope. meaning variable defined inside functions with (let, const) can not be able to be accessed by outside world. when you define an inner fns. that inner fns has access to the outer fns variables or has access to the outer fns scope. meaning inner fn can access anything which is in the scope of the outer functions.

```
// clouser

const fn_one = (value_one) => (value_two) => value_one + value_two

const fn = fn_one(5)  // (value_two) => 5 + value_two
fn(3) // (3) => 5 + 3 // 8
```

even though fn_one has been poped of from the calltsack when we used fn_one(5) inner_fn still hold/remember the value_one value. this feature is called clouser.

that private variable value_one can only be accessed by inner_fn and that variabe is lock-In. with the inner_fn when ever you invoke the inner fn it will be able to use value_one = 5 which was defined in the scope of fn_one.

clouser is when inner function able to close over the values of it's outer fns. those values are closed over the inner fns it means inner fn can access those values. and no one else can even access those value. becouse we are in a close environment. becouse this is how fns works no one from the outer world can access variable defined inside the scope of the function.

3. currying

currying take advantange clouser. which locked-in variable /close over the scope variable of the outer fn. in another way to say that it partically apply a function it means when you invoke fn_one it will return inner_fn + pass the variable which are defined inside fn_one. so we have invoked fn_one but our function execution has not finished becouse it has returned us another fn (inner_fn) which also need vlaue_two to complete the task.

but after the invoke of fn_one we are in the middle of the completing our function exection. function exection has not done yet we are in the middle that's why we also call it partically appied fn.

To make functions more modular and easier to reuse, we can use techniques like currying, which lets us take advantage of closure to turn a function with any number of arguments into a series of single-argument functions, such that we can provide only some of the input arguments and get a "partially applied" function.

```
// currying
const fn_one = (value_one) => (value_two) => value_one + value_two

const fn = fn_one(5) // partically appied fn
fn(2) // (2) => 5 + 2 // 7
fn(4) // (4) => 5 + 4 // 9

```

we can take advantage of those partically appied fn to producer different results. and thats what currying is.

currying is when we convert a fn that multiple argunment and return an result into nested higher order fn

(fn_one => fn_two => fn_three => fn_four)

so that we can take advantage of closer which create partically appied fn and use those partically appied fn to create different result with out repeating to writte all the argunment in a single fn every time we need a different value.

```
// normal way
const person_place_thing = (person,place,thing) => person + place + thing

// if we have to make 3 different person + place + thing version then
const version_1 = person_place_thing(rahul,kashmir,ball)
const version_2 = person_place_thing(mohit,agra,glasses)
const version_3 = person_place_thing(golu,mathura,vollyball)

// if we need to make 3 version of rahul,kasmir but differnt thing
const rahul_kashmir_thing_Vone = person_place_thing(rahul,kashmir,ball)
const rahul_kashmir_thing_Vtwo = person_place_thing(rahul,kashmir,bat)
const rahul_kashmir_thing_Vthree = person_place_thing(rahul,kashmir,bike)

---------------------------------------------------------------------------------------------------
const person_place_thing_with_currying = (person) => (place) => (thing) => person + place + thing

// if we have to make 3 different person + place + thing version then
const version_1 = person_place_thing_currying(rahul)(kashmir)(ball)
const version_2 = person_place_thing_currying(mohit)(agra)(glasses)
const version_3 = person_place_thing_currying(golu)(mathura)(vollyball)

// if we need to make 3 version of rahul,kasmir but differnt thing
const rahul_kashmir = person_place_thing_currying(rahul)(kashmir)
rahul_kashmir(ball)
rahul_kashmir(bat)
rahul_kashmir(bike)
```

curring allow us to not repeat the same argunment and still return different result in case of rahul_kashmir case this is possible due to partically appied fn.

---

# highjeck the disptach fn without middlewear

what we want to do is that we want to highjeck the dispatch fn and then check something on the action object before dispatch fn reaches the reducer fn.

ex: check that action.name contain bitcoin on it or not.if it does not then do the normal dispatch. if it does then we will throw an error which indicate that user can not submit bitcoin keyword in our form.

```
// create a fn which checks
checkAndDispatch(store,action){
    if(
        action.type === ADD_TODO &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    if(
        action.type === ADD_GOALS &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    return store.dispatch(action)
}

// now we have to replace all the TODO and GOALS store.dispatch fn with checkAndDispatch fns

// which can be in hundreds in big application
```

with this solution we are just over writing the store.dispatch fns with checkAndDispatch fn which solves the problem but not erron pron becouse it's possibile that you might have missed one or two dispatch fn which were in another modules.

thats why redux library provide a better way tos solve this problem. it has appyMiddleware fn which take middleware and these middleware intercept the dispatch(action) before action reaching the reducer.

## use redux middlewear to highjeck the dispatch fn

middlewear has the ability to intercept the dispatch(action) before action reaching the reducer.

dispatched action flows through the middleware before it reaches the reducers. The middleware has the ability to inspect the action, perform asynchronous tasks, modify the action or the state, and even dispatch additional actions.

```
dispatch(action) > middlewear() > rootreducer()
```

middleware is just a currying fn which locks in store,next,action before passing it to the next fn

- store : we lock in store so that we can intereact with the redux store ex: store.getState()

you may want to intreact with the store inside middleware Area to do something before and after the dispatch send action to the reducer.

- next: if you have multiple middleware then next refer to next in line/chain of the middleware fns. how to know which middleware is next in line. the one which you declear inside applyMiddleware(middleware_one,middleware_two) etc.

after all the middleware fns had been called next refer to dispatch fn so it's like dispatch(action) that's why it's important to return next(action) that will chain the next middleware till all the middleware get invoked then in the end it will return dispatch(action)

Note: if you don't return next(action) then the chain will break and next middleware or the dispatch will not run.

- action: we also lockin the action object which is needed for dispatch(action) (next(action)) and you can modify and do other stuff with it.

---

middleware currying help us in breaking the dispatch fn into a partically appied fn so that we can breakdown dispatch fucntionally into single fn which also allow us to lockIn variable (store,next,action) and we can intercept the dispatch action before it dispatches the action to the reducer.

---

```
// middlewear layout

const middleware = store => next => action => {
---------------------------------------------------------
code placed here will run before the dispatch(action)
get called
---------------------------------------------------------
const result = next(action) // we are invoking next middleware fn and passing action or dispatching action it
---------------------------------------------------------
code placed here will run after the dispatch(action)
get called
---------------------------------------------------------
return result // Returing out of middlewear fn help us to follow the middleware chain// required!!!
}

```

1. The middleware function is defined as a series of arrow functions that take one argument each: store, next, and action.

2. Code placed before const result = next(action); will run before the action is passed to the next middleware or the reducers. This section allows you to perform pre-processing or additional logic on the action or the store.

3. The next(action) statement is called to pass the action to the next middleware or to the reducers. This is the crucial step that allows the action to continue its journey through the middleware pipeline. The result of next(action) is typically assigned to a variable (result in this case) for further processing or to be returned later.

4. Code placed after const result = next(action); will run after the action has been processed by the subsequent middleware or the reducers. This section allows you to perform post-processing or additional logic based on the updated state or other information.

5. Finally, return result; is required to ensure that the result of next(action) is returned from the middleware function. Returning the result is important for the proper flow of the action through the middleware pipeline and for any subsequent middleware or the Redux store to receive the result.

```
import {createStore,combineReducer,appyMiddlewear} from redux

const checker = (store) => (next) => (action) =>{

      if(
        action.type === ADD_TODO &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    if(
        action.type === ADD_GOALS &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    return next(action)
}

const slice = combineReducer({
    todos,
    goals
})

const store = createStore(slice,appyMiddlewear(checker))

// first we need to write middlewear logic (check Fn)
// then attatch it to the store with the help of applyMiddlewear fn (redux inbuilt extension)
// use applymiddlewear fn as second argunment to the createStore fn.
// now your middlewear is attached to the store which will intrecept all the dispacted action.
```

## add multiple middlewares

```
import {createStore,combineReducer,appyMiddlewear} from redux

const checker = (store) => (next) => (action) =>{

      if(
        action.type === ADD_TODO &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    if(
        action.type === ADD_GOALS &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    return next(action)
}

const logger = (state) => (next) => (action) => {
    const result = next(action)
    console.log(result)
    return result

}

const slice = combineReducer({
    todos,
    goals
})

const store = createStore(slice,appyMiddlewear(checker,logger))

// just passin as second argunment after checker inside appyMiddlewear and that's it
// first checker middlerwear fn will be called then next = logger middlewear then next = dispatch(action)
```

```
// let dig depper and gain better understanding

const logger_one = state => next => action => {
    console.log(one)
    const result = next(action)
    console.log(two)
    return result
}

const logger_two = state => next => action => {
    console.log(three)
    const result = next(action)
    console.log(four)
    return result
}

const store = createStore(rootReducer,applyMiddleware(logger_one, logger_two))
---------------------------------------------------------------------------------------------------
// logged version
one
> jump into logger_two (chained) // also pass the updated action object to next middleware
three
> dispatch(action) // action dispatches to the reducer
four
> return back to logger_one
two
> return back to javascript environment

// if we don't use return in any of the chained middleware then we will not be able to go back to prev middleware and execute the code bellow the next(action) of the prev middleware.
```

### syncing redux state with the server state

```
// basic redux setup
----------------------------------------
// library code / just for refrence
function createStore(reducer){
let state;
let listiner = []

const getState = () => state;

const subscribe = (subFn) =>{

listiner.push(subFn)

return () => {
        return listener = listiner.filter((allSub)=> allSub !== subFn)
    }
}

const dispatch = (action){
    state = reducer(state, action)
    listiner.forEach((allSub)=> allSub())
}


return {
    getState,
    subscribe,
    dispatch
}

}
------------------------------------------------------
// redux code
import (createStore)

const slice = combineReducer({
    todos,
    goals
})


const store = createState(slice)

const ADD_TODO = 'ADD_TODO';
const REMOVE_TODO = 'REMOVE_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';
const ADD_GOAL = 'ADD_GOAL';
const REMOVE_GOAL = 'REMOVE_GOAL';

// actionCreater

const addTodoAction = (payload) => {
    return {
        type: 'ADD_TODO',
        payload
    }
}

const removeTodoAction = (payload) => {
    return {
        type: 'REMOVE_TODO',
        payload
    }
}

const toggleTodoAction = (payload) => {
    return {
        type: 'TOGGLE_TODO',
        payload
    }
}

const addGoalAction = (payload) => {
    return {
        type: 'ADD_GOAL',
        payload
    }
}

const removeGoalAction = (payload) => {
    return {
        type: 'REMOVE_GOAL',
        payload
    }
}

// create a reducer for todos
function todos(state = [] , action){

    switch(action.type){
        case ADD_TODO:
        return state.concat([action.todo])

        case REMOVE_TODO:
        return state.filter( allState => allstate.id !== action.id )

        case TOGGLE_TODO:
        return state.map( allState => allState.id === action.id ? Object.assign({},allState,{complete: !allState.complete}) : allState)

        default :
        return state
    }

}

// create a reducer for goals
function goals(state = [] , action){

    switch(action.type){
        case ADD_GOAL:
        return state.concat([action.goal])

        case REMOVE_GOAL:
        return state.filter( allState => allstate.id !== action.id )

        default :
        return state
    }

}
```

# adding data to redux store state from server (get method)

create new Action creater which will add all the items returned from the fetch all at once rather then using addTodosAction and addGoalsAction to add one item at a time.

you can add one item at a time but you have to itr over the return object and do forEaach on both(addTodosAction and addGoalsAction). which is extra work so it is better to create new actionCreater which will add all the items all at once when fetching completes

```
const RECIVED_DATA = 'RECIVED_DATA'

function recivedDataAction (todos,goals){
    return {
        type: RECIVED_DATA,
        todos,
        goals
    }
}
```

add that action type in both the reducer (Todos and Goals). when you dispatch the recivedDataAction it will invoke the rootReducer which will invoke All the childReducer (Todos and Goals) decleared in it. both (Todos and Goals) reducer will check action.type (RECIVED_DATA) and add there respective data to there respective slice of childState.

> NewLearning : you can listen for the same action type in multiple reducer.

```
// create a reducer for todos
function todos(state = [] , action){

    switch(action.type){
        // other cases

        case RECIVED_DATA :
        return action.todos

        default :
        return state
    }

}

// create a reducer for goals
function goals(state = [] , action){

    switch(action.type){
        // other cases

        case RECIVED_DATA :
        return action.goals

        default :
        return state
    }

}
```

now you just need to dispatch the recivedDataAction with recived data from the server.

```
// inside React component
promise.all([
    fetch(todos),
    fetch(goals)
    ]).then(([todos,goals])=>{
        store.dispatch(recivedDataAction(todos,goals))
})
```

then simply add that store data into your UI and dispay that data.

> this works fine but you will see a layout shift. becouse your page is loaded on the screen first without the data being added to the redux store. becouse fetching take time. so add a loading state which will be visible till the data has not been loaded once the data has been loaded to the store you can show data and make loading state disapear.

you can create a loading state inside your React component but we have a unique solution:

```
//create a new loading reducer

function loading (state = true, action){
    switch(action.type){
        case RECIVED_DATA :
        return false

        default :
        return state
    }
}
```

add that reducer to rootReducer/combineReducer. when ever you do anytype of dispatch loading reducer get also invoked

```
const slice = combineReducer({
    todos,
    goals,
    loading,
})

const store = createState(slice)
```

```
//inside your react app

const {todos,goals,loading} = store.getState()

if(loading === true){
    return <h2>Loading...</h2>
}

return JSX code
```

# sycn user action (add,delete) task with redux state and server state (post,delete,put method)

```
// react component
// remove an item

const {todo,goal} = store.getState()

const removeItem = (todo) =>{
---------------------------------------------
// syncing with the redux Store state
    store.dispatch(removeTodoAction(todo.id))
---------------------------------------------
// syncing with the Server state

    return FetchDelete(todo.id)
    .catch(()=>{
        console.log('there was an error while removing')

        store.dispatch(addTodoAction(todo))
    })

--------------------------------------------
}

return (
    <button onClick={removeItem}>remove<button>
)
```

what we are doing we are first updating the redux state so that our UI get updated instently (optimistic update) then we are updating the server state.

what we are doing is we are mixing the UI logic (react component) with data fetching logic

but it would be nice if we can seprate UI code with seprate DATA fetching code.

what we are doing is redux dispatch + server sync for that we can use redux middlewear.

# redux middlewear for mutation (custom Thunk middlewear)

what we will do we will move the DATA fetching logic inside actionCreater fn. so if an action require to sync data with the server. the actionCreater will be responseable for fetching the data.

what we will do we will create an actionCreater which will handle both the task (synk with the redux state and sync with the server state) and dispatch that actionCreater in the react component. then add a middlewear to catch that action before that action reaches to reducer.

actionCreater will be little different then normal. it will be a higher order fn which will return another fn.

```
const syncDeleteMutation = (todo) =>{
    return (dispatch) => {
        ---------------------------------------------
        // update redux state
            dispatch(removeTodoAction(todo.id))
        ---------------------------------------------
        // update server state
            return FetchDelete(todo.id)
            .catch(()=>{
                console.log('there was an error while removing')

                dispatch(addTodoAction(todo))
            })
        ---------------------------------------------
            }
}
```

what we are doing here is that we will call this syncDeleteMutation fn inside our react component which will take todo state and return an function. then with in the thunk middlewear we will catch that action fn and check for (typeof action === 'function') becouse every dispatch return an action and normal dispatch fn will return an object (action) but this is a function (action). which we can use to seprateout this action and perform (something) inside middlewear with this action.

this returned function take dispatch which is easily aviailable inside middlewear so we can invoke this inner fn within the middlewear by passing the store.dispatch.

```
const thunk = (state) => (next) => (action) =>{

if(typeof action === "function"){
    action(state.dispatch)
}

return next(action)
}

const store = createStore(rootReducer,applyMiddleware(thunk))
```

```
// updated react compnent

const {todo,goal} = store.getState()

const removeItem = (todo) =>{

// syncing with the redux Store state + sever state

    store.dispatch(syncDeleteMutation(todo))
}

return (
    <button onClick={removeItem}>remove<button>
)
```

## sync mutation data with redux and server with redux thunk.

using thunk middlewear is so common that there is a library for using thunk middlewear which provide thunk middlewear fn.

install redux-thunk from npm

when you install it ReduxThunk will be aviliable globaly.

then you don't have to provide your own custom thunk middlewear just use the library think middlewear

```
const store = createStore(rootReducer,applyMiddleware(reduxThunk.default))
```

//----------------------------XXX React-Redux (Old version) just for understanding XXX-----------------------------------//
//-------------------------------------------XXX OutDated Methods XXX-----------------------------------//

## redux meets react (old way just for understanding purpose)

Note: before you read understand redux was introduced in 2015 when react was using class component not function component. at that time connecting redux to react pass difficult. at that time we use context provider and context consumer no useContext was there.

redux does not work well with react. if you want to pass the redux store you need to either import it directly in all the modules or use connect and provider utility which is provided by redux-react library so that you don't have to prop drill store for every component who ever want to use redux store.

Note: even though original code for connect and provider use createContext and contextConsumer and code was written in class component i have tried to made the code more undestandable by using useContext and writting functional component for better understadning of the code.

> common solution:1

when a peice of state is required by multiple component. common solution is to lifting the shared state up in the react tree to the nearest common parent and do prop drilling.

this solution works but in big application this become redundent and more error pron.

> common solution :2

you can use react.createContent which provide a way to pass props to a component in react tree without propdrilling on every level of the tree.

how to use it:

ex: we want to use a button when clicked it will toggle a state between two different language. by clicking the button you are changing the language for the entire app. (English, Hindi)

what we need to solve this problem?

1. share data to the entire component tree. (createContext)

2. we need a way for any component in the component tree that require that data to be able to subscribe to it.(useContext())

typically you create a new context for each unique piece of data that needs to be availiable throughout your component tree.

```
const Mycontext = useContent(defaultData)

const data = "sharedData"

<Mycontext.provider value={data}>
<App/>
</Mycontext.provider>
------------------------------------------------
otherModule.js

const data = useContext() // "sharedData"
```

only limitation with passing the value in the content.provider is that you can't provide non-primitive data (object,functions) becouse context re-render when value changes and non-primitve data are unstable becouse react compare the data has changed or not by refrencial equality (oldObj === newObj).

```
const Mycontext = useContent(defaultData)

const [check,letsCheck] = usestate(false)

const state = useMemo(function(){
    return {
        lang: "english",
        toggleLang: function(){
            this.lang = this.lang === "english" ? "hindi" : "engish

            letsCheck(!check)
        }
    }
},[check])

<Mycontext.provider value={state}>
<Home/>
</Mycontext.provider>
---------------------------------------------------
function Home(){
    const {lang,toggleLang} = useContext(Mycontext)
    return <h2>{lang}</h2>
}

```

Note: you should not pass object in context but this is how in old code it was done so ignore it and just try to understand the bigger picture.

New Finding: as i said you should share object as context data becouse it will re-render. that's why we create a wrapper component and pass that object with the help of useContext and share data from that component when ever data changes. so that re-rendering happens inside the wrapper component but our real component get the data the currect way.

lets try to understand what we are tring to create???

1. we are tring to create an abstraction (library code) that can share store and made availiable to all the component of the react component tree. (provider that uses createContext)

2. and create an abstraction that can consume that shared state provided by Provider (called connect that uses useContext) and do three things.

- be able to share dispatch fn + other required state that the component needs.
- re-render the component when ever the requeired state changes in the store.

# basic understanding of useContext and createContext contextProvider

createContext: create a ref point from where data will be shared with the help of contextProvider. createContext take defualtValue which will be shared if no contextProvider was defiend/found by the useContext.

useContext: we consume the shared data by the help of useContext which get's it's data from the nearest context name up in the tree. it will either recive contextProvider (if provided) or defualt value from the contextCreater.

which means you can useContext can two ways.

```
import { createContext } from 'react';

const ThemeContext = createContext('light');
const AuthContext = createContext("No Author");

function App() {
  const [theme, setTheme] = useState('dark');
  return (
    <ThemeContext.Provider value={theme}>
        <Page />
    <AuthContext.Provider value={"Real Author"}>
        <Author/>
    </AuthContext.Provider>
    </ThemeContext.Provider>
  )};
--------------------------------------------------------------------------------
function Page(){
   const theme = useContext(ThemeContext); // "dark"
    const author = useContext(AuthContext); // No Author
}
--------------------------------------------------------------------------------
function Author(){
    const author = useContext(AuthContext); // Real Author

}

```

> inside Page
> as you can see theme get the ( <ThemeContext.Provider value={theme}>) value but author recived (createContext("No Author")) becouse we have not provided any ( <AuthContext.Provider value={value}>).

> inside Page

as you can see this author value is different becouse he get it's value from the (<AuthContext.Provider value={`Real Author`}>)

# part:1 use context to share store in the entire app insted of using props

this part is easy:

we will create a component that will be used as a wrapper and share all the provided prop to all of the children which will be rootComponent.

```
import {createContext,useContext} from react

const GlobalContext = createContext(null)

const Provider = ({store,Children})=>{
    return(
    <>
    <GlobalContext.Provider value={store}>
    {Children}
     <GlobalContext.Provider/>
    </>
    )
}

----------------------------------------------------
const MyApp = () => {
  return (
    <Provider store={store}>
      <ConnectedApp />
    </Provider>
  );
};
=================================================================
// wrapper
import {useContext} from react

const ConnectApp = ()=>{
const store = useContext(GlobalContext)
// now we have access to store here
return <App store={store}>
}
------------------------------------------------------
const App = ({store}) => {
// now we have access to store here
const {loading} = store

if(loading === true){
    return <h2>Loading...</h2>
}
return(
---------------------------------------------------------------------
    <Goals goals={goals} dispatch={store.dispatch}> // before
    <Todos todos={todos} dispatch={store.dispatch}> // before
---------------------------------------------------------------------
    <ConnectGoals/> // after
    <ConntectTodos/> // after
---------------------------------------------------------------------
)
}
=================================================================
// wrapper
import {useContext} from react

const ConnectGoals = ()=>{
const store = useContext(GlobalContext)
// now we have access to store here
const {goals} = store.getState()

return <Goals goals={goals} dispatch={store.dispatch}>
}
------------------------------------------------------
const Goals = ({goals,dispatch}) => {
// now we have access to goals and dispatch here
}
=================================================================
// wrapper
import {useContext} from react

const ConnectTodos = ()=>{
const store = useContext(GlobalContext)
// now we have access to store here
const {todos} = store.getState()

return <Goals goals={todos} dispatch={store.dispatch}>
}
------------------------------------------------------
const Todos = ({todos,dispatch}) => {
// now we have access to todos and dispatch here
}
=================================================================
```

we removed the propDrilling what we did was created a wrapper around each and every component that consume the store and the wrapper take tha value from the from the provider with the help of useContext and return the component that needs the required props. you can use that wrapper directly insted of using the component in the app so that you don't need to do prop driling.

creating wrapper for every component that needs the store is kind of redendent so we will create a utility for that wrapper called connect.

# part:2 create utility for doing all the work (Provider and connect)

//library Code

1. Provider Component: which provide store to entire app

```
// library Code for Provider
import {createContext,useContext} from react

const GlobalContext = createContext(null)

const Provider = ({value,Children})=>{
    // value here is store object
    return(
    <>
    <GlobalContext.Provider value={value}>
    {Children}
     <GlobalContext.Provider/>
    </>
    )
}

----------------------------------------------------
// developer code

const MyApp = () => {
  return (
    <Provider value={store}>
      <ConnectedApp />
    </Provider>
  );
};
```

2. Connect (wrapper) : to consume store.

- return a component , passing that component any data it needs from the store + a dispatch fn as default as a prop.

- re-render the component when ever store data gets updated.

so that user don't have to useContext and write a wrapper for every component.

```
const connectedApp = connect(function that take store as prop and return an object of required data  that component needs)(Component that you want to return)
```

as you can see this is curring fn. we are locking in ab object that we want to pass to the component and passing that object to that Component and returning it.

```
// how we will use connect
const connectApp = connect((store)=>({
loading: state.loading
}))(App)
```

// let's create connect untility

```
// functionObject = mapStateToPorps

import { useContext, useEffect, useState } from 'react';
import { GlobalContext } from './store';

const connect = (functionObject) => (Component) => {

      const [value, setValue] = useState(true);
      const store = useContext(GlobalContext);

      const { dispatch, getState, subscribe } = store;

      useEffect(() => {
        const unsubscribe = subscribe(() => {
          // Force re-render by updating a state value
          setValue((x) => !x);
        });

        return () => {
          unsubscribe();
        };
      }, []);


      const state = getState();
      const stateNeeded = functionObject(state);

      return <Component {...stateNeeded} dispatch={dispatch} />;
};
--------------------------------------
const connectApp = connect((store)=>({
loading: state.loading
}))(App)
```

> > now we can replace the wrapper component with our connect utility

> > code gets smaller Now we can replace the utility with real react-redux

```
import {Provider,connect} from react-redux

const MyApp = () => {
  return (
    <Provider store={store}>
      <ConnectedApp />
    </Provider>
  );
};
=================================================================
// wrapper
const ConnectApp = connect((state)=>({loading:state.loading}))(App)
------------------------------------------------------
const App = ({loading}) => {

if(loading === true){
    return <h2>Loading...</h2>
}
return(
---------------------------------------------------------------------
    <Goals goals={goals} dispatch={store.dispatch}> // before
    <Todos todos={todos} dispatch={store.dispatch}> // before
---------------------------------------------------------------------
    <ConnectGoals/> // after
    <ConntectTodos/> // after
---------------------------------------------------------------------
)
}
=================================================================
// wrapper
const ConnectGoals = connect((state)=>({goals:state.goals})(Goals))
------------------------------------------------------
const Goals = ({goals,dispatch}) => {
// now we have access to goals and dispatch here
}
=================================================================
// wrapper
const ConnectTodos = connect((state)=>({togos:state.todos})(Todos))
------------------------------------------------------
const Todos = ({todos,dispatch}) => {
// now we have access to todos and dispatch here
}
=================================================================
```

Learing here: if you want to pass an object as context value . don't pass tha value directly inside the component. create a wrapper component useContext there to get the value and there create logic when to pass the value to the real component. this will save you from un-nessesery re-render coz by the context with object as value and you can use your object to0 inside your entire app. in react-redux do this same thing: it uses Provider to share the object to the component Tree. and use connect to share the data to those component that needs the object. connect create an wrapper component which contain the logic when to share the object data to the real component that needs it.

connect has 2 main job:

- return a component , passing that component any data it needs from the store + a dispatch fn as default as a prop(useContext).
- re-render the component when ever store data gets updated(subscribe). and unsubscibe to the component when ever component unmounts.

//--------------------------------------XXX React-Redux (Old version) Completes XXX-----------------------------------//

## Folder Structor with redux

inside SRC folder create 4 Folders

- actions
- middlewear
- reducer
- components

1. actions (Folder)

create files based on each childState

each file contain

- action type variable
- action creater (return object)
- Thunk action creater (return function)(do async task)

ex: File Structure

```
- todos.js
- goals.js
- shared.js // those action which don't fall anywhere
```

ex: what you define each file

```
// todos.js

import {FetchDelete} from utility

export const ADD_TODO = 'ADD_TODO';
export const REMOVE_TODO = 'REMOVE_TODO';
export const TOGGLE_TODO = 'TOGGLE_TODO';

const addTodo = (payload) => {
    return {
        type: 'ADD_TODO',
        payload
    }
}

const removeTodo = (payload) => {
    return {
        type: 'REMOVE_TODO',
        payload
    }
}

const toggleTodo = (payload) => {
    return {
        type: 'TOGGLE_TODO',
        payload
    }
}

export const syncDeleteMutation = (todo) =>{
    return (dispatch) => {
        ---------------------------------------------
        // update redux state
            dispatch(removeTodo(todo.id))
        ---------------------------------------------
        // update server state
            return FetchDelete(todo.id)
            .catch(()=>{
                console.log('there was an error while removing')

                dispatch(addTodo(todo))
            })
        ---------------------------------------------
            }
}

```

2. reducer (Folder)

create index.js where you use all the reducer and export a combine reducer which will be used

```
const store = createStore(...here...)
```

```
//file structure
- index.js // combineReducer
- todos.js
- goals.js
- loading.js
```

```
// combineReducer
import {combineReducer} from redux
improt todos from samefolder/todos
improt goals from samefolder/goals
improt loading from samefolder/loading

export default combineReducer({
    todos,
    goals,
    loading
})
```

```
//todos.js

import {ADD_TODO,REMOVE_TODO,TOGGLE_TODO} from action/todos
import {RECIVE_DATA} from action/share

function todos(state = [] , action){

    switch(action.type){

        case ADD_TODO:
        return state.concat([action.todo])

        case REMOVE_TODO:
        return state.filter( allState => allstate.id !== action.id )

        case TOGGLE_TODO:
        return state.map( allState => allState.id === action.id ? Object.assign({},allState,{complete: !allState.complete}) : allState)

        case RECIVED_DATA :
        return action.todos

        default :
        return state
    }

}

export default todos
```

3. middlewear (folder)

have index to grouping all the middlewear and return applymiddlewear

```
const store = createStore(rootReducer,...here...)
```

```
// file structure
- index.js //applymiddlewear
- checker.js
- logger.js
```

```
import applyMiddlewear from redux
import checker from samefolder/checker
import logger from samefolder/logger
import thunk from action/thunk

export default applymiddlewear(
    checker,
    logger,
    thunk
)
```

```
// checker.js

import {ADD_TODOS} from action/todos
import {ADD_GOALS} from action/goals

const checker = (store) => (next) => (action) =>{

      if(
        action.type === ADD_TODO &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    if(
        action.type === ADD_GOALS &&
        action.name.toLowerCase().contains("bitcoin")
    ){
       return thow console.log("you can not submit bitcoin)
    }

    return next(action)
}

export default checker
```

4. Component (React)(Folder)

in your global Index.js you need to create store and provide it by wrapping the app component

```
// index.js
import App from src/Component/App
import rootReducer from src/reducer
import middlewear from src/middlewear
import createStore from redux
import Provider from react-redux

const store = createStore(rootReducer,middlewear)

<Provider store={store}>
<App/>
</Provider>
```

Note: in import you don't have to specify the file always. if you don't specify file but specify folder then import will automatically pick the index.js file as the root file of that folder.

//----------------------------------XXX Redux old version is Complete XXX-----------------------------------------//

### Redux Toolkit

# configureStore replace combineReducer + createStore

configurStore = combineReducer + createStore + thunk (redux-thunk libary, middlewear) + redux devTool (middlewaer) + childReducer state mutatation checker (middlewear)

if you accidently mutate state inside any case of childReducer your entire App will crash and you will see a console warning

This error message is a good thing - we caught a bug in our app! configureStore specifically added an extra middleware that automatically throws an error whenever it sees an accidental mutation of our state (in development mode only). That helps catch mistakes we might make while writing our code.

```
// old redux

import todos from todos
import goals from goals
import combineReducer from redux
import createStore from redux
import thunk from redux-thunk
import reduxDevtool from reduxDevTool
const slice = combineReducer({
    todos,
    goals
})

const store = createStore(slice,thunk,reduxDevTool)
-------------------------------------------------------
// redux toolkit
import todos from todos
import goals from goals
import configurStore from reduxToolkit

const store = configurStore({
    reducer: {
        todos,
        goals
    }
})

```

## Package Cleanup

old redux had to install some external library like redux thunk, reselect which are already included in redux-toolkit.

we can switch our createSelector import to be from '@reduxjs/toolkit' instead of 'reselect'.

# createSlice replace childReducer + action + actionCreater

Redux Toolkit has a createSlice API that will help us simplify our Redux reducer logic and actions. createSlice does several important things for us:

- We can write the cases of reducers as functions instead of having to write a switch/case statement
- The reducers will be able to write shorter immutable update logic
- All the action creators will be generated automatically based on the reducer functions we've provided

# Using createSlice

createSlice takes an object with three main options fields:

- name: a string that will be used as the prefix for generated action types
- initialState: the initial state of the reducer
- reducers: an object where the keys are strings, and the values are "case reducer" functions that will handle specific actions

```
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  entities: [],
  status: null
}

const todosSlice = createSlice({
  name: 'todos',
  initialState,
  reducers: {
    todoAdded(state, action) {
      // ✅ This "mutating" code is okay inside of createSlice!
      state.entities.push(action.payload)
    },
    todoToggled(state, action) {
      const todo = state.entities.find(todo => todo.id === action.payload)
      todo.completed = !todo.completed
    },
    todosLoading(state, action) {
      return {
        ...state,
        status: 'loading'
      }
    }
  }
})

//  these are action creater for todos
export const { todoAdded, todoToggled, todosLoading } = todosSlice.actions
// this is childReducer of todos
export default todosSlice.reducer
```

There's several things to see in this example:

- We write case reducer functions inside the reducers object, and give them readable names.
- createSlice will automatically generate action creators that correspond to each case reducer function we provide.
  for example :

```

name: todos

reducer Case: todoToggled(state, action) {
      const todo = state.entities.find(todo => todo.id === action.payload)
      todo.completed = !todo.completed
    }

// automatically generalted for us
actionCreater will be : todoToggle(payload){
    {
        type: todos/todoToggled,
        payload
    }
}

```

- createSlice automatically returns the existing state in the default case.

there is no need to write default case

- createSlice allows us to safely "mutate" our state!

redux-toolkit use lmmer library which uses javascript proxy utility which help us convert any mutable code into immutabale that's why writing mutable code inside createSlice and createReducer. don't write mutable code outside environment it will not be converted to immutable code becouse only createSlice and createReducer are wrapped with lmmer functionality.

Immer uses a special JS tool called a Proxy to wrap the data you provide, and lets you write code that "mutates" that wrapped data. But, Immer tracks all the changes you've tried to make, and then uses that list of changes to return a safely immutably updated value, as if you'd written all the immutable update logic by hand.

```
function handwrittenReducer(state, action) {
  return {
    ...state,
    first: {
      ...state.first,
      second: {
        ...state.first.second,
        [action.someId]: {
          ...state.first.second[action.someId],
          fourth: action.someValue
        }
      }
    }
  }
}
-----------------------------------------------------------
function reducerWithImmer(state, action) {
  state.first.second[action.someId].fourth = action.someValue
}
```

- But, we can also make immutable copies like before if we want to

Immer still lets us write immutable updates by hand and return the new value ourselves if we want to. You can even mix and match. For example, removing an item from an array is often easier to do with array.filter(), so you could call that and then assign the result to state to "mutate" it:

```
// can mix "mutating" and "immutable" code inside of Immer:
state.todos = state.todos.filter(todo => todo.id !== action.payload)
```

The generated action creators will be available as slice.actions.todoAdded, and we typically destructure and export those individually like we did with the action creators we wrote earlier. The complete reducer function is available as slice.reducer, and we typically export default slice.reducer, again the same as before.

So what do these auto-generated action objects look like? Let's try calling one of them and logging the action to see:

```
console.log(todoToggled(42))
// {type: 'todos/todoToggled', payload: 42}
```

createSlice generated the action type string for us, by combining the slice's name field with the todoToggled name of the reducer function we wrote. By default, the action creator accepts one argument, which it puts into the action object as action.payload.

Inside of the generated reducer function, createSlice will check to see if a dispatched action's action.type matches one of the names it generated. If so, it will run that case reducer function. This is exactly the same pattern that we wrote ourselves using a switch/case statement, but createSlice does it for us automatically.

---

Extar: info

> Flux Standard Actions (standerd way to write action object)

The Redux store itself does not actually care what fields you put into your action object. It only cares that action.type exists and is a string. That means that you could put any other fields into the action that you want. Maybe we could have action.todo for a "todo added" action, or action.color, and so on.

However, if every action uses different field names for its data fields, it can be hard to know ahead of time what fields you need to handle in each reducer.

That's why the Redux community came up with the "Flux Standard Actions" convention, or "FSA". This is a suggested approach for how to organize fields inside of action objects, so that developers always know what fields contain what kind of data. The FSA pattern is widely used in the Redux community, and in fact you've already been using it throughout this whole tutorial.

The FSA convention says that:

If your action object has any actual data, that "data" value of your action should always go in action.payload
An action may also have an action.meta field with extra descriptive data
An action may have an action.error field with error information
So, all Redux actions MUST:

- be a plain JavaScript object
- have a type field

And if you write your actions using the FSA pattern, an action MAY

- have a payload field
- have an error field
- have a meta field

---

when we call action creater we provide payload as argumnet if needed be. we can also send empty

```
dispatch(addTodos(35))
disptach(removeTodo())
```

what if we have to pass multiple argunments as payload (additional data)

```
dispatch(addTodos(35,43,92))
```

you can simple use payload inside your reducer and destructure the payload and use the data as you see fit for updating the state.

but what if you need to do some preprocessing on the args pass as payload before passing them to the actual reducer. ex: generating a unique ID.

> you can use preparation

createSlice lets us handle those situations by adding a "prepare callback" to the reducer. We can pass an object that has functions named reducer and prepare. When we call the generated action creator, the prepare function will be called with whatever parameters were passed in. It should then create and return an object that has a payload

```
// reducer fn become reducer object which contain prepare fn and reducer fn. when todoColorSelected action get dispatch first prepare fn will be called with all the payload provided and it will do the preprocessing then return an payload object which later be trasfered by reducer fn as action.payload.

todoColorSelected: {
      reducer(state, action) {
        const { color, todoId } = action.payload
        state.entities[todoId].color = color
      },
      prepare(todoId, color) {
        return {
          payload: { todoId, color }
        }
      }
    }
```

### createAsyncThunk replace thunk middlewear, action, actionCreater(disptach vala)

Redux Toolkit has a createAsyncThunk API that will generate these thunks for us. It also generates the action types and action creators for those different request status actions, and dispatches those actions automatically based on the resulting Promise.

> Using createAsyncThunk

createAsyncThunk accepts two arguments:

- an action types string.
- a callback function that will return a promise that promise will be captured by an action Creater as an argunmet for payload. this callback function also called payload creater. becouse it return a payload.

```
//layout for createAsyncThunk
export const action = createAsyncThunk(action.type, async callback function)
// async callback becouse async fn also returns a promise.
// async callback fn is payload creater
------------------------------------------------------------------------------------
export const fetchTodos = createAsyncThunk('todos/fetchTodos', async () => {
  const response = await fetch('/fakeApi/todos')
  return response.todos
})

export const saveNewTodo = createAsyncThunk('todos/saveNewTodo', async (text) => {
  const initialTodo = { text }
  const response = await client.post('/fakeApi/todos', { todo: initialTodo })
  return response.todo
})


dispatch(fetchTodos())
dispatch(saveNewTodo(text))
```

---

> let's understand payloadCreater a little better:

The process for saveNewTodo is the same as we saw for fetchTodos. We call createAsyncThunk, and pass in the action prefix and a payload creator. Inside the payload creator, we make an async API call, and return a result value.

In this case, when we call dispatch(saveNewTodo(text)), the text value will be passed in to the payload creator as its first argument.

While we won't cover createAsyncThunk in more detail here, a few other quick notes for reference:

- You can only pass one argument to the thunk when you dispatch it. If you need to pass multiple values, pass them in a single object
- The payload creator will receive an object as its second argument, which contains {getState, dispatch}, and some other useful values
- The thunk dispatches the pending action before running your payload creator, then dispatches either fulfilled or rejected based on whether the Promise you return succeeds or fails

> payload Creater callback function parameters:

- action : the dispatched action object. this is the first parameter that you can access.
- thunkAPI: The second argument is generated by createAsyncThunk is a thunkAPI object. This object contains several useful properties and functions.

> ThunkAPI object

- dispatch & getState: these are references to the actual dispatch and getState methods of the Redux store. You can use them inside the thunk to dispatch more actions or access the latest state of the Redux store.
- signal : This property provides an AbortController.signal function that can be used to cancel an in-progress request. It is helpful for handling cancellations or aborting ongoing operations.

- extra
- requestId
- rejectWithValue

---

createAsyncThunk will generate three action creators and action types, plus a thunk function that automatically dispatches those actions when called. In this case, the action creators and their types are:

```
//  action creater           action.type
- fetchTodos.pending: todos/fetchTodos/pending
- fetchTodos.fulfilled: todos/fetchTodos/fulfilled
- fetchTodos.rejected: todos/fetchTodos/rejected
```

as we will use createAsyncThunk for making a request to the server we can use these actionCreater to handle these three states of a request

- pending (in the fly)(loading)
- fulfilled (succesfully made the request and recived the response)
- rejected (the request has failed)

we can use these action.type to handle three different case in the reducer and change the state depending on the state of the request.

However, these action creators and types are being defined outside of the createSlice call. We can't handle those inside of the createSlice.reducers field, because those generate new action types too. We need a way for our createSlice call to listen for other action types that were defined elsewhere.

createSlice also accepts an extraReducers option, where we can have the same slice reducer listen for other action types which are not created by others. This field should be a callback function with a builder parameter, and we can call builder.addCase(actionCreator, caseReducer) to listen for other actions.

builder manage all the state changes of the server request

```
extraReducer: (builder) => { builder.addcase(action.pending,reducer fn)
                            .builder.addcase(action.fulfilled,reducer fn)
                            .builder.addcase(action.rejected,reducer fn)
}

```

```
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit'

// omit imports and state

export const fetchTodos = createAsyncThunk('todos/fetchTodos', async () => {
  const response = await client.get('/fakeApi/todos')
  return response.todos
})

const todosSlice = createSlice({
  name: 'todos',
  initialState,
  reducers: {
    // omit reducer cases
  },
  extraReducers: builder => {
    builder
      .addCase(fetchTodos.pending, (state, action) => {
        state.status = 'loading'
      })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        const newEntities = {}
        action.payload.forEach(todo => {
          newEntities[todo.id] = todo
        })
        state.entities = newEntities
        state.status = 'idle'
      })
  }
})

// omit exports
```

---

Redux-saga: also an external API which is an middlewear for handling async data just like asyncthunkmiddlewear both do the same job.

---

### Normalization and createEntityAdepter from redux-toolkit

# Why we need Normalization at first place ?

Noralization follow some Rules which helps us to minimize repetation of data at multiple places in the same object.

> Problem cozed by data redundency?

- if we update the redundent data at one place we need to look at all the palce where the redundent data is place and update them too. it is error pron easy to miss-out data at some place.
- we when insert some data we need to repeat the same data everytime we add new data to the object.(increase data redeundecy)
- non-Normalize data are generally nested which makes it harder to do immutable updates. and also harder to access spefic data.
- also non-normalize data is less performant becouse to find some data from a nested object we need to loop over entire object and do maping and filtering to just update single state.

> one more problem with nested data set

- if your data is nested let's say an array of data. who ever in your entire app is using useSelector for even a single value from the array will re-render coz re-render. becouse anytime even a single value changes in the array all the array value consumer useSelector will re-render to get the updated value even if what ever they consuming have not changed.

# what is Normalization?

Normalization is a technique for organizing the data into multiple related table, to minimize Data redundancy.

Normalization is an act of breaking an data object into multiple object and building relationship between those objects.

Goals of Normalization:

- Reducer redundant data (duplicacy) by dividing objects.
- Logically organlize data dependency.

benifits of Normalisation:

- less redundent data in the store.
- Performant in updating & accessing data.

> before we go deeper in Normalization we need to have some understanding of some basic terminology.

1. Entity: in simple terms entity is an object.

Google Defination: An object with distinct and independent existence.

an entity is an object which is unique and should not have duplicate (2 object with the same name) or two different objects name storing same data.

so that we don't have duplicate data inside our store object.

2. Attributes : all the keys inside of the object which store data are attributes. these key store data related to the object only.

```
const Student = {
    id: 1,
    name: "Dogesh",
    age: 26,
    address: "C-11/5 New york city"
}

Student = object/Entity
id,name,age,address = keys/Attributs
// all the keys are directly related to the object. there is no key there which don't have any direct relationship with the object.
```

> How to decide which is the entity in a large group of data.

look for Noun (tangleble or non-tengable) works. which standout and also check the keys and figureout which keys can this object can group together. if there are keys which are directly realted to a noun then thats a perfect entity/object.

# Normalization Form one (1NF)

each Normalization follow certain rules. Normalization form one follow the basic and min rules of Normalization technique.

Rules of 1NF:

- Each object key should only contain only single value. no array no object of data.
- all object keys should be atomic. meaning the data they store can not be ferther broken down.

```
// NON-1NF
const student = {
    1: {
        id:1,
        name : "Rajat",
        subjectID: ["Javascript","React"]
    },
    2:{
        id:2,
        name : "Rahul",
        subjectID: ["HTML","CSS"]
    },
    3:{
        id:3,
        name : "Manish",
        subjectID: ["CSS","React"]
    },
    4:{
        id:3,
        name : "Jaideep",
        subjectID: "Javascript",
    },
    Ids:[1,2,3]
}
-------------------------------------------------------
//1NF Version:1
// problem: we have null field and if we have 200 student and 100 of them only have one subjectID we will have 100 null field.
we want to avoid null field in our store.

const student = {
    1: {
        id:1,
        name : "Rajat",
        subjectIDOne: "Javascript",
        subjectIDTwo: "React",
    },
    2:{
        id:2,
        name : "Rahul",
        subjectIDOne: "HTML",
        subjectIDTwo: "CSS"
    },
    3:{
        id:3,
        name : "Manish",
        subjectIDOne: "CSS",
        subjectIDTwo: "React"
    },
    4:{
        id:3,
        name : "Jaideep",
        subjectIDOne: "Javascript",
        subjectIDTwo: Null,
    },
    Ids:[1,2,3]
}

-------------------------------------------------------
//1NF Version:2
// problem: we surely don't have nulls in our data but there is alot of redundent (id,name) data.

const student = {
    1: {
        id:1,
        name : "Rajat",
        subjectID: "Javascript"
    },
    1: {
        id:1,
        name : "Rajat",
        subjectID: "React"
    },
    2:{
        id:2,
        name : "Rahul",
        subjectID: "HTML"
    },
    2:{
        id:2,
        name : "Rahul",
        subjectID: "CSS"
    },
    3:{
        id:3,
        name : "Manish",
        subjectID: "CSS"
    },
    3:{
        id:3,
        name : "Manish",
        subjectID: "React"
    },
    4:{
        id:3,
        name : "Jaideep",
        subjectID: "Javascript",
    },
    Ids:[1,2,3]
}

-------------------------------------------------------
//1NF Version:3 final
// create a seprate object for student and subject. remove redundency. create relationship between them (we will learn this later)
const student = {
    1: {
        id:1,
        name : "Rajat",
    },
    2:{
        id:2,
        name : "Rahul",
    },
    3:{
        id:3,
        name : "Manish",
    },
    4:{
        id:3,
        name : "Jaideep",
    },
    Ids:[1,2,3]
}

const subjectID = {
    1:{
        id:1,
        subjectID: "HTML"
    },
    2:{
        id:2,
        subjectID: "CSS"
    },
    3:{
        id:3,
        subjectID: "Javascript"
    },
    4:{
        id:4,
        subjectID: "React"
    },
    ids:[1,2,3,4],
}
```

disadvantage of multi-Valued data:

1. harder to validate.
   for ex: you are validation location: it will contain house No, country code, etc. if all the value are group together without any context then it will be hard to validate the data.

2. retrieval of the data from the object.

- filtering user based on the country code. first you need to extract out the multi-valued input then you need to filter it. then find the user to the filtered value. it will be hard.

if an object contain key which contain multi-valued data in form of object or array. it will get's harder to (update,access) the value from the nested array or object we have to jump through alot of hoops with itr to reach to perticuler data.

3. consistany in format: the data should be inputed in the same format house NO. first then country code etc. which will never be the case or not all user will follow these rules.

4. sorting data: a group of value is harder to sort if they don't follow proper format.

solution: break the multi-value down into two different keys. in case of subject you can say subject_one, subject_two

# type of keys

every object that store data should have keys so that they can retrive data easyly:

- primary Key
- candidate key
- Foreign key
- Non-key attributs
- composite key

```
const student = {
    1: {},
    2:{}

1 and 2 are primary key which we can use to extract out a group of data which fether contain more keys and values.
}
```

> role of keys:

- extract out recods of data from the object.
- estabslish relationship across different objects.

> how to identify key?

- first identify candidate keys.
- choose primary key out of the candidate keys

> stages

- first candidate key
- second primary key
- third establish relationship
- fouth establish Foreign key

primary key and candidate key are keys that follow some rules and are used for keep and extract out a recod

```
const student = {
    1: {
        id:1,
        name : "Rajat",
    },
    2:{
        id:2,
        name : "Rahul",
    }
}

in student object 1,2 these are student_ID and are used as primary key which we can use to extract out some recods from the object

these are recods {id:1, name : "Rajat"}. key:value pair.
```

> step:1 candidate key

> how to identify candidate key from all the object keys/attributes.

1. every candidate key should be unique it should not have any duplication.
2. Must not contain Null or blank values. meaning all the candidate keys should have value
3. candidate key string should not be multip-valued or multi-part value. should be atomic and single value
4. key's value should not change.
5. avoid keys to be candidate keys which contain sensitive data.

```
const object ={
    studentid: 1
    examid: 124
    emailid: rajat@gmail.com
    firstname:
    lastname:
}

```

in this student object studentid, examid and emailid all three of them are potential candidate keys. there value will always be unique and never change.

these are natural key those keys which are already present in the givin data. but sometimes we don't have any natural key / candidate key then

we can use composite key: a key that it self is not unique but by combining it with other keys it becomes a unique identifier. for ex: username, password. it might possible to have same username and also same password to someone else usename . but combining both together makes a unique key.

but we don't want our key to be multi-part value so there is another solution:
we need to generate artifical key which act as candidate key ex ids. like nanoId or incrementing id one by one for each recods like we are already doing in students.

```
const student = {
    1: {
        id:1,
        name : "Rajat",
    },
    2:{
        id:2,
        name : "Rahul",
    }
}
```

> Step:2 primary key

- it should be candidate key first.
- an object should have only one type of primary key.

suggestion:

- it should be integer. helps in making relation with other objects.
- it should has fixed width. the number of char should not change ex: emailid can be a candidate key but the width can be big.
  so it is better to be initger becouse they have fix width in entire object. width don't change very much.
- primary key will be used as foreign key to make relation.

```
const object ={
    studentid: 1
    examid: 124
    emailid: rajat@gmail.com
    firstname:
    lastname:
}

examID may contain sensitive data and emailid has variable width so studentid will be our primary key
```

> if possible select primary key from the candidate keys but if not possible then generate key : ex:id

> Forign keys: primary keys that are used as refrence in other objects are Forign key and we have to often make realtionship between object we generally use generated keys as primary keys becouse they are easy to refrence in objects.

> Non-key: rest of the keys which are not primary key are non-key

# Data dependency

inside recods key:value pair is called data.

when value of one data key is dependent or can be determine by another data:key this is called functional dependency.

ex: in an object of employees, employee name is directly dependennt on employee ID. meaning if you know employee ID you can find out the employee name. if you change employee ID and look for employee name inside employee object then the employee name will also defferent/changed.

# Normalization second Form: 2NF

rules:

- it should be first form first.
- all the non-key should only be directly dependent on the primary key.

- if we have composite key (primary key made by two candidate keys) then all non-key should depend on both the keys.
- if a non-key is dependent on one of the primary key but not both. this is known as partical dependency.

```
const student = {
    1: {
        studentId:1,
        name:"Rajat",
        subjectId: "javascript",
        subjectFree: 200,
    },
    2: {
        studentId:2,
        name:"Jaideep",
        subjectId: "Python",
        subjectFree: 300,
    },
    3: {
        studentId:3,
        name:"Rahul",
        subjectId: "react",
        subjectFee: 500,
    },
}
you can only know student name if you know studentID, direclty dependent
you can know studentfee if you know studentID but you cal also know if you know subjectID so subject fee has dual dependendcy
---------------------------------------------------------
// break the table into two student and subject

const student = {
    1: {
        studentId:1,
        name:"Rajat",
    },
    2: {
        studentId:2,
        name:"Jaideep",
    },
    3: {
        studentId:3,
        name:"Rahul",
    },
}

const subject = {
        1: {
        subjectId: "javascript",
        subjectFree: 200,
        },
    2: {
        subjectId: "Python",
        subjectFree: 300,
    },
    3: {
        subjectId: "react",
        subjectFee: 500,
    },
}
```

student id (1,2,3) are primary keys and name, subjectId, subjectfee are non-keys. all the non should be directly dependent on the primary key. which also means a non-key should not be dependent on another non-key. but here subjectFee is dependent on subjectID . how if you change subjectid subject fee will also change. which is wrong.

it can also mean a primary key should only contain values that only change if the primary key changes otherwise not.

# RelationShip:

uptil now with the help of normalization we have learned to break the objects into two different entities. now we will lean to make relationship between these two entities:

there are different type of relationship between two different entities:

- one to one
- one to many or many to one
- many to many

Process:

- first findout that there is somekind of relationship between these two objects?
- then identify which type of relationship is this?
- Make changes into the object accordingly.

> first findout that there is somekind of relationship between these two objects?

you can findout that relationship exist by checking:

- is non-key partically dependent.
- we will try to check can either of the objects come under each other umrella or not. if yes then there is relationship.

```
case 1: can catagories and user be in any relationship?
is it possiblie to categories user in your app . No
are user have categories in your app. No
No relationship betweem these two.

case 2:  can catagories and post be in any relationship?
is it possiblie to categories post in your app . Yes
is it possible post have categories in your app. No
there is partical relationship. but there is.

```

> then identify which type of relationship is this?

in order to check which type of relation both object are we need to look at the relationship from both side.

- can one record from object1 can be realted or associated with one or many record object2.
- can one record from object2 can be realted or associated with one or many record object1.

1. One to Many Relationship

```
case student and subject.
can one student can take/study one or more subject. // one student can take one subject (assume).
can one subject can be taken/studied by one or many student. // one subject can be taken by many student.

      student  subject
        1         1
   +    M         1
-------------------------
final   M         1       = many to one

we add from up-to down and if any of the value is many then it's final value is many otherwise it is one
our relation come out to be many to one
```

> Make changes into the object accordingly.

we make changes on the object which has many relationship or the child between the two object.

---

how to know which object is the parent and which object is the child. simple check the dependendy. meaning the one who need to depend on other object for ferther information. the one useing a refrence value is the child becouse that object can only be used to read the value we can not update the subject value from the student object.

student can only read the values of subject. and dependent on the subject to provide the value so it is dependent on the subject object.

---

in student, subject objects. student has many to one relationship with subject so we will make changes on student.

```
const student = {
    1: {
        studentId:1,
        name:"Rajat",
        subject: 1                      // updated added new record add Forign key
    },
    2: {
        studentId:2,
        name:"Jaideep",
        subject:2                       // updated added new record add Forign key
    },
    3: {
        studentId:3,
        name:"Rahul",
        subject:3                       // updated added new record add Forign key
    },
}

const subject = {
    1: {
        subjectId: "javascript",
        subjectFree: 200,
    },
    2: {
        subjectId: "Python",
        subjectFree: 300,
    },
    3: {
        subjectId: "react",
        subjectFee: 500,
    },
}

// Forign keys are primary keys of object which is used as a refrence in other object
// we add forign key to make relationship between two object

```

when we ask recod of student we we will get studentID,name,subject (refrence/forign key) then later use that forign key to extract data related to student from the subject object by providing that forign/primary key to the subject object.

2. Many to Many Relationship

same example:

```
case student and subject.
can one student can take/study one or more subject. // one student can take Many subject !!! MANY
can one subject can be taken/studied by one or many student. // one subject can be taken by many student.

      student  subject
        1         M
   +    M         1
-------------------------
final   M         M       = many to one

our relation come out to be many to one
```

> Make changes into the object accordingly.

When we are in many to many relationship we have to create an additional object call link object.

```
// without linkTable if we try to solve it as one to many relationship.
// we will break the rule of 1NF: one key should only have one value.
//here we have array of value which is not currect.

const student = {
    byId:{
        1: {
            Id:1,
            name:"Rajat",
            subject: [1,2]
        },
        2: {
            Id:2,
            name:"Jaideep",
            subject: [2,3]
        },
        3: {
            Id:3,
            name:"Rahul",
            subject: [3]
        },
    }
    allIds:[1,2,3]
}

const subject = {
  byId:{
        1: {
            id:1
        subjectId: "javascript",
        subjectFree: 200,
        },
    2: {
        id:2
        subjectId: "Python",
        subjectFree: 300,
    },
    3: {
        id:3
        subjectId: "react",
        subjectFee: 500,
    },
  }
  allIds:[1,2,3]
}
```

```
// link object

const studentSubject = {
    byId:{
            1:{
                id:1,
                student: 1,
                subject:1
            },
            2:{
                id:2,
                student: 1,
                subject:2
            },
            3:{
                id:3,
                student: 2,
                subject:2
            },
            4:{
                id:4,
                student: 2,
                subject:3
            },
            5:{
                id:5,
                studentid: 3,
                subjectid:1
            },
    }
    allIds:[1,2,3,4,5]
}

const deleteSubjectRelation = (sub) => {
  let newArray = [];

  studentSubject.allIds.forEach((val) => {
    if (studentSubject.byId[val].subject === sub) {
      delete studentSubject.byId[val];
    } else {
      newArray.push(val);
      studentSubject.allIds = newArray;
    }
  });
};

// primarly key here is unique id we have generated for studentSubject : byId
// inside that we store relationship object : which contain:
-   studentSubjectId,studentId,subjectId
```

3. One to One Relationship

```
case student and subject.
can one student can take/study one or more subject. // one student can take one subject (assume).
can one subject can be taken/studied by one or many student. // one subject can be taken by one student.

      student  subject
        1   ->    1
   +    1   <-    1
-------------------------
final   M         1       = many to one

we add from up-to down and if any of the value is many then it's final value is many otherwise it is one
our relation come out to be many to one
```

> Make changes into the object accordingly.

Now we don't have any many side object. now we judge on the bases of who is dependent on whom More.

in student and subject they both can live alone becouse subject has it's own identity and student has it's own identity. now we will see the overall context. of app? who is more important. if we think this is a library app then books are more important. we will like to know which subject is borrowed by which student. if we see this as school and collage app then student is more importent. we like to know which student has taken which subject.

we look at both the object and see which object can not exist stand alone in the store.

```
const product = {
    byId:{
        1:{
            id:1,
            name: car
        },
        2:{
            id:2,
            name: bike
        }
    },
    allIds:[1,2]

}

const stock = {
    byId:{
        1:{
            id:1,
            price:200
        },
        2:{
            id:2,
            price:300
        },
    }
}
```

in these objects of product and stocks. we can clearly see stocks are dependent on price direclty. becouse you can not tell which product these stock are talking about.

parent: product
child: stocks

in these kind of seniros we need to add a new recod for linking the relationship: new forign key to the child key.

parent can exist alone but child need refrence to know in what context is this is talking about.

```
const stock = {
    byId:{
        1:{
            id:1,
            product:1,
            price:200
        },
        2:{
            id:2,
            product:2,
            price:300
        },
    }
}

product is product id
```

# Normalization Third Form (3NF)

> Rules to be Third Form:

- it should be in second Form first
- All non-keys should be directly dependent on the Primary key. these should not be any transitive dependency.

Transitive dependency is when a non-key is dependent on another non-key and that other non-key is dependent on the primary key.

you can also think this in terms of parent and child relationship: child:non-key should be direct decentent of the the parent:primaryKey. child:non-key should not be ansenter/indirect decendent of the the parent:primaryKey.

```
A<-B<-C

A: Primary key.
B,C : non-keys

C is dependent on B,
B is dependent on A,
C is dependent on A through B,
C is indirectly dependent on A,
C is in transitive dependency to A which is not allowed in 3NF.
```

```
const Books = {
    byId:{
        1:{
            id:1,
            genreId:4,
            genreName:SciFi,
            price:200
        },
        2:{
            id:2,
            genreId:3,
            genreName:Horror,
            price:200
        },
    },
    allIds:[1,2]

}


in this Books object BooksId is primary key and genreId,genreName,price are non-key and dependent on bookID.
BookId <-  genreId,genreName,price
but if we look carefully genreName is dependent on genreId directly and genreName is depending on the booksId indirectly.
how we know? becouse if you change the genreId it will change genreName.
BookId <- genreId <- genreName

genreName is transitivly dependent on the BookId.
if we don't remove the genreName we will find our self repreating genreName in the books object.
```

How to fix this?

```
----------------------------------------------------------------------------------
lets figurout the relationship first:
Book: one book can only blong to one Genre
genre: one genre can have Multiple book

        Books  Genre
        1      1
        M      1
------------------
final   M      1
----------------------------------------------------------------------------------
// Books will add a new Recod genre:ForignKey
const Books = {
    byId:{
        1:{
            id:1,
            genre:4,
            price:200
        },
        2:{
            id:2,
            genre:3,
            price:200
        },
    },
    allIds:[1,2]
}

----------------------------------------------------------------------------------
const Genre = {
    byId:{
        3:{
            id:3,
            name:Horror,
        },
        4:{
            id:4,
            name:SciFi,
        },
    },
    allIds:[3,4]
}

```

### Redux createAdepterEntities

Note: best part of normalized data is that all the keys of the object has only one value. which mean when we access the value one those useSelect will re-render who are accessing that specific value. if we have non-normalized data meaning nested or array. then anytime the array data updates all those who were accessing data from the array need to re-render becouse array has been updated. eventhough the value they were consuming haven't changed. that's why using normalized data is helpful.

createAdepterEntities is a a function when invoked generates a set of prebuilt reducers (reducerCreater fns), selectors and initalState for performing CRUD operations (create/read/update/delete) on a normalized state structure containing instances of a particular type of data object. These prebuilt reducer functions can be passed as case for reducers to createReducer and createSlice.

createAdepterEntities when invoke provide three things:

- initalState function
- a group of prebuilt reducer function.
- a group of selector function.

1. initalState function: getInitalState() provide normalize formated object as default initalState.

```
const booksAdapter = createEntityAdapter()
const initalState = bookAdapter.getInitalState()

const bookSlice = createSlice({
    name:"books",
    initalState
    reducer:{}
})
```

initalState of the createAdepterEntities looks like this:

```
// redux devtool
Books = {
  ids: []
  entities: {}
}
----------------------------------
// normalize object
Books = {
    byId:{},
    allIds:[]
}

ids === allIds and entities === byId
```

as you can see createAdepterEntities provide an normalize formated object as initalstate which makes it easier to setup normalized object.

> getInitalstate fn parameters

getInitalstate fn accepts an optional object as an argument. where you can define additional properties you want to add to the initalstate which are other then defualt initalState (entities object and ids array)

```
const booksAdapter = createEntityAdapter()
const initalState = bookAdapter.getInitalState({
    loading: false
})

const bookSlice = createSlice({
    name:"books",
    initalState
    reducer:{}
})
---------------------------------------------
Books = {
  ids: [],
  entities: {},
  loading: false
}
```

> let's talk about createEntityAdapter parameters.

```
const booksAdapter = createEntityAdapter()
```

createEntityAdapter fn take one argunmnet which is object parameter. and there are two options in object parameter.

- selectId function.
- sortComparer function.

1. selectId function: is a helper fn used to define ids array in the normalized object. when you add data to your store (Ex: books) by reducer methods then it's possible that. the data is not formated as (ids,entities). generally the data you provide is an array of objects (fetched data).

```
// general data that you recive from the server
 const data = {
    id: 1,
    name: "id labore ex et quam laborum",
    email: "Eliseo@gardner.biz",
    body: "laudantium enim quasi est quidem magnam voluptate ipsam eos\ntempora quo necessitatibus\ndolor quam autem quasi\nreiciendis et nam sapiente accusantium",
  }
```

this data array has object (entity) part currect but don't have seprate ids part. selectId function take in a function as argument . that function take entitie as parameter and what ever you return will be taken as id and that will be used as entities primarykey id and ids array.

```
// default selectId

const booksAdapter = createEntityAdapter({
    initalId: (entity) => entity.id
})
--------------------------------------------
our data object has id property and we want to use the same id as selectedID so we can go with the default setting
```

if your your object don't have id property like data.id then it is required to specify the location or property that your adapter use as unique id for your State.

```
// if you object don't have object.id then
 const data = {
    ProductId: 1,
    name: "id labore ex et quam laborum",
    email: "Eliseo@gardner.biz",
    body: "laudantium enim quasi est quidem magnam voluptate ipsam eos\ntempora quo necessitatibus\ndolor quam autem quasi\nreiciendis et nam sapiente accusantium",
  }
----------------------------------------------------------
const booksAdapter = createEntityAdapter({
    initalId: (entity) => entity.ProductId
})
```

2. sortComparer function: a function to sort the ids array.

if you define sortComparer fn and update the state by predefined Reducer fn provided by entityadapter then it will use sortComparer fn to update the Ids array accordingly which later can be used to show sorted data in the UI.

> sortComparer function parametes

it take a callback fn as argument which is normal array.sort fn. that callback fn take two entites instance as parameter which can be used to sort the ids array. sorting is based on numeric result (1, 0, -1) to indicate their relative order for sorting.

Note: sortComparer function will only be invoked if you use predefined CRUD reducer fn.

3. prebuilt reducer functions (CRUD functions)

createEntityAdepter also provide Ruducer fn to intereact with the normalized entity state.

- addOne: accepts a single object, and adds it if it's not already present.
- addMany: accepts an array of object or an object in the shape of Normalize Record, and adds them if not already present.

```
const todos:{
  '1': { id: '1', title: 'Buy groceries', completed: false },
  '2': { id: '2', title: 'Walk the dog', completed: true },
  '3': { id: '3', title: 'Do laundry', completed: false },
};
```

NOTE: add will do nothing if same object is already present in the entitiy.

- setOne,setMany: both are same as add + if object already exist in the entity it will replace it with the new recod object. (Ex:PUT method)

- setAll: accepts an array of object or an object in the shape of Normalized Record, and replaces all existing entities with the values in the array. meaning replace entire entity recods with the new entity.

- updateOne: accepts an update object which contain entity ID that you want to update the recod of and an object by the name of change which containing one or more new key:value that you want to change/replace/update the value of . it perform shallow copy of entire object and replace the keys: old value with the new one. (Ex: PATCH)

```
// general meaning

let obj = {
    1:{
        id:1,
        name:"ramu",
        age:23
    }
}

obj = obj.1.{...allProp , age:21}
------------------------------------------------------------
// what updateOne do
updateOne(state,{
        id: action.payload.id,
        changes: action.payload.changes,
      })
```

- updateMany: accepts an array of update objects, and performs shallow updates on all corresponding entities.

---

Note: update reducer

- If updateMany() is called with multiple updates targeted to the same ID, they will be merged into a single update, with later updates overwriting the earlier ones.

if you dispatch updateMany() multiple times targating same entities ID in a single render then all the update change will be merzed and last last one will overwite the previous one if they both are changing same key.

```
// think of every dispatch update many be like

{
    ...chaned1,
    ...chaned2,
    ...chaned3,
}

--------------------------------------------------------------
// App.js

// first time
dispatch(updateMany({
    id:3,
    changed:{
        name: "bhola",
        age: 16,
    }
}))

// second time
dispatch(updateMany({
    id:3,
    changed:{
        name: "gopi",
    }
}))

// both will be merzed and single dispatch will reach the rededucer

dispatch(updateMany({
    id:3,
    changed:{
        name: "gopi",
        age: 16,
    }
}))


```

- For both updateOne() and updateMany(), changing the ID of one existing entity to match the ID of a second existing entity will cause the first to replace the second completely.

if you change the ID (EX id changed from 2-> 3 of entitie_one) of an entities while using updateOne() or updateMany(). and there is pre-exsisting entitie by the same ID that you changed to (already there was an entitie_two with the ID of 3) then entitie_one will replace the entities_two completely.

```
const store = {
   user:{
       ids:[1,2],
       entitie:{
           1:{
               id:1,
               name:"John"
           },
           2:{
               id:2,
               name:"Alina",
               age: 23,
           },
       }
   }
}


dispatch(updateOneJohn({
   id:1,
   changed:{
       id:2
   }
}))

//this will happen
const store = {
   user:{
       ids:[1,2],
       entitie:{
           2:{
               id:2,
               name:"John"
           },
       }
   }
}

```

---

- upsertOne,upsertMany : accepts a single object. which contain {id,entity}. If an entity with that ID exists, then it will perform a shallow update. in shallow update: if the same key exisit it will change/update the value. otherwise it will add new key:value to the existing object.if entitiy does not exist with the same id. it will create new recod object.

upsertMany: accepts an array of object or an object in the shape of nromalized Record object that will be shallowly upserted.

- removeOne: accepts a single entity ID value, and removes the entity with that ID if it exists.
- removeMany: accepts an array of entity ID values, and removes each entity with those IDs if they exist.
- removeAll: removes all entities recods from the entity state object.

---

# Extra

- add : Exist: Do Nothing, Doesn't Exist: add it.
- set : Exist: replace it, Doesn't Exist: add it.
- update: Exist: Do Shallow copy and replace some values , Doesn't Exist: Do nothing.
- upsert: Exist: Do Shallow copy and replace some values , Doesn't Exist: Add new Recod object.
- remove: Exist: Remove recod from state object , Doesn't Exist: Do nothing.

---

these prebuld reducer methods don't have prebuild action creater thats why you should use them inside createslice reducer object by creating your own action creater.

```
dispatch(updateName({
    id: 1,
    change:{
        name:"bhola"
    }
})
)
-----------------------------------------------------
const userAdapter = createEntityAdepter()

const user = createSlice({
    initalState:userAdapter.getInitalState()
    ...
    reducer:{
        updateName:(state,payload) =>{
            updateOne(payload)
        }
    }
})
----------------------------------------
you can direclty pass the prebuild fns

const userAdapter = createEntityAdepter()

const user = createSlice({
    initalState:userAdapter.getInitalState()
    ...
    reducer:{
        updateName:updateOne
    }
})
// it will take the action object automatically becose as we created userAdapter he already knows the state object. all the prebuild reduce fn are reducerCreater fns.
```

Note: These methods do not have corresponding Redux actions created - they are just standalone reducers / update logic. It is entirely up to you to decide where and how to use these methods! Most of the time, you will want to pass them to createSlice or use them inside another reducer.

Note on shallow updates: updateOne, updateMany, upsertOne, and upsertMany only perform shallow updates in a mutable manner. This means that if your update/upsert consists of an object that includes nested properties, the value of the incoming change will overwrite the entire existing nested object. This may be unintended behavior for your application. As a general rule, these methods are best used with normalized data that do not have nested properties.

4. Prebuild Selector functions (memoized selector)

just like getInitalState() fn provide inital state from the entityAdapter. getSelectors() fn provide a bunch of selector to acess data from the Store State. these selector are prebuild with reselect createSelector fn so they are memoized. and you use them you need to use useSelector fn.

getSelectors() provide prebuild reselect selector which are simply properties and methods to acess data from the state.

there are two ways to setup getSelectors() fns.

1. Globalized: these selector already know the path to the entitie state you want to access data from.

you can specify path: by providing a fn as argunmemt to the getSelector fn.

```
// Globalized (i recommend using it becouse you can setup the state location once and use it to extract data)

const userAdapter = createEntityAdepter()
export const userSelector = userAdapter.getSelector( state => state.user)
------------------------------------------------------------------------------
//App.js

const user = useSelector(userSelector.selectIds)
```

2. Un-globalized: default, these reselect selector don't know the path location and you need to specify path location every time you access state.

you can simply use getSelectors() fn without any params

```
const userAdapter = createEntityAdepter()
export const userSelector = userAdapter.getSelector()
------------------------------------------------------------------
//App.js

const user = useSelector(userSelector.selectIds(state.user))
```

> different type of reselect fns

- selectIds: returns the state.ids array.

```
user = {
    ids:[],                      // return this ids array
    entities:{}
}
------------------------------------------------------------
const userGlobalized = useSelector(userSelectorGlobal.selectIds);
 const userUnGlobalized = useSelector((state) =>
    userSelectorUnGlobal.selectIds(state.user)
  );
```

- selectEntities: returns the state.entities lookup table.

```
user = {
    ids:[],
    entities:{}                  // return this entites object
}
------------------------------------------------------------------
  const userGlobalized = useSelector(userSelectorGlobal.selectEntities);
  const userUnGlobalized = useSelector((state) =>
    userSelectorUnGlobal.selectEntities(state.user)
  );
```

- selectAll: maps over the state.ids array, and returns an array of entities in the same order.

```
returns an array of entities by itr over the ids array.

function selectAll(){
    let array = [];
     ids.forEach((val) =>{array.push(entities[val])
    return array
})
}
------------------------------------------------------------
const userGlobalized = useSelector(userSelectorGlobal.selectAll);
const userUnGlobalized = useSelector((state) =>
    userSelectorUnGlobal.selectAll(state.user)
  );
```

- selectTotal: returns the total number of entities being stored in this state.

```
return ids.length
------------------------------------------------------------------
 const userGlobalized = useSelector(userSelectorGlobal.selectTotal);
 const userUnGlobalized = useSelector((state) =>
    userSelectorUnGlobal.selectTotal(state.user)
  );
```

- selectById: this is a function which take state and entitiy id , returns the entity[id] or undefined.

> if selector is globalized then provide global state as first arg and secnod arg the entitieID you want to access.
> if selector is unGlobalized then provide complete path of state as first arg and second arg is entitieID you want to access.

```
return entities[id]
--------------------------------------------------------------------------
  const userGlobalized = useSelector((state) =>
    userSelectorGlobal.selectById(state, 2)
  );
  const userUnGlobalized = useSelector((state) =>
    userSelectorUnGlobal.selectById(state.user, 1)
  );
```

### React-redux

# useSelector(selector , customComparitor )

Allows you to extract data from the Redux store state, when using a selector function in the component.

useSelector can take two parameters:

- First: selector: callback fn to extract data from the redux store.
- Second: shallow Equality comparitor function : used to define custom comparitor fn for re-rendering logic. (optional)

> First: selector: callback fn to extract data from the redux store.

this selector fn will take only one argunment that is redux store state. this fn is use to extract data from the redux store.

- it can return value by directly returning a value that was nested inside state.

```
const user = useSelector( state => state.user)
```

- or you can also return drived value

```
const user = useSelector( state => {
    const post = state.user.filter( val = val.id === 3 )
    return post
})
```

When will useSelector function invoked?

- The selector will be run whenever the function component renders.
- whenever an action is dispatched.

but selector fn actually Run when ever it's dependency changed (returnd value). if the dependency value hasn't changed then it will return the catched value (prev returned value). selector fn is more like a useMemo() fn. similar to useMemo it checks for value change by refrencial comparision.

which means if your return value is non-premitive value (fn or obj). then it will run on every component render and every dispatch fn.

You may call useSelector() multiple times within a single function component. just like react react-redux do batching of it's hooks together which will result in single re-render update. not multiple update for each hook.

> Second: shallow Equality comparitor function (Optional)

as we have discused earlier that selector fn do refrencail comparision. due to that if you return non-premitive value (fn or obj). you selector fn will run multiple times. which is not good you can provide your own comparison fn which take prev value and new value and if return true it will render.

```
import { useSelector } from 'react-redux'

// equality function
const customEqual = (oldValue, newValue) => oldValue === newValue

// later
const selectedData = useSelector(selectorReturningObject, customEqual)

```

eventhough you can provide custom compariotor it is recommended to use reSelect library which is commonly used for this problem.

> useSelector extra features Development mode checks (only for development)

useSelector runs some extra checks in development mode to watch for unexpected behavior. These checks do not run in production builds.

1. Selector result stability

In development, the provided selector function is run an extra time with the same parameter during the first call to useSelector, and warns in the console if the selector returns a different result.

a selector that returns a different result reference when called again with the same inputs will cause unnecessary rerenders.

By default, this will only happen when the selector is first called. You can configure the check in the Provider or at each useSelector call.

```
// set Globally for all useSelector
<Provider store={store} stabilityCheck="always">
  {children}
</Provider>
-----------------------------------------------------
// set locally for individual check

function Component() {
  const count = useSelector(selectCount, { stabilityCheck: 'never' })
  // run once (default)
  const user = useSelector(selectUser, { stabilityCheck: 'once' })
  // ...
}
```

2. No-op selector check

In development, a check is conducted on the result returned by the selector. It warns in the console if the result is the same as the parameter passed in, i.e. the root state.

A useSelector call returning the entire root state is almost always a mistake, as it means the component will rerender whenever anything in state changes. Selectors should be as granular as possible,
like state => state.some.nested.field.

By default, this will only happen when the selector is first called. You can configure the check in the Provider or at each useSelector call.

```
//Global setting via context
<Provider store={store} noopCheck="always">
  {children}
</Provider>
-------------------------------------------------------------------
//Individual hook setting
function Component() {
  const count = useSelector(selectCount, { noopCheck: 'never' })
  // run once (default)
  const user = useSelector(selectUser, { noopCheck: 'once' })
  // ...
}
```

# useDispatch()

This hook returns a reference to the dispatch function from the Redux store. You may use it to dispatch actions as needed.

```
const dispatch = useDispatch()

dispatch(action)
```

Note: when passing the dispatch(action) to child as props it is recomanded to memoized it with useCallback or useMemo.

Note: The dispatch function reference will be stable as long as the same store instance is being passed to the <Provider>. Normally, that store instance never changes in an application.

However, the React hooks lint rules do not know that dispatch should be stable, and will warn that the dispatch variable should be added to dependency arrays for useEffect and useCallback. The simplest solution is to do just that:

```
export const Todos = () => {
  const dispatch = useDispatch()

  useEffect(() => {
    dispatch(fetchTodos())
    // Safe to add dispatch to the dependencies array
  }, [dispatch])
}
```

# useStore()

This hook returns a reference to the same Redux store that was passed in to the <Provider> component.

This hook should probably not be used frequently. Prefer useSelector() as your primary choice. However, this may be useful for less common scenarios that do require access to the store, such as replacing reducers.

```
import React from 'react'
import { useStore } from 'react-redux'

export const ExampleComponent = ({ value }) => {
  const store = useStore()

  const onClick = () => {
    // Not _recommended_, but safe
    // This avoids subscribing to the state via `useSelector`
    // Prefer moving this logic into a thunk instead
    const numTodos = store.getState().todos.length
  }

  // EXAMPLE ONLY! Do not do this in a real app.
  // The component will not automatically update if the store state changes
  return <div>{store.getState().todos.length}</div>
}
```

### Reselect library's createSelector method

First Problem: as we have discused earlier that useSelector fn do refrencial comparision. due to that if you return non-premitive value (fn or obj). you selector fn will run multiple times.

Second Problem: if you have to drive it meaning to get the value you need to do expensive calculation. (Ex: maybe chain of map().filter()) then every time your useSelector fn runs these expensive calculation drivin value makes your app slow. you need a way to memoized the value

```
const todos = useSelector( state => state.todos) // works fine
----------------------------------------------------------------------------------
// if the logic is expensinve
// this will make our app slow
const todos = useSelector( state => state.map().filter()) // data transformation

```

solution: use createSelector inbulit redux-toolkit for memoization the logic

createSelector was introduced in reslect library but due to it's requeirement on every project of redux app. redux-toolkit included the createSelector in it. so you don't have to download reselect library additionally.

# how createSelector work

createSelector only has a default cache size of 1, and this is per each unique instance of a selector. this is similer to useMemo and useCallback of react.

createSelector can accept multiple input selectors, which can be provided as separate arguments or as an array. The results from all the input selectors are provided as separate arguments to the output selector:

```
// slice
//layout
// both are same
const selector = createSelector([inputSelector_one,inputSelector_two,inputSelector_three],outputSelector)
const selector = createSelector(inputSelector_one,inputSelector_two,inputSelector_three,outputSelector)

//react component

const Todo = useSelector(selector)
// Todo will we run on every dispatch then createSelector fn will be called

all the inputselector are like dependency array of react all the inputSelector will Run and they will compare there returndValue from there own previously returnd value. if any of the inputSelector fn return value has changed then outputSelector will take all of the inputSelector return value as parameter and return a new value which will be cached.

if all of the inputSelector return value is same as the previously returned value then outputSelector fn will not be run it will be skipped and the catch value of the outputSelector will be used.

--------------------------------------------------------------------------------------
const selectA = state => state.a
const selectB = state => state.b
const selectC = state => state.c

const selectABC = createSelector([selectA, selectB, selectC], (a, b, c) => {
  // do something with a, b, and c, and return a result
  return a + b + c
})

// could also be written as separate arguments, and works exactly the same
const selectABC2 = createSelector(selectA, selectB, selectC, (a, b, c) => {
  // do something with a, b, and c, and return a result
  return a + b + c
})

// Call the selector function and get a result
const abc = selectABC(state)
```

When you call the selector, Reselect will run your input selectors with all of the arguments you gave, and looks at the returned values. If any of the results are === different than before, it will re-run the output selector, and pass in those results as the arguments. If all of the results are the same as the last time, it will skip re-running the output selector, and just return the cached final result from before.

This means that "input selectors" should be used to extract value out of the store object and check if anyValue has changed and return values, that value to output selecotor when value changes and the "output selector" should do the transformation/expensive calculatation are return the value.

# example of createSelect memoization

```
const state = {
  a: {
    first: 5
  },
  b: 10
}

const selectA = state => state.a
const selectB = state => state.b

const selectAvalue = createSelector([selectA], a => a.first)

const selectResult = createSelector([selectAvalue, selectB], (avalue, b) => {
  console.log('Output selector running')
  return avlue + b
})

const result = selectResult(state)
// Log: "Output selector running" // createselector is running for the first time (No memoization/no caching)
console.log(result)
// 15

const secondResult = selectResult(state)
// No log output // createselector is running for the second time with no change in the state (memoized/cached data return)
console.log(secondResult)
// 15

```

# createSelector Behavior

1.  by default, createSelector only memoizes the most recent set of parameters. That means that if you call a selector repeatedly with different inputs, it will still return a result, but it will have to keep re-running the output selector to produce the result:

```
const a = someSelector(state, 1) // first call, not memoized
const b = someSelector(state, 1) // same inputs, memoized
const c = someSelector(state, 2) // different inputs, not memoized
const d = someSelector(state, 1) // different inputs from last time, not memoized
```

2. you can pass multiple arguments into a selector. Reselect will call all of the input selectors with those exact inputs:

```
// layout for passing value

const todos = useSelector((state)=> someSelector(param1,param2,param3))

const someSelector = createSelector(
[inputSelector_one, inputSelector_two, inputSelector_three],outputSelector
)

// same param will be passed to all the inputSelector which was passed to the createSelector
const inputSelector_one = (param1)=>{
    return input_one
}
const inputSelector_one = (param1,param2,param3)=>{
    return input_two
}
const inputSelector_three = (param1,param2)=>{
    return input_three
}

const outputSelector = (input_one,input_two,input_three){
    return output
}
```

Note: it's important that all of the "input selectors" you provide should accept the same types of parameters. Otherwise, the selectors will break

# Reselect Usage Patterns and Limitations

1. Nesting Selectors : it is possible to pass one createSelector to another createSelector as inputSelector.

```
const selectTodos = state => state.todos

const selector_one = createSelector([selectTodos], todos =>
  todos.filter(todo => todo.completed)
)

const selector_two = createSelector(
  [selector_one],
  completedTodos => completedTodos.map(todo => todo.text)
)
```

2. Passing Input Parameters: you can pass n number of params to createSelector which will later be used by all the inputSelector.
   but for convinence you should use additional params as object so it will be easier for the inputSelector and select params rather then placing all the params in the fn params . ex: someSelector(state,otherArgs) here otherArgs is an object.

3. Selector Factories: it is quite possible for someSelector to be required by multiple useSelector. if you use the same someSelector at multiple place then it will not work becouse createSelector only has a default cache size of 1, and this is per each unique instance of a selector. then the solution is to create a selector factory fn which when called return a new createSelector instance everytime it is invoked and we can use that at multiple useSelector.

```
const makeSelectItemsByCategory = () => {
  const someSelector = createSelector(
    [state => state.items, (state, category) => category],
    (items, category) => items.filter(item => item.category === category)
  )
  return someSelector
}
```

# using createSelector with useSelect and react

1. Calling Selectors with Parameters

It's common to want to pass additional arguments to a selector function. However, useSelector always calls the provided selector function with one argument - the Redux root state.

The simplest solution is to pass an anonymous selector to useSelector, and then immediately call the real selector with both state and any additional arguments:

```
import { selectTodoById } from './todosSlice'

function TodoListitem({ todoId }) {
  // Captures `todoId` from scope, gets `state` as an arg, and forwards both
  // to the actual selector function to extract the result
  const todo = useSelector(state => selectTodoById(state, todoId))
}
```

2. Creating Unique Selector Instances

There are many cases where a selector function needs to be reused across multiple components. If the components will all be calling the selector with different arguments, it will break memoization - the selector never sees the same arguments multiple times in a row, and thus can never return a cached value.

previously we have created a factory selector fn which return a new instance of the someSelector fn but

in react we can use useMemo or useCallback pass emipty array as dependency array. what will happen if you call the useMemo version someSelector. it will call the someSelector for the first time and memoize it. so from next time you call useMemo version someSelector it it will return someSelector from useMemo's catch. which is like creating a new instance. useMemo catch will never refresh becouse there is no dependency on it.

```
import { makeSelectItemsByCategory } from './categoriesSlice'

function CategoryList({ category }) {
  // Create a new memoized selector, for each component instance, on mount
  const someSelectorWithMemo = useMemo(someSelector, [])

  const itemsByCategory = useSelector(state =>
    someSelectorWithMemo(state, category)
  )
}

```

# standered practice on how to use Selectors

just like action have action creater which makes creating action easy. similarly useSelecter require createSelector which makes it easy to select state from the store.

1. Define Selectors inthe same file as Reducers

multiple parts of the application may want to use the same lookups. Also, conceptually, we may want to keep the knowledge of how the todos state is organized as an implementation detail inside the todosSlice file, so that it's all in one place.

Because of this, it's a good idea to define reusable selectors alongside their corresponding reducers. In this case, we could export selectTodos from the todosSlice

```
import { createSlice } from '@reduxjs/toolkit'

const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    todoAdded(state, action) {
      state.push(action.payload)
    }
  }
})

export const { todoAdded } = todosSlice.actions
export default todosSlice.reducer

// Export a reusable selector here
export const selectTodos = state => state.todos
```

Note: in redux-toolkit we don't need seprate files for action,middlewear thats why we define selector in the reducer file.

if we happen to make an update to the structure of the todos slice state, the relevant selectors are right here and can be updated at the same time, with minimal changes to any other parts of the app.

2. don't memoize every selector

don't make every single selector memoized!. Memoization is only needed if you are truly deriving results, computationally expensive calculation and if the derived results would likely create new references every time.

A selector function that does a direct lookup and return of a value should be a plain function, not memoized.

```
// ❌ DO NOT memoize: will always return a consistent reference
const selectTodos = state => state.todos
const selectNestedValue = state => state.some.deeply.nested.field
const selectTodoById = (state, todoId) => state.todos[todoId]

// ❌ DO NOT memoize: deriving data, but will return a consistent result
const selectItemsTotal = state => {
  return state.items.reduce((result, item) => {
    return result + item.total
  }, 0)
}
const selectAllCompleted = state => state.todos.every(todo => todo.completed)

// ✅ SHOULD memoize: returns new references when called
const selectTodoDescriptions = state => state.todos.map(todo => todo.text)
```

3. reshape/Transform state as needed for the component

A Redux state often has data in a "raw" form, because the state should be kept minimal, and many components may need to present the same data differently. You can use selectors to not only extract state, but to reshape it as needed for this specific component's needs. That could include pulling data from multiple slices of the root state, extracting specific values, merging different pieces of the data together, or any other transformations that are helpful.

It's fine if a component has some of this logic too, but it can be beneficial to pull all of this transformation logic out into separate selectors for better reuse and testability.

### redux tool-kit (API)

# configureStore

A friendly abstraction over the standard Redux createStore function that adds good defaults to the store setup for a better development experience.

> Parameters

configureStore accepts a single configuration object parameter, with the following options:

that object can take multiple options:

1. reducer: it can be a single reducer fn and it can be an object of multiple reducer fns.

- if it is a single reducer fn then it will be taken as the root reducer.
- if it is an object of multiple reducer fn then a root reducer is creatred as object automatically and then all the object reducer (key:reducer) will creater there own slice automatically adding to the root reducer.

2. middlewear: An optional array of Redux middleware functions

If this option is provided, it should contain all the middleware functions you want added to the store. configureStore will automatically pass those to applyMiddleware.

If not provided, configureStore will call getDefaultMiddleware and use the array of middleware functions it returns.

Note: if you provide middlewear then redux-toolkit will remove all the default middlewear. and only apply middlewear that you have provided.

if you want to use the default middlewear added by the redux-toolkit (which you should) then use getDefaultMiddlewear which will keep all the default middle of redux-toolkit and add your own custom middlewear with them.

---

- getDefaultMiddleware: Returns an array containing the default list of middleware.

when you supply the middleware option, you are responsible for defining all the middleware you want added to the store. configureStore will not add any extra middleware beyond what you listed.

getDefaultMiddleware is useful if you want to add some custom middleware, but also still want to have the default middleware added as well:

How to use it?

```
// getDefaultMiddleware() will keep all the default + add new
const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
})
```

It is preferable to use the chainable .concat(...) and .prepend(...) methods of the returned MiddlewareArray instead of the array spread operator, as the latter can lose valuable type information under some circumstances.

> getDefaultMiddleware() parameters?

getDefaultMiddleware accepts an options object that allows customizing each middleware in two ways:

- you can exclude default middlewears by providing boolean false value.
- you can provide your own version of middlewear for exsisiting middlewear which will be used inplace of the defualt middlewear.

```
const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      thunk: {
        extraArgument: myCustomApiService,
      },
      serializableCheck: false,
    }),
})
```

---

3. devTools : default: true

If this is a boolean, it will be used to indicate whether configureStore should automatically enable support for the Redux DevTools browser extension.

4. preloadedState: An optional initial state value to be passed to the Redux createStore function.

5. enhancers: An optional array of Redux store enhancers, or a callback function to customize the array of enhancers.

An enhancer is a function that allows you to add functionality to Redux that it doesn't come with out of the box. an enhancer is a fn that take store as parameter and return a new store with additional fns. that enhance the redux. so enhancer is a parent wrapper around redux store.

middlewear is a form of enhancer

if you want to use some custom version of dispatch fn, or any new fns throughout the app. you can add those custom fns to enhancers and you access those methods directly from the store.

```
import {newDispatch} from redux-toolkit
```

### nanoId

redux toolkit also provide nanoId which you can use to provide unqiue Id when using normalized store.
