# Application Architecture, MVC Design Pattern

**1.** What are the Pillars of Object Oriented Programming (`OOP`)?
<!-- enter you answer in the space below -->
```
The pillars of OOP are encapsulation, inheritance, and polymorphism. 
```
**2.** How would you access the `name` of the below object using the `property` variable?
```js
let staff = {
  name: "Tim",
  age: 26,
  job: "Code Monkey"
  }
let property = 'name'
```
<!-- enter you answer in the space below -->
```
staff.property
```
**3.** What is Encapsulation?
<!-- enter you answer in the space below -->
```
Encapsulation wraps up similar data and methods into separate files, as well as prevents the user from accessing certain information depending on where within the JavaScript files you place it, especially any actual data that is stored in the AppState.
```
**4.** What does the S stand for in the `SOLID` principles?
<!-- enter you answer in the space below -->
```
S = Single Responsibility principle
```
**5.** What the difference between a `class` and an instance of a `class`?
<!-- enter you answer in the space below -->
```
A class is a data type used to assign properties to elements, whereas an instance of a class refers to a specific object with that data type that has actually been stored in memory. 
```
**6.** What is a `Proxy` object?
<!-- enter you answer in the space below -->
```
A proxy object takes the place of the original object so that users do not have direct access to the original object.
```

**7.** What is the purpose of the `MVC` pattern?
<!-- enter you answer in the space below -->
```
The MVS pattern gives order and structure to the many files that are used within a project. Without it, files have more than one purpose, which doesn't adhere to our SOLID principles.
```
**8.** What is the job of the `Controller` in the `MVC` Pattern?
<!-- enter you answer in the space below -->
```
The controller's job is to handle information inputted by the user and to update the DOM.
```

**9.** What is the job of the `Service` in `MVC`?
<!-- enter you answer in the space below -->
```
The service's job is to handle the business logic. It communicates with the controller in order to execute functions.
```
**10.** What is the job of the `Model` in `MVC`?
<!-- enter you answer in the space below -->
```
The model is an example/model of what you want the screen to look like for the user. 
```
