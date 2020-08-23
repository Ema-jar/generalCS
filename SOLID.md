## SOLID 

Solid is an acronym used to remind five principles in software architecture.
Solid principles are:

### Single responsibility principle 

A class should only have one single responsibility defined by the model that we are trying to implement.

A class should ideally be small and do as few things as possible in order to reduce confusion and redundancy.

Another very important aspect related to this principle is __Cohesion__ of a class.
We say that a class has _Low cohesion_ if it does many things without being focused on the goal fo the class.

The following is an example of low cohesion
```
-------------------
| Staff           |
-------------------
| checkEmail()    |
| sendEmail()     |
| emailValidate() |
| PrintLetter()   |
-------------------
```

We say that a class has _High cohesion_ if it is extremely focused on one single task, hopefully described by the class name.

The following is an example of high cohesion:

```
----------------------------
| Staff                   |
----------------------------
| -salary                 |
| -emailAddr              |
----------------------------
| setSalary(newSalary)    |
| getSalary()             |
| setEmailAddr(newEmail)  |
| getEmailAddr()          |
----------------------------
```

It's even important to talk about __coupling__. We say that two classes, A and B, are _strongly coupled_ if modifying A impacts B and vice versa.

Our software should be high in cohesion and low in coupling.

### Open closed principle

In OO software we should build modules (classes, functions) that are open to extension and close to modification.

This is an example of bad application of OCP:

```
public class Addition implements CalculatorOperation {
    private double left;
    private double right;
    private double result = 0.0;
 
    public Addition(double left, double right) {
        this.left = left;
        this.right = right;
    }
}

public class Subtraction implements CalculatorOperation {
    private double left;
    private double right;
    private double result = 0.0;
 
    public Subtraction(double left, double right) {
        this.left = left;
        this.right = right;
    }
}

public class Calculator {
 
    public void calculate(CalculatorOperation operation) {
        if (operation == null) {
            throw new InvalidParameterException("Can not perform operation");
        }
 
        if (operation instanceof Addition) {
            Addition addition = (Addition) operation;
            addition.setResult(addition.getLeft() + addition.getRight());
        } else if (operation instanceof Subtraction) {
            Subtraction subtraction = (Subtraction) operation;
            subtraction.setResult(subtraction.getLeft() - subtraction.getRight());
        }
    }
}
```

As we can se there are two different operation classes (Addition and Subtraction) that implement `CalculatorOperation` in order to be passed to the `calculate()` method.

The `calculate()` method has to check the type of the object in order to perform the right calculation.

Of course this is not a good approach because we are forced to modify the `calculate()` method every time we add a new operation. This impact our code because we can introduce bugs.
In this case we even perform an explicit cast which is not a best practice.

This is what we can do to fix this code and make it OCP compliant.

```
public interface CalculatorOperation {
    void perform();
}

public class Addition implements CalculatorOperation {
    private double left;
    private double right;
    private double result;
 
    // constructor, getters and setters
 
    @Override
    public void perform() {
        result = left + right;
    }
}

public class Division implements CalculatorOperation {
    private double left;
    private double right;
    private double result;
 
    // constructor, getters and setters
    @Override
    public void perform() {
        if (right != 0) {
            result = left / right;
        }
    }
}

public class Calculator {
 
    public void calculate(CalculatorOperation operation) {
        if (operation == null) {
            throw new InvalidParameterException("Cannot perform operation");
        }
        operation.perform();
    }
}
```

Here we can see that the `CalculatorOperation` interface defines a method that is implemented by the two operations.

At this point it's easy to implement and _extend_ the `Calculator` because we don't need a dedicated check for every new operation.

### Liskov substitution principle

The LSP, in simple terms, states that objects of the same superclass should be able to be swapped with each other without breaking anything.

If we have a `Cat` and a `Dog` class derived from an `Animal` class, any functions using the `Animal` class should be able to use `Cat` or `Dog` and behave normally.


### Interface segregation principle

No client should be forced to depend on methods it does not use.

When we create interfaces for our classes we tend to grow those interfaces to the point that it creates a lot of boilerplate.

This is an example of what I'm talking about:

```
public interface CoffeeMachine {
    CoffeeDrink brewFilterCoffee() throws CoffeeException;
    CoffeeDrink brewEspressoCoffee() throws CoffeeException;
}

public class EspressoMachine implements CoffeeMachine {

    @Override
    public CoffeeDrink brewEspressoCoffee() throws CoffeeException {
       return new EspressoCoffee();
    }

    @Override
    public CoffeeDrink brewFilterCoffee() throws CoffeeException {
       throw new CoffeeException("This machine only brew espresso.");
    }
}
```

As we can see the method `brewFilterCoffee()` implemented inside the `EspressoMachine` throws an exception when it's called because an espresso machine is not supposed to brew filter coffee.

This is a clear violation on the ISP. The interface is already too big and the method `brewEspressoCoffee()` should belong to a different interface otherwise all the implementors will be __forced__ to implement a method that they don't use.


### Dependency inversion principle

The DIP is divided in two parts:

 - High-level modules should not depend on low-level modules. Both should depend on abstractions.
 - Abstractions should not depend on details. Details should depend on abstractions.

Let's try to show a bad example of DIP:

```
public class BackEndDeveloper {
    public void writeJava() { // do stuff in Java }
}

public class FrontEndDeveloper {
    public void writeJavascript() { // do stuff in JavaScript }
}

public class Project {
    private BackEndDeveloper backEndDeveloper = new BackEndDeveloper();
    private FrontEndDeveloper frontEndDeveloper = new FrontEndDeveloper();
    public void implement() {
        backEndDeveloper.writeJava();
        frontEndDeveloper.writeJavascript();
    }
}
```

In this case we can see that the `Project` is a high level module depending on two low level modules. This clearly violates the DIP principle.
It's even clear that we create a `new BackendDeveloper()` and a `new FrontEndDeveloper()`, so we are depending on __concrete__ entities. This is not a good approach because it forces us to modify the `Project` class if a new developer type is introduced. So, this could lead to a violation of the OC principle as well.

But what about the second point? Are our abstractions depending on details?

Let's take a closer look at the two classes `BackEndDeveloper` and `FrontEndDeveloper`. They respectively implement a `writeJava()` and a `writeJavaScript()` method. This is not good, if our backend developer decides to switch to _Kotlin_ we would have to modify the class `Project`, again, a violation of the OC principle.

To solve this problem we should focus on what a developer does: a developer develops software.

This is a common behavior for all the developers, so we can model this behavior using an interface. In this case our code would become:


```
public interface Developer {
    void develop();
}

public class BackEndDeveloper implements Developer {
    @Override
    public void develop() {
        writeJava();
    }
    private void writeJava() { // do stuff in Java }
}

public class FrontEndDeveloper implements Developer {
    @Override
    public void develop() {
        writeJavaScript();
    }
    private void writeJavaScript() { // do stuff in JavaScript }
}
```

As we can see all the classes that implement the `Developer` interface have to implement a `develop()` method and this is _the only_ things that external classes have to know when they interact with a developer.

At this point our class `Product` can become something like that:

```
public class Project {
    private List<Developer> developers;
    public Project(List<Developer> developers) {
        this.developers = developers;
    }
    public void implement() {
        developers.forEach(d->d.develop());
    }
}
```

This makes completely sense from a OO perspective because a project can be developed by a team of backend engineers or by a cross functional team composed by frontend and backend developers. It doesn't matter at all, the only important thing is that all the developers expose one single method called `develop()`.

Extend the `Project` class is extremely easy, you just have to pass the right list of developers you want to put on a project. This class is closed to modification and open to extension.