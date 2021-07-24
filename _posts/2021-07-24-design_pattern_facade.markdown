---
layout: post
title:      "Structural Design Pattern: Facade"
date:       2021-07-24 04:40:22 -0400
description: "Facade Design Pattern provides a unified interface to a set of interfaces in a subsystem. This design pattern defines a higher-level interface that...."
permalink:  design_pattern_facade
---

Facade Design Pattern provides a unified interface to a set of interfaces in a subsystem. This design pattern defines a higher-level interface that makes the subsystem easier to use.

When you create a simplified interface that performs many other actions behind the scenes.

Use the Facade pattern
* to provide a simple interface to a complex subsystem. This makes the subsystem more reusable and easier to customize. 
* to decouple the subsystem from clients and other subsystems, thereby promoting subsystem independence and portability.
* to define an entry point to each subsystem level. 

For example, we attempt to create a program for bank accounts.  The program needs to do account checking and password checking before deposit or withdrawal.

First, create two classes for account checking and password checking.

```
public class AccountCheck {
    private int defaultAcc = 12345678;

    public boolean accountChecker(int acc){
        if(this.defaultAcc != acc) System.out.println("Account Not Exist");
        return this.defaultAcc == acc;
    }
}
```

```
public class PasswordCheck {
    private int defaultPW = 1234;

    public boolean passwordChecker(int password){
        if(defaultPW != password) System.out.println("Wrong Password\n");
        return defaultPW == password;
    }
}
```

Then, create a class to handle money deposits and money withdrawal.

```
public class FundsCheck {
    private double cash = 0.00;

    public void increaseCash(double money){
        this.cash += money;
    }

    public void decreaseCash(double money){
        this.cash -= money;
    }

    public boolean enoughMoney(double cash){
        if(this.cash >= cash) {
            decreaseCash(cash);
            System.out.println("Withdrawal Complete: Current Balance is " + this.cash);
            return true;
        }else{
            System.out.println("Not Enough Money");
            System.out.println("Current Balance: " + this.cash);
            return false;
        }
    }
    public void depositCash(double cash){
        increaseCash(cash);
        System.out.println("Deposit Complete: Current Balance is " + this.cash);
    }
}
```

Finally, create a facade class to put the previous three classes together.

```
public class AccountFacade {
    private int acc;
    private int pw;

    private AccountCheck accountCheck;
    private PasswordCheck passwordCheck;
    private FundsCheck fundsCheck;

    public AccountFacade(int acc, int pw){
        this.acc = acc;
        this.pw = pw;
        this.accountCheck = new AccountCheck();
        this.passwordCheck = new PasswordCheck();
        this.fundsCheck = new FundsCheck();
    }

    public void withdrawCash(double cash){
        if(accountCheck.accountChecker(this.acc) && passwordCheck.passwordChecker(this.pw) && fundsCheck.enoughMoney(cash)){
            System.out.println("Transaction Complete\n");
        }else{
            System.out.println("Transaction Failed\n");
        }
    }

    public void depositCash(double cash){
        if(accountCheck.accountChecker(this.acc) && passwordCheck.passwordChecker(this.pw)){
            fundsCheck.depositCash(cash);
            System.out.println("Transaction Complete\n");
        }else{
            System.out.println("Transaction Failed\n");
        }
    }

}
```

Now, test out.

```
public static void main(String[] args){
    AccountFacade account = new AccountFacade(12345678, 1234);
    account.withdrawCash(1000.50);
    account.depositCash(500);
    account.withdrawCash(100.60);
}
```
```
// Outputs:
Not Enough Money
Current Balance: 0.0
Transaction Failed

Deposit Complete: Current Balance is 500.0
Transaction Complete

Withdrawal Complete: Current Balance is 399.4
Transaction Complete
```

To sum up, the facade design pattern can do reducing the number of objects that clients deal with and making the subsystem easier to use.  Also, this pattern isolates the code from the complexity of a subsystem.