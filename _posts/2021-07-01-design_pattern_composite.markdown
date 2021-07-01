---
layout: post
title:      "Structural Design Pattern: Composite"
date:       2021-07-01 04:40:22 -0400
description: 'Composite design pattern composes objects into tree structures to represent part-whole hierarchies that components can be further divided into smaller components...'
permalink:  design_pattern_composite
---

Composite design pattern composes objects into tree structures to represent part-whole hierarchies that components can be further divided into smaller components. This pattern allows us to treat individual objects and compositions of objects uniformly. we can structure data, or represent the inner working of every part of a whole object individually.

To use the composite pattern when
• we want to represent part-whole hierarchies of objects.
• we want users to be able to ignore the difference between compositions of objects and individual objects. Clients will treat all objects in the composite structure uniformly.

For example, we attempt to build a file management system that a folder object can contain files and other folders.

First, create an interface as Component.

```
public interface Component {
    void displayFileInfo();
}
```

Then, create two subclasses as Folder and File, implementing Component.

```
public class Folder implements Component {
    private String name, type;
    private List<Component> components = new ArrayList<>();

    public Folder(String name){
        this.name = name;
        this.type = "Folder";
    }

    public void add(Component component){
        this.components.add(component);
    }

    @Override
    public void displayFileInfo() {
        System.out.println(this.type+": "+ this.name+" contains");
        int index = 1;
        for(Component component : components) {
            System.out.print((index++)+". ");
            component.displayFileInfo();
        }
    }
}
```

```
public class File implements Component {
    private String name, type;

    public File(String name){
        this.name = name;
        this.type = "File";
    }

    public String getName(){
        return this.name;
    }

    public String getType(){
        return this.type;
    }

    @Override
    public void displayFileInfo() {
        System.out.println(this.getType()+": "+this.getName());
    }
}
```

Now, test out.

```
public static void main(String[] args){
        Folder superheros = new Folder("superheros");
        superheros.add(new File("Superman"));
        superheros.add(new File("Batman"));

        Folder marvel = new Folder("Marvel");
        superheros.add(marvel);
        marvel.add(new File("Spider man"));
        marvel.add(new File("Iron Man"));
        marvel.add(new File("Captain America"));

        superheros.displayFileInfo();
    }
```
```
// Outputs:
Folder: superheros contains
1. File: Superman
2. File: Batman
3. Folder: Marvel contains
// Inside marvel folder
1. File: Spider man
2. File: Iron Man
3. File: Captain America
```

To sum up, the composite design pattern makes the client simple, so clients don't need to know whether they're dealing with a leaf or a composite component. Also, this pattern makes it easier to add new components. New composites or leaves work automatically with existing structures and client code.