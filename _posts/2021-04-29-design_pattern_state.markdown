---
layout: post
title:      "Design Pattern: State"
date:       2021-04-29 14:40:22 -0400
description: 'State Pattern allows an object to alter its behavior when its internal state changes.'
permalink:  design_pattern_state
---

State Pattern allows an object to alter its behavior when its internal state changes.

The State Pattern can use in those scenarios:

* An object's behavior depends on its state, and it must change its behavior at runtime depending on that state.
* Operations have large, multipart conditional statements that depend on the object's state. This state is usually represented by one or more enumerated constants. Often, several operations will contain this same conditional structure. The State pattern puts each branch of the conditional in a separate class. This lets you treat the object's state as an object in its own right that can vary independently from other objects.

In the general steps to implement state pattern: 

1. create an abstracted/interface class for behaviors.
2. create several classes for different states, inherited with these behaviors.
3. create a top-level class that contains these states.

For example, an ATM machine will meet some situations such as no card, has card, has PIN, and no cash. In each situation, there are similar behaviors as insertCard, ejectCard, enterPIN, and requestCash.

First, we create an interface for insertCard, ejectCard, enterPIN, and requestCash.

```
// ATMState
public interface ATMState {
    void insertCard();
    void ejectCard();
    void enterPIN(int pin);
    void requestCash(int cash);
}
```

Then, we create states in different scenarios as no card, has card, has PIN, and no cash.

```
// NoCard
public class NoCard implements ATMState{
    ATMMachine atm;
    public NoCard(ATMMachine atm){
        this.atm = atm;
    }

    @Override
    public void insertCard() {
        System.out.println("Enter PIN");
        atm.setAtmState(atm.getHasCardState());
    }

    @Override
    public void ejectCard() {
        System.out.println("No Card");
    }

    @Override
    public void enterPIN(int pin) {
        System.out.println("No Card");
    }

    @Override
    public void requestCash(int cash) {
        System.out.println("No Card");
    }
}
```

```
// HasCard
public class HasCard implements ATMState {
    ATMMachine atm;
    public HasCard(ATMMachine atm){
        this.atm = atm;
    }

    @Override
    public void insertCard() {
        System.out.println("Card already inserted");
    }

    @Override
    public void ejectCard() {
        System.out.println("The Card is ejected");
        atm.setAtmState(atm.getNoCardState());
    }

    @Override
    public void enterPIN(int pin) {
        if(pin == 0000){
            System.out.println("PIN is correct");
            atm.rightPin = true;
            atm.setAtmState(atm.getHasPinState());
        }else{
            System.out.println("PIN is wrong");
            atm.rightPin = false;
            System.out.println("The card is ejected");
            atm.setAtmState(atm.getNoCardState());
        }
    }

    @Override
    public void requestCash(int cash) {
        System.out.println("Enter PIN");
    }
}
```

```
// HasPin
public class HasPin implements ATMState {
    ATMMachine atm;
    public HasPin(ATMMachine atm){
        this.atm = atm;
    }

    @Override
    public void insertCard() {
        System.out.println("Card already inserted");
    }

    @Override
    public void ejectCard() {
        System.out.println("Card is ejected");
        atm.setAtmState(atm.getNoCardState());
    }

    @Override
    public void enterPIN(int pin) {
        System.out.println("PIN is already entered");
    }

    @Override
    public void requestCash(int cash) {
        if(cash <= atm.cash){
            atm.setCash((atm.cash-cash));
            System.out.println(cash+" is provided");
            System.out.println("Card is ejected");
            atm.setAtmState(atm.getNoCardState());
            if(atm.cash <= 0){
                atm.setAtmState(atm.getNoCashState());
            }
        }else{
            System.out.println("You don't have that much cash available");
            atm.setAtmState(atm.getNoCardState());
        }
    }
}
```

```
// NoCash
public class NoCash implements ATMState {
    ATMMachine atm;
    public NoCash(ATMMachine atm){
        this.atm = atm;
    }

    @Override
    public void insertCard() {
        System.out.println("No Cash");
    }

    @Override
    public void ejectCard() {
        System.out.println("No Cash");
    }

    @Override
    public void enterPIN(int pin) {
        System.out.println("No Cash");
    }

    @Override
    public void requestCash(int cash) {
        System.out.println("No Cash");
    }
}
```

Finally, we create an ATM machine class.


```
// ATMMachine
public class ATMMachine {
    ATMState atmState;

    ATMState hasCard;
    ATMState noCard;
    ATMState hasPin;
    ATMState noCash;

    int cash = 1000;
    boolean rightPin = false;

    public ATMMachine(){
        hasCard = new HasCard(this);
        noCard = new NoCard(this);
        hasPin = new HasPin(this);
        noCash = new NoCash(this);
        atmState = noCard;
        if(this.cash <= 0){
            atmState = noCash;
        }
    }

    public void setAtmState(ATMState state){
        this.atmState = state;
    }

    public void setCash(int cash){
        this.cash = cash;
    }

    public void insertCard() {
        this.atmState.insertCard();
    }

    public void ejectCard() {
        this.atmState.ejectCard();
    }

    public void enterPIN(int pin) {
        this.atmState.enterPIN(pin);
    }

    public void requestCash(int cash) {
        this.atmState.requestCash(cash);
    }

    public ATMState getHasCardState(){
        return hasCard;
    }
    public ATMState getNoCardState(){
        return noCard;
    }
    public ATMState getHasPinState(){
        return hasPin;
    }
    public ATMState getNoCashState(){
        return noCash;
    }
}
```

Now, we test ATM machine states without using conditional statements.

```
// Main
public static void main(String[] args){
        ATMMachine atm = new ATMMachine();
        atm.insertCard();
        atm.ejectCard();

        atm.insertCard();
        atm.enterPIN(0000);
        atm.insertCard();
        atm.requestCash(1000);

        atm.insertCard();
        atm.enterPIN(1234);
    }
```

To sum up, the state design pattern can efficiently reduce the frequency of overusing condition statements and performance in the complex code. However, we should be aware of abusing the state pattern to complicate the code that can be written in a simple way.