---
layout: post
title:      "Structural Design Pattern: Decorator"
date:       2021-07-17 04:40:22 -0400
description: "The decorator design pattern attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for..."
permalink:  design_pattern_decorator
---

The decorator design pattern attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality. 

We can simplify code by adding functionality using many simple classes. Rather than rewrite old code, we can extend with new code.

We use this decorator pattern:
* to add responsibilities to individual objects dynamically and transparently, that is, without affecting other objects.
* for responsibilities that can be withdrawn.
* when extension by subclassing is impractical. Sometimes a large number of independent extensions are possible and would produce an explosion of subclasses to support every combination. Or a class definition may be hidden or otherwise unavailable for subclassing.

For example, we want to create a program that makes orders from a menu. The orders have a main course optionally combined with dessert and/or beverage.

First, create an interface for all dishes.

```
public interface Dish {
    String getDescription();
    double getPrice();
}
```

Then, create a class for main courses.

```
public class MainCourse implements Dish {

    private String description;
    private double price;

    public MainCourse(String newDescription, double price){
        this.description = newDescription;
        this.price = price;
    }

    @Override
    public String getDescription() {
        return description;
    }

    @Override
    public double getPrice() {
        System.out.println("Cost of "+description+": $"+price);
        return price;
    }
}
```

Third, create an abstract class as a decorator for extra dishes.

```
abstract class DishDecorator implements Dish {

    protected Dish dish;

    public DishDecorator(Dish newDish){
        this.dish = newDish;
    }

    @Override
    public String getDescription() {
        return dish.getDescription();
    }

    @Override
    public double getPrice() {
        return dish.getPrice();
    }
}
```

Finally, create two classes for Dessert and Beverage, extended with the decorator.

```
public class Dessert extends DishDecorator {
    public Dessert(Dish newDish) {
        super(newDish);
    }
    public String getDescription(){
        return dish.getDescription() +", dessert";
    }

    public double getPrice(){
        System.out.println("Cost of dessert: $"+8.00);
        return dish.getPrice() + 8.00;
    }
}
```

```
public class Beverage extends DishDecorator {

    public Beverage(Dish newDish) {
        super(newDish);
    }

    public String getDescription(){
        return dish.getDescription() +", beverage";
    }

    public double getPrice(){
        System.out.println("Cost of beverage: $"+5.50);
        return dish.getPrice() + 5.50;
    }
}
```

Now, test out.

```
public static void decoratorTest(){
    Dish order1 = new Beverage(new MainCourse("Beef Stew", 12.50));

    System.out.println(order1.getDescription());
    System.out.println("Total cost: $"+order1.getPrice());

    Dish order2 = new Dessert(new Beverage(new MainCourse("Stack", 15.50)));

    System.out.println(order2.getDescription());
    System.out.println("Total cost: $"+order2.getPrice());
}
```

```
// Outputs:
Beef Stew, beverage
Cost of beverage: $5.5
Cost of Beef Stew: $12.5
Total cost: $18.0
Stack, beverage, dessert
Cost of dessert: $8.0
Cost of beverage: $5.5
Cost of Stack: $15.5
Total cost: $29.0
```

To sum up,  the decorator design pattern allows us to extend an object’s behavior without making a new subclass.  Also, this pattern allows to add or remove responsibilities from an object at runtime.  Furthermore, it allows to combine several behaviors by wrapping an object into multiple decorators.