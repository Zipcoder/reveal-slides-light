# Lambda Expressions

-
-
# What is a Lambda Expression for?

A simple example sans lambda

```
public interface FileFilter {
    boolean accept(File file);
}
```

-
Implement the interface

```
public class JavaFileFilter implements FileFilter{
    public boolean accept(File file){
        return file.getName().endsWith(".java");
    }
}
```

-
Usage example

```
JavaFileFilter fileFilter = new JavaFileFilter();
File dir = new File("d:/tmp");
File[] javaFiles = dir.listFiles(fileFilter);
```

-
## What is a Lambda Expression for?

1. To make instances of anonymous classes easier to write, and thereby easier to read

```
FileFilter filter = (File file) -> file.getName().endsWith(".java");
```

-
## Several ways to implement a lambda Expression
```
FileFilter filter = (File file) -> file.getName().endsWith(".java");
```

```
Runnable r = () -> {
	for (int i = 0; i < 5; i++){
		System.out.println("Hello world!");
	}
};
```

```
Comparator<String> c =
	(String s1, String s2) ->
		Integer.compare(s1.length(), s2.length());
```

-
-
## Three Questions About lambdas

* What is the type of a lambda expression?

* Can a lambda be put in a variable

* Is a lambda expression an object

-
### What is the type of a lambda expression?
* A Functional Interface

-
### Functional Interface

A function interface is an interface with only one abstract method

```
public interface Runnable {
	run();
};
```

```
public interface Comparator<T> {
	int compare(T t1, T t2);
};
```

```
public interface FileFilter {
	boolean accept(File pathname);
};
```

-
A function interface can be annotated
```
@FunctionalInterface
public interface MyFunctionalInterface {
	someMethod();
}
```
This is just a convenience, the compiler can tell me whether the interface is functionl or not

-
## Three Questions About lambdas
What is the type of a lambda expression?

Answer: a function interface

-
## Three Questions About lambdas

* What is the type of a lambda expression?
Answer: a function interface

* Can a lambda be put in a variable

* Is a lambda expression an object

-
### Can a lambda be put in a variable
* Yes
```
Comparator <String> c =
	(String s1, String s2) ->
		Integer.compare(s1.length(), s2.length());
```
* Consequences: a lambda can be taken as a method parameter, and can be returned by a method

-
## Three Questions About lambdas

* What is the type of a lambda expression?
Answer: a function interface

* Can a lambda be put in a variable
Answer: Yes

* Is a lambda expression an object

-
### Is a lambda expression an object
* Compare the following
```
Comparator <String> c =
	(String s1, String s2) ->
		Integer.compare(s1.length(), s2.length());
```
```
Comparator <String> c =
	new Comparator<String>(String s1, String s2){
		Integer.compare(s1.length(), s2.length());
	}
};
```
* A lambda expression is created without using <<new>>

-
## Three Questions About lambdas

* What is the type of a lambda expression?
Answer: a function interface

* Can a lambda be put in a variable
Answer: Yes

* Is a lambda expression an object
Answer is complicated, but no
Exact answer: a lambda is an object without an identity

-
-
## Functional Interface Package

* java.util.function is a new package as of Java 8
* offers around 40 general porpose interfaces that are used by other classes ans are available to developers for their own uses
* these interfaces are divided up into 4 categories
	* Supplier
	* Consumer
	* Predicate
	* Function
	
-
### Supplier

```
@FunctionalInterface
public interface Supplier<T> {
	T get();
}
```
A single interface that doesn't take any object and that provides a new object.

-
### Consumer

```
@FunctionalInterface
public interface Consumer<T>{
	void accept(T t);
}
```
The consumer accepts an object and doesn't return anything. (`System.out.println`)

**BiConsumer**

```
@FunctionalInterface
public interface BiConsumer<T, U> {
	void accept(T t, U u);
}
```
takes two objects instead of one

-
### Predicate

```
@FunctionInterface
public interface Predicate<T> {
	boolean test(T t);
}
```
A predicate takes an object as a parameter and returns a boolean

**BiPredicate**

```
@FunctionalInterface
public interface BiPredicate<T, U> {
	boolean test(T t, U u);
}
```

-
### Function

```
@FunctionaInterface
public interface Function<T, R> {
	R apply (T t);
}
```
A function takes an object as a parameter and returns another object

**BiFunction**

```
@FunctionalInterface
public interface BiFunction<T, U, R> {
	R apply (T t, U u);
}
```

-
## More Lambda Expressions Syntax
Most of the time, parameter types can be omitted

```
Comparator<String> c = 
	(String s1, String s2) ->
		Integer.compare(s1.lingth(), s2.length());
```
Becomes:

```
Comparator<String> c =
	(s1, s2) ->
		Integer.compare(s1.length(), s2.length());
```

-
-
## Method References

* This lambda expression
 
`Consumer<String> c = s -> System.out.println(s);`

* can be written like this:

`Consumer<String> c = System.out::println;`
	
It is composed of the object System.out and the method println seperated by `::`. This is just a shortcut to writting a lambda expression

-

* This lambda expression:

`Comparator<Integer> c = (i1, i2) -> Integer.compare(i1, i2);`

* Can be written like this:

`Comparator<Integer> c = Integer::compare;`

	
	
	
	
	
	
	
	
	
	
	
	
	
	

