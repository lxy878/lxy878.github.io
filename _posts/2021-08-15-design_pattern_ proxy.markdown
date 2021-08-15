---
layout: post
title:      "Structural Design Pattern: Proxy"
date:       2021-08-15 04:40:22 -0400
description: "The proxy design pattern provides a surrogate or placeholder for another object to control access to it....."
permalink:  design_pattern_proxy
---

The proxy design pattern provides a surrogate or placeholder for another object to control access to it.

It can be applied for security reasons because an object is intensive to create, or is accessed from a remote location.

Proxy is applicable whenever there is a need for a more versatile or sophisticated reference to an object than a simple pointer. Here are several common situations in which the Proxy pattern is applicable:

* A remote proxy provides a local representative for an object in a different address space.
* A virtual proxy creates expensive objects on demand. The ImageProxy described in the Motivation is an example of such a proxy.
* A protection proxy controls access to the original object. Protection proxies are useful when objects should have different access rights.
* A smart reference is a replacement for a bare pointer that performs additional actions when an object is accessed.

For example, we want to create an ebook reader by implementing the proxy design pattern.

First, create an interface for general ebooks.

```
public interface Ebook {
    void show();
    String getTitle();
}
```

Then, create a class to store the real ebook and a class as the proxy for the ebook, both inherited with the interface.

```
public class RealEbook implements Ebook {
    private String title;
    public RealEbook(String title){
        this.title = title;
        loading();
    }

    private void loading(){
        System.out.println("Loading ebook "+this.title);
    }
    @Override
    public void show() {
        System.out.println("Reading ebook "+this.title);
    }

    @Override
    public String getTitle() {
        return this.title;
    }
}
```

```
public class EbookProxy implements Ebook {
    private RealEbook ebook;
    private String title;

    public EbookProxy(String title){
        this.title = title;
    }

    @Override
    public void show() {
        if(ebook == null)
            ebook = new RealEbook(this.title);
        ebook.show();
    }

    @Override
    public String getTitle() {
        return this.title;
    }
}
```

At last, create a class to store a list of ebooks.

```
public class Library {
    private Map<String, Ebook> ebooks = new HashMap<>();

    public void add(Ebook ebook){
        ebooks.put(ebook.getTitle(), ebook);
    }

    public void openEbook(String title){
        ebooks.get(title).show();
    }
}
```

Now, test out.

```
public static void main(String[] args)){
    Library library = new Library();
    String[] books = {
            "Philosopher's Stone"
            ,"Chamber of Secrets"
            ,"Prisoner of Azkaban"
            ,"Goblet of Fire"
            ,"Order of the Phoenix"
            ,"Half-Blood Prince"
            ,"Deathly Hallows"
    };

    for(String title : books){
        library.add(new EbookProxy(title));
    }
    // only loading the selected book
    library.openEbook("Order of the Phoenix");
    library.openEbook("Deathly Hallows");

}
```
```
// Outputs:
Loading ebook Order of the Phoenix
Reading ebook Order of the Phoenix
Loading ebook Deathly Hallows
Reading ebook Deathly Hallows
```

To sum up, the Proxy design pattern introduces a level of indirection when accessing an object. The additional indirection has many uses, depending on the kind of proxy. Also, this pattern can hide from the client as another optimization.