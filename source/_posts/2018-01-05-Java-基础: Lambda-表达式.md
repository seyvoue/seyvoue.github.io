---
title: '@Java 基础: Lambda 表达式'
comments: true
categories: languages
tags: java
abbrlink: 73ff6742
date: 2018-01-05 00:40:50
updated: 2018-02-01 18:37:29
keywords:
description:
---


>- 虽然看着很先进，其实 Lambda 表达式的本质只是一个**"语法糖"**,由编译器推断并帮你转换包装为>规的代码,因此你可以使用更少的代码来实现同样的功能。
>- Lambda 表达式是 Java8 中一个重要的新特性。Lambda 表达式允许你通过表达式来代替功能接口。 Lambda 表达式就和方法一样,它提供了一个正常的参数列表和一个使用这些参数的主体(body,可以是一个表达式或一个代码块)。
>- Lambda 表达式还增强了集合库。 Java8 添加了2个对集合数据进行批量操作的包：`java.util.function` 包以及 `java.util.stream` 包。 **流**(stream)就如同迭代器 `iterator`，但附加了许多额外的功能。 总的来说，lambda 表达式和 stream 是自 Java 语言添加**泛型**(Generics)和**注解**(annotation)以来最大的变化。 

<!--more-->

## Lambda 基础

**为什么使用 Lambda 表达式？**
Lambda 是一个匿名函数，我们可以把 Lambda表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

*从匿名内部类到 Lambda 表达式的转换:*

```java
// 匿名内部类
Runnable r1 = new Runnlable(){
    @Override
    public void run(){
        System.out.println("Hello World!");
    }
};

// Lambda 表达式
Runnable r1 = () -> System.out.println("Hello World!");
```

*Lambda 表达式作为参数传递:*
为了将 Lambda 表达式作为参数传递，接收 Lambda 表达式的参数类型必须是与该 Lambda 表达式兼容的函数式接口的类型。

```java
String newStr = toUpperString(str -> str.toUpperString(), "abcdefg");
System.out.println(newStr);
```

### 函数式接口

> 只包含一个抽象方法的接口，称为函数式接口。

可以通过 Lambda 表达式来创建该接口的对象。（若 Lambda表达式抛出一个受检异常，那么该异常需要在目标接口的抽象方法上进行声明）。

可以在任意函数式接口上使用 `@FunctionalInterface` 注解，这样做可以检查它是否是一个函数式接口，同时 `javadoc` 也会包含一条声明，说明这个接口是一个函数式接口。

```java
@FunctionalInterface
public interface MyFun<T>{
    public T getValue(T t);
}
```

### Labmda 的语法

Lambda 表达式在 Java 语言中引入了一个新的语法元素和操作符。这个操作符为 `->` ， 该操作符被称为Lambda 操作符或箭头操作符。它将 Lambda 分为两个部分：

- 左侧：指定了 Lambda 表达式需要的所有操作符
- 右侧：指定了 Lambda 体，即 Labmda 表达式要执行的功能。

Lambda 表达式返回的是一个对象(所以实现Lambda表达式就是实现了匿名内部类)，他们必须依附于一类特别的对象类型——函数式接口。

**基本语法：**

- (parameters) -> expression
- (parameters) -> { statements; }

**语法格式一：无参，无返回值，Lambda 只有一条语句，花括号可以省略**

```java
Runnable r1 = () -> System.out.println("Hello Lambda!");
```

**语法格式二：一个形参，无返回值**

```java
Consumer<String> fun = (args) -> System.out.println(args);
```

**语法格式三：一个形参，无返回值，参数的小括号可以省略**

```java
Consumer<String> fun = args -> System.out.println(args);
```

**语法格式四：两个形参，有返回值**

```java
BinaryOperator<Long> bo = (Long x, Long y) -> {
    System.out.println("实现函数接口方法。");
    return x+y;
};
```

**语法格式五：形参数据类型可以省略，由编译器根据上下文进行“类型推断”**

```java
BinaryOperator<Long> bo = (x,y) -> {
    System.out.println("实现函数接口方法。");
    return x+y;
};
```

**语法格式六：两个形参，有返回值，Lambda 体只有一条语句，花括号可以省略，`return` 可以省略**

```java
BinaryOperator<Long> bo = (x,y) -> return x+y;

```

> 总结
>- 一个参数，括号可以省
>- 一条语句`{}`可以省
>- 一条语句，`return`可以省
>- 参数类型可以省略

### 实战

lambda表达式可以将我们的代码缩减到一行。

**实战一：遍历 List**

```java
String[] atp = {"Rafael Nadal", "Novak Djokovic",  
       "Stanislas Wawrinka",  
       "David Ferrer","Roger Federer",  
       "Andy Murray","Tomas Berdych",  
       "Juan Martin Del Potro"};  
List<String> players =  Arrays.asList(atp);  
  
// 以前的循环方式  
for (String player : players) {  
     System.out.print(player + "; ");  
}  
  
// 使用 lambda 表达式以及函数操作(functional operation)  
players.forEach((player) -> System.out.print(player + "; "));  
   
// 在 Java 8 中使用双冒号操作符(double colon operator)  
players.forEach(System.out::println);  
```

**实战二：使用 Lambda 表达式来实现 Runnable接口**

```java
// 使用匿名内部类实现
new Thread(new Runnable() {  
    @Override  
    public void run() {  
        System.out.println("Hello world !");  
    }  
}).start();  
  
// 使用 Lambda 表达式
new Thread(() -> System.out.println("Hello world !")).start();  
  
// 使用匿名内部类  
Runnable race1 = new Runnable() {  
    @Override  
    public void run() {  
        System.out.println("Hello world !");  
    }  
};  
  
// 使用 lambda 表达式  
Runnable race2 = () -> System.out.println("Hello world !");  
   
// 直接调用 run 方法(没开新线程哦!)  
race1.run();  
race2.run();  
```

**实战三：使用 Lambda 表达式 和 Stream**

Stream是对集合的包装,通常和lambda一起使用。 使用lambdas可以支持许多操作,如 map, filter, limit, sorted, count, min, max, sum, collect 等等。 同样,Stream使用懒运算,他们并不会真正地读取所有数据,遇到像getFirst() 这样的方法就会结束链式语法。

*下面这个例子中：我们创建了一个`Person`类并使用这个类来添加一些数据到 list 中,将用于进一步流操作。*

- 创建一个 Person 类

```java
public class Person {  
  
private String firstName, lastName, job, gender;  
private int salary, age;  
  
public Person(String firstName, String lastName, String job,  
                String gender, int age, int salary)       {  
          this.firstName = firstName;  
          this.lastName = lastName;  
          this.gender = gender;  
          this.age = age;  
          this.job = job;  
          this.salary = salary;  
}  
// Getter and Setter   
// . . . . .  
}  
```

- 创建两个list,都用来存放Person对象

```java
List<Person> javaProgrammers = new ArrayList<Person>() {  
  {  
    add(new Person("Elsdon", "Jaycob", "Java programmer", "male", 43, 2000));  
    add(new Person("Tamsen", "Brittany", "Java programmer", "female", 23, 1500));  
    add(new Person("Floyd", "Donny", "Java programmer", "male", 33, 1800));  
    add(new Person("Sindy", "Jonie", "Java programmer", "female", 32, 1600));  
    add(new Person("Vere", "Hervey", "Java programmer", "male", 22, 1200));  
    add(new Person("Maude", "Jaimie", "Java programmer", "female", 27, 1900));  
    add(new Person("Shawn", "Randall", "Java programmer", "male", 30, 2300));  
    add(new Person("Jayden", "Corrina", "Java programmer", "female", 35, 1700));  
    add(new Person("Palmer", "Dene", "Java programmer", "male", 33, 2000));  
    add(new Person("Addison", "Pam", "Java programmer", "female", 34, 1300));  
  }  
};  
  
List<Person> phpProgrammers = new ArrayList<Person>() {  
  {  
    add(new Person("Jarrod", "Pace", "PHP programmer", "male", 34, 1550));  
    add(new Person("Clarette", "Cicely", "PHP programmer", "female", 23, 1200));  
    add(new Person("Victor", "Channing", "PHP programmer", "male", 32, 1600));  
    add(new Person("Tori", "Sheryl", "PHP programmer", "female", 21, 1000));  
    add(new Person("Osborne", "Shad", "PHP programmer", "male", 32, 1100));  
    add(new Person("Rosalind", "Layla", "PHP programmer", "female", 25, 1300));  
    add(new Person("Fraser", "Hewie", "PHP programmer", "male", 36, 1100));  
    add(new Person("Quinn", "Tamara", "PHP programmer", "female", 21, 1000));  
    add(new Person("Alvin", "Lance", "PHP programmer", "male", 38, 1600));  
    add(new Person("Evonne", "Shari", "PHP programmer", "female", 40, 1800));  
  }  
};  
```

- 使用forEach方法来迭代输出上述列表:

```java
System.out.println("所有程序员的姓名:");  
javaProgrammers.forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));  
phpProgrammers.forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));  
```

- 过滤器filter() ,让我们显示指定筛选条件的程序员

```java
// 定义 filters  
Predicate<Person> ageFilter = (p) -> (p.getAge() > 25);  
Predicate<Person> salaryFilter = (p) -> (p.getSalary() > 1400);  
Predicate<Person> genderFilter = (p) -> ("female".equals(p.getGender()));  
  
System.out.println("下面是年龄大于 24岁且月薪在$1,400以上的女PHP程序员:");  
phpProgrammers.stream()  
          .filter(ageFilter)  
          .filter(salaryFilter)  
          .filter(genderFilter)  
          .forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));  
  
// 重用filters  
System.out.println("年龄大于 24岁的女性 Java programmers:");  
javaProgrammers.stream()  
          .filter(ageFilter)  
          .filter(genderFilter)  
          .forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));  
```

## 参考

- [Java Lambda表达式入门](http://blog.csdn.net/renfufei/article/details/24600507)


