---
layout: post
title:      "Design Pattern: Command"
date:       2021-05-29 04:40:22 -0400
description: ''
permalink:  design_pattern_command
---

Template Method Pattern is the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

This pattern can be used:

* to implement the invariant parts of an algorithm once and leave it up to subclasses to implement the behavior that can vary.
* when common behavior among subclasses should be factored and localized in a common class to avoid code duplication.
* to control subclasses extensions. You can define a template method that calls "hook" operations at specific points, thereby permitting extensions only at those points.

For example, we want to design a package for build houses.  There are two kinds of houses such as woodmade and brickmade.

First, create an abstract class that contains steps of building a house.

```
public abstract class HouseTempale {
    public final void buildHouse(){
        buildFoundation();
        buildPillars();
        buildWindows();
        buildWalls();
        buildRoof();
        if(withChimney()) buildChimney();
        System.out.println("The house is completed.\n");
    }

    private void buildFoundation(){
        System.out.println("building foundation");
    }

    private void buildPillars(){
        System.out.println("building pillars");
    }

    private void buildRoof(){
        System.out.println("building roof");
    }

    private void buildChimney(){
        System.out.println("building chimney");
    }

    // hook can be over-written for optional choices
    protected boolean withChimney(){
        return false;
    }

    abstract void buildWindows();
    abstract void buildWalls();
}
```

Then, create a brckHouse class and a woodHouse class and both inherit with houseTemplete

```
public class BrickHouse extends HouseTempale{

    @Override
    void buildWindows() {
        System.out.println("build brick windows");
    }

    @Override
    void buildWalls() {
        System.out.println("build brick walls");
    }

    @Override
		// build with a chimney
    protected boolean withChimney() {
        return true;
    }
}
```


```
public class WoodHouse extends HouseTempale{

    @Override
    void buildWindows() {
        System.out.println("build wood window");
    }

    @Override
    void buildWalls() {
        System.out.println("build wood walls");
    }
}
```

Now, test out.

Main
```
    public static void main(String[] args){
        System.out.println("building a wood house");
        WoodHouse woodHouse = new WoodHouse();
        woodHouse.buildHouse();

        System.out.println("building a brick house");
        BrickHouse brickHouse = new BrickHouse();
        brickHouse.buildHouse();
    }
```

To sum up, The template method design pattern allows users to override only certain parts of a large algorithm, making them less affected by changes that happen to other parts of the algorithm. Also, it can avoid duplicate code into a superclass. However, the cons of this pattern are template methods tend to be harder to maintain too many steps, and users may be limited by the provided skeleton of an algorithm.
