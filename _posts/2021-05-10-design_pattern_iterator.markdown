---
layout: post
title:      "Design Pattern: Iterator"
date:       2021-05-10 04:40:22 -0400
description: 'The iterator design pattern is a way to access the elements of a different collection sequentially without exposing its underlying representation'
permalink:  design_pattern_iterator
---

The iterator design pattern is a way to access the elements of a different collection sequentially without exposing its underlying representation(list, stack, tree, etc).

The problem is that collections(list, stack, tree, or complex data structures) have their methods to access the data, such as ArrayList has get() or stack has pop(). Since those different ways to get data, we need to create similar loops to iterate for each data structure. 

The Iterator pattern is a solution to those situations:

* traverse the list in different ways, depending on what you want to accomplish.
* give you a way to access its elements without exposing its internal structure.

Now, Let's apply the iterator design pattern to iterate stored data from an arrayList and a stack. 

First, create an interface Collection for creating an Iterator object.

```
public interface Collection<T> {
    Iterator<T> getIterator();

    void add(String a);
}
```

Second, create an interface Iterator. 

Iterator
```
public interface Iterator<T> {
    boolean hasNext();
    void next();
    T current();
}

```

Third, create two classes SomeList and SomeStack with a nested class for own iteration.

```
public class SomeList implements Collection {
    private List<String> list;

    public SomeList(){
        list = new ArrayList<String>();
    }

    @Override
    public void add(String content){
        list.add(content);
    }

    @Override
    public Iterator getIterator() {
        return new ListIterator(this);
    }

    private String getElement(int i){
        return this.list.get(i);
    }

    private int size(){
        return this.list.size();
    }

    public class ListIterator implements Iterator<String>{

        private int index;
        private SomeList list;

        public ListIterator(SomeList list){
            this.list = list;
            index = 0;
        }

        @Override
        public boolean hasNext() {
            return index < list.size();
        }

        @Override
        public void next() {
            index++;
        }

        @Override
        public String current() {
            return list.getElement(index);
        }
    }
}
```

```
public class SomeStack implements Collection {
    Stack<String> stack;

    public SomeStack(){
        stack = new Stack<String>();
    }

    @Override
    public Iterator getIterator() {
        return new StackIterator(this);
    }

    @Override
    public void add(String content) {
        stack.push(content);
    }

    private String getCurrent(){
        return stack.peek();
    }

    private boolean isEmpty(){
        return stack.empty();
    }

    private void next(){
        stack.pop();
    }

    public class StackIterator implements Iterator{
        SomeStack stack;

        public StackIterator(SomeStack stack){
            this.stack = stack;
        }

        @Override
        public boolean hasNext() {
            return !this.stack.isEmpty();
        }

        @Override
        public void next() {
            this.stack.next();
        }

        @Override
        public String current() {
            return this.stack.getCurrent();
        }
    }
}
```

Now, test out.

```
  public static void main(String[] args){
        Collection list = new SomeList();
        list.add("a in List");
        list.add("b in List");
        list.add("c in List");
        printAll(list.getIterator());
        SomeStack stack = new SomeStack();
        stack.add("1 in stack");
        stack.add("2 in stack");
        stack.add("3 in stack");
        printAll(stack.getIterator());
    }

    public static void printAll(Iterator it){
        Iterator<String> iterator = it;
        while(iterator.hasNext()){
            System.out.println(iterator.current());
            iterator.next();
        }
    }
```

To sum up, the Iterator design pattern is a simple design pattern, which we use almost in every project. This pattern allows collections to iterate in parallel. Also, it makes code clear and reusable. However, like other design patterns, this pattern can be overused in situations, like applying if your app only works with simple collections or using an iterator may be less efficient than going through elements of some specialized collections directly.