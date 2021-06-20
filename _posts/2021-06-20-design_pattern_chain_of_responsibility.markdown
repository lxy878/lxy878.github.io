---
layout: post
title:      "Design Pattern: Chain of Responsibility"
date:       2021-06-20 04:40:22 -0400
description: ''
permalink:  design_pattern_chain_of_responsibility
---

Chain of Responsibility Design Pattern is to avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain **until an object handles it**.

The pattern is used when:

* more than one object may handle a request, and the handler isn't known a
priori. The handler should be ascertained automatically.
* you want to issue a request to one of several objects without specifying the
receiver explicitly.
* the set of objects that can handle a request should be specified dynamically.

For example, we attempt to design a program that automanticlly assigns tasks to engineers by difficult levels.

The program follows those Rules:
junior engineer -> easy task
middile engineer -> medium task
senior engineer -> difficult task

First, create a Task class to store title and difficult of the task.

```
public class Task {
    private String title;
    private String difficulty;

    public Task(String title, String difficulty){
        this.title = title;
        this.difficulty = difficulty;
    }

    public String getTitle(){
        return title;
    }

    public String getDifficulty(){
        return difficulty;
    }
}
```

Then, create a interface as Engineer.

```
public interface Engineer {
    void setNext(Engineer engineer);
    void assignTask(Task task);
}
```

Last, create classes for three levels of Engineer

```
public class JuniorEngineer implements Engineer{
    private Engineer nextEngineer = null;
    @Override
    public void setNext(Engineer engineer) {
        this.nextEngineer = engineer;
    }

    @Override
    public void assignTask(Task task) {
        if(task.getDifficulty() == "easy"){
            System.out.println(task.getTitle() + " is assigned to a Jr. engineer.");
            return;
        }

        if(nextEngineer != null) nextEngineer.assignTask(task);
    }
}
```

```
public class MiddleEngineer implements Engineer{
    private Engineer nextEngineer = null;
    @Override
    public void setNext(Engineer engineer) {
        this.nextEngineer = engineer;
    }

    @Override
    public void assignTask(Task task) {
        if(task.getDifficulty() == "medium"){
            System.out.println(task.getTitle() + " is assigned to a Mid engineer.");
            return;
        }

        if(nextEngineer != null) nextEngineer.assignTask(task);
    }
}
```

```
public class SeniorEngineer implements Engineer{
    private Engineer nextEngineer = null;
    @Override
    public void setNext(Engineer engineer) {
        this.nextEngineer = engineer;
    }

    @Override
    public void assignTask(Task task) {
        if(task.getDifficulty() == "difficult"){
            System.out.println(task.getTitle() + " is assigned to a Sr. engineer.");
            return;
        }

        if(nextEngineer != null) nextEngineer.assignTask(task);
    }
}
```

Now, test out.

```
public static void main(String[] args){
        Engineer je = new JuniorEngineer();
        Engineer me = new MiddleEngineer();
        Engineer se = new SeniorEngineer();

        je.setNext(me);
        me.setNext(se);

        je.assignTask(new Task("Task 1", "medium"));
        je.assignTask(new Task("Task 2", "easy"));
        je.assignTask(new Task("Task 3", "difficult"));
    }
```

```
// Outputs:
Task 1 is assigned to a Mid engineer.
Task 2 is assigned to a Jr. engineer.
Task 3 is assigned to a Sr. engineer.
```

To sum up, the chain of responsibility design pattern frees an object from knowing which other object handles a request.  Also, it allows us to add flexibility in distributing responsibilities among objects.