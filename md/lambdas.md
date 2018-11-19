# Lambdas

-
## What is it?
* Lambda expressions are used to perform computation based on function abstraction and application.
* Lambda expressions denote _method-invokation deference_.

-
## What is the purpose?
* Lambda expression can be passed as a parameter in a method.
* We treat code in Lambda expressions as data.
	* This piece of code can be passed to other objects and methods.

-
# When is this used?
* A method reference or lambda expression can be used to abstract any logic with dynamic _functionalities_ rather than _values_.


-
## What is a `FunctionalInterface`?
* A functional interface is an interface that contains only one abstract method.
* Lambda expressions can be used to represent the instance of a functional interface.
* The `@FunctionalInterface` annotation explicitly denotes that
	* this interface can only declare a single method-signature
	* this interface can be expressed as a lambda.

-
## Syntax
<img src = "https://cdncontribute.geeksforgeeks.org/wp-content/uploads/lambda-expression.jpg">

-
## Types of Functional Interfaces
<table>
  <tr>
    <th>Name<br></th>
    <th>Has First Argument<br></th>
    <th>Has Second Argument</th>
    <th>Has Return-Type</th>
  </tr>
  <tr>
    <td>Runnable<br></td>
    <td>no<br></td>
    <td>no</td>
    <td>no</td>
  </tr>
  <tr>
    <td>Consumer<br></td>
    <td>yes<br></td>
    <td>no<br></td>
    <td>no</td>
  </tr>
  <tr>
    <td>BiConsumer<br></td>
    <td>yes<br></td>
    <td>yes<br></td>
    <td>no<br></td>
  </tr>
  <tr>
    <td>Supplier</td>
    <td>no</td>
    <td>no</td>
    <td>yes</td>
  </tr>
  <tr>
    <td>Function</td>
    <td>yes</td>
    <td>no</td>
    <td>yes</td>
  </tr>
  <tr>
    <td>BiFunction</td>
    <td>yes</td>
    <td>yes</td>
    <td>yes</td>
  </tr>
</table>


-
## Runnable
* Takes no arguments
* Return nothing
* An expression which performs a mutation and returns nothing

-
-
## Runnable Syntax <br>(Lambda)
```java
public class MyClass {
  public static void main(String[] args) {
  	Runnable r = () -> System.out.println("Hello world");
  	r.run(); // invoke the method
  }
}
```


-
-
## Runnable Syntax<br>(Static Method Reference)

```java
public class MyClass {
	public static void main(String[] args) {
		Runnable r = MyClass::myMethod;
		r.run();
	}

	public static void myMethod() {
		System.out.println("Hello world");
	}
}
```


-
-
## Runnable Syntax<br>(Non-Static Method Reference)

```java
public class MyClass {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        Runnable r = obj::myMethod;
        r.run();
    }

    public void myMethod() {
        System.out.println("Hello world");
    }
}
```









-
## Consumer
* An expression which can `.accept` a single argument and returns nothing

-
-
## Consumer Syntax <br>(Lambda)
```java
public class MyClass {
  public static void main(String[] args) {
  	Consumer<String> consumer = (str) -> System.out.println(str);
  	consumer.accept("Hello world"); // invoke the method
  }
}
```


-
-
## Consumer Syntax<br>(Static Method Reference)

```java
public class MyClass {
	public static void main(String[] args) {
		Consumer<String> consumer = MyClass::myMethod;
		consumer.accept("Hello world");
	}

	public static void myMethod(String str) {
		System.out.println(str);
	}
}
```

-
-
## Consumer Syntax<br>(Non-Static Method Reference)

```java
public class MyClass {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        Consumer consumer = obj::myMethod;
        consumer.accept("Hello world");
    }

    public void myMethod(String str) {
        System.out.println(str);
    }
}
```








-
## BiConsumer
* An expression which can `.accept` two arguments and returns nothing
-
-
## BiConsumer Syntax <br>(Lambda)
```java
public class MyClass {
  public static void main(String[] args) {
    BiConsumer<String, Integer> bc;
    bc = (str, num) -> System.out.println(str + " " + num);
    bc.accept("Hello ", "World");
  }
}
```


-
-
## BiConsumer Syntax<br>(Static Method Reference)

```java
public class MyClass {
  public static void main(String[] args) {
    BiConsumer<String, Integer> bc = MyClass::myMethod;
    bc.accept("Hello", "World");
  }

  public static void myMethod(String str, Integer num) {
    System.out.println(str + " " + num);
  }
}
```


-
-
## BiConsumer Syntax<br>(Non-Static Method Reference)

```java
public class MyClass {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        BiConsumer<String, Integer> bc = obj::myMethod;
        bc.accept("Hello", "World");
    }

    public void myMethod(String str, Integer num) {
        System.out.println(str + " " + num);
    }
}
```













-
## Supplier
* An expression which takes a single argument and returns nothing
* Can `.get` reference to an object

-
-
## Supplier Syntax <br>(Lambda)
```java
public class MyClass {
  public static void main(String[] args) {
    Supplier<String> supplier = () -> "Hello world";
    String result = supplier.get();
    System.out.println(result);
  }
}
```


-
-
## Supplier Syntax<br>(Static Method Reference)

```java
public class MyClass {
  public static void main(String[] args) {
    Supplier<String> supplier = MyClass::myMethod;
    String result = supplier.get();
    System.out.println(result);
  }

  public static String myMethod() {
    return "Hello world";
  }
}
```

-
-
## Supplier Syntax<br>(Non-Static Method Reference)

```java
public class MyClass {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        Supplier<String> supplier = obj::myMethod;
        String result = supplier.get();
        System.out.println(result);
    }

    public void myMethod() {
        return "Hello world";
    }
}
```















-
## Function
* An expression which takes a single argument and returns a value
* Can `.apply` an argument to compute and return a value.
* `Predicate` is a specialized `Function` which always returns a `Boolean` value.

-
-
## Function Syntax<br>(Lambda)
```java
public class MyClass {
  public static void main(String[] args) {
    Function<Date, String> function;
    function = (dateObject) -> String.valueOf(dateObject);
    Date result = function.apply(new Date());
    System.out.println(result);
  }
}
```


-
-
## Function Syntax<br>(Static Method Reference)

```java
public class MyClass {
  public static void main(String[] args) {
    Function<Date, String> function = MyClass::myMethod;
    Date result = function.apply(new Date());
    System.out.println(result);
  }

  public static String myMethod(Date date) {
    return String.valueOf(date);
  }
}
```


-
-
## Function Syntax<br>(Non-Static Method Reference)

```java
public class MyClass {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        Function<Date, String> function = obj::myMethod;
        String result = function.apply(new Date());
        System.out.println(result);
    }

    public void myMethod(Date date) {
        return String.valueOf(date);
    }
}
```

















-
## BiFunction
* An expression which takes a two arguments and returns a value
* Can `.apply` two arguments to compute and return a value.

-
-
## BiFunction Syntax <br>(Lambda)
```java
public class MyClass {
  public static void main(String[] args) {
    BiFunction<Date, Long, String> function;
    function = (date, num) -> String.valueOf(date.getTime() + num);
    Date result = function.apply(new Date(), 15L);
    System.out.println(result);
  }
}
```


-
-
## BiFunction Syntax<br>(Static Method Reference)

```java
public class MyClass {
	public static void main(String[] args) {
		BiFunction<Date, String> function = MyClass::myMethod;
		Date result = function.apply(new Date(), 180L);
		System.out.println(result);
	}

	public static String myMethod(Date dateObj, Long longObj) {
		return String.valueOf(dateObj + longObj);
	}
}
```


-
-
## BiFunction Syntax<br>(Non-Static Method Reference)

```java
public class MyClass {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        BiFunction<Date, Long, String> function = obj::myMethod;
        String result = function.apply(new Date(), 972L);
        System.out.println(result);
    }

    public void myMethod(Date dateObj, Long longObj) {
        return String.valueOf(dateObj + longObj);
    }
}
```
