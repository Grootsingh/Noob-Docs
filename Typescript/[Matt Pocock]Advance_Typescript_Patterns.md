### Matt Pocock Cource TotalTypescript

---

1. Extra: typescript utility library

use ts-toolbelt is a library which contain large collection of typescirt utlitys.

2. Extra :https://github.com/type-challenges/type-challenges

A github repo which contain typescript challanges which you can use to improve your typescript skill

---

## Brandred types or Nominal Typing

```
declare const brand: unique symbol;
type Brand<T, BrandName> = T & { [brand]: BrandName };
```

Note: declear allow us to allow this type to be accesed Globally.

> why we create bread Type?

Brand type is helpful to validate data by assigning Label types which represent Different validation process. you can think of these validation process as Tickets. which let us know that data i am about to use come from the current path. by checking the label/ticket.

Branded types let you assign different ‘labels’ to values. which give them different type signatures to the values. you can define different type signature to different process to data and later pass them around. this will make sure that the data you are passing has been currectly processed and has the right signature type. this help us to create validation boundries of data that this data has the current signature and can be used currently.

```
// Password and Email type signatures
type ValidPassword = Brand<string, "Password">;
type ValidEmail = Brand<string, "Email">;


export const validateValues = (values: { email: string; password: string }) => {
  if (!values.email.includes("@")) {
    throw new Error("Email invalid");
  }
  if (values.password.length < 8) {
    throw new Error("Password not long enough");
  }

  return {
    email: values.email as Email,
    password: values.password as Password,
  };
};

const sendDataToTheServer = (values: { email: ValidEmail; password: ValidPassword }) => {
  // here we submit the values object which contain ValidEmail and ValidPassword
};

const onSubmitHandler = (values: { email: string; password: string }) => {
  const validatedValues = validateValues(values);
  sendDataToTheServer(validatedValues);
};
```

- in our Code before submiting email and password to the server we check that our email and password are in current format or not.

> How we did it Work?

- we have created two type signatures (Tickets) ValidEmail, ValidPassword which indicate that email type is ValidEmail indicating that EmailData has been validated same with ValidPassword. if it has the right ticket it means we can allow it.

- onSubmitHandler will take value object which contain email type string and password type string. we send that data to validatedValues Fn which check that data and validate it.

- in ValidateValues Fn we validate the email and password. one the data has been validated we will give assign them A ticket/signature/label/brand by changing the type signature of both of them.

```
type value = {
    email:string,
    password:string
}
-----------------------------------------
//Ticket as been assigned  by changing the type signature

type value = {
    email:ValidEmail,
    password:ValidPassword
}
```

- then we send the value to sendDataToTheServer fn which only accept props with the type signature of ValidEmail and ValidPassword. which act as type validator. like a Guard or TT to check if the data has the current Ticket or not. only allow the data to pass that has the current ticket.

- we can't pass the string type of email and password data to sendDataToTheServer fn. which is pretty cool.

- which make sure that data passed to the server is always validated. and this is how brand type can be helpful.

# Recod<key,value> vs object index value

```
// Record<key,value>
type Record<K extends string | number | symbol, T> = { [P in K]: T; }
---------------------------------------------------------------------------
// Indexing value in object
[index:Key]: Value
------------------------------------------------------------------------------
[index:Key]: Value === [P in K]:T
```

Record and Indexing in object do the same thing.

```
interface User {
  id: UserId;
  name: string;
}

interface Post {
  id: PostId;
  title: string;
}

```

// By indexing
interface checkById{
[index:PostId] : Post,
[index:UserId] : User
}
const db: checkById = {}

```
// By Record
const db: Record<PostId,Post> & Record<UserId,User> = {}
```

## typescript declear keyword

declare is used to tell the compiler that a particular variable or function exists but is outside the scope of typescript. we need to tell this becouse compiler check the code to provide type-checking and auto-complite and if we use wrong type defination then thow an error. declear keyword allow us to provide type defination for fns or variables without typescript checking that these defination are currect or not. becouse typescript can't access the implemetantion of those functions and variable to check it.

this also means that typescript will not thow an error when we use these declear type defination or provide type-checking or autocompletion. becouse as i already said typescript has no idea about the implimentation of these variable or function.

> when it will be useful?

- when you are using a third party library written in JavaScript (not TypeScript) and when you use that library in your own Typescript file. typescript will thow error for library code becouse it doesn't have type definations. you can declear type defination for the library. so you don't face typescript error.

```

declare let name: string;
declare function myLibraryFunction(arg: number): string;

```

## declear global

you can use declear global to provide declear defination globally.

globally here means to provide there declear defination in the outer Most scope that Typescript can access.

this is similar to window which is top most object in javascript environment but declear global here is specifically about typescipt outer most object.

in ts.config file in lib = [] is also used to tell the typescipt which files are allowed by the typescript to access. we place all these file in the scope of typescipt. similarly we are placing declear defiantion into global context.

> how to use it

```

declear global {
function myFun (params:string):boolean;
var myFun : () => boolean
var name:string
}

```

Note: inside this object we don't need to type declear on every declear defination type

Note: in placing variable in global scope we need to use var. (No let or const will work becouse they are scoped variable)

things to remember that in global space if you define same var or function twice it will overwrite the previous one becouse in global there can only be one unique variable that can exist.

Remember that using global variables should be done with caution, as they can lead to potential issues with maintainability and potential conflicts with other parts of your codebase. Whenever possible, it's a good practice to use modules and avoid polluting the global scope with variables.

## d.ts file

what every type defination you use add in a file that ends with ABC.d.ts that will automatically take all of the type defination you have defined in that file and add them in declare global object (lib.d.ts file) of the typescript.

```

// yoyo.d.ts

interface honey{
sweetness:number
}

---

// lib.d.ts // this file represent the declare global object

interface honey{
sweetness:number
}

```

---

decleartion mergin: when there are two interface with the same name there key:value merge into one.

so in context of global decleartion we already have a honey interface which contain many properties on it. and we declear a new interface with the same name with new properties. then they will merge on global level.

```

// lib.d.ts // this file represent the declare global object

interface honey{
quality:number;
brand:string;
location:string
}

---

// yoyo.d.ts

interface honey{
sweetness:number
}

---

## or

declare global{
interface honey{
sweetness:number
}
}

---

// lib.d.ts // updated

interface honey{
quality:number;
brand:string;
location:string;
sweetness:number;
}

```

---

## namespace

namespace provide Scope to types.

namespace is like a folder which is used to group together mulitple types which are related to eatch other. it provide a scope which help us with name colision. meaning if same named type exisit outside the scope of namespace then it will not colied with the nameed type inside namespace eventhough they both use the same name.

alot of library use namespace structure becouse when you download @type-version of that library from the npm. after installation that library's types get placed in the lib.d.ts(global) of typescript. so if other library use the same name for there types it might create colision and break the type system so they use namespace for grouping and scoping there types so they don't colied with other libraries types.

```
declare global {
  namespace NodeJS {
    interface ProcessEnv {
      MY_SOLUTION_ENV_VAR: string;
    }
  }
}
```

### impove your understanding of keyof and object indexing

```
declare global {
  interface DispatchableEvent {
    LOG_IN: {
      username: string;
      password: string;
    };
    LOG_IN_2: {
      username2: string;
      password2: string;
    };
  }

  type UnionOfDispatchableEvents = {
    [K in keyof DispatchableEvent]: {
      type: K;
    } & DispatchableEvent[K];
  }[keyof DispatchableEvent];
}
----------------------------------------------------
// let's try to understand how UnionOfDispatchableEvents works

//Pase:1
 type UnionOfDispatchableEvents = {
    [K in keyof DispatchableEvent]: {
      type: K;
    } & DispatchableEvent[K];
  }
---------------------------------------------
//return
type UnionOfDispatchableEvents = {
    LOG_IN: {
        type: "LOG_IN";
    } & {
        username: string;
        password: string;
    };
    LOG_IN_2: {
        type: "LOG_IN_2";
    } & {
        username2: string;
        password2: string;
    };
}
-------------------------------------
// Pase:2
type UnionOfDispatchableEvents[keyof DispatchableEvent];
-------------------------------------
// return (keyof return union of all the keys of the interface object)
type UnionOfDispatchableEvents = (
--------------------------------------------------------
// for key LOG_IN
    {
    type: "LOG_IN";
} & {
    username: string;
    password: string;
})
------------------------------------------------------
| Union
------------------------------------------------------
// for key LOG_IN_2
 ({
    type: "LOG_IN_2";
} & {
    username2: string;
    password2: string;
})
--------------------------------------------------------
final result
type UnionOfDispatchableEvents = ({
    type: "LOG_IN";
} & {
    username: string;
    password: string;
}) | ({
    type: "LOG_IN_2";
} & {
    username2: string;
    password2: string;
})
```

### assetion function

Note: Assertion Functions don't work if you declear Assertion Functions with Arrow Functions.

### overwrite library types with Declaration Merging

if you are using some external library and that library comes with it's own types. but you want to overwrite some of the types. or library doesn't come with type and you want to provide it.

Declaration merging allows you to extend or modify types from external libraries without modifying the original library code.

> steps to achive this:

1. create a new file with d.ts extension it doesn't matter the name of the file what matters is it's has the right extension. This extension indicates that it is a declaration file.

```
// regular file where you are using the library
// our library is coming from  "fake-animation-lib" Module path
import { getAnimatingState } from "fake-animation-lib";

const animatingState = getAnimatingState(); // type string
----------------------------------------------------------------------
// mydeclaration.d.ts (new file)
// declare module Path {}

declare module "fake-animation-lib"{
export function animatingState (): number
}
// after adding mydeclaration.d.ts "fake-animation-lib" type has been overwriten globally
---------------------------------------------------------------------
// now in regular file
import { getAnimatingState } from "fake-animation-lib";

const animatingState = getAnimatingState(); // type number
```

namespace old way of doing declaration merging : where declaration merging happen on the scope of namespace level

Modules are new way of doing declaration merging: where declaration merging happen on the scope of file path level

Remember that this approach only works for types. It does not actually modify the implementation or behavior of the external library. You are only modifying how TypeScript interprets and enforces the types from that library.

## identity function (by the use of generic function we mange to achive implicit inerence)

identity function means a function that take an argunment and return the same argunment.

it's sound stupid why anyone do that? why we pass the same value to a function and return the same value. what's the benifit there?

in typescript we use generic function as identity functions which allow us to take advantage of inference mechanism of generic functions. which is really helpful when making dynamic types(auto refers).

Basic idea here is that we create a generic function which take <T> (used to infer) and later we pass invoke that function by passing some data that we want to auto infer. then we write some logic with help us to infer the type of data provided as funtion argunment and then we return the function argunment as function return values. asign a variable constant to the function and it will inherit the type from the return value of our generic function.

1. fn take any[] and return const/litral typeed version of []

```
// in generics we can use "const T" which is same as saying "as const"
const asConst = <const T>(t:T)=> t

const fruits = asConst([
  {
    name: "apple",
    price: 1,
  },
  {
    name: "banana",
    price: 2,
  },
]);

type fruits = readonly [{
    readonly name: "apple";
    readonly price: 1;
}, {
    readonly name: "banana";
    readonly price: 2;
}]

```

2. we have an object (configObj) what we want is that we want to infer the fetchers object key to be litral type of one of the routes array values.

```
configObj = {
  routes: ["/", "/about", "/contact"],
  fetchers: {
    // @ts-expect-error
    "/does-not-exist": () => {
      return {};
    },
  },
}

```

> without identity function (explicit inference)

here we have to do explicit inference we have to provide that infer value is one of these
("/" | "/about" | "/contact") which means any time we add a new value in our route we need to pass the updated routes value as inference manually to let this work.

```
interface ConfigObj<TRoute extends string> {
  routes: TRoute[];
  fetchers: {
    [K in TRoute]?: () => any;
  };
}

export const configObj: ConfigObj<"/" | "/about" | "/contact"> = {
  routes: ["/", "/about", "/contact"],
  fetchers: {
    // @ts-expect-error
    "/does-not-exist": () => {
      return {};
    },
  },
};
```

> with indentity function auto inference (implicit inference)

we are passing the configObj to the makeConfigObj as params. only generic function in typescipt has the ability to use inference.

so in makeConfigObj we say <TFn extends string> meaning in this function where ever we use TFn generic it can only be type of string. then we use it in props. Note: TFn generic type is dependent on the configObj. and when we pass configObj to makeConfigObj it again passed to objType interface.

in interface we have passed the TFn generic which is extended to string already but here we have to again extends to string. becouse TObj does not no that it can only be string.

now when objType read the real routes

```
Tobj[] === ["/", "/about", "/contact"]
which infer to TObj can be any of there ("/" | "/about" | "/contact") becouse all of there are type of string.
```

this is how fethcers manage to work.

now we have figurout the type of function params: ("/" | "/about" | "/contact"). which will be also infered to function makeConfigObj type and later inherited to configObj.

```
interface objType<TObj extends string> {
  routes: TObj[];
  fetchers: {
    [K in TObj]?: () => any;
  };
}


const makeConfigObj = <TFn extends string>(config: objType<TFn>) =>
  config;

export const configObj = makeConfigObj({
  routes: ["/", "/about", "/contact"],
  fetchers: {
    // @ts-expect-error
    "/does-not-exist": () => {
      return {};
    },
  },
});
```

Note: idenitity function gets removed at the time of compiler by minifires.

## Builder pattern // lession 39

Builder pattern is used when you want to your generic type to add new types as you add new data: type(prev data) + new types(new data).

```
export class BuilderTuple<TList extends any[] = []> {
  list: TList;

  constructor() {
    this.list = [] as any;
  }
// we need to return this which allow us to build .push(1).push(2).push(3) which means when we push(1)
// it return the updated this which is class BuilderTuple and we can chain it again as much we like

// we return BuilderTuple<[...TList, TNum]> which means as we push new number to the list it will
// generate new generic by updating it as we push new data. which is like this [...TList] already
// pushed data + [TNum] newly pushed data on the generic level.

  push<TNum extends number>(num: TNum): BuilderTuple<[...TList, TNum]> {
    this.list.push(num);
    return this as any
  }
}

const builderBeforePush = new BuilderTuple();
const listBeforePush = builderBeforePush.list; // []

const builderAfterPush = builderBeforePush.push(1).push(2).push(3);
const listAfterPush = builderAfterPush.list; // [1,2,3]
```
