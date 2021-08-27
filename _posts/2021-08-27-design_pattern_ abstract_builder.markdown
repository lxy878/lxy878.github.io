---
layout: post
title:      "Creational Design Pattern: Builder"
date:       2021-08-27 04:40:22 -0400
description: "The builder design pattern separates the construction of a complex object from its representation so that the same construction process can create...."
permalink:  design_pattern_abstract_builder
---

The builder design pattern separates the construction of a complex object from its representation so that the same construction process can create different representations. In other words, the pattern decouples a complex object into parts so that create/set parts independently, so the object can be flexible and dynamic.

Use the Builder pattern when
* the algorithm for creating a complex object should be independent of the parts that make up the object and how they're assembled.
* the construction process must allow different representations for the object that's constructed.

For example, we want to create a program that handles orders for pizzas.  One is Veggie pizza and another is Hawaiian pizza.

First, create a class for the general pizza.

```
public class Pizza {
    private String dough = "";
    private String sauce = "";
    private String topping = "";

    public void setDough(String dough) {
        this.dough = dough;
    }

    public void setSauce(String sauce){
        this.sauce = sauce;
    }

    public void setTopping(String topping){
        this.topping = topping;
    }

    public String getDough(){
        return this.dough;
    }

    public String getSauce(){
        return this.sauce;
    }

    public String getTopping(){
        return this.topping;
    }

}
```

Then, create an interface for the general builder.

```
public interface PizzaBuilder {
    void makeDough();
    void makeSauce();
    void makeTopping();
    Pizza getPizza();
}
```

Third, create two classes for Hawaiian pizza and Veggie pizza.

```
public class HawaiianPizzaBuilder implements PizzaBuilder {
    private Pizza pizza;

    public HawaiianPizzaBuilder(){
        this.pizza = new Pizza();
    }

    @Override
    public void makeDough() {
        this.pizza.setDough("Cross");
    }

    @Override
    public void makeSauce() {
        this.pizza.setSauce("Marinara Sauce");
    }

    @Override
    public void makeTopping() {
        this.pizza.setTopping("Ham, Pineapple");
    }

    @Override
    public Pizza getPizza() {
        return this.pizza;
    }
}
```

```
public class VeggiePizzaBuilder implements PizzaBuilder {
    private Pizza pizza;

    public VeggiePizzaBuilder(){
        this.pizza = new Pizza();
    }

    @Override
    public void makeDough() {
        pizza.setDough("Pan-baked");
    }

    @Override
    public void makeSauce() {
        pizza.setSauce("Tomato Sauce");
    }

    @Override
    public void makeTopping() {
        pizza.setTopping("Baby Spinach");
    }

    @Override
    public Pizza getPizza() {
        return this.pizza;
    }
}
```

At last, create a class for the order as the director.

```
public class Order {
    private PizzaBuilder pizzaBuilder;

    public Order(PizzaBuilder pizzaBuilder){
        this.pizzaBuilder = pizzaBuilder;
    }

    public void makePizza(){
        pizzaBuilder.makeDough();
        pizzaBuilder.makeSauce();
        pizzaBuilder.makeTopping();
    }

    public Pizza getPizza(){
        return pizzaBuilder.getPizza();
    }
}
```

Now, test out.

```
public static void main(String[] args){
    Order order1 = new Order(new HawaiianPizzaBuilder());
    Order order2 = new Order(new VeggiePizzaBuilder());

    order1.makePizza();
    order2.makePizza();

    Pizza p1 = order1.getPizza();
    System.out.println("Dough: "+p1.getDough()+", Sauce: "+p1.getSauce()+", Topping: "+p1.getTopping());
    Pizza p2 = order2.getPizza();
    System.out.println("Dough: "+p2.getDough()+", Sauce: "+p2.getSauce()+", Topping: "+p2.getTopping());
}
```
```
// Outputs:
Dough: Cross, Sauce: Marinara Sauce, Topping: Ham, Pineapple
Dough: Pan-baked, Sauce: Tomato Sauce, Topping: Baby Spinach
```

To sum up, the builder design pattern hides the creation of the parts from the client so both aren't dependent. Also, the pattern allows reusing the same construction code when building various representations of products. However, the overall complexity of the code increases since the pattern requires creating multiple new classes.