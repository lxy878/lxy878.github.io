---
layout: post
title:      "Design Pattern: Mediator"
date:       2021-06-13 04:40:22 -0400
description: 'The mediator design pattern is used to handle communication between related objects.  All communication is handled by the Mediator and the objects....'
permalink:  design_pattern_mediator
---

The mediator design pattern is used to handle communication between related objects. All communication is handled by the Mediator and the objects don't need to know anything about each other. It allows loose coupling by encapsulating the way disparate sets of objects interact and communicate with each other. Also, it allows for the actions of each object set to vary independently of one another.

The Mediator pattern is applied in those situations:

* a set of objects communicate in well-defined but complex ways. The resulting interdependencies are unstructured and difficult to understand.
* reusing an object is difficult because it refers to and communicates with many other objects.
* a behavior that's distributed between several classes should be customizable without a lot of subclassing.

For example, we attempt to create a program that posts renting information for lessors and tenants.

First, create a class to store the room information.

```
public class Room {
    private String location;
    private int bedroom;
    public Room(int bedroom, String location){
        this.bedroom = bedroom;
        this.location = location;
    }

    public int getBedroom() {
        return bedroom;
    }
    public String getLocation(){
        return location;
    }
}
```

Then, create an abstract class for general tenants.

```
public abstract class Tenant {
    private Mediator mediator;

    public Tenant(Mediator mediator){
        this.mediator = mediator;
        this.mediator.addTenant(this);
    }
    public void rentRoom(int bedroom, String location){
        mediator.findRoom(bedroom, location);
    }
}
```

Also, create classes for two groups of people such as students and workers, inherit Tenant.

```
public class Student extends Tenant {

    public Student(Mediator mediator) {
        super(mediator);
    }

}
```

```
public class Worker extends Tenant {

    public Worker(Mediator mediator) {
        super(mediator);
    }
}
```

Third, create an abstract class for general lessor and two classes inherit Lessor, for house owners and apartment managers.

```
public abstract class Lessor {
    private Mediator mediator;

    public Lessor(Mediator mediator){
        this.mediator = mediator;
        this.mediator.addLessor(this);
    }

    public void postRoom(int bedroom, String location){
        mediator.postRoom(bedroom, location);
    }
}
```

```
public class ApartmentManager extends Lessor{
    public ApartmentManager(Mediator mediator) {
        super(mediator);
    }
}
```

```
public class HouseOwner extends Lessor {
    public HouseOwner(Mediator mediator) {
        super(mediator);
    }
}
```

Finally, create a class for Mediator to connect between lessors and tenants.

```
public class Mediator {
    private List<Room> roomList = new ArrayList<Room>();
    private List<Room> roomRequest = new ArrayList<Room>();
    private List<Lessor> lessors = new ArrayList<Lessor>();
    private List<Tenant> tenants = new ArrayList<Tenant>();

    public void addLessor(Lessor lessor){
        lessors.add(lessor);
    }

    public void addTenant(Tenant tenant){
        tenants.add(tenant);
    }

    public void findRoom(int bedroom, String location){
        for(Room room : roomList){
            if(room.getBedroom()==bedroom && room.getLocation()==location){
                System.out.println("Room for "+ room.getBedroom()+" in "+ room.getLocation()+" is rented out.");
                roomList.remove(room);
                return;
            }
        }
        System.out.println("No room is found");
        System.out.println("Add new Request: bedroom for "+bedroom+" in "+location);
        roomRequest.add(new Room(bedroom, location));
    }

    public void postRoom(int bedroom, String location){
        for(Room room : roomRequest){
            if(room.getBedroom()==bedroom && room.getLocation() == location){
                System.out.println("Room for "+ room.getBedroom()+" in "+ room.getLocation()+" is rented out");
                roomRequest.remove(room);
                return;
            }
        }
        System.out.println("No request is found.");
        System.out.println("Add new room: bedroom for "+bedroom+" in "+ location);
        roomList.add(new Room(bedroom, location));
    }

    public void getRoomList(){
        System.out.println("Room Posts:");
        for(Room room : roomList){
            System.out.println("Room for "+ room.getBedroom()+" in "+room.getLocation());
        }

        System.out.println("Room Requests:");
        for(Room room : roomRequest){
            System.out.println("Room for "+ room.getBedroom()+" in "+room.getLocation());
        }

        System.out.println("");
    }
}
```

Now, test out.

```
public static void main(String[] args){
        Mediator mediator = new Mediator();
        Student student = new Student(mediator);
        Worker worker = new Worker(mediator);
        HouseOwner ho = new HouseOwner(mediator);
        ApartmentManager am = new ApartmentManager(mediator);

        ho.postRoom(1, "NY");
        ho.postRoom(2, "FL");
        worker.rentRoom(1, "NY");

        mediator.getRoomList();

        am.postRoom(2, "LA");
        student.rentRoom(1, "NY");

        mediator.getRoomList();

    }
```

```
// Output
No request is found.
Add new room: bedroom for 1 in NY
No request is found.
Add new room: bedroom for 2 in FL
Room for 1 in NY is rented out.
Room Posts:
Room for 2 in FL
Room Requests:

No request is found.
Add new room: bedroom for 2 in LA
No room is found
Add new Request: bedroom for 1 in NY
Room Posts:
Room for 2 in FL
Room for 2 in LA
Room Requests:
Room for 1 in NY
```

To sum up, the mediator design pattern allows us to extract the communications between various components into a single place, making it easier to comprehend and maintain. Also, it is convenient for us to introduce new mediators without having to change the actual components.