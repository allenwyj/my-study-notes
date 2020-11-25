# Java

## 简便写法

### If-else statement

Using ternary operator to short if-else statement: → **the ? : operator** 

```java
// example
if (a > b) {
	max = a;
}
else {
	max = b;
}
// same as:
max = (a > b) ? a : b;
```

### Iterating List

```java
for (Student tempStudent : theStudentList) {
	... 
}
```

### Get Random Number

```java
// Math Class
Math.pow(5, 3);
Math.sqrt(64);

// Random Class
import java.util.Random

Random rand = new Random();
int randomNumber = rand.nextInt();
int randomNumberWithBound = rand.nextInt(10); // [0, 9]
```

## Principles

### Cohesion

- **Cohesion** is the indication of the relationship **within** a module. - **High cohesion is good.**
- Low cohesion would mean that the class does a great variety of actions - it is broad, unfocused on what it should do.
- High cohesion means that the class is focused on what it should be doing, i.e. only methods relating to the intention of the class.

### Coupling

- **Coupling** is the indication of the relationships **between** modules. - **Low coupling is good**
- For low coupled classes, changing something major in one class should not affect the other.
- High coupling would make it difficult to change and maintain your code; since classes are closely knit together, making a change could require an entire system revamp.

### Code Tangling 代码纠缠

- 软件系统中的组件可能要同时兼顾几个方面的需 要。所以各个组件包含满足不同的关注点的实现。
- For a given method: addAccount(...)
    - we have logging and security code tabgled in.

### Code Scattering 代码分散

- If we need to change logging or security code
    - we have to update ALL classes.

## **Constructor**

When you don’t define any constructor in your class, compiler defines default one for you, however when you declare any non-default constructor (in your example you have already defined a parameterized constructor), compiler doesn’t do it for you.

## **Array**

```java
Array.get(arrayList, indexNumber); // go to the array, and find the value of the index position.

Arrays.sort(arrayList); // sort the array
```

 

### Difference between Array and ArrayList

Array: Simple **fixed sized arrays** that we create in Java, like below
      int arr[] = new int[10]   
ArrayList : **Dynamic sized arrays** in Java that implement List interface.
      ArrayList<Type> arrL = new ArrayList<Type>();

- Array cannot add more elements once it has been created, but ArrayList can.
- **Array** can contain both **Primitive** and **Object** type, but **ArrayList** can only contain **Object** type.
- **Note**: arrayList.add(1) → it converts primitive data int 1 to integer object 1.

- **Storing in Memory**:

    Members of **ArrayList** are always references to objects at different memory locations,  therefore, **the actual objects** are never stored at contiguous locations. **References of the actual objects** are stored at contiguous locations.

    In Array, primitive types → actual values are contiguous locations, 

    objects type → allocation is similar to ArrayList.

## **Inheritance**

you can overwrite or customise classes.

- Advantage:
    - Minimise duplication of code
- Disadvantage:
    - Superclass and subclass can be tightly coupled
    - Increased effort to jump through levels of abstraction to get appropriate functionality.

```java
// Using extends keyword.
Class xxx extends xxxx {
	public Spider(int age, boolean numberOfLegs) {
	super(age, numberOfLegs); // super class constructor
	}
	// child class creates the same class name will 
	// overwrite the parent class method.

}
```

- 继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。
- 父类中的private和构造器是子类不能继承的。构造器本身只可被调用，无法被继承。如需使用父类的构造方法，则需使用super().
- 构建过程是从父类开始向子类一级一级地完成构建。而且我们并没有显示的引用父类的构造器，这就是java的聪明之处：**编译器会默认给子类调用父类的构造器**。但是，这个默认调用父类的构造器是有**前提**的：**父类有默认构造器**。如果父类没有默认构造器，我们就要必须显示的使用super()来调用父类构造器，否则编译器会报错：无法找到符合父类形式的构造器。
- 对于继承而已，子类会默认调用父类的构造器，但是如果没有默认的父类构造器，子类必须要显示的指定父类的构造器，而且必须是在子类构造器中做的第一件事(第一行代码)。
- 子类拥有父类非private的属性和方法。
- 子类可以拥有自己属性和方法，即子类可以对父类进行扩展
    - 只有当子类重写了父类的方法或属性时，如果需要用到父类的方法或属性时，才要用super，表明这个方法时父类的方法或属性不是子类的方法或属性。

[java提高篇(二)-----理解java的三大特性之继承](https://www.cnblogs.com/chenssy/p/3354884.html)

[使用父类方法一定要用super吗（写给新人）_Java_doye_chen的博客-CSDN博客](https://blog.csdn.net/doye_chen/article/details/78887382)

[【java基础】--java中父类声明子类实例化_Java_汤庆-CSDN博客](https://blog.csdn.net/weixin_40449300/article/details/84558692)

## 多态 Polymorphism

- 向上转型 Upcasting
- 是使用父类类型的引用指向子类的对象， e.g. Father child = new child();
- 该引用只能调用父类中定义的方法和变量，若子类中有但父类中没有的方法则不可以被调用。
- 如果子类中重写（override）了父类中的一个方法，那么在调用这个方法的时候，将会调用子类中的这个方法（动态连接，动态调用）。子类中的重载（overload）并不能被调用。
    - 当子类重写父类的方法被调用时，只有对象继承链中的最末端的方法才会被调用。
- 变量不能被重写（覆盖），“重写”的概念只针对方法
- 从父类或是从子类中调用成员变量只与声明的引用变量类型有关，与实例化(instantiate)的类型无关。在父类方法被子类重写的情况下，调用方法只与变量的实例化的类型有关，与声明的类型无关。若无重写，则调用的方法为父类中的方法。
- **当 超类对象(Father) 的 引用变量(father) 引用了 子类对象(new Son()) 时，被引用对象的类型(new Son()) 而不是 引用变量的类型(Father) 决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法，但仍需遵从继承链中方法调用的优先级。**
- 子类 → 超类 → Object
- **继承链中，对象方法的调用存在一个优先级： this.show(O), super.show(O), this.show((super)O), super.show((super)O)； Object除外**
    - this, super 指的是引用变量的类型
    - O, (super)O 指的是参数的类和它的超类。
    - **例子：A a2 = new B(); B b = new B(); C c = new C(); D d = new d();**
    - 首先我们分析5，a2.show(c)，a2是A类型的引用变量，所以**this**就代表了A，a2.show(c),它在A类中找发现没有找到，于是到A的超类中找(super)，由于A没有超类（Object除外），所以跳到第三级，也就是this.show((super)O)，C的超类有B、A，所以(super)O为B、A，this同样是A，这里在A中找到了show(A obj)，**同时由于a2是B类的一个引用且B类重写了show(A obj)，因此最终会调用子类B类的show(A obj)方法**，结果也就是B and A。

[java提高篇(四)-----理解java的三大特性之多态](https://www.cnblogs.com/chenssy/p/3372798.html)

## Interfaces

- Interfaces are a way to enforce certain fields or methods on a class.
- Interfaces do not enforce exactly how these methods should be implemented - just that you must have them.
- The differences between an interface and a regular class is that in **an interface** you **can not implement** any of the declared methods. Only the class that "implements" the interface can implement the methods. The C++ equivalent of an interface would be an abstract class (not **EXACTLY** the same but pretty much).
- Java doesn't support **multiple** **inheritance** for classes. This is **solved by using multiple interfaces**.

[https://blog.csdn.net/weixin_39938767/article/details/80056922](https://blog.csdn.net/weixin_39938767/article/details/80056922)

```java
public interface Bread {
	public void eatBread(); // the method play() has to be implemented in the subclass.
}

public interface Milk() {
	public void eatMilk();
}

public class BreadImpl implements Bread {
	public void eatBread() {
		// it has to be in this class because interfaces.
		// ...
	}
}

public class MilkImpl implements Milk {
	public void eatMilk() {
		// it has to be in this class because interfaces.
		// ...
	}
}

// OR
public class Impl implements Bread, Milk {
	public void eatBread() {
		// it has to be in this class because interfaces.
		// ...
	}

	public void eatMilk() {
		// it has to be in this class because interfaces.
		// ...
	}
}

// in the main Class, we can do a guard to make sure codes working properly
MilkImpl m = new MilkImpl();
if (m instanceof Milk) {
	m.methodName();
}
```

**NOTE:**

**extends** is for *extending* a class.

**implements** is for *implementing* an interface.

## Functional Programming

- Functional programming focuses on computing results from functions, rather than performing actions on objects.

### **Lambda functions**

- Lambda functions are anonymous functions that you can create in Java without the usual code overhead.
- A great tool if you need a quick function for a calculation in the code.
- Usually have a single purpose and do not affect other objects.

```java
public interface Answerable {
	String answer();
}

public interface Predicate {
	boolean test(Int n);
}

public class Main {

	public static void main(String[] args) {
		Answerable phone = () -> {return "Hello";}; // 0 input, return String
		System.out.println(phone.answer()); // result: Hello

		Predicate isOdd = n -> n % 2 != 0; // n input, return boolean
		System.out.println(isOdd.test(2)); // result: false

		Predicate isEven = n -> n % 2 == 0;
		System.out.println(isEven.test(2)); // result: true
	}

}

```