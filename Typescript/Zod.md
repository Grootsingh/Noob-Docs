### Zod Docs

# basic usecase

> first you define the type-Defination that you are expecting the data to be just like typescript.

```
import { z } from "zod";

// creating a schema for strings
const mySchema = z.string();
---------------------------------
type mySchema = string // this is what it means
```

here z is the parent object and string is subset which extends to z.

> second you check that data asigned to this type is actually string type or not.

- parse will check and throw error if data is of incurrecct type
- safe parse will check and give you an object which contain the success , data or error if fails.

```
// parsing - throws an error
mySchema.parse("tuna"); // => "tuna"
mySchema.parse(12); // => throws ZodError

// "safe" parsing (doesn't throw error if validation fails) - give you an object
mySchema.safeParse("tuna"); // => { success: true; data: "tuna" }
mySchema.safeParse(12); // => { success: false; error: ZodError }
```

# create an object

```
import { z } from "zod";

const User = z.object({
  username: z.string(),
});

User.parse({ username: "Ludwig" });
-----------------------------------------
// extract the inferred type
type User = z.infer<typeof User>;
// { username: string }
------------------------------------------
// type user infers

type user = {
    username: string
}
```

you can use z.infer<typeof ABC> to create a type for typescript so that you don't have to write type declation twice. one for Zod and one for typescript.

# primitve data type defination in zod

```
import { z } from "zod";

// primitive values
z.string();
z.number();
z.bigint();
z.boolean();
z.date();
z.symbol();

// empty types
z.undefined();
z.null();
z.void(); // accepts undefined

// catch-all types
// allows any value
z.any();
z.unknown();

// never type
// allows no values
z.never();

```

# Coercion for primitives (Convertion)

Zod also provide a way to guarantee that a data type is a particuler data type. if the user provide different data type then what it is to be expected. then it will convert that data to the expected data format so you can safely use that data.

```
const schema = z.coerce.string();
schema.parse("tuna"); // => "tuna"
schema.parse(12); // => "12"
schema.parse(true); // => "true"
```

under the hood Zod use javascript String(),Object() and other function which convert the data to there expected data format.

Zod also provide additional string validation methods so you can be sure string is of currect type

```
z.coerce.string().email().min(5);
```

Note: you can chain methods

All primitive types support coercion.

```
z.coerce.string(); // String(input)
z.coerce.number(); // Number(input)
z.coerce.boolean(); // Boolean(input)
z.coerce.bigint(); // BigInt(input)
z.coerce.date(); // new Date(input)
```

Boolean coercion

Zod's boolean coercion is very simple! It passes the value into the Boolean(value) function, that's it. Any truthy value will resolve to true, any falsy value will resolve to false.

```
z.coerce.boolean().parse("tuna"); // => true
z.coerce.boolean().parse("true"); // => true
z.coerce.boolean().parse("false"); // => true
z.coerce.boolean().parse(1); // => true
z.coerce.boolean().parse([]); // => true

z.coerce.boolean().parse(0); // => false
z.coerce.boolean().parse(undefined); // => false
z.coerce.boolean().parse(null); // => false
```

# Literals

Literal schemas represent a literal type, just like typescript.

```
const tuna = z.literal("tuna");
const twelve = z.literal(12);
const twobig = z.literal(2n); // bigint literal
const tru = z.literal(true);

const terrificSymbol = Symbol("terrific");
const terrific = z.literal(terrificSymbol);

// retrieve literal value
tuna.value; // "tuna"
```

### String

# String specific-Validation methods

```
// validations
z.string().max(5);
z.string().min(5);
z.string().length(5);
z.string().email();
z.string().url();
z.string().emoji();
z.string().uuid();
z.string().cuid();
z.string().cuid2();
z.string().ulid();
z.string().regex(regex);
z.string().includes(string);
z.string().startsWith(string);
z.string().endsWith(string);
z.string().datetime(); // defaults to UTC, see below for options
z.string().ip(); // defaults to IPv4 and IPv6, see below for options

// transformations
z.string().trim(); // trim whitespace
z.string().toLowerCase(); // toLowerCase
z.string().toUpperCase(); // toUpperCase
```

# Custom Error

you can provide custom Error when declearing the data type so that when error occers you can be sure what's wrong here.

You can customize some common error messages when creating a string schema.

```
const name = z.string({
  required_error: "Name is required",   // when some validation is expect
  invalid_type_error: "Name must be a string", // when data is of different type
});
```

When using validation methods, you can pass in an additional argument to provide a custom error message.

```
z.string().min(5, { message: "Must be 5 or more characters long" });
z.string().max(5, { message: "Must be 5 or fewer characters long" });
z.string().length(5, { message: "Must be exactly 5 characters long" });
z.string().email({ message: "Invalid email address" });
z.string().url({ message: "Invalid url" });
z.string().emoji({ message: "Contains non-emoji characters" });
z.string().uuid({ message: "Invalid UUID" });
z.string().includes("tuna", { message: "Must include tuna" });
z.string().startsWith("https://", { message: "Must provide secure URL" });
z.string().endsWith(".com", { message: "Only .com domains allowed" });
z.string().datetime({ message: "Invalid datetime string! Must be UTC." });
z.string().ip({ message: "Invalid IP address" });
```

## you can also check DATE validation and IP address validation.. (Skiped)

### Number

You can also provide custom Error for numbers too

```
const age = z.number({
  required_error: "Age is required",
  invalid_type_error: "Age must be a number",
});
```

# Number-specific validations.

```
z.number().gt(5);  // greater then
z.number().gte(5); // greater then and equal too  // alias .min(5)
z.number().lt(5);  // less then
z.number().lte(5); // less then and qual too // alias .max(5)

z.number().int(); // value must be an integer // No float are allowed (Ex:1.23 ❌)

z.number().positive(); //     > 0
z.number().nonnegative(); //  >= 0
z.number().negative(); //     < 0
z.number().nonpositive(); //  <= 0

z.number().multipleOf(5); // Evenly divisible by 5. Alias .step(5)

z.number().finite(); // value must be finite, not Infinity or -Infinity
z.number().safe(); // Range // I Don't know // value must be between Number.MIN_SAFE_INTEGER and Number.MAX_SAFE_INTEGER
```

Optionally, you can pass in a second argument to provide a custom error message.

```
z.number().lte(5, { message: "this👏is👏too👏big" });
```

# NaNs

You can customize certain error messages when creating a nan schema.

```
const isNaN = z.nan({
  required_error: "isNaN is required",
  invalid_type_error: "isNaN must be not a number",
});
```

# Booleans

You can customize certain error messages when creating a boolean schema.

```
const isActive = z.boolean({
  required_error: "isActive is required",
  invalid_type_error: "isActive must be a boolean",
});
```

# Dates

Use z.date() to validate Date instances.

```
z.date().safeParse(new Date()); // success: true
z.date().safeParse("2022-01-12T00:00:00.000Z"); // success: false
```

You can customize certain error messages when creating a date schema.

```
const myDateSchema = z.date({
  required_error: "Please select a date and time",
  invalid_type_error: "That's not a date!",
});
```

Zod provides a handful of date-specific validations.

```
z.date().min(new Date("1900-01-01"), { message: "Too old" });
z.date().max(new Date(), { message: "Too young!" });
```

# Data Coerece

Since zod 3.20, use z.coerce.date() to pass the input through new Date(input).

```
const dateSchema = z.coerce.date();
type DateSchema = z.infer<typeof dateSchema>;
// type DateSchema = Date

/* valid dates */
console.log(dateSchema.safeParse("2023-01-10T00:00:00.000Z").success); // true
console.log(dateSchema.safeParse("2023-01-10").success); // true
console.log(dateSchema.safeParse("1/10/23").success); // true
console.log(dateSchema.safeParse(new Date("1/10/23")).success); // true

/* invalid dates */
console.log(dateSchema.safeParse("2023-13-10").success); // false
console.log(dateSchema.safeParse("0000-00-00").success); // false
```

### Zod enums [Not recommended to use enums use union types]

there are two ways you can define enums in Zod. and Zod enum are converted into union type not typescript Enums which is great.

1. directly define array of string inside z.enum()

```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);
type FishEnum = z.infer<typeof FishEnum>;
// 'Salmon' | 'Tuna' | 'Trout' // Union type
```

2. use Tuple with as const becouse Zod is not smart enough to infer extect (litral type) from array of string.

```
// The currect way ✅
const VALUES = ["Salmon", "Tuna", "Trout"] as const;
const FishEnum = z.enum(VALUES);

//The Wrong way ❌: since Zod isn't able to infer the exact values of each element.

const fish = ["Salmon", "Tuna", "Trout"];
const FishEnum = z.enum(fish);
```

> Additonal features with Zod enums

1. Autocompletion

To get autocompletion with a Zod enum, use the .enum property of your schema:

```
const fish = ["Salmon", "Tuna", "Trout"];
const FishEnum = z.enum(fish);

FishEnum.enum.Salmon; // => autocompletes

// from Zod not typescript
FishEnum.enum;
/*
=> {
  Salmon: "Salmon",
  Tuna: "Tuna",
  Trout: "Trout",
}
*/
```

2. You can also retrieve the list of options as a tuple with the .options property:

```
FishEnum.options; // ["Salmon", "Tuna", "Trout"];
```

# Native enums [Not recommended to use enums use union types]

if you have declared enums in your typescript and want to use them as the Zod enums then you can use z.nativeEnum(). to do that. it is not recommended .

1. Numeric enums

```
enum Fruits {
  Apple,
  Banana,
}

const FruitEnum = z.nativeEnum(Fruits);
type FruitEnum = z.infer<typeof FruitEnum>; // Fruits

FruitEnum.parse(Fruits.Apple); // passes
FruitEnum.parse(Fruits.Banana); // passes
FruitEnum.parse(0); // passes
FruitEnum.parse(1); // passes
FruitEnum.parse(3); // fails
```

2. String enums

```
enum Fruits {
  Apple = "apple",
  Banana = "banana",
  Cantaloupe, // you can mix numerical and string enums
}

const FruitEnum = z.nativeEnum(Fruits);
type FruitEnum = z.infer<typeof FruitEnum>; // Fruits

FruitEnum.parse(Fruits.Apple); // passes
FruitEnum.parse(Fruits.Cantaloupe); // passes
FruitEnum.parse("apple"); // passes
FruitEnum.parse("banana"); // passes
FruitEnum.parse(0); // passes
FruitEnum.parse("Cantaloupe"); // fails
```

3. Const enums

The .nativeEnum() function works for as const objects as well.

```
const Fruits = {
  Apple: "apple",
  Banana: "banana",
  Cantaloupe: 3,
} as const;

const FruitEnum = z.nativeEnum(Fruits);
type FruitEnum = z.infer<typeof FruitEnum>; // "apple" | "banana" | 3

FruitEnum.parse("apple"); // passes
FruitEnum.parse("banana"); // passes
FruitEnum.parse(3); // passes
FruitEnum.parse("Cantaloupe"); // fails
```

You can access the underlying object with the .enum property:

```
FruitEnum.enum.Apple; // "apple"
```

### Optionals (data type | undefined)

You can make any schema optional with z.optional(). This wraps the schema in a ZodOptional instance and returns the result.

Optional is same as typescript optional params which will convert data into a union type with undefined.

```
const schema = z.optional(z.string()); // string | undefined

schema.parse(undefined); // => returns undefined
type A = z.infer<typeof schema>; // string | undefined
```

For convenience, you can also call the .optional() method on an existing schema.

```
const user = z.object({
  username: z.string().optional(),
});
type C = z.infer<typeof user>; // { username?: string | undefined };
```

> you can also make an optional type into normal type useing .unwrap()

You can extract the wrapped schema from a ZodOptional instance with .unwrap().

```
const stringSchema = z.string();
const optionalString = stringSchema.optional();
optionalString.unwrap() === stringSchema; // true
```

Note: .unwrap() method is not specific to options it can be used with any z0d method that wrap the data and you can use it to unwrap it to make it normal.

### Nullables (data type | null)

Nullables convert data into a union type with null.

Similarly, you can create nullable types with z.nullable().

```
const nullableString = z.nullable(z.string());
nullableString.parse("asdf"); // => "asdf"
nullableString.parse(null); // => null
```

Or use the .nullable() method.

```
const E = z.string().nullable(); // equivalent to nullableString
type E = z.infer<typeof E>; // string | null
```

Extract the inner schema with .unwrap().

```
const stringSchema = z.string();
const nullableString = stringSchema.nullable();
nullableString.unwrap() === stringSchema; // true
```

### Object

```
// all properties are required by default
const Dog = z.object({
  name: z.string(),
  age: z.number(),
});

// extract the inferred type like this
type Dog = z.infer<typeof Dog>;
-----------------------------------------
// equivalent to:
type Dog = {
  name: string;
  age: number;
};
```

1. .shape // typeof from typescript (object key type)

Use .shape to access the schemas (type defination) for a particular key.

```
Dog.shape.name; // => string schema
Dog.shape.age; // => number schema
```

2. .keyof // keyof from typescript

Use .keyof to create a ZodEnum schema from the keys of an object schema.

```
const keySchema = Dog.keyof();
keySchema; // ZodEnum<["name", "age"]>
-----------------------------------------------
type DogUnion = z.infer<typeof keySchema>;
type DogUnion = "name" | "age"
```

3. .extend // extends from typescript (interface A extends B)

You can add additional fields to an object schema with the .extend method. just like typescript extends.

```
const DogWithBreed = Dog.extend({
  breed: z.string(),
});
```

Note: You can use .extend to overwrite fields! Be careful with this power!

4. .merge // & from typescript (type merge = A & B)

Equivalent to A.extend(B.shape).

```
const BaseTeacher = z.object({ students: z.array(z.string()) });
const HasID = z.object({ id: z.string() });

const Teacher = BaseTeacher.merge(HasID);
type Teacher = z.infer<typeof Teacher>; // => { students: string[], id: string }
```

Note: If the two schemas share keys, the properties of B overrides the property of A.

5. .pick/.omit // just like typescript utility Pick<> & Omnit<>

all Zod object schemas have .pick and .omit methods that return a modified version.

```
// layout
const newType = oldType.Pick({keyfromObjType: True})
const newType = oldType.Omit({keyfromObjType: True})
```

here an example:

```
const Recipe = z.object({
  id: z.string(),
  name: z.string(),
  ingredients: z.array(z.string()),
});
```

To only keep certain keys, use .pick .

```
const JustTheName = Recipe.pick({ name: true });
type JustTheName = z.infer<typeof JustTheName>;
// => { name: string }
```

To remove certain keys, use .omit .

```
const NoIDRecipe = Recipe.omit({ id: true });

type NoIDRecipe = z.infer<typeof NoIDRecipe>;
// => { name: string, ingredients: string[] }
```

6. .partial // same as typescript utility partical // datatype | undefined

the .partial method makes all properties optional.

Note: just like typescript partical. zod partical unility make shallow partical of the object keys. it only applies one level deep. for deep partical use deepPartical()

there is two ways you can use .partial().

- first way is that you don't provide an keys in .partial() and it will make all the object keys partical

- second way you provide which keys you wanted to be partical by .partial({key:true})

Starting from this object:

```
const user = z.object({
  email: z.string(),
  username: z.string(),
});
// { email: string; username: string }
```

> first way

```
const partialUser = user.partial();
/*
{
    email?: string | undefined;
    username?: string | undefined
}
*/
```

> second way

```
const optionalEmail = user.partial({
  email: true,
});
/*
{
  email?: string | undefined;
  username: string
}
*/
```

7. .deepPartial // just like ts-toolbelts Partial<O, depth> // datatype | undefined

it make deep partical can go n number deep in the object.

```
const user = z.object({
  username: z.string(),
  location: z.object({
    latitude: z.number(),
    longitude: z.number(),
  }),
  strings: z.array(z.object({ value: z.string() })),
});

const deepPartialUser = user.deepPartial();

/*
{
  username?: string | undefined,
  location?: {
    latitude?: number | undefined;
    longitude?: number | undefined;
  } | undefined,
  strings?: { value?: string}[]
}
*/
```

8. .required // object.partical() => object

Contrary to the .partial method, the .required method makes all properties required.

Starting from this object:

```
const user = z.object({
  email: z.string(),
  username: z.string(),
}).partial();
// { email?: string | undefined; username?: string | undefined }
```

We can create a required version:

```
const requiredUser = user.required();
// { email: string; username: string }
```

You can also specify which properties to make required:

```
const requiredEmail = user.required({
  email: true,
});
/*
{
  email: string;
  username?: string | undefined;
}
*/
```

9. .passthrough

in typescript when we define an interface and asign it to a variabe and that vairable asigned value is a refrence of another variable then typescript only check the values that are defined on the interface and if any additional values are defined on the varibale then it let them pass thorugh and allow them to add into the variable. but in zod this does not happens.

By default Zod object schemas strip out unrecognized keys during parsing.

```
const person = z.object({
  name: z.string(),
});

person.parse({
  name: "bob dylan",
  extraKey: 61,
});
// => { name: "bob dylan" }
// extraKey has been stripped
```

Instead, if you want to pass through unknown keys, use .passthrough() .

```
person.passthrough().parse({
  name: "bob dylan",
  extraKey: 61,
});
// => { name: "bob dylan", extraKey: 61 }
```

this is more secure and better solution.

10. .strict

By default Zod object schemas strip out unrecognized keys during parsing. but it does that silently. if you want to notify that an unrecognized value has been passed and you can use .strict() method which will throw an error if this happens.

```
const person = z
  .object({
    name: z.string(),
  })
  .strict();

person.parse({
  name: "bob dylan",
  extraKey: 61,
});
// => throws ZodError
```

11. .catchall

if you want to allow certians keys to passthrough and don't thorw an error if they match a specific signature then you can use .cathall() if any unrecognize value try to pass in but doesn't match the catchall signature then it will thorw an error.

```
const person = z
  .object({
    name: z.string(),
  })
  .catchall(z.number());

person.parse({
  name: "bob dylan",
  validExtraKey: 61, // works fine
});

person.parse({
  name: "bob dylan",
  validExtraKey: false, // fails
});
// => throws ZodError
```

### Arrays

```
const stringArray = z.array(z.string());

// equivalent
const stringArray = z.string().array();
```

Be careful with the .array() method. It returns a new ZodArray instance. This means the order in which you call methods matters. For instance:

```
z.string().optional().array(); // (string | undefined)[]
z.string().array().optional(); // string[] | undefined
```

1. .element // similar to object.shape() but for array

Use .element to access the schema (typeof arrays specific vlaue) for an element of the array.

```
stringArray.element; // => string schema
```

2. .nonempty

If you want to ensure that an array contains at least one element, use .nonempty().

```
const nonEmptyStrings = z.string().array().nonempty();
// the inferred type is now
// [string, ...string[]]

nonEmptyStrings.parse([]); // throws: "Array cannot be empty"
nonEmptyStrings.parse(["Ariana Grande"]); // passes
```

3. You can optionally specify a custom error message:

```
// optional custom error message
const nonEmptyStrings = z.string().array().nonempty({
  message: "Can't be empty!",
});
```

4. additional array-specific validation

.min/.max/.length

```
z.string().array().min(5); // must contain 5 or more items
z.string().array().max(5); // must contain 5 or fewer items
z.string().array().length(5); // must contain 5 items exactly
```

Note: Unlike .nonempty() these methods do not change the inferred type.

### Tuples // same as typescript

Unlike arrays, tuples have a fixed number of elements and each element can have a different type.

```
const athleteSchema = z.tuple([
  z.string(), // name
  z.number(), // jersey number
  z.object({
    pointsScored: z.number(),
  }), // statistics
]);

type Athlete = z.infer<typeof athleteSchema>;
// type Athlete = [string, number, { pointsScored: number }]
```

> you can also use .rest() methods which work similar to ...rest in array. to define rest of the data type defination.

```
const variadicTuple = z.tuple([z.string()]).rest(z.number());
const result = variadicTuple.parse(["hello", 1, 2, 3]);
// => [string, ...number[]];
```

### Unions // same as typescript

Zod includes a built-in z.union method for composing "OR" types.

```
const stringOrNumber = z.union([z.string(), z.number()]);

stringOrNumber.parse("foo"); // passes
stringOrNumber.parse(14); // passes
```

Zod will test the input against each of the "options" in order and return the first value that validates successfully.

> For convenience, you can also use the .or method:

const stringOrNumber = z.string().or(z.number());

> Optional string validation:

To validate an optional form input, you can union the desired string validation with an empty string literal.

This example validates an input that is optional but needs to contain a valid URL:

```
const optionalUrl = z.union([z.string().url().nullish(), z.literal("")]);

console.log(optionalUrl.safeParse(undefined).success); // true
console.log(optionalUrl.safeParse(null).success); // true
console.log(optionalUrl.safeParse("").success); // true
console.log(optionalUrl.safeParse("https://zod.dev").success); // true
console.log(optionalUrl.safeParse("not a valid url").success); // false
```

Note: nullish allow both null and undefined to be a value . nullabe() is different which only allow null

### Discriminated unions // same as typescript

A discriminated union is a union of object schemas that all share a particular key.

```
// typescript
type MyUnion =
  | { status: "success"; data: string }
  | { status: "failed"; error: Error };
```

Such unions can be represented with the z.discriminatedUnion method. This enables faster evaluation, because Zod can check the discriminator key (status in the example above) to determine which schema should be used to parse the input. This makes parsing more efficient and lets Zod report friendlier errors.

With the basic union method, the input is tested against each of the provided "options", and in the case of invalidity, issues for all the "options" are shown in the zod error. On the other hand, the discriminated union allows for selecting just one of the "options", testing against it, and showing only the issues related to this "option".

```
//layout
// first argunment is discriminator key which is the common key in the options.
// your parse object key will be matched against the discriminator key and that option will be selected and we do as what the option implies.

const discriminatedUnion = z.discriminatedUnion("discriminator key",optionObj1,optionObj2)
----------------------------------------------------------------
const myUnion = z.discriminatedUnion("status", [
  z.object({ status: z.literal("success"), data: z.string() }),
  z.object({ status: z.literal("failed"), error: z.instanceof(Error) }),
]);

myUnion.parse({ status: "success", data: "yippie ki yay" });
```

### Recods

Recods method is used to validate the keys and value type defination of an object.

```
Ex: { [k: string]: number }.
```

1. z.record(valueType)

If you want to validate the values of an object against some schema but don't care about the keys type then use z.record(valueType).

```
const NumberCache = z.record(z.number());

type NumberCache = z.infer<typeof NumberCache>;
// => { [k: string]: number } // key type default to string
```

2. z.record(keyType, valueType)

If you want to validate both the keys and the values, use z.record(keyType, valueType).

```
const NoEmptyKeysSchema = z.record(z.string().min(1), z.number());
NoEmptyKeysSchema.parse({ count: 1 }); // => { 'count': 1 }
NoEmptyKeysSchema.parse({ "": 1 }); // fails
```

---

Extra : a Note on numaric key type

in typescipt you can asign Recod<number,any> means key can be number in an object. but in javascript when you create an object key can never be a number it will always be converted into string type. so it is recoomended to use key to be a string type.

---

### Maps

```
const stringNumberMap = z.map(z.string(), z.number());

type StringNumberMap = z.infer<typeof stringNumberMap>;
// type StringNumberMap = Map<string, number>
```

### Sets

```
const numberSet = z.set(z.number());
type NumberSet = z.infer<typeof numberSet>;
// type NumberSet = Set<number>
```

Set schemas can be further constrained with the following utility methods.

```
z.set(z.string()).nonempty(); // must contain at least one item
z.set(z.string()).min(5); // must contain 5 or more items
z.set(z.string()).max(5); // must contain 5 or fewer items
z.set(z.string()).size(5); // must contain 5 items exactly
```

### Intersections // same as type & [it is recommended to use A.merge(B) over Intersections]

Intersections is used to define type & form typescript which is used to combine two object types into one. same with intersection

```
const Person = z.object({
  name: z.string(),
});

const Employee = z.object({
  role: z.string(),
});

const EmployedPerson = z.intersection(Person, Employee);

// equivalent to:
const EmployedPerson = Person.and(Employee);
// equivalent to:
const EmployedPerson = Person.merge(Employee);
```

Though in many cases, it is recommended to use A.merge(B) to merge two objects. The .merge method returns a new ZodObject instance, whereas A.and(B) returns a less useful ZodIntersection instance that lacks common object methods like pick and omit.

### Recursive types (Skip...)

### Promise

```
const numberPromise = z.promise(z.number());

```

promise validation happens in two steps in zod.

1. Zod synchronously checks that the input is an instance of Promise. if it is then move to second step. if not then thow an error.

2. Zod after checking that it is an promise it check the type matches with the promise or not. by automatically adding .then method and checking the data type of the return value and match it with the z.promise(data type).

```
numberPromise.parse("tuna");
// ZodError: Non-Promise type: string

numberPromise.parse(Promise.resolve("tuna"));
// => Promise<number>
```

### Instanceof

You can use z.instanceof to check that the input is an instance of a class. This is useful to validate inputs against classes that are exported from third-party libraries.

```
class Test {
  name: string;
}

const TestSchema = z.instanceof(Test);

const blob: any = "whatever";
TestSchema.parse(new Test()); // passes
TestSchema.parse("blob"); // throws
```

### Functions

You can create a function schema with z.function(args, returnType) .

> without arg and return type

```
const myFunction = z.function();

type myFunction = z.infer<typeof myFunction>;
// => ()=>unknown
```

> Define inputs and outputs.

```
const myFunction = z
  .function()
  .args(z.string(), z.number()) // accepts an arbitrary number of arguments
  .returns(z.boolean());

type myFunction = z.infer<typeof myFunction>;
// => (arg0: string, arg1: number)=>boolean
```

Function schemas have an .implement() method which accepts a function and returns a new function that automatically validates its inputs and outputs.

```
const trimmedLength = z
  .function()
  .args(z.string()) // accepts an arbitrary number of arguments
  .returns(z.number())
  .implement((x) => {
    // TypeScript knows x is a string!
    return x.trim().length;
  });

trimmedLength("sandwich"); // => 8
trimmedLength(" asdf "); // => 4
```

If you only care about validating inputs, just don't call the .returns() method. The output type will be inferred from the implementation.

```
const myFunction = z
  .function()
  .args(z.string())
  .implement((arg) => {
    return [arg.length];
  });

myFunction; // (arg: string)=>number[]
```

Extract the input and output schemas from a function schema.

```
myFunction.parameters();
// => ZodTuple<[ZodString, ZodNumber]>

myFunction.returnType();
// => ZodBoolean
```

Note: You can use the special z.void() option if your function doesn't return anything. This will let Zod properly infer the type of void-returning functions. (Void-returning functions actually return undefined.)

### Schema methods

1. .parse

```
.parse(data: unknown): T
```

Given any Zod schema, you can call its .parse method to check data is valid. If it is, a value is returned with full type information! Otherwise, an error is thrown.

IMPORTANT: The value returned by .parse is a deep clone of the variable you passed in.

```
const stringSchema = z.string();

stringSchema.parse("fish"); // => returns "fish"
stringSchema.parse(12); // throws error
```

2. .safeParse

```
.safeParse(data:unknown): { success: true; data: T; } | { success: false; error: ZodError; }
```

If you don't want Zod to throw errors when validation fails, use .safeParse. This method returns an object containing either the successfully parsed data or a ZodError instance containing detailed information about the validation problems.

```
stringSchema.safeParse(12);
// => { success: false; error: ZodError }

stringSchema.safeParse("billie");
// => { success: true; data: 'billie' }
```

the output of safePase is an object (discriminated union) which you can easy use to check if the data is expected data or not.

```
const result = stringSchema.safeParse("billie");
if (!result.success) {
  // handle error then return
  result.error;
} else {
  // do something
  result.data;
}
```

4. .safeParseAsync Alias: .spa

An asynchronous version of safeParse.

```
await stringSchema.safeParseAsync("billie");

For convenience, this has been aliased to .spa:
await stringSchema.spa("billie");
```

Note: pase can handle promise normally without any syntex change. but it will also throw error.

5. .refine(validator fn,ErrorObject)

```
.refine(validator: (data:T)=>any, params?: RefineParams)
```

Zod lets you provide custom validation logic via refinements. (For advanced features like creating multiple issues and customizing error codes, see .superRefine.)

Zod was designed to mirror TypeScript as closely as possible. But there are many so-called "refinement types" you may wish to check for that can't be represented in TypeScript's type system. For instance: checking that a number is an integer or that a string is a valid email address.

> argunments of refine method

1. validatior function:

The first is the validation function. This function takes one input (of type T — the inferred type of the schema) and returns any. if the return type is truthy and validation will pass other wise it will throw an error.

2. Error object:

this is an object where you can define error message,path,parms.

```
{
 // override error message
  message?: string;

  // appended to error path
  path?: (string | number)[];

  // params object you can use to customize message
  // in error map
  params?: object;
};
```

```
const myString = z.string().refine((val) => val.length <= 255, {
  message: "String can't be more than 255 characters",
});
```

for advance cases second argunment can also be an function where it take one argunmentt which is the return value of the validator fn and you return an error object.

```
const longString = z.string().refine(
  (val) => val.length > 10,
  (val) => ({ message: `${val} is not more than 10 characters` })
);
```

---

> Extra : Asynchronous refinements

```
Refinements can also be async:

const userId = z.string().refine(async (id) => {
  // verify that ID exists in database
  return true;
});
```

Note: If you use async refinements, you must use the .parseAsync method to parse data! Otherwise Zod will throw an error.

> Extra : Relationship to transforms

Transforms and refinements can be interleaved:

```
z.string()
  .transform((val) => val.length)
  .refine((val) => val > 25);
```

---

Note: refine is a sugercoated version of superRefine which give more control over how you can custom validate the data. read if needed be.. we are skipping superRefine.

6. .transform

To transform data after parsing, use the transform method.

it take an function which take one argument which is the return value of previously attached validator and you can take that value and return after transforming data with any javascript method.

```
const stringToNumber = z.string().transform((val) => val.length);

stringToNumber.parse("string"); // => 6
```

> Validating during transform

The .transform method can simultaneously validate and transform the value. This is often simpler and less duplicative than chaining transform and refine.

you can simply do validation with simple javascript and if things are not the way you intended to be then you can thow error.

---

Exta: Async transforms

Transforms can also be async.

```
const IdToUser = z
  .string()
  .uuid()
  .transform(async (id) => {
    return await getUserById(id);
  });

```

Note: If your schema contains asynchronous transforms, you must use .parseAsync() or .safeParseAsync() to parse data. Otherwise Zod will throw an error.

---

---

Extra: .parseAsync

```
.parseAsync(data:unknown): Promise<T>
```

If you use asynchronous refinements or transforms you'll need to use .parseAsync.

```
const stringSchema = z.string().refine(async (val) => val.length <= 8);

await stringSchema.parseAsync("hello"); // => returns "hello"
await stringSchema.parseAsync("hello world"); // => throws error
```

---

7. .default

If the input is undefined, the default value will be returned. if the input is defined then defined value will be returned.

```
const stringWithDefault = z.string().default("tuna");

stringWithDefault.parse(undefined); // => "tuna"
```

> Optionally, you can pass a function into .default that will be re-executed whenever a default value needs to be generated:

```
const numberWithRandomDefault = z.number().default(Math.random);

numberWithRandomDefault.parse(undefined); // => 0.4413456736055323
numberWithRandomDefault.parse(undefined); // => 0.1871840107401901
numberWithRandomDefault.parse(undefined); // => 0.7223408162401552
```

8. .describe

Use .describe() to add a description property to the resulting schema.

```
const documentedString = z
  .string()
  .describe("A useful bit of text, if you know what to do with it.");
documentedString.description; // A useful bit of text…
```

9. .catch

if the data parsed failed then catch value will be returned.

Use .catch() to provide a "catch value" to be returned in the event of a parsing error.

```

const numberWithCatch = z.number().catch(42);

numberWithCatch.parse(5); // => 5
numberWithCatch.parse("tuna"); // => 42
```

> Optionally, you can pass a function into .catch that will be re-executed whenever a default value needs to be generated. A ctx object containing the caught error will be passed into this function.

```
const numberWithRandomCatch = z.number().catch((ctx) => {
  ctx.error; // the caught ZodError
  return Math.random();
});

numberWithRandomCatch.parse("sup"); // => 0.4413456736055323
numberWithRandomCatch.parse("sup"); // => 0.1871840107401901
numberWithRandomCatch.parse("sup"); // => 0.7223408162401552
```

10. .optional (data type | undefined)

A convenience method that returns an optional version of a schema.

```
const optionalString = z.string().optional(); // string | undefined

// equivalent to
z.optional(z.string());
```

11. .nullable (data type | null)

A convenience method that returns a nullable version of a schema.

```
const nullableString = z.string().nullable(); // string | null

// equivalent to
z.nullable(z.string());
```

12. .nullish (data type | undefined | null)

A convenience method that returns a "nullish" version of a schema. Nullish schemas will accept both undefined and null. Read more about the concept of "nullish" in the TypeScript 3.7 release notes.

```
const nullishString = z.string().nullish(); // string | null | undefined

// equivalent to
z.string().nullable().optional();
```

13. .array
    A convenience method that returns an array schema for the given type:

```
const stringArray = z.string().array(); // string[]

// equivalent to
z.array(z.string());
```

14. .promise

A convenience method for promise types:

```
const stringPromise = z.string().promise(); // Promise<string>

// equivalent to
z.promise(z.string());
```

15. .or

A convenience method for union types.

```
const stringOrNumber = z.string().or(z.number()); // string | number

// equivalent to
z.union([z.string(), z.number()]);
```

16. .and

A convenience method for creating intersection types.

```
const nameAndAge = z
  .object({ name: z.string() })
  .and(z.object({ age: z.number() })); // { name: string } & { age: number }

// equivalent to
z.intersection(z.object({ name: z.string() }), z.object({ age: z.number() }));
```

17. .brand // same as typescipt

```
.brand<T>() => ZodBranded<this, B>
```

you can attach a brand name and that brandnamed data can only be parse be the same variable parser. it can't be parrsed by other variable parser.

```
const Cat = z.object({ name: z.string() }).brand<"Cat">();
const simba = Cat.parse({ name: "simba" }); // works
const mogli = dog.parse({ name: "mogli" }); // this doesn't
```

18. .pipe()

you can define validation logic inside .pipe() so that you can validate the data before moving forward in the chain of the methods.

```
z.string()
  .transform((val) => val.length)
  .pipe(z.number().min(5));
```

The .pipe() method returns a ZodPipeline instance.

> You can use .pipe() to fix common issues with z.coerce.

```
// without constrained input:

const toDate = z.coerce.date();

// works intuitively
console.log(toDate.safeParse("2023-01-01").success); // true

// might not be what you want
// null is judged on truthy or falsy. even though null return a falsy.
//if value check retun a value it is a success.

console.log(toDate.safeParse(null).success); // true
-----------------------------------------------------------------
// with constrained input:

const datelike = z.union([z.number(), z.string(), z.date()]);

// here we are validating that value need to be an isntane of date other wise it is incurrect.
const datelikeToDate = datelike.pipe(z.coerce.date());

// still works intuitively
console.log(datelikeToDate.safeParse("2023-01-01").success); // true

// more likely what you want
console.log(datelikeToDate.safeParse(null).success); // false
```

### Guides and concepts

1. Type inference z.infer<typeof mySchema>.

You can extract the TypeScript type of any schema with z.infer<typeof mySchema> .

```
const A = z.string();
type A = z.infer<typeof A>; // string

const u: A = 12; // TypeError
const u: A = "asdf"; // compiles
```

2. transform type:

What about transforms?

In reality each Zod schema internally tracks two types: an input and an output. For most schemas (e.g. z.string()) these two are the same. But once you add transforms into the mix, these two values can diverge. For instance z.string().transform(val => val.length) has an input of string and an output of number.

You can separately extract the input and output types like so:

```
const stringToNumber = z.string().transform((val) => val.length);

// ⚠️ Important: z.infer returns the OUTPUT type!
type input = z.input<typeof stringToNumber>; // string
type output = z.output<typeof stringToNumber>; // number

// equivalent to z.output!
type inferred = z.infer<typeof stringToNumber>; // number
```

# Writing generic functions

ZodTypeAny is just a shorthand for ZodType<any, any, any>, a type that is broad enough to match any Zod schema.

```
function makeSchemaOptional<T extends z.ZodTypeAny>(schema: T) {
  return schema.optional();
}
```

As you can see, schema is now fully and properly typed.

```
const arg = makeSchemaOptional(z.string());
arg.unwrap(); // ZodString
```

> Constraining allowable inputs

The ZodType class has three generic parameters.

```
class ZodType<
  Output = any,
  Def extends ZodTypeDef = ZodTypeDef,
  Input = Output
> { ... }
```

By constraining these in your generic input, you can limit what schemas are allowable as inputs to your function:

```
function makeSchemaOptional<T extends z.ZodType<string>>(schema: T) {
  return schema.optional();
}

makeSchemaOptional(z.string());
// works fine

makeSchemaOptional(z.number());
// Error: 'ZodNumber' is not assignable to parameter of type 'ZodType<string, ZodType
```

# Error handling

Zod provides a subclass of Error called ZodError. ZodErrors contain an issues array containing detailed information about the validation problems.

```
const result = z
  .object({
    name: z.string(),
  })
  .safeParse({ name: 12 });

if (!result.success) {
  result.error.issues;
  /* [
      {
        "code": "invalid_type",
        "expected": "string",
        "received": "number",
        "path": [ "name" ],
        "message": "Expected string, received number"
      }
  ] */
}
```

Zod's error reporting emphasizes completeness and correctness. If you are looking to present a useful error message to the end user, you should either override Zod's error messages using an error map (described in detail in the Error Handling guide) or use a third-party library like zod-validation-error.

---

Extra: zod-validation-error library

zod give you an detailed error object which contain everything. but not easy to read or understand. it is not recommended to use the zod error message for user. becouse it's to much not easy to undertsand.

there are two solution to this.

1. either provide your custom error messages. every where you think error can ocers.
2. use zod-validation-error library which is a library which return user frendly humanly readable error message that you can use to show to the user.

how to work with zod-validation-error library.

step 1:simply install the library : npm install zod-validation-error

step 2: import { fromZodError } from 'zod-validation-error';

```
let data = z.string().min(5)

let result = data.parse("ABC")

if(!result.success){
    console.log(fromZodError(result.error))
}

```

fromZodError(Error object) return simplified error out of the error object provided by the zod.

---

> Error formatting

result.error.issues return an array of object. if you like the return error to be in nested object format then use .format()

You can use the .format() method to convert this error into a nested object.

```
const result = z
  .object({
    name: z.string(),
  })
  .safeParse({ name: 12 });

if (!result.success) {
  const formatted = result.error.format();
  /* {
    name: { _errors: [ 'Expected string, received number' ] }
  } */

  formatted.name?._errors;
  // => ["Expected string, received number"]
}
```
