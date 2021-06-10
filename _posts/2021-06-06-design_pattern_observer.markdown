---
layout: post
title:      "Design Pattern: Observer"
date:       2021-06-06 04:40:22 -0400
description: 'Observer design pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.'
permalink:  design_pattern_observer
---

Observer design pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

The benefit of this pattern is that the subject(data) doesn't need to know anything about the observer(any output form such as a chart, a table...).

This pattern is applied to those situations:
* When an abstraction has two aspects, one dependent on the other. Encapsulating these aspects in separate objects lets you vary and reuse them independently.
* When a change to one object requires changing others, and you don't know how many objects need to be changed.
* When an object should be able to notify other objects without making assumptions about who these objects are. In other words, you don't want these objects tightly coupled.

For example, we create a simple program that maintains our information simultaneously in different places such as bank, company, and a shopping account.

First, create an interface for observers.

```
public interface Observer {
		void Update();
}
```

Then, create subclasses for Bank, Company, and Shopping, inherited from Observer.

```
public class BankObserver implements Observer {
    private UserData userData;

    public BankObserver(UserData user){
        this.userData = user;
    }

    @Override
    public void Update() {
        System.out.println("Bank Account Info: "+userData.getUserName() + " is "+ userData.getAge()+" years old.");
    }
}
```

```
public class CompanyObserver implements Observer{
    private UserData userData;

    public CompanyObserver(UserData user){
        this.userData = user;
    }

    @Override
    public void Update() {
        System.out.println("Company Account Info: "+userData.getUserName() + " is "+ userData.getAge()+" years old.");
    }
}
```

```
public class ShoppingObserver implements Observer{
    private UserData userData;

    public ShoppingObserver(UserData user){
        this.userData = user;
    }

    @Override
    public void Update() {
        System.out.println("Shopping Account Info: "+userData.getUserName() + " is "+ userData.getAge()+" years old.");
    }
}
```

Third, create an abstract class as Subject.

```
public abstract class Subject {
    private List<Observer> observerList = new ArrayList<Observer>();

    public void attach(Observer observer){
        observerList.add(observer);
    }

    public void detach(Observer observer){
        int index = observerList.indexOf(observer);
        System.out.println(observer+" "+" removed.");
        observerList.remove(index);
    }

    public void notifyObservers(){
        for(Observer ob : observerList){
            ob.Update();
        }
    }
}
```

Last, create a class that maintains our data, inherited Subject.

```
public class UserData extends Subject {
    private String userName;
    private int age;
    public UserData(String name, int age){
        this.userName = name;
        this.age = age;
    }

    public void setAge(int age) {
        System.out.println("Age changed from " + this.age+ " to "+age);
        this.age = age;
        notifyObservers();
    }

    public int getAge(){
        return age;
    }

    public void setUserName(String name){
        System.out.println("Name changed from " + this.userName+ " to "+name);
        this.userName = name;
        notifyObservers();
        System.out.println("");
    }

    public String getUserName() {
        return userName;
    }
}
```

Now, test out

```
public static void observerTest(){
    UserData user = new UserData("X Liu", 26);
    user.attach(new BankObserver(user));
    user.attach(new ShoppingObserver(user));
    user.attach(new BankObserver(user));

    user.setUserName("Jr Chan");

    user.setAge(18);
}
```

```
// output:
Age changed from X Liu to Jr Chan
Bank Account Info: Jr Chan is 26 years old.
Shopping Account Info: Jr Chan is 26 years old.
Bank Account Info: Jr Chan is 26 years old.

Age changed from 26 to 18
Bank Account Info: Jr Chan is 18 years old.
Shopping Account Info: Jr Chan is 18 years old.
Bank Account Info: Jr Chan is 18 years old.
```

To sum up, the observer design pattern allows users can introduce new Observer classes without having to change the Subject’s code. Also, the pattern allows users to establish relations between objects at runtime. The negative of the observer design pattern is that the subject may send updates that don't matter to the observer.