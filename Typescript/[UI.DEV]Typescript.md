### UI.dev typeScript

typescript v4

the complete UI.dev Notes: https://coursehunters.online/t/ui-dev-learn-typescript-part-1/4201

### what is typescript

TypeScript checks your code as you write it to make sure the types are correct, and then compiles it to whatever version of JavaScript you need to support.

typeScript = linter + compiler

linter: check type Errors
Compiler: allow us to convert our TypeScript code to Any version of Javascript so that you can support old or new browser.

TypeScript is not a runtime language - all of the types are removed at compile time. It’s purpose is to encourage you, the developer, to use the types to add checks to your program so you have fewer errors and bugs.

TypeScript can also help with refactoring. Without TypeScript, you have no way of knowing all of the different places that need to be updated when you change something. TypeScript checks your change against any other files that might be affected and warns you if anything is broken. All that’s left is to follow each warning and make the appropriate change without needing to run your code.

### syntex

1. union : think of union as a group or commitee of types. if all the the member (type) agrees then you can use the methods asociated to them without nerrowing. if some of fewer member of the commitiee agrees then you need to nerrow it down.

```
type name = string | number

function checksize (param: name){
  retrun name.slice(0,2)
}

checksize(23454)  // 234
checksize("Raju")// Raj
```

both works becouse string and number both have sice method on them so there is no need to nerrowing the type becouse all member of the commity of types agrees.

```
type name = string | number

function checksize (param: name){
  if(typeof param === "string" ){
    return param.toLowerCase()
  }

  return param.tofixed(2)
}

checksize(23.454)  // 23.45
checksize("Raju")// raju
```

toLowercase can onlu work with string so we need to nerrow it down. + note that we don't need to newrrow it down for number becouse if it's not string then it is gurentied to be number. this is also one way to nerrowing .

2. Type Assertions: Sometimes you will have information about the type of a value that TypeScript can’t know about. TypeScript only allows type assertions which convert to a more specific or less specific version of a type. This rule prevents “impossible” coercions like:

```
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

typescript knows that myCanvas will return a HTMLElement but don't know the specific which one so you can make it more specific by telling it by yourself.

Note: that you can only speficify more specific or less specific to the typescript original infered assesment. you can't change the asserstion type.

```
const x = "hello" as number;
Error: Conversion of type 'string' to type 'number' may be a mistake
```

Sometimes typescript can be too restrictive even if you are providing the right assertion. at that time you can use two assertions, first to any (or unknown, which we’ll introduce later), then to the desired type:

```
const a = (expr as any) as T;
const a = (expr as undefined) as T;

```

3. Literal Types: there are type which are taken litraly meaning they can't be changed.

```
type name = "Ramu"
const name = "Ramu"
```

both are same: you can't specify const name with any other value. similarly eventhough type name is a string but it is a specific string ("Ramu") and can't be specify anyother string.

Note: type boolean === type true | false

Literal Type works great with premitve data type : number,string,boolean.

with non-premitive/structual types litral type does not work they become generic/non-litral . this is becouse structural type value can be modified later.

When you initialize a variable with an object, TypeScript assumes that the properties of that object might change values later.

```
const obj = { counter: 0 };
obj.counter = 1;
console.log(obj.counter) // 1
```

even though obj was stored in const variable still we are able to change the values of the objects.

TypeScript doesn’t assume the assignment of 1 to a field which previously had 0 is an error. Another way of saying this is that obj.counter must have the type number, not 0, because types are used to determine both reading and writing behavior.

so you can think of obj as

```
const obj = { counter: 0 };
type obj = { counter: number} //  ✅
type obj = {counter: 0} // 	❌
```

How to Fix this :

```
// this will not work
function handleRequest(url: string, method: "GET" | "POST"): void;

const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);
----------------------------------------------------------------
const req = { url: "https://example.com", method: "GET" };
type req = { url: string, method: string };

string !== "GET"
```

> use type assersion to overwrite the typescript judgement.

```
// you are telling that i am sure that method is "GET" typescript don't need to worry about that
function handleRequest(url: string, method: "GET" | "POST"): void;

const req = { url: "https://example.com", method: "GET" as "GET" };
handleRequest(req.url, req.method);
```

> use const type assersion which will convert entire req obj to litral type + readonly No writting allowed.

```
function handleRequest(url: string, method: "GET" | "POST"): void;

const req = { url: "https://example.com", method: "GET" } as  const ;
handleRequest(req.url, req.method);
---------------------------------------------------------------------
const req = { url: "https://example.com", method: "GET" } as  const ;
type req = { url: "https://example.com", method: "GET" }
```

as const convert anytype to litral type (take it litrally not genericly) and also make it Read only so you can't modify it later which is useful with working with structural type data.

4. Non-null Assertion Operator (Postfix!)

if you want to not get warning realted to undefiend (non) and null data variable then use non-null assertion operator.

```
// typescript will not worn you in strictNullChecks: on mode
const value!;
```

don't use it. most of the error in an code is cozed by null and undefined.

5. Call Signatures: how to write function as parameter in object

```
type obj = {
  name:"string",
  (param:number):boolean
}

function chole(fn:obj){
  return obj.name + obj(6)
}

function myFunc(someArg: number) {
  return someArg > 3;
}
myFunc.name = "Ramu";

chole(myFunc) // Ramu True
```

6. extends : this type is needed/Required to be a subset of that type. you can also think of it as === sign

> extend can be used with interface to not re-write same property and we can include them from another interface.

```
interface Book = {
name: string,
page: number,
}

interface Library extends Book ={
gener: string
}

let Book:Book = {name:"cookBook",page:201}
let library:Library={name:"cricket",page:"150",genre:"Sports"}
```

here we are saying is that Library is a subset of Book.
Book is superset and Library is a subset.
which implise the relationship that Library is also a part of Book. which implies Library has access to all the type properties of Books.

> extends can be used with generics to make your generic more constrents.

```
function longest<T extends { length: number }> (a: T, b: T) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}

// longerArray is of type 'number[]'
const longerArray = longest([1, 2], [1, 2, 3]);
// longerString is of type 'alice' | 'bob'
const longerString = longest("alice", "bob");
// Error! Numbers don't have a 'length' property
const notOK = longest(10, 100);
```

---

> Extra

in generics : when we use <T extends {}> it means T can be any data type except null or undefined. becouse in javascript every data type is a subset of object. ex: string is an object which contain many methods.

---

5.  Generics

generic is used when there is multiple place you need to specify same data type which is either specified explicitly or infer from the user input.

Note that it is not possible to create generic enums and namespaces.

```
function identity<Type>(arg: Type): Type {
  return arg;
}
-----------------------------------------------
//explicit
let output = identity<string>("myString");
-----------------------------------------------
// infered
let output = identity("myString");

```

> Specifying Type Arguments (some times generic infer don't work)

TypeScript can usually infer the intended type arguments in a generic call, but not always.

typescript is not good at infering values which are inside of an array. that's why we need to use tuples sometimes.

```
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}
------------------------------------------------------------------------------------------------
// Normally it would be an error to call this function with mismatched arrays:
const arr = combine([1, 2, 3], ["hello"]); //Type 'string' is not assignable to type 'number'.
------------------------------------------------------------------------------------------------
// If you intended to do this, however, you could manually specify Type
const arr = combine<string | number>([1, 2, 3], ["hello"]);
```

here we are saying union of string and number is allowed manually.

> > Guidelines for Writing Good Generic Functions

- When possible, use the type parameter itself rather than constraining it.

firstElement1 is a much better way to write this function. Its inferred return type is Type from the provided Argunment to the firstElement1 fn.

firstElement2’s inferred return type is "any" because TypeScript has to resolve the arr[0] expression using the constraint type, rather than “waiting” to resolve the arg provided by the user.

as we constrant type to any[] which means the provided arg should contain anything.

any is a superset to all the data type. and we already constained that arg should be any so it infer that return value should be any becouse any is the only arg that follow the constrains.

```
function firstElement1<Type>(arr: Type[]) {
  return arr[0];
}

function firstElement2<Type extends any[]>(arr: Type) {
  return arr[0];
}

// a: number (good)
const a = firstElement1([1, 2, 3]);
// b: any (bad)
const b = firstElement2([1, 2, 3]);
```

> Always use as few type parameters as possible

filter2 uses Func generic and extend it to a callback fn that take type as params and return a boolean. which add unnessesery complexity becouse Func generic is not doing something different or unnessesery here. look at finter1 which is much simpler.

don't add extra generics if they don't provide different data type.

we use generics so that in a function there is same type data is being used at multiple places (params,return value).

in filter2 Func just used Type type which is stupid. if Func was different data type then it can be useful but in this case it' just unnnessery.

```
function filter1<Type>(arr: Type[], func: (arg: Type) => boolean): Type[] {
  return arr.filter(func);
}

function filter2<Type, Func extends (arg: Type) => boolean>(
  arr: Type[],
  func: Func
): Type[] {
  return arr.filter(func);
}
```

> If a type parameter only appears in one location, strongly reconsider if you actually need it.

if there is only place where you need to speficy the data type then there is no need to go for generic just simply specify the data type on that one place.

```
// bad practice
function greet<Str extends string>(s: Str) {
  console.log("Hello, " + s);
}

greet("world");
--------------------------------------------------
// good practice
function greet(s: string) {
  console.log("Hello, " + s);
}
```

6. void: void represents the return value of functions which don’t return a value. It’s the inferred type any time a function doesn’t have any return statements, or doesn’t return any explicit value from those return statements:

```
// The inferred return type is void
function noop() {
  return;
}
```

In JavaScript, a function that doesn’t return any value will implicitly return the value undefined. However, void and undefined are not the same thing in TypeScript.

7. never: Some functions never return a value:

```
function fail(msg: string): never {
  throw new Error(msg);
}
```

The never type represents values which are never observed. In a return type, this means that the function throws an exception or terminates execution of the program.

8. readonly in objects: a property marked as readonly can’t be written to during type-checking.

the readonly modifier doesn’t necessarily imply that a value is totally immutable - or in other words, that its internal contents can’t be changed. It just means the property itself can’t be re-written.

```
type obj2 = {
readonly   resident: { name: "mukesh"; age: 23 },
}

obj2.resident = {game:"cricket"} ❌ // you can not re-asign value to resident
obj2.resident.name = "dogesh" ✅ // not applied to inside properties

```

9. Index Signatures: string index signatures are a powerful way to describe the “dictionary” pattern, they also enforce that all properties match their return type.

means that all the properties inside the interface should follow the same Index signature other wise typescript will throw error.

```
interface NumberDictionary {
  [index: string]: number;

  length: number; // ok
  name: string; // Errpr Property 'name' of type 'string' is not assignable to 'string' index type 'number'.
}
```

properties of different types are acceptable if the index signature is a union of the property types:

```
interface NumberOrStringDictionary {
  [index: string]: number | string;
  length: number; // ok, length is a number
  name: string; // ok, name is a string
}
```

10. Tuple:

> tuple can have optional type

```
type Either2dOr3d = [number, number, number?]; //where last type is number | undefined
```

> tuple can have spread operator too.

```
type StringNumberBooleans = [string, number, ...boolean[]];
type StringBooleansNumber = [string, ...boolean[], number];
type BooleansStringNumber = [...boolean[], string, number];
```

11. keyof : The keyof operator takes an object type and produces a (string or numeric) literal union of its keys. The following type P is the same type as type P = "x" | "y":

```
type Point = { x: number; y: number };
type P = keyof Point; // P = "x" | "y"
```

if object has index type signature. then keyof will return those types:

```
type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish; // A = number

type Mapish = { [k: string]: boolean };
type M = keyof Mapish; // M = string | number
```

Note: M = string | number becouse in javascript object obj[string] and obj["string"] both are same

12. Indexed Access Types

We can use an indexed access type to look up a specific property on another type:

```
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"]; // Age = number
```

The indexing type is itself a type, so we can use unions, keyof, or other types entirely:

```
type I1 = Person["age" | "name"]; // type I1 = string | number

type I2 = Person[keyof Person]; // type I2 = string | number | boolean

type AliveOrName = "alive" | "name";

type I3 = Person[AliveOrName]; // type I3 = string | boolean
```

13. Key Remapping via as

as is used to re-asign the key litral with something else.

```
//normal
type UserDataAccessors = {
  [K in keyof UserData]: () => UserData[K];
}

{
  name: () => string;
  age: () => number;
  registered: () => boolean;
}
----------------------------------------------------
// as keyremapping

type UserDataAccessors = {
  [K in keyof UserData as `get${Capitalize<K>}`]: () => UserData[K];
}

{
  getName: () => string;
  getAge: () => number;
  getRegistered: () => boolean;
}
```

14. template litral type built-in manupulation type :

To help with string manipulation, TypeScript includes a set of types which can be used in string manipulation. These types come built-in to the compiler for performance and can’t be found in the .d.ts files included with TypeScript.

- Uppercase<StringType> : Converts each character in the string to the uppercase version.

```
type Greeting = "Hello, world"
type ShoutyGreeting = Uppercase<Greeting> // type ShoutyGreeting = "HELLO, WORLD"
```

- Lowercase<StringType> : Converts each character in the string to the lowercase equivalent.

```
type Greeting = "Hello, world"
type QuietGreeting = Lowercase<Greeting> // type QuietGreeting = "hello, world"
```

- Capitalize<StringType>: Converts the first character in the string to an uppercase equivalent.

```
type LowercaseGreeting = "hello, world";
type Greeting = Capitalize<LowercaseGreeting> // type Greeting = "Hello, world"
```

- Uncapitalize<StringType>: Converts the first character in the string to a lowercase equivalent.

```
type UppercaseGreeting = "HELLO WORLD";
type UncomfortableGreeting = Uncapitalize<UppercaseGreeting>; // type UncomfortableGreeting = "hELLO WORLD"
```

15. typescript Module:

- Inline type imports

TypeScript 4.5 also allows for individual imports to be prefixed with type to indicate that the imported reference is a type:

```
// @filename: app.ts
import { createCatName, type Cat, type Dog } from "./animal.js";

export type Animals = Cat | Dog;
const name = createCatName();
```

Together these allow a non-TypeScript transpiler like Babel, swc or esbuild to know what imports can be safely removed.

## utility type

1. Partial<Type>: Constructs a type with all properties of Type set to optional. This utility will return a type that represents all subsets of a given type.

```
type User = {
    firstName: string,
    lastName: string
}

let firstUser:Partial<User> = {
    firstName: "John"
}
```

what it means that for firstUser. type user's all properties are optional. but only for firstUser not for all.

```
// this is similer to this but only for firstUser not all
type User = {
    firstName?: string,
    lastName?: string
}

let firstUser:user = {
    firstName: "John"
}
```

2. Required<Type> : opposite to Partical here it means all the properties are required.

```
type User = {
    firstName: string,
    lastName: string
}

let firstUser:Required<User> = {
    firstName: "John",
    lastName:"rastogi"
}
```

3. Readonly<Type>: Constructs a type with all properties of Type set to readonly, meaning the properties of the constructed type cannot be reassigned.

```
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};

todo.title = "Hello"; // Error:Cannot assign to 'title' because it is a read-only property.
```

Note: any[] means any array that contain anytype of values. but it does not include arrays/object which contain readonly too. which means readonly are special kind of object data. so to include then when selecting use readonly any[].

Note: when filtering out the readonly arr you need to filter it every step. it get pass through easily. so filter it on every stage. it might be repeative but it is what it is.

4. Record<Keys, Type>: this is used to combine/use two Types and use one as key and second one as value. and by combining both you made an Type interface object.

```
interface CatInfo {
  age: number;
  breed: string;
}

type CatName = "miffy" | "boris" | "mordred";

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

5. Pick<Type, Keys>: where you combine two types where first one is used as the base type and second is a union type which deside which properties from the basetype are allowed to pick/select.

```
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};

```

6. Omit<Type, Keys>: opposite to Pick. here we define which properties are not allowed to pick from the base type.

7. Exclude<UnionType, ExcludedMembers>: it take two params first one is Union Type (Base) which contain multiple types as union and second one is one or union type that you want to exclude the member from the base Union type.

```
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;

type T1 = "c"
```

8. Extract<Type, Union>: opposite to Exclude. here you define which properties you want to take if exist from the base Union type.

```
type T0 = Extract<"a" | "b" | "c", "a" | "f">;

type T0 = "a"
```

9. NonNullable<Type>: Constructs a type by excluding null and undefined from Type

```
type T0 = NonNullable<string | number | undefined>;

type T0 = string | number
```

10. Parameters<Type>: Constructs a tuple type from the types used in the parameters of a function type Type

```
type T1 = Parameters<(s: string) => void>;

type T1 = [s: string]
```

11. ReturnType<Type>: Constructs a type consisting of the return type of function Type.

```
type T0 = ReturnType<() => string>;

type T0 = string
```

### how to work with javascript set and map in typescript

1. new<Type>Set(): define which type of data it will take;
2. new <Type1,Type2>Map(): define type-1 as what type of keys you use and type-2 as what type of data you can store.

### how to catch error in typescript in try catch.

use instanceof : it will help us to narrow down that the params we have gonnen in catch is an error and do what you have to do. otherwise ignore it. and catch will always get error this is what catch cath's to begain with.

If you get something that isn't an error, it will fall down to the next block where you could do something else with it.

```
try{

}catch(error){
  if(error instanceof Error){
    return error.message
  }
}
```

### how to compose types from other types

```
interface User {
  id: string;
  firstName: string;
  lastName: string;
}

interface Post {
  id: string;
  title: string;
  body: string;
}

-------------------------------------------------
// what we want's is a combination of both type that looks like this :
let combineObj = {
    id: "1",
    firstName: "Matt",
    lastName: "Pocock",
    posts: [
      {
        id: "1",
        title: "How I eat so much cheese",
        body: "It's pretty edam difficult",
      },
    ],
  };

type combineUserAndPost = User & {posts:Post[]}

```

with this we have composed two types into one which have boths type .

### React + typescript

@React-typescript package has some built in type that you can use to define types:

- type ReactNode = ReactChild | boolean | null | undefined;
- type ReactChild = ReactElement | ReactText;
- type ReactText = string | number;

- type ReactElement = element that is returned by React.createElement(). under the hood that is what JSX code do. JSX code is compiled by babel to React.createElement().

React.createElement(component,props,children) and return an immutale object.

```
{
  '$typeof': Symbol(react.element),
  type: [Function: World],
  key: null,
  ref: null,
  props: {},
  _owner: null,
  _store: {}
}
```

this is ReactElement. reactElement is an object which contain some specific properties and it represet an react element which later react.render() uses to convert it into HTML and render to the DOM.

Note: type ReactElement and type JSX.Element both are same.

# what if your component doesn't return JSX.

like: string,number,boolean,undefined etc.

```
function RenderString() {
  return "Hello world!";
}
```

then typescript will throw an error. there are two ways to slove this problem:

1. overWrite typecript judgement by telling yourself what is the type with as assertions.

you can tell this is an reactNode which is like any for react and then say from any it is reactElement to be specific.

```
function RenderString() {
  return ("Hello world!" as ReactNode) as ReactElement;
}
```

2. use fragment around your return value which will automatically convet it into JSX.

```
function RenderString() {
  return <Fragment>Hello world!</Fragment>;
}
```

### some other types:

- type ComponentType<Params> : represent both class component or function component.

```
type ComponentType<P = {}> = ComponentClass<P> | FunctionComponent<P>;
```

- type FunctionalComponent or alias FC<Params> : This type includes annotations for common static properties that are added to function components, like displayName , propTypes , and defaultProps . It also includes the children prop {children:ReactNode} in your prop definition, and has an explicit return type.

```
import { FC } from "react";

const Message: FC<{ message: string }> = ({
  message,
  children,
}) => {
  return (
    <div>
      A message: {message}
      <br />
      Children: {children}
    </div>
  );
};
```

# useState

when you pass the inital value to the useState it will automatically set the type for the state. but sometime you want to specify type of the state. so that you can restrict types that can be used with the state.

useState is a generic function, so we can pass the type in before we call the function. when you don’t pass in an initial value, or you want to use a union type you can use that generic fn to tell the type

```
const [stringState, setStringState] = useState< string | undefined >();
```

when you are passing the state and statesetter fn to one component to another component as a prop it is importent to provide there types.

- type Dispatch<SetStateAction<Type>> :.this is the type of useState setFn().(inbuild in reactType) you can define the type too.

```
  stringState: string;
  setStringState: Dispatch<SetStateAction<string>>;
```

# useReducer

```
 const [state, dispatch] = useReducer(reduceFn, []);
```

- state type will be infered by typescript from the reducerFn state params automatically
- dispatch type will also be inffered from the reducer action params automatically and defined it like Dispatch<Actiontype>

```
type state : reducerFn state params type
type dispatchL Dispatch<Actiontype> from reducerFn action params type
```

Note: useState setFn() type and useReducer dispatchFn() has similar type signature

now we need to make reducerFn typescipt prof

reducerFn take two params (state,action) and return new state. so to make reducer fn typesafe we need to create interface for both params

- state interface
- action interface

```
// State type

type Category =
  | "Bread"
  | "Fruit"
  | "Vegetable"
  | "Meat"
  | "Milk";

interface ShoppingListItem {
  id: string;
  title: string;
  completed: boolean;
  category: Category;
}

type ShoppingListState = ShoppingListItem[];

------------------------------------------------------------------

// action type

interface AddAction {
  type: "add";
  category: Category;
}
interface EditAction {
  type: "edit";
  id: string;
  title: string;
}
interface DeleteAction {
  type: "delete";
  id: string;
}
interface CompleteAction {
  type: "complete";
  id: string;
}
export type ShoppingListAction =
  | AddAction
  | EditAction
  | DeleteAction
  | CompleteAction;

---------------------------------------------------------------
// now use these type inside your reducer fn and make reducer fn

function shoppingReducer(
  state: ShoppingListState,
  action: ShoppingListAction
): ShoppingListState {
  switch (action.type) {
    case "add":
      return state.concat({
        // Good enough random ID generator.
        id: `${Math.random()}-${Date.now()}`,
        title: "",
        category: action.category,
        completed: false,
      });
    case "complete":
      return state.map((item) => {
        if (item.id === action.id) {
          return { ...item, completed: true };
        }
        return item;
      });
    case "delete":
      return state.filter((item) => item.id !== action.id);
    case "edit":
      return state.map((item) => {
        if (item.id === action.id) {
          return { ...item, title: action.title };
        }
        return item;
      });
    default:
      return state;
  }
}

```

now inside your component whenyou define dispatch typescript will help you autocomplete the action type and also warn you if you specify an action type that does not exist.

# useEffect

```
function useEffect(
  effect: EffectCallback,
  deps?: DependencyList
): void;
```

- type DependencyList = ReadonlyArray<any>;
- type EffectCallback = () => void | (() => void | undefined); // take a callback fn which either return void or another callback

This type definition tells us three things:

- useEffect takes a function, called EffectCallback, as it’s first parameter.
- EffectCallback has to return either void , or a cleanup function. That cleanup function has to return void .
- useEffect can optionally take a list of dependencies, which is just an array of any .

## Memoization

useMemo, useCallabck are generic fn which automatically infer the type. so you don't have to.

# useMemo

```
function useMemo<T>(
  factory: () => T,
  deps: DependencyList
): T;
```

```
useMemo<string[]>(() => {
    // generate 100 random string uuid's
    return Array.from({ length: 100 }, () =>
      Math.random().toString(36).substr(2, 9)
    );
  }, []);
----------------------------------------------------
// or infer it useMemo<string[]>
useMemo(() => {
    // generate 100 random string uuid's
    return Array.from({ length: 100 }, () =>
      Math.random().toString(36).substr(2, 9)
    );
  }, []);
------------------------------------------------
or return explictly
useMemo(():string[] => {
    // generate 100 random string uuid's
    return Array.from({ length: 100 }, () =>
      Math.random().toString(36).substr(2, 9)
    );
  }, []);

```

# useCallabck

```
function useCallback<T extends (...args: any[]) => any>(
  callback: T,
  deps: DependencyList
): T;
```

```
useCallback<(userName:string) => void>(
    (buttonName) => {
      console.log(props.id, buttonName);
    },
    [props.id],
  );
----------------------------------------------
// or infer it useCallback<(userName:string)=> void
useCallback(
    (buttonName:string) => {
      console.log(props.id, buttonName);
    },
    [props.id],
  );
```

# Event handleing

all the Intrinsic Elements (HTML Tags/ HTML Elements) have preDefined type set to them which comes from JSX.IntrinsicElements which is an interface aviliable globally so you don't have to specify any type defination to the HTML tags which make our life eaiser when making an UI in react return JSX. these Intrinsic Elements can use any HTML attributes which are predefined for us in JSX.IntrinsicElements globally. as they are predefined they will guid us by autocomplete when providing a attribute to the HTML element and throw an error if you type wrong attribute in a HTML Element which does not use that.

> Types for Intrinsic Elements are predefined and aviliablle you don't need to define them

- HTML Elements
- HTML Attributes
- Aria Attributes
- React Synthatic Event handler

In React DOM, events are handled in a specific way for different types of elements. Intrinsic elements refer to the standard HTML elements such as <div>, <button>, <input>, etc. When you use these intrinsic elements in your React components and attach Event handlers like onClick or onKeyDown to them, the events work as expected.

However, when it comes to other custom components that you create yourself, React treats the onClick or onKeyDown prop just like any other prop. and you need to use ComponentPropsWithoutRef<"HTML Element"> to solve this more on this later...

If we are passing an event handler directly to an intrinsic element, TypeScript will infer the type of the function’s parameters for us.

```
<button onClick={(event) => {
  // (parameter) event: React.MouseEvent<HTMLButtonElement, MouseEvent>
}}>
```

TypeScript automatically knows that onClick is a mouse event and that its target is an HTMLButtonElement . This is the easiest way to use event handlers.

If you need to write out your event handler function separately, such as if we are passing the event handler as a prop, you have to define the appropriate types of the event.

you have two options to define the event type:

1. use the type annotation which TypeScript infers.

you can hoverOver the onclick event and typescript will tell the infer type. copy it and define it on the handler fn. and all the event return type is void. so that's make it easy for us to work with.

```
const App = () => {
  function handleOnChange(event: React.FormEvent<HTMLInputElement>):void {
    // ...
  }

  return <input type="text" onChange={handleOnChange}> // infer React.FormEvent<HTMLInputElement>
}
```

2. @React-typescript comes with a set of types for Event. you can choose from them and define it.

- AnimationEvent:Used for CSS Animations.

- ChangeEvent:Occurs when changing the value of <input>, <select>, and <textarea> elements.

- FocusEvent:Occurs when an element gains or loses focus.

- FormEvent:Occurs whenever a form or form element gains/loses focus, a form element value is changed, or the form is submitted.

- InvalidEvent:Fired when the validity restrictions of an input fail. For example, when someone tries to insert the number 20 into an input field with type="number" and max="10".

- KeyboardEvent:Represents user interaction with the keyboard, describing a single key interaction.

- MouseEvent: Events that occur due to the user interacting with a pointing device, like the mouse.

- PointerEvent: Events that occur due to user interaction with a variety of pointing devices, such as mouse, pen/stylus, and touchscreen, supporting multi-touch. Recommended for modern browsers (not for IE10 or Safari 12). Extends UIEvent.

- TouchEvent: Events that occur due to the user interacting with a touch device. Extends UIEvent.

- TransitionEvent:Used for CSS Transitions. Not fully supported by all browsers. Extends UIEvent.

- UIEvent: Base Event for Mouse, Touch, and Pointer events.

- WheelEvent: Occurs when scrolling using a mouse wheel or a similar input device. Note: Different from the scroll event.

- SyntheticEvent: this is any of Events. The base event for all the above events. Should be used when unsure about the specific event type to handle. It's a wrapper provided by React for event normalization.

```
const App = () => {
  const handleOnChange:ChangeEventHandler<HTMLInputElement> = (event) => {
    // ...
  }

  return <input type="text" onChange={handleOnChange}>
}
```

Notice: we don’t have to annotate anything inside the function itself; our ChangeEventHandler type takes care of that for us.

Note: all of the prebuild type are generic and requre you to type the HTML element type to use them.

# sometimes Event prop is not complete (event.target):

```
<form onSubmit={handleSubmit}>
  <div>
    <label>
      Email:
      <input type="email" name="email" />
    </label>
  </div>
  <div>
    <label>
      Message:
      <textarea name="message"></textarea>
    </label>
  </div>
</form>

```

take this form as an example: handleSubmit event should contain event.target which will contain all of the form input field with there name property like this:

```
event.type = {
  email : userEmail,
  message: userText,

}
```

but typescript is not smart enough to inffer these values automatically. Note: that it's not like these properties does not exisit on event.type. it's more like typescipt is not aware of them. typescript is not able to infer them automatically. so we need to tell typescipt manually . by as assertion type wideing technique.

```
interface FormFields {
  email: HTMLInputElement;
  message: HTMLTextAreaElement;
}
function handleSubmit(
  event: React.FormEvent<HTMLFormElement>
) {
  event.preventDefault();
  const target = event.target as typeof event.target &
    FormFields;

  const formValues = {
    email: target.email.value,
    message: target.message.value,
  };

  // Do whatever with the form values.
}
```

we can do that by creating an interface that we want the event.target to infer to. then we will use as assertion to tell him that we want all the predefined type by telling typeof event.target and these Formfields too. and now we can access these values too.

## inlince css

eventhough you don't need to specify that you are using style with Intrinsic Elements becouse they are predefined. but if maybe you need to deifne stype type then use CSSProperties as type

```
import { CSSProperties } from "react";

const Button = ({style,children}:{ style : CSSProperties, children : ReactNode}) => {
  return <button style={style}>{children}</button>;
};
```

you can also limit which properties you want to use from CSSProperties interface with omit or Pick utility.

```
type AllowedStyles = "display" | "backgroundColor";

const Button = ({ style, children }{style: Pick<CSSProperties, AllowedStyles>, children:ReactNode}) => {
  return <button style={style}>{children}</button>;
};

<Button style={{ fontSize: 24 }}>Click me!</Button>;
// Type Error: Type '{ fontSize: number; }' is not assignable to type 'Pick<CSSProperties, AllowedStyles>'
```

# wrapping Intrinsic Elements by Function component as using Function component as Intrinsic Elements

as we have already talked about Intrinsic Elements (HTMLElement,HTMLattributes,React Event) are predefined type you need not bother yourself.

but what if we use a component that return an Intrinsic Elements (like:Button) and we are using that component in our app as HTML element (like:button). the problem is when we use HTML attributes on the <Button> component in function params we need to define what type that HTML attributes is. but we don't know what type all the HTMLAttributes are ex(onclick,src,href,name,etc). becouse when we use HTMLAttributes on HTMLElement they are automatically takencare off but when we pass to our Component they act as prop for the component so we have to handle them manually.

to solve this problem @react-typescript provide ComponentPropsWithoutRef type which help us to extracts the props (HTMLAttibutes) from an HTML element. so that you don't have to define those HTMLAttributes type yourself.

Note: you can use ComponentProps<HTMLElement> Too which also do the same thing but omponentPropsWithoutRef is more explicit about not using ref.

Often, if we are creating a component that wraps an intrinsic element, we likely want to add more props that control some special behavior. We can use type widening to combine our props with the intrinsic props by either creating an intersection type with & or creating an interface that extends the element’s props.

```
// here we are using our Button component which take type,onClick,name,children as props
function App() {
  return <Button type="button" onClick={()=> do something} name={"raju"}> text </Button>
}

// here we defined name prop type becouse that not a normal HTMLAttribute type + using ComponentPropsWithoutRef<"HTMLElement"> to extract out all the common type from the button element which will cover in this case type,onclick which we are defining on our Button component

interface ButtonProps extends React.ComponentPropsWithoutRef<'button'> {
  name?: string;
}

function Button(props: ButtonProps) {
  const { name, ...props } = props;
  // do something with name prop
  const name = specialProp + Joshi
  return <button {...props} className={"name"} />;
}
```

Note: the syntex here is React.ComponentPropsWithoutRef<'button'> button here represent HTMLElement which will be refer to extract out common button Attributes and added to the button the real HTML element so No inforamtion get losset.

there are two ways you can combine your props with Intrinsic Elements props

```
// Option 1
type ButtonProps = {
  variant?: "primary" | "success" | "warning" | "danger";
} & ComponentPropsWithoutRef<"button">;

// Option 2
interface ButtonProps
  extends ComponentPropsWithoutRef<"button"> {
  variant?: "primary" | "success" | "warning" | "danger";
}
```

If we wanted to restrict which props the consumer of our component can assign to this element, we could also use TypeScripts utility types to Pick or Omit properties from the ButtonProps interface.

```
type AllowedAttributes = "onClick" | "type" | "name";

type ButtonProps = {
  variant?: "primary" | "success" | "warning" | "danger";
} & Pick<ComponentPropsWithoutRef<"button">,AllowedAttributes>;

```

just like ComponentPropsWithoutRef there is ComponentPropsWithRef too which is used to add HTMLAttributes + forwardRef Props too. more on this later...

# Context

React Context lets you pass values from a parent component to any of the children components. The way context is implemented makes it possible for the type definition of your context to be available wherever you consume it.

We create context using React.createContext . We have to pass in a default value. This is the value the context provides if it is accessed outside of a <Context.Provider> tree. The type of the entire context is inferred from the type of the default value.

Often, the default value is null or undefined . If we need to set an explicit type for our context, we can pass it in as a generic type.

```
import {
  createContext,
  Dispatch,
  SetStateAction,
} from "react";

interface ThemeModeInterface {
  mode: "dark" | "light";
  setMode: Dispatch<SetStateAction<"dark" | "light">>;
}
-------------------------------------------------------------------------------
// option:1 use union but downside is that everytime we acess data with useContext we need to check if the data exist or not

const ThemeModeContext = createContext<ThemeModeInterface | null>(
  null
);
----------------------------------------------------------------------------------
// option:2 non-null assersion operator. Lose typesafety. (My prefrence)

const ThemeModeContext = createContext<ThemeModeInterface>(
  undefined!
);
-----------------------------------------------------------------------------------
option:3 overrite it with as assertion

const ThemeModeContext = createContext(
  {} as ThemeModeInterface
);
```

so the thing is in context we don't have a solution where we can have our cake and eat it too. we have to take a round about.

i like option:2 but we loose some typesafety but we can fix it. the thing is we use the context default value only if the context is used out of context. means this is the only place where we can loose typesafety let's make that part typesafe. and everything will be ok.

we can create a custom checker fn which will use context and check if the context contain value or not . if it does it means we are inside context tree and everything is good. if not then we will intentically throw an error. which will take care of the typesafety.

```
// we will use useThemeMode inplace of useContext to access values inside components.

export const useThemeMode = () => {
  const themeMode = useContext(ThemeModeContext);
  if (!themeMode?.mode)
    throw new Error(
      "The theme mode context was accessed outside of the provider tree"
    );
  return themeMode;
};
```

now let's create context and use it.

```
export const ThemeModeProvider: FC = ({ children }) => {
  const [mode, setMode] = useState<"dark" | "light">("light");

  // Memoize the context value to avoid unnecessary re-renders
  const contextValue = useMemo(() => ({ mode, setMode }), [
    mode,
    setMode,
  ]);

  return (
    <ThemeModeContext.Provider value={contextValue}>
      {children}
    </ThemeModeContext.Provider>
  );
};
```

now where ever we use the context value will be automatically infered becouse we have set the values explicity with createContext<Type>().

# ref

refs are used for both holding references to DOM elements and storing bits of data that don’t affect rendering.

> holding references to DOM elements

if we are using ref as holding references to DOM elements. then we will initalize the ref value as null. and provide generic type to restric what types. we will need to check if the value exist or not becouse we have initalized the value to be null.

# Note: the format to tell the generical for if it's an input element is HTML-type-Element you can use as its as a format to define different types.

```
const Form = () => {
  const inputRef = useRef<HTMLInputElement>(null)

  const onClick = () => {
    if (inputRef.current) {
      inputRef.current.focus()
    }
  }
  return <input type="text" ref={inputRef} onClick={onClick}>
}
```

Note: we are explicitly passing a null value to our ref. we can leave it empity and then it will automatically infer to undefined so in our case inputRef will be type <HTMLInputElement> | undefined.

But as a rule ref can never be defined as type undefined. but why??.
the reason is: when your HTMLElement get's unmounted from the page. react-types will Set your HTMLElement automatically equal to null. (not undefined) so for react types they have choosen that ref can't be unassignable to undefined.

> storing bits of data that don’t affect rendering

Using refs for any other value, such as arbitrary strings or objects, works exactly the same as useState - whatever we pass as the initial value of the ref determines the type of the ref.

If we need to override that, we can provide a type to the generic.

```
const App = () => {
  const stringRef = useRef("Hello there!");
  const maybeNumberRef = useRef<number | null>(null);
  // ...
};
```

---

Extra: Ref

on type level ref function type can be defined by three ways: there are three type of overloard on ref function:

interface MutableRefObject<T>{
current:T
}

interface RefObject<T>{
readonly current:T | null
}

1. function useRef<T>(initialValue:T):MutableRefObject<T>

```
// when you pass a value
useRef<string>("abcd") // this value is mutable
```

2. function useRef<T>(initialValue:T | null):RefObject<T>

```
// when you pass null
useRef<string>(null) // this value is Readonly
```

3. function useRef<T = undefined>():MutableRefObject<T | undefined>

```
// when you don't pass anything
useRef<string>() // this value is mutable
```

---

# forwardRef

Ref forwarding lets you pass a ref from parent component to one of its children component.

When we wrap our component in React.forwardRef , we have to provide generic types for the ref itself and for the wrapped component’s props. The ref type is defined first , followed by the props, even though the function parameters put the ref after the props.

We have to provide the props type even if we’ve already defined those types in the function declaration. Typically, you’ll have the easiest time if you wrap your component as you define it, like this:

```
// format
const Input = forwardRef<refType, propType>((prop,ref)=>{})
-------------------------------------------------------------------------
import { forwardRef } from "react";
const Input = forwardRef<HTMLInputElement, { disabled?: boolean }>(
  ({ disabled }, ref) => {
    return <input ref={ref} disabled={disabled} />;
  }
);
```

## how to extract types from Components props;

type Props = Parameters<typeof Fn()>[0]

or
//with react types
type Props = ComponentProps<typeof Fn()>
