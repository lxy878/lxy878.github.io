---
layout: post
title:      "Four Fundamental OOP concepts in Java"
date:       2021-04-19 01:28:22 -0400
description: 'Object-oriented programming is a programming paradigm where everything is represented as an object. Objects pass messages to each other. Each object decides what to do with a received message.'
permalink:  four_fundamental_oop_concepts_in_java
---


Object-oriented programming is a programming paradigm where everything is represented as an object. Objects pass messages to each other. Each object decides what to do with a received message. OOP focuses on each object’s states and behaviors.

Before diving into the four pillars of OOP, There are some of the basic terminologies.

**Coupling**: is a class is dependent on another class.
``` 
public class Dog{
    public name;
    public Dog(String name){
        this.name = name;
    }
    public void bark(){
        System.out.println(this.name+" is barking!!!");
    }
}

public static void main(String args[]){
    Dog lucky = new Dog("Lucky");
    lucky.bark();
}
```

In the code above, the main class is dependent on the Dog class. So it means that if the Dog class edits its functions, the action can cause errors to the main class and recompilation between the classes.

**Interface**: contains a list of functions that do not have a body.

```
interface Pet{
    public String giveName(String name);
    public String owner(String ownerName);
}

public class Dog implements Pet{
    public String petName;
    public String ownerName;
    @Override
    public String giveName(String name){
        this.petName = name;
        return this.petName;
    }
    public String owner(String ownerName){
        this.ownerName = ownerName;
        return this.ownerName;
    }
}
```		
In the code above, the Dog class implements the Pet interface, so Dog must have all functions in Pet.

**Four fundamental object-oriented programming concepts:**

**Encapsulation**: keeps the data(variables) as hidden in a class, so the data can be only accessed for the current class.

```
public class Person{
    private String name;
    private String gender;
    public Person(String name, String gender){
        this.name = name;
        this.gender = gender;
    }

    public String getName(){
        return this.name;
    }

    public String getGender(){
        return this.gender;
    }
}
```

The Person class keeps name and gender as encapsulated. Once, a Person instance is created. The variables of name and gender won't be modified and read by the getters of the two variables.

**Abstraction**: a process of hiding the implementation details. In other words, the information represents what the object does instead of how it does it.

```
public abstract class Person{
    private String lastName;
    private String firstName;
    private String gender;
    public Person(String lastName, String firstName, String gender){
        this.lastName = lastName;
        this.firstName = firstName;
        this.gender = gender;
    }

    public String getFirstName(){
        return this.firstName;
    }

    public String getLastName(){
        return this.lastName;
    }

    public String getGender(){
        return this.gender;
    }

    public abstract String fullName(String ln, String fn);
}
```

Adding the keyword **"abstract"** to the Person class can turn a class into an abstract class. So, that means that Person can not be used to create its instances. The only way to do so is inherited by another class.

**Inheritance**: a class extends an abstract class.

```
public class Firefighter extends Person{
    private String department;
    public Firefighter(String name, String gender, String department){
        super(name, gender);
        this.department = department;
    }

    @Override
    public String fullName(String ln, String fn){
        return getFirstName()+" "+getLastName();
    }

    public String getDepartment(){
        return this.department;
    }
}
```
Since the Firefighter class extends the Person abstract class, the class allows to access all methods in Person and it. 

**Polymorphism**: can use inherited methods to perform different tasks. In other words, a single action can perform in different ways.

```
public abstract Animal{
    public void sound(){
        System.out.println("The aminal makes a sound.");
    }
}

public class Dog extends Animal{

    public void sound(){
        System.out.println("wolf, Wolf!!!");
    }
}

public static void main(String args[]){
    Animal animal = new Animal()
    Animal lucky = new Dog();
    animal.sound();
    lucky.sound();
}
```
In the code above, Since the Dog class inherits the Animal class, the sound() is not only allowed to access in Dog, but it also can be overwritten by Dog. Therefore, when sound() is called by classes, the results are different.

To sum up, the OOP concepts make complex code easier to develop, more reliable, more maintainable, and generally better.

