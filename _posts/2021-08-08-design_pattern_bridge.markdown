---
layout: post
title:      "Structural Design Pattern: Bridge"
date:       2021-08-08 04:40:22 -0400
description: "Bridge design pattern decouples an abstraction from its implementation so that the two can vary independently, which means progressively adding functionality while...."
permalink:  design_pattern_bridge
---

Bridge design pattern decouples an abstraction from its implementation so that the two can vary independently, which means progressively adding functionality while separating out major differences using abstract classes.

We use the pattern when:
* attempt to change both the abstractions(abstract classes) and concrete classes independently
* the first abstract class to define rules
* the concrete class adds additional rules
* an abstract class has a reference to the device and it defines abstract methods that will be defined
* the concrete remote defines the abstract methods required

For example, we want to create a program that can set up computers with devices so that computers can execute the paired devices. 

First, create an interface for the general device.

```
public interface Device {
    String getName();
    void operate(String message);
}
```

Second, create an abstract class for the general computer.

```
public abstract class Computer {
    protected Device device;
    public Computer(Device device){
        this.device = device;
    }

    public abstract void execute(String message);
}
```

Third, create classes as fax machines and printers, inherited the device interface.

```
public class FaxMachine implements Device {
    private String name;
    public FaxMachine(String name){
        this.name = name;
    }
    @Override
    public String getName() {
        return this.name;
    }

    @Override
    public void operate(String message) {
        System.out.println(this.name+" sends out "+ message);
    }
}
```

```
public class Printer implements Device{
    private String name;
    public Printer(String name){
        this.name = name;
    }


    @Override
    public String getName() {
        return this.name;
    }

    @Override
    public void operate(String message) {
        System.out.println(this.name+" prints "+ message);
    }

}
```

At last, create classes as mac and windows, inherited the computer interface.

```
public class Windows extends Computer {
    public Windows(Device device) {
        super(device);
        System.out.println(this.device.getName()+ " pairs with windows");
    }

    @Override
    public void execute(String message) {
        System.out.println("Windows calls: ");
        device.operate(message);
    }
}
```

```
public class Mac extends Computer {

    public Mac(Device device) {
        super(device);
        System.out.println(this.device.getName()+ " pairs with mac");
    }

    @Override
    public void execute(String message) {
        System.out.println("Mac calls: ");
        device.operate(message);
    }
}
```

Now, test out.

main
```
public static void main(String[] args){
    Computer mac = new Mac(new Printer("Printer_1"));
    Computer windows = new Windows((new FaxMachine("FM_1")));

    mac.execute("Hello World!");
    windows.execute("Hello World!");
}
```
```
// Outputs:
Printer_1 pairs with mac
FM_1 pairs with windows
Mac calls: 
Printer_1 prints Hello World!
Windows calls: 
FM_1 sends out Hello World!
```

To sum up, the bridge design pattern allows that client code works with high-level abstractions. The code doesn't expose to the platform details. Also, this design pattern allows us to introduce new abstractions and implementations independently from each other.  Furthermore, this pattern allows us to focus on high-level logic in the abstraction and on platform details in the implementation.
