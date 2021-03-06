---
title: Initialization order of inheritance
classes: wide
categories:
  - Blog
---

Inheritance is one of the most important features of Java. A subclass can inherit methods, attributes (member variables) from its parent class. When parent class and sub class both have static code block, constructing code block and default constructor, If we instantiate a subclass object, what is the order of execution of the code and the order of initialization of the subclass object? We all know that method of parent class can be overridden in subclass, but the constructor of parent class is not allowed to be overridden or inherited, what is meaning of the existence of the default constructor method of the parent class? 

By creating a parent class Animal, a subclass Cat inherits Animal, and adding static an constructing code blocks to both subclass and the parent class, instantiate a Cat object in the Test class and observe the initialization process through the breakpointn debugging method. Also to reflect the process more visually, print a statement in the constructor of both parent class and subclass.

```java
public class Animal{
  private String name = "Tom";
  private int month = 2;
  private static int st1 = 11;
  private static int st2 = 22;
  public int temp = 1
    
  static{
    System.out.println("Parent class static code block");
  }  
  {
    System.out.println("Parent class constructing code block");
  }
  public Animal(){
    System.out.println("Parent class default constructor");
  }
}
```
>Creat parent class Animal. Creat static member variables st1,st2 and temp to show the calling order.

```java
public class Cat extends Animal{
  private double weight;
  
  public static int st3 = 33;
  
  static{
    System.out.println("Subclass static code block");
  }
  {
    System.out.println("Subclass constructing code block");
  }
  public Cat(){
    System.out.println("Subclass default constructor");
  }
}
```

> Creat subclass Cat

```java
public class Test{
  public static void main(String[] args){
    Cat one = new Cat();//Set break point here
    System.out.println(one.temp)
  }
}
```



![](/assets/images/first.png)

![](/assets/images/2021_06_11/02.png)

> The first priority is to load static members of parent class, including static code blocks and static memebr variables

![](/assets/images/2021_06_11/03.png)

![](/assets/images/2021_06_11/04.png)

> Then load the static members of the subclasses

![](/assets/images/2021_06_11/05.png)

![](/assets/images/2021_06_11/06.png)

> Back to main method for instantiation, but it doesn't load the constructor of subclass directly, it first goes to the parent class's unparametric constructor.

![](/assets/images/2021_06_11/07.png)

![](/assets/images/2021_06_11/08.png)

> The top parent class in Java is object class, all classes are subclasses of object class. When enter the parent class, it will enter object class first, but it doesn't show up in IDEA, then back to parent class. First the member variables, then constructing code block, finally the constructor.

![](/assets/images/2021_06_11/09.png)

> Back to Cat subclass, first load constructing code block,  then constructor.

![](/assets/images/2021_06_11/10.png)

> Finish object instantiation, print member property.

By single-step debugging with breakpoint, when the inheritance conditions are satisfied, the initialization order of subclasses is: first load parent class's static members, then subclass's static members, then parent class's constructor, finally is subclass's constructor. When loading static members, the loading only according to the writing order of the members, and the access modifier doesn't affect the loading order of the members.

If the constructor if subclass has parameters, by default, still call the parent class's unparameteric constructor, so the parent's unparameteric constructor is very important and it will affect the process of instantiating subclass objects. If you wish to call the specified constructor of the parent class, you can call it with the super keyword, but `super()` must be placed on the first lin of the valid code of the constructor.
