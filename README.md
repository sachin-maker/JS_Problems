# JS_Problems

## 🚀Debouncing:-
#### Debouncing is a technique used to improve the performance of applications by reducing the rate of functions call

#### 👉 If the function is getting called continuously, We can delay the function call for some time with the help of debouncing to optimize the performance of applications

## 💡use cases :-
* #### 👉 For mostly we used Search Box
* #### 👉 Continuous button click event function call can be delay.
* #### 👉 Resize of window event function call can be delay.

## 💡Let's take an example search input value-
https://www.youtube.com/watch?v=pkvXAM3uIZ8  (watch this video for better understanding)


#### In a search bar, you can use debouncing to delay sending an API request until the user has finished typing or paused for a moment. 
#### This prevents excessive requests for each keystroke.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>

  <body>
    <h3>Debouncing Example</h3>
    <input type="text" id="search-input" placeholder="Search..." />
    <script src="debounce.js"></script>
  </body>
</html>
```

## debounce.js
```js
// Debounce function to delay the execution of the function
const debounce = (func, delay) => {
  let decouncing;
  return function () {
    const args = arguments;
    const ctx = this;
    clearTimeout(decouncing); // Clear the previous timeout
    decouncing = setTimeout(() => func.apply(ctx, args), delay); // Set the new timeout
  };
};

// Function to call the API (simulated)
const callAPI = function (term) {
  console.log("Calling API for", term);
};

// Create the debounced version of the API call
const debouncedAPI = debounce(callAPI, 500);

// Listen to input changes and trigger the debounced API call
document.getElementById("search-input").addEventListener("input", function (e) {
  debouncedAPI(e.target.value);
});

```
### 📝 Explanation:
`debounce(func, delay):`

* #### This function accepts a callback function (func) and a delay (in milliseconds).

* #### It ensures that func is only executed after the user stops performing the action (e.g., typing in the search bar) for delay milliseconds.

`callAPI(term):`

* #### This is a function that simulates calling an API based on the search term.

`Event Listener:`

* #### The input event is added to the search input field. As the user types, the debouncedAPI function is triggered, but due to debouncing, the actual callAPI function will only be called after the user stops typing for 500ms.

---  

## 🚀Throttling
### Throttling is a technique used to improve the performance of applications by limiting the rate of function calls
* #### 👉 If the function is getting called continuously, We can execute the function once in the given time interval with the help of Throttling to optimize the performance of applications
### 💡Debouncing vs Throttling-
* #### In Debouncing, the attached function will be executed only after the given time once the user stops calling the event.
* #### While in Throttling, the attached function will be executed only once in a given time interval.
* #### 💡Which technique to use in a particular scenario?
* #### For Search bar, It’s recommended to use Debouncing technique as it will give better user experience
* #### For Shooting game, Browser resizing and Scrolling feeds, It’s recommended to use Throttling technique.

```html
HTML 

<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>


  <body>
    <h3>Throttling example</h3>
    <button id="myButton">shoot enemy</button>


    <script src="throttling.js"></script>
  </body>
</html>
```
## throttling.js
```js
// Throttling function to limit the frequency of a function call
const throttle = (func, limit) => {
    let isThrottling;
    return function () {
        const args = arguments;
        const ctx = this;
        if (!isThrottling) {
            func.apply(ctx, args);  // Execute the function
            isThrottling = true;
            setTimeout(() => isThrottling = false, limit);  // Reset after the time limit
        }
    };
};

// Function to simulate shooting (e.g., in a game)
function shoot() {
    console.log("Function called");
}

// Create a throttled version of the shoot function with a limit of 500ms
const throttledShoot = throttle(shoot, 500);

// Listen for button click event and trigger throttledShoot
document.getElementById("myButton").addEventListener("click", () => {
    throttledShoot();
});


```

## 📝 Explanation:
`throttle(func, limit):`

* #### This function accepts a callback function (func) and a limit (in milliseconds).

* #### It ensures that func is executed only once every limit milliseconds, even if the event is triggered multiple times during that interval.

`shoot():`

* #### This is a function that simulates shooting in a game or any other scenario that requires repeated actions.

`Event Listener:`

* #### The throttled version of the shoot() function is triggered every time the button is clicked, but due to throttling, it can only be executed once every 500ms.

---  

## 🚀Function currying
### It is a technique in functional programming that transforms the function of multiple arguments into several functions of a single argument in sequence. 

```js
function simpleFunction(param1, param2, param3) =>

function curriedFunction(param1)(param2)(param3)
```

#### We simply wrap a function inside a function, which means we are going to return a function from another function to obtain this kind of translation. The parent function takes the first provided argument and returns the function that takes the next argument and this keeps on repeating till the number of arguments ends. Hopefully, the function that receives the last argument returns the expected result.  


### Why is currying useful in javaScript ?

* It helps us to create a higher-order function
* It reduces the chances of error in our function by dividing it into multiple smaller functions that can handle one responsibility.
* It is very useful in building modular and reusable code
* It helps us to avoid passing the same variable multiple times
* It makes the code more readable

```js
function calculateVolume(length) {
	return function (breadth) {
		return function (height) {
			return length * breadth * height;
		}
	}
}
console.log(calculateVolume(4)(5)(6));
```




## Write a currying function that takes infinite arguments.
```js
const sum = function(a) {
    // Define an inner function that will take the next argument
    const innerSum = function(b) {
      // If there is a second argument (b), continue the recursion by calling sum with the accumulated value
      if (b) {
        return sum(a + b);
      }
      // If no second argument (b) is provided, return the accumulated sum (a)
      return a;
    };
 
    // Return the inner function so that more arguments can be provided
    return innerSum;
  };
 
  // Usage Examples
  console.log(sum(1)(2)(3)());       // 6
  console.log(sum(5)(10)(-3)(2)());  // 14
```




## Interview Question 1

### So how do we write a wrapper function curry which accepts a function say func and returns the curried version of func.

### Solution🚀

* Let's pass a function func as input.
* So to get the curried version of func, first thing we should do is return a function which takes arguments. In our case we are returning curried.
* Now in curried we need to check the length of arguments that are passed to it. If all the arguments are passed we call func else we need to again return a function to get the remaining arguments and so on until all the arguments are passed.
* So we can see a recursive behavior here. So how do we achieve this?
* We call curried recursively until all the arguments are received.

```js
function curry(func) {
    function curried(...args) {
        if(args.length >= func.length) {
            return func(...args);
        } else {
            return function(...more) {
                return curried(...args,...more);
            }
        }
    }
    return curried;
}

function multiply(a, b, c) {
    return a*b*c;
}

// To get the curried version of multiply we pass it to our above curry function.
let curried = curry(multiply);
console.log(curried(2)(3)(4)); // 24
console.log(curried(2,3)(4));  // 24
console.log(curried(2,3,4));  // 24
console.log(curried(5)(6,7)); // 210
```
---  

## 🚀Flatten an array

#### Means to convert an original array into a new array. It does this by collecting and concatenating the sub-arrays in an array into a single array.

#### We have two method in js to flatten the array
`Falt()`    and `flatmap()`
```js
const arr2 = [ [ [ 1,  2 ] , 3, 4, 5] ] ;
Console.log ( arr2.flat ( 2 ) );
// expected output 
[1, 2, 3, 4, 5]
```

* #### The flatMap() method uses a combination of the map() and flat() methods to perform operations.
* #### The flatMap() loops through the elements of an array and concatenates the elements into one level. flatMap() takes in a callback function which takes in the current element of the original array as a parameter.
```js
const arr3 = [1, 2, [ 4, 5 ], 6, 7, [ 8 ] ] ;
console.log(arr3.flatMap((element) => element)); 
// expected output 
[1, 2, 4, 5, 6, 7, 8]
```
## Key Takeaways:
* #### Use flat() when you need to flatten arrays of any depth.

* #### Use flatMap() when you want to map the array and flatten it in a single operation.

  ---
  

## 🚀Write custom function for Array.flat() using both recursive and iterative

## 📌Recursive Approach (No Depth)
```js
const flattenRecursive = (arr) => {
  if (!Array.isArray(arr)) {
    throw new Error("Input must be an array");
  }
  const result = [];
  for (const ele of arr) {
    if (Array.isArray(ele)) {
      result.push(...flattenRecursive(ele)); // Recursively flatten sub-arrays
    } else {
      result.push(ele); // Add non-array elements directly
    }
  }
  return result;
};

// Test case for Recursive Approach
const resultRecursive = flattenRecursive(
  [[[[0]], [1]], [[[2], [3]]], [[4], [5]]]
);
console.log(resultRecursive, "Recursive Result");
// Expected Output: [0, 1, 2, 3, 4, 5]
```
## 📌Iterative Approach (No Depth)
```js
const flattenIterative = (arr) => {
  if (!Array.isArray(arr)) {
    throw new Error("Input must be an array");
  }
  const stack = [...arr];
  const result = [];
  while (stack.length) {
    const ele = stack.pop();
    if (Array.isArray(ele)) {
      stack.push(...ele); // Push elements of the sub-array to the stack
    } else {
      result.push(ele); // Add non-array elements directly
    }
  }
  return result.reverse(); // Reverse the result to maintain order
};

// Test case for Iterative Approach
const resultIterative = flattenIterative([[[[0]], [1]], [[[2], [3]]], [[4], [5]]]);
console.log(resultIterative, "Iterative Result");
// Expected Output: [0, 1, 2, 3, 4, 5]
```
## 📌Recursive Approach with Depth

```js
const flattenRecursiveWithDepth = (arr, depth) => {
  if (!Array.isArray(arr)) {
    throw new TypeError("The first argument must be an array.");
  }
  let result = [];
  if (depth === 0) return arr; // Return the array as is when depth is 0
  for (let ele of arr) {
    if (Array.isArray(ele) && depth > 0) {
      result.push(...flattenRecursiveWithDepth(ele, depth - 1)); // Flatten sub-arrays up to the given depth
    } else {
      result.push(ele); // Add non-array elements directly
    }
  }
  return result;
};

// Test case for Recursive Approach with Depth
const result = flattenRecursiveWithDepth(
  [[[[[0]]], [1]], [[[2], [3]]], [[4], [5]]], 1
);
console.log(result);
// Expected Output: [ [ [ 0 ] ], 1, [ [ 2 ], [ 3 ] ], 4, 5 ]
```
## 📌Iterative Approach with Depth
```js
const flattenIterativeWithDepth = (arr, depth) => {
  if (!Array.isArray(arr)) {
    throw new TypeError("The first argument must be an array.");
  }
  let result = [];
  let stack = [...arr]; // Start with the original array
  let currentDepth = 0;

  // Iterate while there are elements in the stack and we haven't reached the max depth
  while (stack.length) {
    const ele = stack.pop();

    // If depth is greater than 0 and the element is an array, we need to flatten further
    if (Array.isArray(ele) && currentDepth < depth) {
      stack.push(...ele); // Push the elements of the array onto the stack
      currentDepth++; // Increment the depth level
    } else {
      result.push(ele); // Add non-array elements or if we've reached the depth to the result
    }
  }

  return result.reverse(); // Reverse to maintain original order
};

// Test case for Iterative Approach with Depth
const resultIterativeWithDepth = flattenIterativeWithDepth(
  [[[[[0]]], [1]], [[[2], [3]]], [[4], [5]]], 1
);
console.log(resultIterativeWithDepth);
// Expected Output: [ [ [ 0 ] ], 1, [ [ 2 ], [ 3 ] ], 4, 5 ]
```

## 🚀Chain calculator
### How would you implement a calculator class with methods for addition,subtraction, and multiplication, supporting method chaining? calculator.add(3).multiply(4).subtract(5).getValue()

```js
class Calculator {
 constructor(initialValue = 0) {
 this.value = initialValue;
 }
add(amount) {
 this.value += amount;
 return this; // Enable method chaining
 }
 subtract(amount) {
 this.value -= amount;
 return this; // Enable method chaining
 }
 multiply(factor) {
 this.value *= factor;
 return this; // Enable method chaining
 }
 getValue() {
 return this.value;
 }
// this will return same instance of the Calculator, will allow method 
// chaining
const calculator = new Calculator(2);
console.log(calculator.add(3).multiply(4).subtract(5).getValue());
```
#### How It Works
* #### new Calculator(2): Initializes this.value = 2.
* #### .add(3): Adds 3 to 2, so this.value = 5. Returns this.
* #### .multiply(4): Multiplies 5 by 4, so this.value = 20. Returns this.
* #### .subtract(5): Subtracts 5 from 20, so this.value = 15. Returns this.
* #### .getValue(): Returns the final value, 15.

---  

## 🚀Promises in sequence
#### How would you implement a function to execute an array of asynchronous tasks sequentially, collecting both resolved values and errors?
#### Let’s design a function to execute an array of asynchronous tasks sequentially, collecting both resolved values and errors.
#### I’ll provide a clear, practical implementation, explain how it works, and offer variations based on common use cases

#### Core Implementation (Iterative with async/await)
```js
const executeTasksSequentially = async (tasks) => {
  const results = []; // Store resolved values
  const errors = [];  // Store rejected values

  for (const task of tasks) {
    try {
      const value = await task(); // Wait for the task to resolve or reject
      results.push(value);
    } catch (error) {
      errors.push(error);
    }
  }

  return { results, errors }; // Return both arrays
};

// Example usage with your `createAsyncTask` from before
const createAsyncTask = () => {
  const randomVal = Math.floor(Math.random() * 10);
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (randomVal > 5) resolve(randomVal);
      else reject(randomVal);
    }, randomVal * 100);
  });
};

const tasks = Array(5).fill(null).map(() => createAsyncTask);

executeTasksSequentially(tasks).then(({ results, errors }) => {
  console.log("Results:", results); // e.g., [6, 8, 7]
  console.log("Errors:", errors);   // e.g., [3, 5]
});
```
### How It Works
* #### Input: Takes an array of task functions (tasks), where each task returns a Promise.
* #### Execution: Uses a for...of loop with await to run tasks one by one (sequentially).
* #### Error Handling: Wraps each task in a try/catch block:
* #### If the promise resolves, the value is pushed to results.
* #### If it rejects, the error is pushed to errors.
* #### Output: Returns an object with two arrays: results (resolved values) and errors (rejected values).
* #### Sequential Nature: The await ensures the next task doesn’t start until the current one finishes.

## Alternative Implementation (Callback Style)
#### If you prefer a callback-based approach (like your original Iterative), here’s how it could look:
```js
const executeTasksSequentially = async (tasks) => {
  const results = []; // Store resolved values
  const errors = [];  // Store rejected values

  for (const task of tasks) {
    try {
      const value = await task(); // Wait for the task to resolve or reject
      results.push(value);
    } catch (error) {
      errors.push(error);
    }
  }

  return { results, errors }; // Return both arrays
};

// Example usage with your `createAsyncTask` from before
const createAsyncTask = () => {
  const randomVal = Math.floor(Math.random() * 10);
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (randomVal > 5) resolve(randomVal);
      else reject(randomVal);
    }, randomVal * 100);
  });
};

const tasks = Array(5).fill(null).map(() => createAsyncTask);

executeTasksSequentially(tasks).then(({ results, errors }) => {
  console.log("Results:", results); // e.g., [6, 8, 7]
  console.log("Errors:", errors);   // e.g., [3, 5]
});
```
### How It Works
* #### Uses a recursive-like structure with an inner runTask function, but avoids true recursion by calling it iteratively within the async flow.
* #### Still sequential due to await, but delivers results via a callback instead of a promise.

## Recursive Implementation (Promise-Based)
```js
const executeTasksSequentiallyRecursive = (tasks) => {
  const results = [];
  const errors = [];

  const run = (index) => {
    if (index >= tasks.length) {
      return Promise.resolve({ results, errors }); // Base case
    }

    return tasks[index]()
      .then((value) => {
        results.push(value);
      })
      .catch((error) => {
        errors.push(error);
      })
      .then(() => run(index + 1)); // Continue regardless of success/failure
  };

  return run(0);
};

// Usage
executeTasksSequentiallyRecursive(tasks).then(({ results, errors }) => {
  console.log("Results:", results);
  console.log("Errors:", errors);
});
```
### How It Works
* #### Uses recursion with promises instead of async/await.
* #### Chains .then() and .catch() to handle outcomes, then recurses with the next index.
* #### Returns a promise that resolves with the final { results, errors }.

---  

## 🚀Compose and Pipe
* ### Compose => it is a higher order function which take multiple function as a arguments and executed them one by one from right to left
* ### pipe=> it is a higher order function which take multiple function as a arguments and executed them one by one from left to right

#### Both the functions work like The output of one function becomes the input to the next function and so on.

```js
// Pipe: Executes functions from LEFT to RIGHT
const pipe = (...fns) => {
  return function (x) {
    // Apply each function from left to right using reduce
    return fns.reduce((value, func) => func(value), x);
  };
};

// Compose: Executes functions from RIGHT to LEFT
const compose = (...fns) => {
  return function (x) {
    // Apply each function from right to left using reduceRight
    return fns.reduceRight((value, func) => func(value), x);
  };
};

const add5 = (x) => x + 5;
const multiply2 = (x) => x * 2;
const subtract3 = (x) => x - 3;
const toString = (x) => `${x}`; // Converts a number to a string

console.log("Pipe");

// pipe(add5, multiply2, subtract3)(10)
// Step-by-step: 
// add5(10) => 15
// multiply2(15) => 30
// subtract3(30) => 27
const result1 = pipe(add5, multiply2, subtract3)(10);
console.log(result1); // 27

// pipe(toString, add5)(5)
// Step-by-step:
// toString(5) => "5"
// add5("5") => "55" (JS will coerce and concatenate string + number)
const result2 = pipe(toString, add5)(5);
console.log(result2); // "55"

console.log("Compose");

// compose(add5, multiply2, subtract3)(10)
// Step-by-step:
// subtract3(10) => 7
// multiply2(7) => 14
// add5(14) => 19
const result3 = compose(add5, multiply2, subtract3)(10);
console.log(result3); // 19

// compose(toString, add5)(5)
// Step-by-step:
// add5(5) => 10
// toString(10) => "10"
const result4 = compose(toString, add5)(5);
console.log(result4); // "10"
```
## Output
```js
Pipe
27
55
Compose
19
10
```
---  

## 🚀Interview Question
#### Explain how prototypal inheritance works in JavaScript and how a child inherits properties from a parent using prototype
```js
// This can be easily done using a class-based but to judge your prototype 
// knowledge such questions can come.
//parent function
function Person(name) {
 this.name = name;
}
Person.prototype.hello = function() {
 return `Hello ${this.name}`;
};
//child function
function Developer(name, title) {
 Person.call(this, name);
 this.title = title;
}
// Override person prototype in Developer's prototype
Developer.prototype = Object.create(Person.prototype);
// Reset the constructor property of Developer's prototype
Developer.prototype.constructor = Developer;
// Now you can add any methods in developer prototype 
Developer.prototype.getTitle = function() {
 return this.title;
};
const obj = new Developer("Alice", "Software Engineer");
console.log(obj.hello()); // Output: Hello Alice
console.log(obj.getTitle()); // Output: Software Engineer
```
#### Learn more here about - https://www.youtube.com/watch?v=CpmE5twq1h0
#### https://youtu.be/wstwjQ1yqWQ?si=gXOhIy4v_9NELs-j

---  

## 🚀 Event emitter
#### How does the EventEmitter class handle multiple event subscriptions and allow unsubscribing from individual events?

```js
class EventEmitter {
  constructor() {
    // A Map to store event names and their corresponding callback maps
    this._eventSubscriptions = new Map();
  }

  // ✅ Subscribe to an event
  subscribe(eventName, callback) {
    // Ensure the callback is a function
    if (typeof callback !== "function") {
      throw new TypeError("Callback should be a function");
    }

    // If the event doesn't exist yet, initialize it with a new Map
    if (!this._eventSubscriptions.has(eventName)) {
      this._eventSubscriptions.set(eventName, new Map());
    }

    // Create a unique subscription ID using Symbol
    const subscriptionId = Symbol();
    const subscriptions = this._eventSubscriptions.get(eventName);

    // Save the callback with the unique ID
    subscriptions.set(subscriptionId, callback);

    // Return an object that lets you remove the subscription later
    return {
      remove: () => {
        if (!subscriptions.has(subscriptionId)) {
          throw new Error("Subscription has already been removed");
        }
        subscriptions.delete(subscriptionId);
      }
    };
  }

  // ✅ Emit an event
  emit(eventName, ...args) {
    const subscriptions = this._eventSubscriptions.get(eventName);

    // Throw an error if no one is listening to this event
    if (!subscriptions) {
      throw new Error("No event found");
    }

    // Call each subscribed callback with the passed arguments
    subscriptions.forEach(callback => callback(...args));
  }
}
```
## 🧪 Example Usage:
```js
const emitter = new EventEmitter();

// Subscribe to "greet" event
const sub1 = emitter.subscribe("greet", (name) => {
  console.log(`Hello, ${name}!`);
});

// Subscribe to "greet" again
const sub2 = emitter.subscribe("greet", (name) => {
  console.log(`Nice to meet you, ${name}.`);
});

// Emit the "greet" event
emitter.emit("greet", "Alice");

// Output:
// Hello, Alice!
// Nice to meet you, Alice.

// Remove one subscription
sub1.remove();

// Emit again
emitter.emit("greet", "Bob");

// Output:
// Nice to meet you, Bob.
```




