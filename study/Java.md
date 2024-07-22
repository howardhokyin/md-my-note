# Java

## Method

> private void

> private

> static -> can call without object

> non-static method ->

## Node

1.  Hold itself value

2.  Hold next node value

```java
public class Node {
    private int data;
    private Node next;

    public Node(int data, Node next) {
        this.data = data;
        this.next = next;
    }

    public int getNode() { return data;}
    public Node getNext() {return next;}
    public void setData(int d) { data = d; }
    public void setNext(Node n) { next = n; }
}

```

## Generic
