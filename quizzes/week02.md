# Intro to JavaScript

**1.** Which keywords are used to declare a variable in JavaScript?
<!-- enter you answer in the space below -->
```
You can use var, let, or const to declare variables in JavaScript, though let and const are the options of the three. 
```
**2.** What is the definition of a function?
<!-- enter you answer in the space below -->
```
A function is an object and subprorgram used to communicate that a specific action needs to take place. There are 3 different types of functions: a function declaration, a function expression, and an arrow function expression. 
```
**3.** What are the `SOLID` principles?
<!-- enter you answer in the space below -->
```
The SOLID principles are used to help developers be conscientious about how a project may grow/change over time and to write flexible code in a way that is easier to maintain and understand.
S - single-responsibility principle
O - open-closed principle
L - Liskov substitution principle
I - Interface Segregation principle
D - Dependency Inversion principle
```
**4.** Given this array: 
```js
let fruit = ['apple', 'banana', 'pineapple',  'orange', 'strawberry']
``` 
What index is the pineapple's current position? How do you know?
<!-- enter you answer in the space below -->
```
The pineapple's current position is 2. I know this because the first item in an array is always in position 0, so since pineapple is 2 spots after apple, it would be position 2.
```
**5.** With these two objects: 
```js
let you = { name:"You", hair: true, friends: [] }
let them = { name:"Them", hair: false, friends: [] }
```
how would you .push the `them` object into the `you` object's array of friends?
<!-- enter you answer in the space below -->
```
you.friendsArr.push(them)
```

**6.** Give an example of a JavaScript `Conditional`:
<!-- enter you answer in the space below -->
```
if (i > 10) {
  console.log('i is greater than 10')
} else {
  console.log('i is less than 10')
}
```
**7.** In the `for loop` below, what is the name of the piece belongs inside the empty "______" space? What would you put here to increase `i` by one on every iteration?
```js
for ( let i = 0; i < arr.length; _______ ) {
  //...
```
<!-- enter you answer in the space below -->
```
The name of the empty space is the final-expression, or the actual iteration incrementer.
To increase i by 1 on every iteration, you would put i++ as the final expression.
```
**8.** What does the `DOM` acronym stand for? Which file is first accessed to render the `DOM`?
<!-- enter you answer in the space below -->
```
DOM stands for Document Object Model. The first file accessed to render the DOM is the HTML file.
```

**9.** What are the `9` ECMAScript types as defined by MDN?
<!-- enter you answer in the space below -->
```
The 9 ECMAScript types as defined by MDN are string, number, boolean, symbol, bigInt, null, undefined, objects, and arrays.
```
**10.** When it comes to functions or methods, what is the difference between a `parameter` and an `argument`?
<!-- enter you answer in the space below -->
```
The difference between a parameter and an argument is that parameters are used to help define the function, while arguments are the value that the function returns after it has been invoked.
```
**11.** What is the difference between a `primitive` value and a `reference` value?
<!-- enter you answer in the space below -->
```
Primitive values are simple/singular values, while reference values are more complex, like objects and arrays.
```