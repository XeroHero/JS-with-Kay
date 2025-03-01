# Session 1: JS Syntax Fundamentals

NB: To execute these scripts, you can either set up the toolchain in your IDE of choice, or create a HTML file and call the JS file as follows:

```
<!DOCTYPE html>
<head>
	<script type="text/javascript" src="./path/to/script.js"></script>
</head>
<body>
	<p>This page just demonstrates including a basic JavaScript file. The JavaScript file just tells an alert box to pop up when the page loads.</p>
</body>
</html>
```
## Part 1: Basic Types

### Static vs Dynamic types

The quickest way to understand static types is to contrast them with dynamic types. A language with static types is referred to as a statically-typed language. On the other hand, a language with dynamic types is referred to as a dynamically-typed language.

Statically-typed languages perform type checking at compile time, while dynamically-typed languages perform type checking at runtime. In simple terms, this means that in dynamically-typed languages (such as JS and Python), we are able to just call variables `var`, whereas in statically-typed languages (such as Java and C), we need to explicitly state the type of variable we are going to be using (eg. `int`, `String`, `double`, etc.)

In statically typed languages, the *compiler* performs checks on the code, and throws an error when something isn't right. For example, if we were to try the following in Java, it wouldn't work:
`boolean result = 123;`
This is because the `boolean` data type is only able to take `true` and `false` as types (or their numerical representations, `1` and `0` respectively)

In dynamically-typed languages, the errors occur only once the program is executed. That is, at runtime.

This means that a program written in a dynamically-typed language (like JavaScript or Python) can compile even if it contains type errors that would otherwise prevent the script from running properly.

Convenient, but not always ideal. Which is why tools like Flow and TypeScript have recently stepped in to give JavaScript developers the _option_ to use static types.

[Flow](https://flowtype.org/) is an open-source static type checking library developed and released by Facebook that allows you to incrementally add types to your JavaScript code. We will start using this.

[TypeScript](https://www.typescriptlang.org/), on the other hand, is a superset that compiles down to JavaScript — although it feels almost like a new statically-typed language in its own right. That said, it looks and feels very similar to JavaScript and isn’t hard to pick up.

In either case, when you want to use types, you explicitly tell the tool about which file(s) to type-check. For TypeScript you do this by writing files with the `.ts` extension instead of `.js`. For Flow, you include a comment on top of the file with `@flow`.

## Part 2: Syntax and language

Let’s begin by examining some JavaScript primitives, as well as constructs like Arrays, Object, Functions, and etc.

NB: in Flow, the syntax for declaring static types is as follows: `var myBoolean: boolean = false;`. It always follows the patters `var` followed by the name of our variable, followed by a colon (`:`) and a space, followed by the type of the variable and optionally an assignment. Note that it is not required to terminate a statement with a semicolon (`;`).

The main types of variables in JavaScript are given below:

* *boolean*: Describes a `true` or `false` value in JavaScript
* *number*: Describes a double-precision floating-point number. There are no such thing as integers, doubles, floats, long, shorts, etc. in JavaScript.
`number` includes `Infinity` and `NaN`.
* *string*: Sequence of characters
* ⚠️ *null* and *void*: These are some special data types, we will look at them later on.
* *Array*: Arrays are "collections" of similarly-typed objects. These can be strings, numbers for example.
* *Object*: This is another special data type. Essentially, it allows us to spawn up new, custom data types, with various *attributes* inside them. For example:

```
var bird = {
  genus: "corvus",
  commonName: "raven",
  callType: "squawky",
  deadly: false
};
```
This allows us to define our own data type (above we have defined a `bird` data type, which is obviously not part of native JS).

* *any*: This is a generic type, and is effectively unchecked. Is is not recommended to use this type anywhere in code unless a specific scenario requires it.
* *Maybe*: This is another special type, which allows its contents to be nullable (`null`) or `undefined`, as well as any of the above. To define a `maybe` type, we simply add a `?` before the type, like follows:

```
function acceptsMaybeNumber(value: ?number) {
  //some code
}
```

### Objects in JavaScript

Objects allow us to model real-world scenarios with properties. For example, for a `dog` object, I could have a property called `breed` which accepts a `string` value representing the dog's breed. This is called an *attribute* or property of an object.

Going back to our bird object we created above, we can access its properties by using `bird.[propertyName]`, where the property name is one of the following:
* callType
* commonName
* deadly
* genus

If we try to do the following in our JS console in the browser, we will get the following result.

```
var bird = {
  genus: "corvus",
  commonName: "raven",
  callType: "squawky",
  deadly: false
};
```
followed by
`bird.genus` we will get back the value `corvus`. This is known as accessing the properties of the object.
Remember we do not surround the key (the thing to the left of the colon) with quotes. If we were to do that we would get a Syntax Error from JavaScript. However, this will work: `bird["genus"]`. If your keys have special characters you must use this syntax.

To "set" a property, we follow a similar syntax. Suppose I wanted to add a color to the bird. I can do that by typing in `bird.color = "black"`. The console will echo this back to indicate that it has been successfully processed.

To inspect every property of the object, simply type in the object name.

To delete a property of an object, use the `delete` keyword: `delete bird.color`

#### Objects and References

Objects are references to particular locations in the system's memory.

If we take our bird object from above, and copy it to a new object, say `bird2`, like so:

```
var bird = {
  genus: "corvus",
  commonName: "raven",
  callType: "squawky",
  deadly: false
};
var bird2 = bird;
```

If I chance the attribute `deadly` of `bird2` what happens?

`bird2.deadly = true;`.

If we inspect the `bird2` object we notice as expected that it has changed, but if we inspect the `bird` object it too has updated. This is because we have only copied a references to the object, and not the object itself. It's like if we had made another shortcut to the object. The shortcut and the actual object still point to the same thing, so it is natural that changes in one will reflect on the other.

If we want to make a copy of the object, we must execute the statement above to put in all the data into it, changing the object name.

🪄 To make a copy of an object safely, we could also use a built-in JS object called JSON to convert the object to a string of JSON as follows: `bird2 =  JSON.parse(JSON.stringify(bird))`.

This makes the two objects really separate and different, so they can be modified independently.

### Arrays

Arrays are collections of elements of the same type. We can declare them as follows:
`var myArray = [];`.

This will create an empty array called myArray.

Example: Days of the week

`var daysOfTheWeek = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]; //constructs the array of days of the week`

To retrieve the full array, we just need to type in the variable's name, in this case `daysOfTheWeek`. To get a specific day of the week from the array, we use array indexing. Indexing allows us to assign a number to every element in the array, starting from 0.
So, if I was to type in `daysOfTheWeek[2];`, I would expect to get back the value `Wednesday`.

Arrays can contain objects, or other arrays even.

```
var arrayOfStuff = [
  {'name': 'value'},
  [1,2,3], true, 'nifty'
];
```
Another interesting property of arrays is the fact that you can check their size (how many elements they contain): `arrayOfStuff.length` and that will return the value `4`.

#### Manipulating Arrays (LIFO)

Operations that can be performed on arrays:

* `push` adds an element to the end of the array
* `pop` removes the last element in the array.
* `delete` removes an item at a particular location in the array, replacing it with `empty` value.
* `splice` removes value(s) between two positions in an array. The syntax is as follows: `splice(start, numberOfElements)`.
* `length` retrieves the size of the array.

### Comparison Operators

Suppose we declare two variables as follows:
```
var one = 1, two = 2;
```
We can compare these two variables together using the following operations:
* Equal: `one === one;` => true
* Not Equal: `one !== one;` => false
* Less than: `one < two;` => true
* Greater than: `one > two;` => false
* Less than or Equal to: `one <= two;` => true
* Greater than or Equal to: `one >= two;` => false

We can also use these operators with values not in variables: eg. `10 >= two` => true

### Taking Actions Based on Conditions

We saw above how to verify conditions relating to numbers. Now suppose we wanted to take action based on these. For example, we may want to print a message telling them about the result.

This is where the `if ... else` clause comes in handy. A clause here is simply meant as a seriens of statements that usually go together to perform a desired behaviour, sort of like a rule of grammar in a language.

A very simple and quite limited example of this is veryfying the assignation of a variable. The script below shows how to do this with if-else clauses.

```
var variable = 123;

if (variable === 123){
  console.log("Variable has been assigned correctly");
  }
else {
  console.log("Variable assignment error");
  }
  
 ```
 
 If we play this small script in our console, we notice the output as `Variable has been assigned correctly`. If however, we change the value on the first line to something like `var variable = "abc";`, and re-execute our code, the output will be different. 
 
 This simple concept allows us to introduce variance into our code, and build a decision-making strategy into the code.
 
 ### Switch Statements
 
 Switch statements are another fashion of doing the same thing as above. It is often not very aesthetic to have lots of `if-else` blocks in code. A better approach is using a `switch` statement. 

Switch statements compare the value of an expression against 1 or more values and executes different sections of code based on that comparison.

Let's examine its syntax:
 
 ```
var value = 1; switch (value) {
case 1:
  console.log('I will always run'); break;
case 2:
  console.log('I will never run'); break;
case ...

}
```

As you can see, switch statements allow us to reduce the code lenght considerably. The `value` variable is compared to each of the `case` values, and when one is found to match, the corrponding code is executed.

⚠️ If we omit the `break` keyword, the code will continue to check for matches in the remainder of the switch statement and could lead to undesired behaviour. 

### Strategies

A strategy pattern can be used in JavaScript in many cases to replace a switch statement. It is especially helpful when the number of conditions is dynamic or very large. It allows the code for each condition to be independent and separately testable.

Strategy object is simple an object with multiple functions, representing each separate condition. Let's examine a simple example:

```
const AnimalSays = { 
  dog () {
    return 'woof';
  },
  cat () {
    return 'meow';
  },
  lion () {
    return 'roar';
  },
  // ... other animals
  default () { 
    return 'moo';
  }
};
```

The above object can be used as follows:

```
function makeAnimalSpeak (animal) {
  // Match the animal by type
  const speak = AnimalSays[animal] || AnimalSays.default; console.log(animal + ' says ' + speak());
}
```
and willn yield the following results:

```
makeAnimalSpeak('dog') // => 'dog says woof' 
makeAnimalSpeak('cat') // => 'cat says meow' 
makeAnimalSpeak('lion') // => 'lion says roar' 
makeAnimalSpeak('snake') // => 'snake says moo'
```

    
