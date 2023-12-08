---
layout: post
title:  "[Software Engineering]Design and Implementation"
date:   2023-12-7
excerpt: "The Software Design and Implementation."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## Overviews

**Software design and implementation** is the stage in the software engineering process at which **an executable software system is developed**. 

Software **design** and **implementation** activities are **invariably inter-leaved**. 
* identify software components and their relationships, based on a customer's requirements.
* Implementation is the process of realizing the design as a program.

**Question: Build or Buy?**

In a wide range of domains, it is now possible to buy off-the-shelf systems (COTS) that can be adapted and tailored to the users’ requirements. When you develop an application in this way, the design process becomes concerned with **how to use the configuration features of that system to deliver the system requirements.** 


## 1. Object-oriented design using the UML

### 1.1 Object-oriented Design Process

**Structured object-oriented design processes** involve **developing a number of different system models**.

They require a lot of effort for **development and maintenance** of these models and, for small systems, this may not be **cost-effective**. However, for large systems developed by different groups design models are **an important communication mechanism**.

### 1.2 Process Stages

There are **variety of different object-oriented design processes** that depend on the organization using the process. Common activities in these processes include:
* define the context and modes of use of the system
* design the system architecture
* identify the principal system objects
* develop design models
* specify object interfaces

#### 1.2.1 System Context and Interactions

**Understanding the relationships between the software that is being designed and its external environment** is essential:
* for deciding how to provide the required system functionality and how to structure the system to communicate with its environment. 
* establish the boundaries of the system. **Setting the system boundaries** helps you decide what features are implemented in the system being designed and what features are in other associated systems. 

**A system context model** is **a structural model** that demonstrates the other systems in the environment of the system being developed.

**An interaction model** is **a dynamic model** that shows how the system interacts with its environment as it is used.


#### 1.2.2 Architectural Design

**Once interactions between the system and its environment have been understood**, you use this information for designing the system architecture.
* identify the major components that make up the system and their interactions.
* organize the components using an architectural pattern such as a layered or client-server mode.

#### 1.2.3 Object class Identification

Identifying object classes is often a difficult part of object-oriented design.
* no magic-formula for object identification
* an iterative process

Approaches to Identification:

(1) Use **a grammatical approach** based on a natural language description of the system.

(2) Base **the identification on tangible things** in the application domain.

(3) Use a **behavioural approach** and identify objects based on what participates in what behaviour.

(4) Use a **scenario-based analysis**. The objects, attributes and methods in each scenario are identified.

#### 1.2.4 Design Models

**Design models** show the objects and object classes and r**elationships between these entities**.

(1) Structural models describe the **static structure of the system** in terms of object classes and relationships.

(2) Dynamic models describe **the dynamic interactions** between objects.

The Design Model Examples:

* Subsystem Model: show how the design is organised into logically related groups of objects.
    * an encapsulation construct is a logical model. **packages**.
* Sequence Models: show the sequence of object interactions.
    * objects are arranged horizontally across the top
    * Time is represented vertically so models are read top to bottom
* State Machine Models: show how individual objects change their state in response to events.
    *  useful high-level models of a system or an object’s run-time behavior. 
    *  don’t usually need a state diagram for all of the objects in the system. 
* Use-case Models, Aggregation Models, Generalisation Models.

#### 1.2.5 Interface  Specification

**Object interfaces** have to be specified so that the **objects and other components can be designed in parallel.**

Designers should avoid **designing the interface representation** but should hide this in the object itself.

Objects may have several interfaces which are viewpoints on the methods provided.

The UML uses class diagrams  for interface specification but Java may also be used.



## 2. Design Patterns

### 2.1 Conception

Pattern: Each pattern **describes a problem which occurs over and over again** in our environment, and then **describes the core of the solution to that problem**, in such a way that you can use this solution a million times over, without ever doing it the same way twice.

* Design patterns capture **the best practices of experienced object-oriented software developers**. 
* Design patterns are solutions to general software development problems. 

Benefits:
* **Speed up the development process** by providing well tested, proven development/design paradigm.
* Common Language with other programmer.
* Code Reusability
* Highly Maintainability
* Loosely coupled application

Design Pattern Description:
* Template, not Solution
* Language Independent
* Implemented in different ways


Four Essential Elements of a Design Pattern:
* Name: a common vocabulary 
* Problem: particular design issue or problem
* Solution: the basic elements providing the solution to the problem in terms of: structure, participants, collaborations.
* Consequences: the results and trade-offs

Design Pattern Types:
* Creational
    * objective: deals with complexity of creating objects. Create objects for you, rather than having you instantiate objects directly.
* Structural 
    * objective: **deals with the composition of the classes or objects**. Concern how classes and objects can be combined to form large structures.
* Behavioural
    * objective: More focused with the interaction and communication between objects.

### 2.2 Creational Pattern

The creational patterns deal with the best way to create instances of objects.

 Abstracting the creation process into a special “creator” class can make your program more **flexible** and **general**.

#### (1) Singleton

The Singleton:
* **Ensure that only one instance of a class** is created. 
* Provide **a global point of access to the object**.
* Allow multiple instances in the future without affecting a singleton class' clients.


The implementation by java.
```
public class SingleObject{
    private static SingleObject instance = null;
    private SingleObject(){}

    public static SingleObject getInstance(){
        if(instance == null)
            instance = new SingleObject();
        return instance;
    }

    public void ShowMessage(){
        System.out.printIn("xxx");
    }
}
```

### 2.3 Structural Pattern

#### (1) Adapter

Like any adapter in the real world it is used to be an interface, **a bridge between two objects that have the same functionality**, but that are to be used in **a different manner**, i.e. have a different specification to their interfaces. 

**The same concept applies to software components**. You may have some class expecting some type of object and you have an object **offering the same features but exposing a different interface**. You **don't want to change existing classes**, so you want to create an adapter


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_a.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_a.png" align="center"></a>
    <figcaption>The adapter pattern structure.</figcaption>
</figure>

The classes roles:
* **Target**:  defines **the domain-specific interface** that Client uses.
* **Adapter**: adapts **the interface Adaptee** to the Target interface.
* **Adaptee**: defines **an existing interface** that needs adapting.
* **Client**: **collaborates with objects** conforming to the Target interface.

When you have **a class (Target)** that **invokes methods defined in an interface** and you have **another class (Adaptee)** that doesn't **implement the interface but implements** the operations that should be invoked from the first class through the interface. 

```
public class SquarePeg {
  /**
   * This is the interface that the client class knows about Pegs  
   */    
  public void insert(String str) {
    System.out.println("SquarePeg insert(): " + str);
  }
}

public class RoundPeg {
  /**
   * The client does not know this interface, though
   * it provides the service that the client wants.  
   */
  public void insertIntoHole(String msg) {
    System.out.println("RoundPeg insertIntoHole(): " + msg);
  }
}

public class SquareToRoundPegAdapter extends SquarePeg {
  /**
   * The adapter contains the RoundPeg Adaptee
   */
  private RoundPeg roundPeg; 
  /**
   * Upon creation, the Adapter is plugged into the RoundPeg Adaptee
   */
  public SquareToRoundPegAdapter(RoundPeg peg) {
    //the roundPeg is plugged into the adapter
    this.roundPeg = peg;
   }
  /**
   * The Adapter provides the Target's method and translates it
   * to the corresponding Adaptee's method call.  
   */
  public void insert(String str) {
    //the roundPeg can now be inserted in the same manner as a squarePeg!
    roundPeg.insertIntoHole(str);
    }
}

public class AdapterDriver{
  public static void main(String args[]) {

    // Create some pegs.
    RoundPeg roundPeg = new RoundPeg();
    SquarePeg squarePeg = new SquarePeg();

    // Do an insert using the square peg (standard).
    squarePeg.insert("I am a SquarePeg in a square hole.");

    // Now we'd like to do an insert using the round peg.
    // But this client only understands the insert()
    // method of pegs, not a insertIntoHole() method.
    // The solution: create an adapter that adapts
    // a square peg to a round peg!

    SquarePeg wannabeRound = new SquareToRoundPegAdapter(roundPeg);
    wannabeRound.insert("I am a SquarePeg in a round hole!");}
}

```

#### (2) Proxy

Proxy: a structural design pattern that **lets you provide a substitute or placeholder** for another object.

Solution:
* Create a proxy object that **implements the same interface** as the real object.
* The proxy object(usually) contains **a reference to real object**
* Clients are **given a reference to the proxy** not the real object
* **All client operations on the objects pass through the Proxy**, allowing the Proxy to **perform additional processing**
* **Proxies are useful** wherever there is **a need** for **a more sophisticated reference to an object than a simple pointer** or simple reference can provide.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_p.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_p.png" align="center"></a>
    <figcaption>The proxy pattern structure.</figcaption>
</figure>

The implementation:

```
public interface Subject { 
    void doIt(); 
}

public class RealSubject implements Subject { public RealSubject(…) { ….} 
    @Override 
    public void doIt() { 
        System.out.println(…);
    }
}

public class Proxy implements Subject{ 
private RealSubject wrapee; 
    @Override 
    public void doIt() 
    {    
        if (wrapee == null) { 
            wrapee = new RealSubject(…); 
        } 
        wrapee.doIt(); 
    }
}
```

### 2.4 Behavioral Pattern

#### (1) Chain of Responsibility

Avoid **coupling sender of request to its receiver** by giving more than one object chance to handle request. Chain receiving objects and pass request along until an object handles it.

Description:
* **Decouple senders and receivers** by **giving multiple objects** (in a set order) a chance to handle a request. Request passed until an object handles it. 
* 1st object in chain receives request – **either handles it or forwards to next object in chain**.

Object that makes request has **no explicit knowledge of who will handle it** – request has an implicit receiver. 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_cr.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_cr.png" align="center"></a>
    <figcaption>The chain of responsibility.</figcaption>
</figure>

The class roles:
* **Handler**: defines interface for **handling requests**. Can also implement **successor link**.
* **ConcreteHandler**: handles requests it is responsible for; otherwise **forwards requests to successor**.
* Client: **initiates request to a ConcreteHandler** in the chain.

Use Chain of Responsibility when:

(1) More than 1 object may handle a request, and handle isn’t known beforehand.

(2) Want to issue request to one of several objects without specifying receiver explicitly.

(3) Set of objects that can handle a request should be specified dynamically.

Benefist:
* Decoupling of senders and receivers
* Added flexibility
* Sender doesn't need to know specifically who the handlers are.

Drawback:
* Client can’t explicitly **specify who handles a request**
* **No guarantee of request being handled** (request falls off end of chain)

Implementation:

```
import java.util.Random;

class Handler {
    private final static Random RANDOM = new Random();
    private static int nextID = 1;
    private int id = nextID++;
    private Handler nextInChain;

    public void add(Handler next) {
        if (nextInChain == null) {
            nextInChain = next;
        } else {
            nextInChain.add(next);
        }
    }

    public void wrapAround(Handler root) {
        if (nextInChain == null) {
            nextInChain = root;
        } else {
            nextInChain.wrapAround(root);
        }
    }

    public void execute(int num) {
        if (RANDOM.nextInt(4) != 0) {
            System.out.println("   " + id + "-busy  ");
            nextInChain.execute(num);
        } else {
            System.out.println(id + "-handled-" + num);
        }
    }
}


public class ChainDemo {
    public static void main(String[] args) {
        Handler rootChain = new Handler();
        rootChain.add(new Handler());
        rootChain.add(new Handler());
        rootChain.add(new Handler());
        rootChain.wrapAround(rootChain);
        for (int i = 1; i < 6; i++) {
            System.out.println("Operation #" + i + ":");
            rootChain.execute(i);
            System.out.println();
        }
    }
}
```


#### (2) Observer

Observer Description:

**Separates the display of the state of an object from the object itself and allows alternative displays to be provided**. When the object state changes, all displays are automatically notified and updated to reflect the change. 

Problem Description:

This pattern may be used in all situations where **more than one display format for state information is required** and where it is not necessary for the object that **maintains the state information to know about the specific display formats used**.


Solution Description:

This involves two abstract objects, **Subject and Observer**, and **two concrete objects, ConcreteSubject and ConcreteObserver**, which inherit the attributes of the related abstract objects. The abstract objects include general operations that are applicable in all situations. The state to be displayed is maintained in ConcreteSubject, which inherits operations from Subject allowing it to add and remove Observers (each observer corresponds to a display) and to issue a notification when the state has changed.

Consequence:

The **subject** only knows **the abstract Observer and does not know details of the concrete class**. Therefore there is **minimal coupling between these objects**. Because of this lack of knowledge, **optimizations that enhance display performance are impractical**.

The class roles:
* Subject: **interface or abstract class defining the operations for attaching and de-attaching observers to the client**. It is often referred to as **“Observable”**.
* ConcreteSubject: concrete Subject class. It **maintain the state of the observed object and when a change in its state occurs it notifies the attached Observers**. 
* Observer:interface or abstract class **defining the operations to be used to notify the registered Observer objects**.
* ConcreteObserver: concrete Observer subclasses that are attached to a particular Subject class. There may be different concrete observers attached to a single Subject that will **provide a different view of that Subject**. 


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_o.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/dp_o.png" align="center"></a>
    <figcaption>The observer pattern.</figcaption>
</figure>

The implementation:

```

public interface Channel {
    public void update(Object o);
}

public class NewsChannel implements Channel {

    private String news;

    @Override
    public void update(Object news) {
        this.setNews((String) news);
    }

    public String getNews() {
        return news;
    }

    public void setNews(String news) {
        this.news = news;
    }

}

public class NewsAgency {
    private String news;
    private List<Channel> channels = new ArrayList<>();

    public void addObserver(Channel channel) {
        this.channels.add(channel);
    }

    public void removeObserver(Channel channel) {
        this.channels.remove(channel);
    }

    public void setNews(String news) {
        this.news = news;
        for (Channel channel : this.channels) {
            channel.update(this.news);
        }
    }
}

public class NewsAgencyTest {

    @Test
    public void whenChangingNewsAgencyState_thenNewsChannelNotified() {

        NewsAgency observable = new NewsAgency();
        NewsChannel observer = new NewsChannel();

        observable.addObserver(observer);

        observable.setNews("news");
        assertEquals(observer.getNews(), "news");
    }   
}
```








