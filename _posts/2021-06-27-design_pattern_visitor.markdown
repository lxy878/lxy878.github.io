---
layout: post
title:      "Design Pattern: Visitor"
date:       2021-06-27 04:40:22 -0400
description: 'Visitor design pattern represents an operation to be performed on the elements of an object structure. Visitor lets you define a new operation...'
permalink:  design_pattern_visitor
---

Visitor design pattern represents an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

This pattern allows you to add methods to classes of different types without much altering to those classes, so you can make completely different methods depending on the class used. It allows you to define external classes that can extend other classes without majorly editing them.

we can use the Visitor pattern when

* an object structure contains many classes of objects with differing interfaces, and you want to perform operations on these objects that depend on their concrete classes.
* many distinct and unrelated operations need to be performed on objects in an object structure, and you want to avoid "polluting" their classes with these operations. Visitor lets you keep related operations together by defining them in one class. When the object structure is shared by many applications, use Visitor to put operations in just those applications that need them.
* the classes defining the object structure rarely change, but you often want to define new operations over the structure. Changing the object structure classes requires redefining the interface to all visitors, which is potentially costly. If the object structure classes change often, then it's probably better to define the operations in those classes.

For example, we want to create a program that does calculations of different kinds of shapes such as rectangles, triangles, and circles.

First, create an interface for different shapes.

```
public interface Shape {
    void accept(Calculator calculator);
}
```

Then, create classes as rectangles, triangles, and circles, implementing the Shape interface.

```
public class Rectangle implements Shape{
    private double width;
    private double length;

    public Rectangle(double width, double length){
        this.width = width;
        this.length = length;
    }

    public double getWidth(){
        return width;
    }

    public double getLength(){
        return length;
    }

    @Override
    public void accept(Calculator calculator) {
        calculator.calculate(this);
    }
}
```

```
public class Circle implements Shape{
    private double radius;

    public Circle(double radius){
        this.radius = radius;
    }

    public double getRadius(){
        return radius;
    }

    @Override
    public void accept(Calculator calculator) {
        calculator.calculate(this);
    }
}
```

```
public class Triangle implements Shape{
    private double side_a, side_b;
    private double base;

    public Triangle(double side_a, double side_b, double base){
        this.side_a = side_a;
        this.side_b = side_b;
        this.base = base;
    }

    public double getSide_a(){
        return side_a;
    }

    public double getSide_b(){
        return side_b;
    }

    public double getBase(){
        return base;
    }

    @Override
    public void accept(Calculator calculator) {
        calculator.calculate(this);
    }
}
```

Third, create an interface for Calculator.

calcuator
```
public interface Calculator {
    void calculate(Triangle triangle);
    void calculate(Circle circle);
    void calculate(Rectangle rectangle);
}
```

Last, create Area class and Perimeter class both implementing Calculator interface.

```
public class Perimeter implements Calculator {
    @Override
    public void calculate(Triangle triangle) {
        System.out.println("The perimeter of triangle is "+(triangle.getSide_a()+triangle.getSide_b()+triangle.getBase()));
    }

    @Override
    public void calculate(Circle circle) {
        System.out.println("The perimeter of circle is "+circle.getRadius()*2*Math.PI);
    }

    @Override
    public void calculate(Rectangle rectangle) {
        System.out.println("The perimeter of rectangle is "+ (rectangle.getWidth()+rectangle.getLength())*2);
    }
}
```

```
public class Area implements Calculator {

    @Override
    public void calculate(Triangle triangle) {
        // Heron's formula:
        // area = sqrt(s(s-a)(s-b)(s-c))
        // s = (a+b+c)/2
        double s = (triangle.getBase()+triangle.getSide_a()+triangle.getSide_b())/2;
        double b = s*(s-triangle.getSide_a())*(s-triangle.getSide_b())*(s-triangle.getBase());
        if(b<0){
            System.out.println("The Area of triangle is invalid.");
        }else{
            System.out.println("The Area of triangle is "+ Math.sqrt(b));
        }
    }

    @Override
    public void calculate(Circle circle) {
        System.out.println("The Area of circle is "+ circle.getRadius()*circle.getRadius()*Math.PI);
    }

    @Override
    public void calculate(Rectangle rectangle) {
        System.out.println("The Area of rectangle is "+ rectangle.getLength()*rectangle.getWidth());
    }
}
```

Now, test out.

```
public static void main(String[] args){
        Calculator area = new Area();
        Calculator perimeter = new Perimeter();

        Shape circle = new Circle(2.5);
        Shape rectangle = new Rectangle(3, 5);
        Shape triangle = new Triangle(3,5,4);

        circle.accept(area);
        rectangle.accept(area);
        triangle.accept(area);

        circle.accept(perimeter);
        rectangle.accept(perimeter);
        triangle.accept(perimeter);
    }
```

```
// Output:
The Area of circle is 19.634954084936208
The Area of rectangle is 15.0
The Area of triangle is 6.0
The perimeter of circle is 15.707963267948966
The perimeter of rectangle is 16.0
The perimeter of triangle is 12.0
```

To sum up, the visitor design pattern makes it easy to add operations that depend on the components of complex objects. Also, A visitor object can accumulate some useful information while working with various objects. 