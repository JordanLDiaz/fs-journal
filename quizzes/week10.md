# C# Fundamentals


**1.** What is the purpose of a `namespace`?
<!-- enter you answer in the space below -->
```
The namespace in C# is used to declare a scope that contains a set of related objects. It is used to organize elements of code and to create globally unique types. 
```
**2.** What is the difference between a `class` and a `struct`?
<!-- enter you answer in the space below -->
```
Structures (struct) are value types that can be used to create objects, while classes are reference types and a user-defined blueprint from which objects can be created. Classes can inherit from other classes, whereas structs cannot.  
```
**3.** What is the method that returns an instance of a class, yet it has no return type?
<!-- enter you answer in the space below -->
```
A void return type returns an instance of a class, but has no return type. 
```
## Example 1
```c#
abstract class Car
{
  ...
  public virtual string Start()
  {
    return "Vroooom";
  }
}
```
**5.** In the example what is the access modifier of the `Start()` method?
<!-- enter you answer in the space below -->
```
Public
```
**6.** In the example what is `string` an indication of?
<!-- enter you answer in the space below -->
```
The value type expected to be returned. 
```
**7.** In the example what is `abstract` preventing?
<!-- enter you answer in the space below -->
```
Abstract is a restricted class that cannot be used to create objects, and in order to access it, it must be inherited from another class. 
```
**8.** In the example what is the purpose of `virtual`?
<!-- enter you answer in the space below -->
```
The purpose of virtual is to override the base class member in its derived class based on the requirement. 
```
**9.** Name four access modifiers:
<!-- enter you answer in the space below -->
```
Private, public, protected, and internal
```
**10.** If you set a class or method to private, what can access it?
<!-- enter you answer in the space below -->
```
Private methods can only be accessed inside that class. 
```