> ## typescript version: 5.1

## Recap on Javascript classes

```jsx
class MyObject {
  constructor(param1, param2) {
    this.name = param1;
    this.age = param2;
  }
}
```

this.name = we need to use this becouse when we convert this function into object. this keyword will refer to that object and this.name = object.key. we can't use let,var,const becouse object don't understand there type of variable defination.

## typescript class

classes represent both the type of a class instance and a constructor function.

so classes contain it's own type defination and it's value defination. when we compile the typescript classes all the types defination are removed and only value defination remains.

Note that we specify this.name type name:string; this is due to when the object is created it refer to currect type. and in constructor we again define name:string becouse it is a function and param needed to be typed currectly so when we create a new object instance we only pass the currect typed value.

```tsx
class Fruit {
  name: string;
  color: string;
  sweetness: number;
  constructor(name: string, color: string, sweetness: number) {
    this.name = name;
    this.color = color;
    this.sweetness = sweetness;
  }
  fullName() {
    const isSweet = this.sweetness > 50;
    return `${isSweet ? "Sweet " : ""}${this.color} ${this.name}`;
  }
}
```

Note: in strick Mode: typescipt demand that you initalize all of the this.varibale otherwise it will thow an error.

```tsx
let name:string
// but you didn't defined the the value
//------------------------------------------
class Fruit {
  name: string;
  color: string;
  sweetness: number;
  constructor(name: string, color: string, sweetness: number) {
    this.name = name;
  }
}

color and sweetness // Error: value were not initalized.
```

Solution: you have three options:

- default values ( name: string = "raju")
- optional properties ( name?: string)
- the non-null assertion operator( name!: string;)

## inheritance

so class can have both type and values. just like interface can extends and gain inheritence from other interface. classes can do that too.

```tsx
class EdibleThing {
  name: string;
  color: string;
  constructor(name: string, color: string) {
    this.name = name;
    this.color = color;
  }
}

class Fruit extends EdibleThing {
  sweetness: number;
  constructor(name: string, color: string, sweetness: number) {
    super(name, color);
    this.sweetness = sweetness;
  }
}
//--------------------------------------------------------------------
in Fruit class you can access both name and color which is coming from the EdibeThing.
and when you define instance of Fruit you need to provide all the properties value defined in the constructor.
const fruit = new Fruit("raju","red",23)
//-------------------------------------------------------------------------
// some more variation of above example
// what if we want the use to only provide sweetness

class Fruit extends EdibleThing {
  sweetness: number;
  constructor(sweetness: number) {
    super("raju", "red");
    this.sweetness = sweetness;
  }
}

const fruit = new Fruit(23)
fruit.name // raju
fruit.color // red
fruit.sweetness // 23

// when you extends from parent in super you need to either provide the parameter manually or let the user type in that case just pass those to parent. but all the param must be provided you can't call the parent with partial params.

case 1: super("raju","color")
case 2:   constructor(name: string, color: string, sweetness: number) + super(name,color)

```

in classes we need to use super to bridge parent class (EdibleThing) with child class(extends) (Fruit). you can think of it like super call the EdibleThing class constructor function with the params and return the new values. and add them into the child class(extends).

super can only be used when we are using extends to inhert from the parent class

Note: 'super' must be called before accessing 'this' in the constructor of a derived class

```tsx
// parent
  constructor(name: string, color: string) {
    this.name = name;
    this.color = color;
  }
//----------------------------------------------
// child
 super(name, color)
//-----------------------------------------------
class Fruit extends EdibleThing {
  sweetness: number;
  constructor(name: string, color: string, sweetness: number) {
//-----------------------------------------------
    this.name = name;
    this.color = color;
//-----------------------------------------------
    this.sweetness = sweetness;
  }
}

```

Note: we need to call the parent using super first before definind your extended this variable.

## Static Properties

Static Properties are (not types they are values) those properties which only exist inside class definations and can not be accessed by class instance (new). these defination are use to transform some data inside the class defination.

```tsx
class Vegetable {
  static cookingTimeSeconds = 5;
  static cook(vegetable: Vegetable) {
    setTimeout(() => {
      console.log(`Cooked ${vegetable.name}`);
    }, Vegetable.cookingTimeSeconds * 1000);
  }
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}

const squash = new Vegetable("squash");
Vegetable.cook(squash); // 5 seconds later: "Cooked squash"
```

## Abstract Classes

Abstract Classes (are type defination) defined on the absetact class defination (Parent).
Abstract Class contain some types defination (which start with abstract keyword) which need to be followed by all of the child class(extract). otherwise it will throw an error.

abstract class does not exist in javascirpt so at the time of compliation javascript convert it to normal class.

```tsx
// parent
abstract class EdibleThing {
  name: string;
  abstract eat(): void;
  constructor(name: string) {
    this.name = name;
  }
}
class Fruit extends EdibleThing {
  constructor(name: string) {
    super(name);
  }
  eat() {
    console.log(`Yum. ${this.name}s are tasty.`);
  }
}
```

## Modifiers

Modifiers are keywords that we place before our property and method definitions which define how and when the fields on our class can be accessed.

- `Public`: By default, class fields are marked as public . This makes the fields accessible both inside class defination and by the class instances (new).

---

What if we were writing a class that will be used by other developers. Many of the parts of the class should be accessed by other developers, but other parts contain implementation details that we would prefer to keep to ourselves. We can use the protected and private modifiers to stop other developers from accessing certain properties and methods.

---

- `Protected`: Protected properties and methods can only be accessed by class defination and any child class(extends) that inhert the class defination. it can not be accessed by class instance (new).

  Note: Protected only work on the typescipt level once the code is compiled it will not be protected can be accessed by class instances(new).

- `Private`: Private properties and methods are only allowed on the class defination only. can not be accessed by any child class(extends) or class instance(new).

```tsx
class EdibleThing {
  private name: string; // typescript privcate modifire
  constructor(name: string) {
    this.name = name;
  }
}
```

- if you use private then it will be removed after compilation and your code will be no longer be private.

  Note: in ES6 javascript classes have it's own private modifier which have a little different syntex.

```tsx
class EdibleThing {
  #private name: string; // javascript private modifier
  constructor(name: string) {
    this.name = name;
  }
}
```

- so if you use #private it will remain private even after compilation

## Property Shorthand

TypeScript gives us a shorthand when our constructor parameters are the same as our properties. By assigning a modifier to constructor parameters, TypeScript will automatically assign them to the appropriate property without us even having to define the property on the class. This can save us a bit of typing.

```tsx
class Fruit {
  constructor(public name: string, protected sweetness: number) {}
}
const apple = new Fruit("Apple", 80);
console.log(apple); // Fruit { name: "Apple", sweetness: 80 }
//--------------------------------------------------------------------
// we don't even have to write this.name = name // infer from the instance
```

to take advatange of shorthand: we need to when defining modifier (public,protected,prived) before defining paramters. which will automatically generate type apropriate type defination. you don't even have to warry about value not defined issue. becouse typescript know when you define the class instane it's parameter is the value type

## Accessors (get and set) from javascript

In JavaScript we have getter and setter functions. they are special methods that allow you to define how properties are accessed and assigned in a class.

They are used to add reading and writing functionality in classes.

`set fn()`: will modify/transform and add data to the class defination.
`get fn()`: will be used to access the data to that class defination.

```tsx
class Fruit {
  constructor(protected storedName: string) {}
  set name(nameInput: string) {
    this.storedName = nameInput;
  }
  get name() {
    return this.storedName[0].toUpperCase() + this.storedName.slice(1);
  }
}

const apple = new Fruit("apple");
apple.name; // "Apple" // get name() get invoked
apple.name = "banana"; // set name("banana") get invoked
apple.name; // "Banana" // get name() get invoked
// if we don't define set name method
apple.name = "mango"; // Error: name value is only readable
```

when you define both get and set methods then that propertiy (Ex: name) is readabe and writeable.
but if you only define get method and that property is only readable not writable.

## implements

abstract class allow us to set Rules for child classes that all the child classes of this parent class defination should contain there types. but we can only impose there restriction on the child class of the parent.

implements means this class should follow the types defination of that class

```Tsx
interface Pingable {
  ping(): void;
}

// worked
class Sonar implements Pingable {
  ping() {
    console.log("ping!");
  }
}

// Error: Class 'Ball' incorrectly implements interface 'Pingable'.
// Property 'ping' is missing in type 'Ball' but required in type 'Pingable'.

class Ball implements Pingable {
  pong() {
    console.log("pong!");
  }
}
```
