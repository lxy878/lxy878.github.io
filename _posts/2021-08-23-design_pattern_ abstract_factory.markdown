---
layout: post
title:      "Creational Design Pattern: Abstract Factory"
date:       2021-08-23 04:40:22 -0400
description: "The abstract factory design pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes....."
permalink:  design_pattern_abstract_factory
---

The abstract factory design pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes.

Use the Abstract Factory pattern when
• a system should be independent of how its products are created, composed, and represented.
• a system should be configured with one of the multiple families of products.
• a family of related product objects is designed to be used together, and you need to enforce this constraint.
• you want to provide a class library of products, and you want to reveal just their interfaces, not their implementations.

For example, we want to design a program that makes different loans from different banks. Such as JP Chase only does business loans and Citibank only does student loans and business loans.

First, create an interface as the general loan.

```
public interface Loan {
    void setPayment(double loan);
    String getInfo();
}
```

Second, create three kinds of loans for home loans, business loans, and student loans, implemented with the loan interface.

```
public class BusinessLoan implements Loan {
    private double payment;
    private double interestRate = 0.071;

    @Override
    public void setPayment(double loan) {
        this.payment = loan;
    }

    @Override
    public String getInfo() {
        return "Payment: $"+String.valueOf(payment)+" APR: "+String.valueOf(interestRate*100)+"%.";
    }
}

```

```
public class StudentLoan implements Loan {
    private double payment;
    private double interestRate = 0.0375;

    @Override
    public void setPayment(double loan) {
        this.payment = loan;
    }

    @Override
    public String getInfo() {
        return "Payment: $"+String.valueOf(payment)+" APR: "+String.valueOf(interestRate*100)+"%.";
    }
}
```

```
public class HomeLoan implements Loan {
    private double payment;
    private double interestRate = 0.0275;

    @Override
    public void setPayment(double loan) {
        this.payment = loan;
    }

    @Override
    public String getInfo() {
        return "Payment: $"+String.valueOf(payment)+" APR: "+String.valueOf(interestRate*100)+"%.";
    }
}
```

Third, create an abstract class for the bank factory.

```
public abstract class BankFactory {
    protected abstract Loan createLoan(String typeLoan);
    protected abstract String getName();

    public Loan setLoanAccount(String typeLoan, double loan){
        Loan newLoan = createLoan(typeLoan);
        if(newLoan!=null) {
            newLoan.setPayment(loan);
            System.out.println(getName()+"create a loan: \n"+newLoan.getInfo());
        }else{
            System.out.println(getName()+" doesn't provide the loan.");
        }
        return newLoan;
    }
}
```

At last, create two classes for the two banks, inherited with the BankFactory.

```
public class JPChase extends BankFactory {
    private String name = "JPChase";

    @Override
    protected Loan createLoan(String typeLoan) {
        Loan newLoan = null;

        if(typeLoan.equals("HL")){
            newLoan = new HomeLoan();
        }else if(typeLoan.equals("BL")){
            newLoan = new BusinessLoan();
        }

        return newLoan;
    }

    @Override
    protected String getName() {
        return this.name;
    }

}
```

```
public class Citibank extends BankFactory {
    private String name = "Citibank";

    @Override
    protected Loan createLoan(String typeLoan) {
        Loan newLoan = null;

        if(typeLoan.equals("HL")){
            newLoan = new HomeLoan();
        }else if(typeLoan.equals("SL")){
            newLoan = new StudentLoan();
        }

        return newLoan;
    }

    @Override
    protected String getName() {
        return name;
    }
}
```

Now, test out.

```
public static void main(String[] args){
    BankFactory chase = new JPChase();
    BankFactory citibank = new Citibank();

    chase.setLoanAccount("HL", 500000);
    chase.setLoanAccount("BL", 1000000);
    citibank.setLoanAccount("SL", 50000);
    chase.setLoanAccount("SL", 50000);
}
```
```
// Outputs:
JPChasecreate a loan: 
Payment: $500000.0 APR: 2.75%.
JPChasecreate a loan: 
Payment: $1000000.0 APR: 7.1%.
Citibankcreate a loan: 
Payment: $50000.0 APR: 3.75%.
JPChase doesn't provide the loan.
```

To sum up, the abstract factory design pattern allows us to create families of related objects without specifying a concrete class.
Also, this pattern allows us to model any objects to interact throughout common interfaces. The disadvantage of the pattern is that the project can grow complicated.