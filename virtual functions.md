# Virtual functions

You use virtual functions when you want to override a certain behavior (read method) for your derived class rather than the one implemented for the base class and you want to do so at run-time through a pointer to the base class.

The classic example is when you have a base class called Shape and concrete shapes (classes) that derive from it. Each concrete class **overrides** (implements a virtual method) called Draw().

The class hierarchy is as follows:
**use getArea() instead of Draw() to explain**

<img src="https://image.ibb.co/cHUn8T/image.png" alt="image" border="0">

The following snippet shows the usage of the example; it creates an array of Shape class pointers wherein each points to a distinct derived class object. At run-time, invoking the Draw() method results in the calling of the method overridden by that derived class and the particular Shape is drawn (or rendered).

``` CPP
Shape *basep[] = { &line_obj, &tri_obj,
                   &rect_obj, &cir_obj};
for (i = 0; i < NO_PICTURES; i++)
    basep[i] -> Draw ();
  ```
The above program just uses the pointer to the base class to store addresses of the derived class objects. This provides a loose coupling because the program does not have to change drastically if a new concrete derived class of shape is added anytime. The reason is that there are minimal code segments that actually use (depend) on the concrete Shape type.

1. They Must be declared in public section of class.
2. Virtual functions cannot be static and also cannot be a friend function of another class.
3. Virtual functions should be accessed using pointer or reference of base class type to achieve run time polymorphism.
4. The prototype of virtual functions should be same in base as well as derived class.
5. They are always defined in base class and overridden in derived class. It is not mandatory for derived class to override (or re-define the virtual function), in that case base class version of function is used.
6. A class may have virtual destructor but it cannot have a virtual constructor.


**Runtime polymorphism** is achieved only through a pointer (or reference) of base class type. Also, a base class pointer can point to the objects of base class as well as to the objects of derived class. In above code, base class pointer ‘bptr’ contains the address of object ‘d’ of derived class.
 
 
### WHY use VF 

Here is how I understood not just what virtual functions are, but why they're required:

Let's say you have these two classes:
```
class Animal
{
    public:
        void eat() { std::cout << "I'm eating generic food."; }
};

class Cat : public Animal
{
    public:
        void eat() { std::cout << "I'm eating a rat."; }
};
```
In your main function:
```
Animal *animal = new Animal;
Cat *cat = new Cat;

animal->eat(); // Outputs: "I'm eating generic food."
cat->eat();    // Outputs: "I'm eating a rat."
```
So far so good, right? Animals eat generic food, cats eat rats, all without virtual.

Let's change it a little now so that eat() is called via an intermediate function (a trivial function just for this example):

```
// This can go at the top of the main.cpp file
void func(Animal *xyz) { xyz->eat(); }
```

Now our main function is:
```
Animal *animal = new Animal;
Cat *cat = new Cat;
```
func(animal); // Outputs: "I'm eating generic food."
func(cat);    // Outputs: "I'm eating generic food."
Uh oh... we passed a Cat into func(), but it won't eat rats. Should you overload func() so it takes a Cat*? If you have to derive more animals from Animal they would all need their own func().

The solution is to make eat() from the Animal class a virtual function:

```
class Animal
{
    public:
        virtual void eat() { std::cout << "I'm eating generic food."; }
};

class Cat : public Animal
{
    public:
        void eat() { std::cout << "I'm eating a rat."; }
};
Main:

func(animal); // Outputs: "I'm eating generic food."
func(cat);    // Outputs: "I'm eating a rat."
```

Done.


# Pure VF and Abstract Classes
```
virtual void display() = 0;
```

**IF base is an abstract class, Derived class will have to define or declare pure again.**


1. An abstract class is a class that is designed to be specifically used as a base class AND not create object
2. A class is abstract if it has at least one pure virtual function.
2. We can have pointers and references of abstract class type. BUT NOT OBJECT
3. If we do not override the pure virtual function in derived class, then derived class also becomes abstract class.
4. An abstract class can have constructors.

## Diamond Problem

[12.7 — Virtual base classes | Learn C++](http://www.learncpp.com/cpp-tutorial/128-virtual-base-classes/)


## Private VF

[Can virtual functions be private in C++? - GeeksforGeeks](https://www.geeksforgeeks.org/can-virtual-functions-be-private-in-c/)

## Pure Virtual Destrcutors

[Pure virtual destructor in C++ - GeeksforGeeks](https://www.geeksforgeeks.org/pure-virtual-destructor-c/)

## Inline VF - no

By default all the functions defined inside the class are implicitly or automatically considered as inline **except virtual functions**

Inline means replacing function call with definition. So how can dynamic binding be inline.

[Can virtual functions be inlined? - GeeksforGeeks](https://www.geeksforgeeks.org/inline-virtual-function/)


**Static member function of a class cannot be virtual or const and volatile**