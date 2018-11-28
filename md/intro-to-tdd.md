#Introduction to Test Driven Development (TDD)


-
-
#3 laws of TDD

###First Law

You may not write production code until you have a written a failing corresponding unit test.

```java
	// This code has no unit test yet. It shouldn't be implemented
	// There is no way for anyone to see if this code is working
	// as intended or if there is a mistake
	public class Cow{
		public String speak(){
			return "Mooooow";
		}
	}
```

-
-
###Second Law

You may not write more of a unit test than is needed to fail

```java
public class CowTest{

	// This is a good test but will not compile
	@Test
	public void speakTest(){
		Cow cow = new Cow();
		String expected = "Moo";
		// The speak method does not exist yet
		String actual = cow.speak();
		Assert.equals(expected, actual);
	}
}
```

-

Get the code compiling

```java
public class Cow{
	public Cow(){}

	// Create the speak method to get the project compiling
	public String speak(){
		return null;
	}
}

```

-

As of now that test will fail. We are expecting cow to say `Moo` but when we call speak it returns `null`.

-

Now that we have a failing test we can write some production code

```java
	public class Cow{
		public String speak(){
			// Code now written to pass the test
			return "Moo";
		}
	}

```

-
#3 laws of TDD

###Third Law

You may not write more production code than is sufficent to pass the current failing test.

```java
public String speak(){
	if(cow.getSize() > 5)
		String sound = "MOOO";
	else
		String sound = "Moo"

	// While this will pass our test, it includes a case we haven't 
	// tested for. Before writing our if statement, we should first 
	// write a test with a large cow and a normal cow speaking. 
	return sound;
}
```

-
-

#Clean Code and Clean Test

A clean test is a readable Test. Your test is your code!

```java
@Test
public void testFunction(){
	// While this test technically works, it is unorganized
	// It also doesn't explain what each variable is doing
	Unicorn unico = New Unicorn();
	int x = 23;
	int y = unico.saveThePrincess();
	Assert.equals(x,y);
}

```

-
```java
@Test
public void testCountToTwo(){
	//Here we know what we are given
	Unicorn unico = New Unicorn();
	// Here we know what our result should be
	int expectedNumber = 2;
	// Here we know this is what we actually got
	int actualNumber = unico.countToTwo();
	// With the variables named properly we can now
	// read this as "Assert that the expected number 
	// and actual number are equal"
	Assert.equals(expectedNumber, actualNumber);
}
```
-
-
## One test one case

A good unit test should only test for one method and often one case. If you have if statements in your method that require multiple assertions to get to, then you will usually split those cases up into separate methods

-

```java
@Test
public void testSpeakLargeCow(){
	Cow cow = new Cow();
	// Make the cow large
	cow.setSize(9)
	// Large cows should say MOO
	String expected = "MOO";
	// What does the fox, err, cow say?
	String actual = cow.speak();

	Assert.equals(expected, actual);
}
```

```java
@Test
public void testSpeakSmallCow(){
	Cow cow = new Cow();
	// Make the cow small
	cow.setSize(3)
	String expected = "Moo";

	// Test runs the same method but has a different
	// expectation because the parameters of the test
	// are different. This ensures line coverage
	String actual = cow.speak();
	Assert.equals(expected, actual);
}
```

-

Single Concept per test

* Given (Sets the stage)
* When (Performs the action)
* Evaluate (Checks the results)

-
-
#F.I.R.S.T

* **F** is for **fast** - The test should be fast.
* **I** is for **Independent** - Executing a test should not effect any other tests
* **R** is for **Repeatable** - When a test is provided input, it should return the same output no matter where it is run
* **S** is for **Self-Validating** - No manual input should be required to see if a test passes or fails
* **T** is for **Timely** - Tests should be written first and should consider most if not all use cases of your program
