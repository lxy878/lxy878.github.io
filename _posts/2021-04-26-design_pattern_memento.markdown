---
layout: post
title:      "Design Pattern: Memento"
date:       2021-04-26 02:30:22 -0400
description: 'Memento Pattern keeps the safe state external to the object. The object’s internal state is not exposed, thus we don’t break encapsulation. The captured state is kept in a memento object.'
permalink:  design_pattern_memento
---

Memento Pattern keeps the safe state external to the object. The object’s internal state is not exposed, so we don’t break encapsulation. The captured state keeps in a memento object. Later we can restore the object with this saved state.

The design pattern is frequently applied in applications like a text editor.

The pattern has three elements.

* Caretaker: a collection of states
* Memento: a state of one input when the editor saves
* Originator: an encapsulated class.

First, create a simple Originator that contains a setter and a getter.

```
public class Originator {
    private String content;

    public void setContent(String content){
        this.content = content;
    }

    public String getContent(){
        return this.content;
    }
}

```

Second, add two actions as save and restore in Originator.

```
   public Memento saveContent(){
        return new Memento(content);
    }

    public void restore(Memento memento){
        content = memento.getContent();
    }
```

Third, create a Memento class that stores one input state.

```
public class Memento {
    private final String content;

    public Memento(String content){
        this.content = content;
    }

    public String getContent(){
        return this.content;
    }
}
```

Forth, create Caretaker class that holds all inputs in a stack.


```
public class Caretaker {
    private Stack<Memento> mementos = new Stack<>();

    public void push(Memento memento){
        mementos.push(memento);
    }

    public Memento pop(){
        return mementos.pop();
    }
}

```

Finally, run a test on the Main.

```
public class Main {

    public static void main(String[] args) {
        Originator editor = new Originator();
        Caretaker history = new Caretaker();
        editor.setContent("1st input");
        history.push(editor.saveContent());
        editor.setContent("2nd input");
        history.push(editor.saveContent());
        editor.setContent("3rd input");
        history.push(editor.saveContent());

        editor.setContent("final input");
        editor.restore(history.pop());
        System.out.println(editor.getContent());
    }
}
```

In Main, the editor is entered four times and saves three times. After the last input occurs, the editor executes the restore action which overwrites the current content to the previous one.

The memento design pattern is the solution of an encapsulated object that can't be accessed from outside the object. However, saving and restoring the state of a complex object may be time-consuming. You should carefully consider performance implications when implementing the memento pattern. Another frequent problem is exposing the originator’s internal details, thus breaking encapsulation. The client should rely solely on the caretaker to save and restore the state of the originator.
