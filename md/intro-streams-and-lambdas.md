## Streams
-

## What is a Stream?

A stream is a sequence of objects that supports various methods which can be pipelined to produce the desired result

A stream is not a data structure instead it takes input from the Collections, Arrays or I/O channels

-
### What is a Stream?

* Specify what to be done, rather than how/when to do it
* Can operate on elements in parallel

-
### Processing a stream
1. Create a stream
2. Specify intermediate(/transformation) operations (e.g. filter, map)
3. Apply terminal operation to produce a result (e.g. toSet, reduce)

-
### Stream
You can only apply terminal operation once.

-
-

### Stream Creation

Array strategy #1

```Java
/** @param stringArray source array to create stream
 *  @return stream representation of this array */
public Stream<String> fromArray1(String[] stringArray) {
    Stream<String> stringStream = Arrays.stream(stringArray);
    return stringStream;
}
```

-
### Stream Creation
Array strategy #2

```Java
/** @param stringArray source array to create stream
 *  @return stream representation of this array */
public Stream<String> fromArray2(String[] stringArray) {
    Stream<String> stringStream = Stream.of(stringArray);
    return stringStream;
}
```

-
### Stream Creation
From varargs

```Java
/** @return stream representation of this array */
public Stream<String> fromVarargs() {
    Stream<String> stringStream1 = Stream.of("The");
    Stream<String> stringStream2 = Stream.of("The", "Quick", "Brown");

    return stringStream2;
}
```

-
### Stream Creation
List

```Java
/** @param stringList source list to create stream
*   @return stream representation of this List */
public Stream<String> fromList(List<String> stringList) {
    Stream<String> stringStream = stringList.stream();
    return stringStream;
}
```
-
### Stream Creation
`.empty`

```Java
public Stream<String> fromEmpty() {
	return Stream.empty();
}
```

-

### Extracting substreams

```Java
public Stream<String> getSubStream(String[] stringArray, int startIndex, int endIndex) {
	return Arrays.stream(stringArray, startIndex, endIndex);
}

public Stream<String> getSubStream(String[] stringArray, int endIndex) {
	return Arrays.stream(stringArray).limit(endIndex);
}
```

-
### Combining substreams

`Stream.concat` concatenates two streams

```Java
public Stream<String> combineStreams(String[] array1, String[] array2) {
    Stream<String> stream1 = Arrays.stream(array1);
    Stream<String> stream2 = Arrays.stream(array2);

    return Stream.concat(stream1, stream2);
}
```

-
-
## Method References

-
### Method References `::`
* Because Java 7 has no syntax to enable a method being passed as an argument, the `::` syntax was introduced in Java 8 to reference methods.

Instance method of a class

```java
Stream<String> words = Stream.of("The", "Quick", "Brown", "Fox");
words.forEach(System.out::print);
```

-

### Method References `::`
`.generate` - creates an infinite stream by calling a static function

```Java
/** @return endless stream */
public Stream<Double> fromGenerator() {
    Stream<Double> randoms = Stream.generate(Math::random);
    return randoms;
}
```

-

### Method References `::`
`.generate` - creates an infinite stream by calling a instance function

```Java
/** @return endless stream */
public Stream<Double> fromGenerator() {
    Stream<Double> echos = Stream.generate(this::echo);
    return echos;
}

public String echo() {
    return "Echo";
}


```

-
### Functional Interface
Instance method of a class

```java
class SquareMaker {
     public double square(double num){
        return Math.pow(num , 2);
    }
}

class DemoSquareMaker {
	public static void main(String[] args) {
		SquareMaker squareMaker = new SquareMaker();
		Function<Double, Double> squareMethod = squareMaker::square;
		double ans = squareMethod.apply(23.0);
	}
}
```
-
-
# Transformations
-

## Filter, Map, and FlatMap
* The `filter` transformation yields a new stream with elements that match the specified criteria
* The `map` transformation takes an argument of a function, applies the function to each element, and returns the respective stream
* The `flatMap` transformation prevents nested stream structures like `Stream<Stream<String>>`

-
## Transformations - Filter
`filter` yields a new stream with <br>elements that match the specified criteria

```Java
public Stream<String> getStringsLongerThan(String[] arr, int length) {
    return Arrays
            .stream(stringArray)
            .filter(this::stringIsLongerThan4);
}

public boolean stringIsLongerThan4(String str) {
    return str.length() > 4;
}
```

-
## Map
`map` takes an argument of a function, applies the function to each element, and returns the respective stream

```Java
public List<Integer> mapArrayLengths(String[] arr) {
    return Arrays
            .stream(arr)
            .map(this::length)
            .collect(Collectors.toList());
}

public Integer length(String x) {
    return x.length();
}
```

-
## FlatMap
`flatMap` prevents nested stream structures like `Stream<Stream<String>>` to `Stream<String>`

```Java
public Stream<String> letters(String someWord) {
    String[] characters = someWord.split("");
    return Stream.of(characters);
}

public Stream<String> wordsFlatMap(String[] stringArray) {
    return Arrays.stream(stringArray).flatMap(this::letters);
}
```

-
## Distinct
`distinct` yields a new stream with duplicates removed

```Java
public Stream<String> uniqueWords(String... words) {
	return Stream.of(words).distinct();
}
```

-
## Sorted
- `.sorted` will call the compareTo method on the object to sort
- Object must implements Comparable

```Java
public Stream<String> sort(String[] words) {
    return Arrays.stream(words).sorted();
}
```
or you must supply a Comparator

```Java
public Stream<String> sort(String[] words) {
    return Arrays.stream(words).sorted(this::compareStringsLength);
}

public int compareStringsLength(String str1, String str2) {
    return str1.length() - str2.length();
}
```

-
-
## Simple Reductions
* Reductions are terminal operations
* They reduce the stream to a nonstream value that can be used in the program.
* Examples include: `.count`, `.max`, `.min`, `.findFirst`, `.findAny`, `.anyMatch`
* Reductions yield `Optional<T>` values

-
## `Optional<T>`
* An `Optional<T>` value either wraps the result of a method, or indicates that there is no result
* The purpose of `Optional<T>` is to prevent potential `NullPointerExceptions`


-
## Reductions
`.count`

```Java
/** @return number of elements in an array using a stream */
public long getCount(String[] stringArray) {
    return Arrays.stream(stringArray).count();
}
```

-
## Reductions
`.min`, `.max`

```Java
/** @return longest String object in an array using a stream */
public Optional<String> getMax(String[] stringArray) {
    return Arrays.stream(stringArray).max(String::compareToIgnoreCase);
}

/** @return longest String object in an array using a stream */
public Optional<String> getMin(String[] stringArray) {
    return Arrays.stream(stringArray).min(String::compareToIgnoreCase);
}
```

-
## Reductions
`.findFirst`, `.findAny`

```Java
/** @return get first String from an array using a stream */
public Optional<String> getFirst(String[] stringArray) {
    return Arrays.stream(stringArray).findFirst();
}

/** @return a random string in an array using a stream */
public Optional<String> getRandom(String[] stringArray) {
    return Arrays.stream(stringArray).findAny();
}
```

-
## Reductions
* The `reduce` method is a general mechanism for computing a value from a stream.

```Java
public Integer sum(Integer[] numbers) {
    Optional<Integer> result = Stream.of(numbers).reduce(Integer::sum);
    Integer sum = result.get();
    return sum;
}
```
-
## Reductions
* The `reduce` method is a general mechanism for computing a value from a stream.

```Java
public Integer sum(Integer[] numbers) {
  Integer sum = Stream.of(numbers).reduce(10, Integer::sum);
  return sum;
}
```

-
-
## Collecting Results

-
## Collecting Results
* `.toArray()`
* It is not possible to create a generic array at runtime.<br>If you want an array of the correct type, pass in the array constructor.

```java
Stream<String> words = Stream.of("The", "Quick", "Brown", "Fox");
String[] array = words.toArray(String[]::new);
```

-
## Collecting Results
`.collect()` to a List

```java
Stream<String> words = Stream.of("The", "Quick", "Brown", "Fox");
List<String> list = words.collect(Collectors.toList());
```



-
## Collecting Results
`.collect()` to a Set

```java
Stream<String> words = Stream.of("The", "Quick", "Brown", "Fox");
Set<String> list = words.collect(Collectors.toSet());
```

-
## Collecting Results
`.collect()` to a `Map`

```java
Stream<String> words = Stream.of("The", "Quick", "Brown", "Fox");
Map<Integer, String> map = words.collect(Collectors.toMap(String::hashCode, String::toString));
```

-
-
# Grouping and Partitioning

-
## Grouping
`.groupingBy()` groups values with the same characteristic

```Java
public Map<String, List<Locale>> groupingByDemo() {
    Stream<Locale> locales = LocaleFactory.createLocaleStream(999);
    return locales.collect(Collectors.groupingBy(Locale::getCountry));
}
```

-
## Partitioning
`.partitioningBy()` yields a Map that contains two groups, one for true values and another for false values.

```Java
public Map<Boolean, List<String>> partitionedStream() {
    return Stream
            .of("The", "Quick", "Brown", "Fox")
            .collect(Collectors.partitioningBy(this::lengthIsGreaterThan4));
}

public boolean lengthIsGreaterThan4(String x) {
    return x.length() > 4;
}
```
-
-
<img src="https://blazingardor.files.wordpress.com/2013/07/tumblr_m8ks4fkel51qdt6bmo1_500.jpg">

-
-
## Relevant Functional Interfaces
* A `Function` is a single-argument, returns a value (use for `map`, `groupBy`).
    ```
      Function<Integer, Integer> function = Math::abs;
    ```
* A `Predicate` is a single-argument, returns a boolean (use for `partitioned`, `filter`).
    ```
      Predicate<String> empty = String::isEmpty;
    ```
* A `lambda` is a function which can be created without belonging to any class.
* A `method reference` is how java handles the nuance of passing methods as arguments.

-
## Downstream Collectors
* The `.groupingBy` method yields a map whose values are lists
* If you want to process those lists in some way, supply a _downstream collector_.
* For example, if you want sets, instead of lists, you can use `Collectors.toSet`

```Java
class Demo {
	public Map<String, Set<Locale>> demoDownstreamCollectors1() {
	    Stream<Locale> locales = LocaleFactory.createLocaleStream();
	    Map<String, Set<Locale>> countryToLocaleSet = locales.collect(
	            groupingBy(Locale::getCountry, Collectors.toSet()));

	    return countryToLocaleSet;
	}
}
```

-
## Downstream Collectors
* Several collectors are provided for reducing grouped elements to numbers.
* `counting` produces a count of collected elements.

```Java
class Demo {
    public Map<String, Long> demoDownstreamCollectors2() {
        Stream<Locale> locales = LocaleFactory.createLocaleStream();
        Map<String, Long> countryToLocaleSet = locales.collect(
                groupingBy(Locale::getCountry, counting()));

        return countryToLocaleSet;
    }
}
```

-
-
## Primitive Type Streams
* The stream library has specialized types `IntStream`, `LongStream`, and `DoubleStream` that store primitive values directly without using wrappers.
* If you want to store `int`, `short`, `char`, `byte`, and `boolean`, use an `IntStream`.
* If you want to store `float`, or `double`, use `DoubleStream`.

-
## Populate `IntStream`
* Use `.of` to populate a stream with respective values

```Java
class PrimitiveStreams {
    public IntStream demoOf() {
        IntStream intStream = IntStream.of(1, 3, 5, 8, 13);
        return intStream;
    }
}
```


-
## Generate `IntStream`
* Use `.generate` to create endless stream

```Java
class PrimitiveStreams {
    public DoubleStream demoGenerate() {
        DoubleStream doubles = DoubleStream.generate(Math::random);
    }
}
```


-
## Create exclusive range `IntStream`
* Use `.range` to generate a set of ints between specified `min` and `max` values

```Java
class PrimitiveStreams {
    /** upper bound is excluded
     *  @param min value to generate
     *  @param max value to generate
     *  @return range of numbers betwen min and max */
    public IntStream demoRange(int min, int max) {
        return IntStream.range(min, max);
    }
}
```


-
## Create inclusive range `IntStream`
* Use `.range` to generate a set of ints between specified `min` and `max` values

```Java
class PrimitiveStreams {
    /** upper bound is included
     *  @param min value to generate
     *  @param max value to generate
     *  @return range of numbers betwen min and max */
    public IntStream demoRange(int min, int max) {
        return IntStream.rangeClosed(min, max);
    }
}
```

-
## Primitive to Object stream
* Use the `.boxed` method to convert form primitive streams to object streams

```Java
public class PrimitiveStreams {
    public Stream<Integer> demoBoxStream() {
        return IntStream.of(0, 1, 2).boxed();
    }
}
```
-
-

## Composing Optional Value Functions<br>With `.flatMap`

-
## Chaining Method Calls
* Consider the following:
	* `S` is a class which contains the definition of method `.f()`
	* Method `.f()` returns `Optional<T>`
	* `T` is a class which contains the definition of method `.g()`
	* Method `.g()` returns `Optional<U>`
	* you would be able to call them, via <br>`s.f().g()`<br>if these methods returned the raw types rather than the respective `Optional` wrapper type.

-
-
## Parallel Streams
* Streams make it easy to parallelize bulk operations.
* The process is mostly automatic, but you need to take note of the following:
	1. Use a parallel stream
		* You can get a parallel stream via `Collection.parallelStream()`
		* You can convert to parallel stream via `Stream.of(wordArray).parallel()`
	2. 	If a stream is in parallel mode when the terminal method executes, all intermediate stream operations will be parallelized.
	3. Operations are stateless and can be executed in an arbitary order.

-
#Improper usage
* Here is an example of something you cannot do.
* Suppose you want to count all short words in a stream of strings.

```Java
class Demo {
  public void demo(List<String> words) {
      int[] shortWords = new int[12];
      words.parallelStream().forEach(s -> {
          if (s.length() < 12) {
              shortWords[s.length()]++;
          }});
      // Error - race condition!
      System.out.println(Arrays.toString(shortWords));
  }
}
```
* The function passed to `forEach` runs concurrently in multiple threads, each updating a shared array.


-
## Using parallel stream
* It is your responsibility to ensure that any functions passed to parallel stream operations are safe to execute in parallel.
* The best way to do that is to stay away from mutable state.
* In this example, you can safely parallelize the computation if you group strings by length and count them.

-

```Java
class Demo {
	public Map<Integer, Long> wordCountMap(Stream<String> words) {
		return words
      .paralellStream()
			.filter(s -> s.length() < 10)
			.collect(groupingBy(String::length, counting()));
	}
}
```
