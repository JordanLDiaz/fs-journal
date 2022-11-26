# UnderStanding Asynchronous Code, and API's

**1.** What is the difference between `asynchronous` code and `synchronous` code?
<!-- enter you answer in the space below -->
```
In coding, synchronous means that only one thing can be done at a time, while asynchronous means you can have multiple things occuring at one time, typically while we wait on a request made to an api. 
```
**2.** What is an event listener?
<!-- enter you answer in the space below -->
```
An event listener is placed stategically to listen for a specific event that is triggered by the user, so that it can then respond to the event. 
```
**3.** What does the `O` represent in the `SOLID` principles?
<!-- enter you answer in the space below -->
```
In SOLID, the O stands for the open-closed principle. 
```
**4.** What is a callback / higher order function?
<!-- enter you answer in the space below -->
```
A higher order function is a function that passes other functions as arguments and can return a function as a value. Callbacks on the other hand are functions that are passed to other functions so that the secondary functions are responsible for invoking them. 
```
**5.** What is a `promise`? How do you capture an error from a `promise`?
<!-- enter you answer in the space below -->
```
A promise is an object that is created when a request is placed to an api. Promises have 3 statuses: pending, resolved, and rejected. In order to capture an error from a promise that is rejected, you can use the try/catch method which can alert the user that an error has occured. You can also build in a console.log that catches the error. 
```
**6.** Name three processes used to make requests over `HTTP`?
<!-- enter you answer in the space below -->
```
The processes used to make requests over HTTP are get, post, put, and delete. 
```
**7.** What does the `API` acronym stand for?
<!-- enter you answer in the space below -->
```
API stands for application programming interface. 
```
**8.** In the `MVC` design pattern, who is responsible for making calls to `APIs`?
<!-- enter you answer in the space below -->
```
In the MVC design pattern, the service is responsible for making calls to api's. 
```
**9.** What is the purpose of encapsulation in programming?
<!-- enter you answer in the space below -->
```
In programming, encapsulation can be used to hide functions and data from users, as well as to help keep things organized. 
```
**10.** What is `HTTP` response code for a successful request?
<!-- enter you answer in the space below -->
```
Successful HTTP requests are denoted by codes between 200 - 299. 
```
**11.** What is a 500 error?
<!-- enter you answer in the space below -->
```
A 500 error means the server encountered an unexpected problem that prevented it from being able to fulfill the request. 
```