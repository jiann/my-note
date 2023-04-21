# TypeScripts

Tags: TypeScripts

## Types

### Common types

1. **Basic types**：boolean、number、string、null、undefined、symbol。
2. **Object types**：object、array、tuple、function。
3. **Other types**：any、void、never、unknown、enum。

### Composite types

1. **Intersection types** : Combine multiple types
2. **Union types** : Represent one of multiple types
3. **Type aliases** : Simplify complex type definitions
4. **Interfaces** : Describe object structures.

## Setup

```bash
npm install typescript
tsc --init

# compile
tsc # or tsc file_name.ts

# in tsconfig.json
- change target option to "ES2015"
- modify outDir # location of compiled file
```

## Create a basic package JSON

```bash
pnpm init
# Add dependencies; Example: typescript vite vitest
pnpm add -D xxx xxx xxx
# Initialize TypeScript
pnpm tsc --init
```

- Vitest : a unit test tool

## Addition type of TypeScript (compare with JavaScript)

- **enum**
- **void**
- **never**: Never return normally, function always throws an exception or enters an infinite loop
- **tuple**
- **any**: Allow you to reassign different types of values, can be useful when you're expecting a value from a third-party library or user inputs where the value is dynamic
    
    ```jsx
    let a1: any = 1;
    let b1: number = a1;
    console.log(b1)
    ```
    
- **unknown**: Unable to assign by other type
    
    ```jsx
    let a2: unknown = 1;
    // need to check variable type to avoid unexpected errors
    if (typeof a2 === 'number') {
      let b2: number = a2;
      console.log(b2)
    }
    ```
    
- **readonly**
- **Partial, Required, Pick, Record**

## Declare

- Function
    
    ```jsx
    export function Counter(_localStorage: Storage, date: Date) {
    	return {}
    }
    // export : Function can be used by other module
    // _ : Avoid misusing global variables
    // args: (var_name: var_type)
    function add(x: number | string, y: number | string) {
        if (typeof x === 'number' && typeof y === 'number') {
            return x + y;
        }
        if (typeof x === 'string' && typeof y === 'string') {
            return x.concat(y);
        }
        throw new Error('Parameters must be numbers or strings');
    }
    ```
    
    ```tsx
    interface Calculator {
        (x: number, y: number): number;
    }
    // OR: type calculator = (x: number, y: number) => number;
    
    let addNumbers: calculator = (x: number, y: number): number => x + y;
    let subtractNumbers: calculator = (x: number, y: number): number => x - y;
    
    let doCalculation = (operation: 'add' | 'subtract'): calculator => {
        if (operation === 'add') {
            return addNumbers;
        } else {
            return subtractNumbers;
        }
    }
    console.log(doCalculation('add')(1, 2))
    ```
    
- Array
    
    ```tsx
    let list: number[] = [1, 2, 3];
    let list: Array<number> = [1, 2, 3];  // there are no difference
    let list: Array<number> = new Array(10);
    ```
    
- Tuple
    
    ```tsx
    let person1: [string, number] = ['Marcia', 35];
    ```
    
- Optional parameters
    
    ```tsx
    function addNumbers (x: number, y?: number): number {
    	// skip
    ```
    
- Rest Parameters
    
    ```tsx
    let addAllNumbers = (n1: number, ...restOfN: number[]): number => {
    	// skip
    
    addAllNumbers(1, 2, 3, 4, 5, 6, 7);
    addAllNumbers(2);
    ```
    
- Deconstructed object parameters
    
    ```jsx
    interface Message {
       text: string;
       sender: string;
    }
    
    function displayMessage({text, sender}: Message) {
        console.log(`Message from ${sender}: ${text}`);
    }
    
    displayMessage({sender: 'Christopher', text: 'hello, world'});
    ```
    

## Interface

```jsx
interface IceCream {
   flavor: string;
   scoops: number;
}

function tooManyScoops(dessert: IceCream) {
  console.log(dessert.flavor);
  console.log(dessert.scoops);
}

tooManyScoops({flavor: 'vanilla', scoops: 5});
```

```jsx
interface Employee {
  employeeID: number;
  age: number;
}
interface Manager {
  stockPlan: boolean;
}
type ManagementEmployee = Employee & Manager; 
let newManager: ManagementEmployee = {
    employeeID: 12345,
    age: 34,
    stockPlan: true
};
```

- ****Intersection**** can combine interface

```jsx
interface myInterface {
  str: string[]
}

let myIceCream1: myInterface = {
  str: ['chocolate', 'vanilla', 'strawberry']
};
console.log(myIceCream1.str[0])
```

```jsx
interface IceCreamArray {
    [index: number]: string;
}
let myIceCream2: IceCreamArray;
myIceCream2 = ['chocolate', 'vanilla', 'strawberry'];
console.log(myIceCream2[0]) // can be callled with index
```

- Interface can be Extends by Interface (use extends)

## Literal type

```jsx
let favoriteColor: 'red' | 'blue' | 'green' = 'blue';

// or
type testResult = "pass" | "fail" | "incomplete";
let myResult: testResult;
myResult = "incomplete";
myResult = "pass";

type dice = 1 | 2 | 3 | 4 | 5 | 6;
let diceRoll: dice;
diceRoll = 1;
diceRoll = 2
```

## Anonymous function

```tsx
const myFunc = function(x: number, y: number): number {
  return x + y;
}
```

## Arrow function

```tsx
// Define Structure
type compareFunctionType = (a:number, b:number) => number;

// Function with structure type
let sortDescending: compareFunctionType = (a,b) => {
    if (a > b) { return -1;} 
    else if (b > a) { return 1; } 
    else { return 0; }
}
let sortAscending: compareFunctionType = (a,b) => {
    if (a > b) { return 1;} 
    else if (b > a) { return -1; } 
    else { return 0; }
}

function buildArray(length: number, sortOrder: 'ascending' | 'descending'): number[] {
    let randomNumbers: number[] = [];
    let nextNumber: number;
    for (let i = 0; i < length; i++) {
        nextNumber = Math.ceil(Math.random() * (100 - 1));
        if (randomNumbers.indexOf(nextNumber) === -1) {
          randomNumbers.push(nextNumber);
        } else {
          i--;
        }
    }
    if (sortOrder === 'ascending') {
      return randomNumbers.sort(sortAscending);
    } else {
      return randomNumbers.sort(sortDescending);
    }
}

let myArray1 = buildArray(12, 'ascending');
let myArray2 = buildArray(8, 'descending');
```

## Difference between Interface and Type

```tsx
// ----- Object ----- //
interface PointI {
  x: number;
  y: number;
}

type PointT = { // use '='
  x: number;
  y: number;
};

// ----- Function ----- //
interface SetPointI {
  (x: number, y: number): void;
}

type SetPointT = (x: number, y: number) => void;

// ----- Extends ----- //
interface ExtendI extends PointI { // use 'extends'
  z: number; 
}

type ExtendT = PointT & { z: number  }; // use '&'
// (interface can extends with type, type can extends with interface)
```

### Interface can

```tsx
// ----- Combine ----- //
interface Point {
  x: number;
}

interface Point {
  y: number;
}

const point: Point = { x: 10, y: 30 };
```

### Type can

```tsx
// ----- get Type ----- //
type B = typeof PointT

// ----- Others ----- //
type StringOrNumber = string | number;  
type Text = string | { text: string };  
type NameLookup = Dictionary<string, Person>;  
type Callback<T> = (data: T) => void;  
type Pair<T> = [T, T];  
type Coordinates = Pair<number>;  
type Tree<T> = T | { left: Tree<T>, right: Tree<T> };
```

## Class

```tsx
class MyClass {
	// TODO Define the properties
	// TODO Define the constructor
	// TODO Define the accessors
	// TODO Define the methods
	private _items: number;
	

	constructor(items:number) {
	  this._items = items;
	}

	get items() {
		return this._items;
	}
	set items(items) {
		this._items = items;
	}

	private multiple = (x: number) => {
		return this.items * x;
	}
	buildRange(): number[] {
		let arr: number[] = [];
		for (let i = 0; i < this.items; i++) {
			arr.push(i);
		}
		return arr;
	}

}
let testArray1 = new MyClass(12);
console.log(testArray1.buildRange());
```

## Access modifier

- **Public :** Default
- **Private :** Can be only access in class which define it
- **Protect :** Same as Private but can also access in subclass

## G****enerics****

```tsx
// ----- To limit types ----- //
type ValidTypes = string | number;

function identity<T extends ValidTypes, U> (value: T, message: U) : T {
    let result: T = value + value;
    console.log(message);
    return result
}

let returnNumber = identity<number, string>(100, 'Hello!');      // OK

// ----- Using keyof ----- //
function getPets<T, K extends keyof T>(pet: T, key: K) {
  return pet[key];
}

let pets1 = { cats: 4, dogs: 3, parrots: 1, fish: 6 };

console.log(getPets(pets1, "fish"));  // Returns 6

// ----- Interface ----- //
interface Identity<T, U> {
    value: T;
    message: U;
}
let returnNumber: Identity<number, string> = {
    value: 25,
    message: 'Hello!'
}
let returnString: Identity<string, number> = {
    value: 'Hello!',
    message: 25
}
```

- Generic interface also can be use as a **function type**, **class type**

```tsx
// ----- Generic class ----- //
class processIdentity<T, U> {
    private _value: T;
    private _message: U;
    constructor(value: T, message: U) {
        this._value = value;
        this._message = message;
    }
    getIdentity() : T {
        console.log(this._message);
        return this._value
    }
}
let processor = new processIdentity<number, string>(100, 'Hello');
processor.getIdentity(); // output: 'Hello'
```

## Import

- Namespace : `/// <reference path="xxx.ts" />`
- Import Module: `import { functionName } from "./fileName";`
- Require : `import moduleName = require("./fileName");`

## API

```jsx
const fetchURL = 'https://jsonplaceholder.typicode.com/posts'

interface Post {
    userId: number;
    id: number;
    title: string;
    body: string;
}
async function fetchPosts(url: string) {
    let response = await fetch(url);
    let body = await response.json();
    return body as Post[];
}
async function showPost() {
    let posts = await fetchPosts(fetchURL);
    let post = posts[0];

    console.log('Post #' + post.id)
    console.log('Author: ' + (post.userId === 1 ? "Administrator" : post.userId.toString()))
    console.log('Title: ' + post.title)
    console.log('Body: ' + post.body)
}

showPost();
```

- **async, await**: Await ****will wait return object pending finish and have to use async to call the function