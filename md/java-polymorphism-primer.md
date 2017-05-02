### Polymorphism

-
#### Liskov Substitution Principle

An instance of type T can be replaced by an instance of type S if S is a subtype of T.

-
## Polymorphic Program design

- Most methods rely on provided interfaces, rather than underlying implementations
- Object fields are accessed through getters/setters

-
### Things that don't behave polymorphically

- field accesses (eg: `Parent p = new Child(); p.x;`)
- Static methods

-

```Java
//: polymorphism/FieldAccess.java
// Direct field access is determined at compile time.
class Soup {
  public int field = 0;
  public int getField() { return field; }
}
class Stew extends Soup {
  public int field = 1;
  public int getField() { return field; }
  public int getSuperField() { return super.field; }
}
```

-

```Java
public class FieldAccess {
  public static void main(String[] args) {
    Soup soup = new Stew(); // Upcast
    System.out.println("soup.field = " + soup.field +
      ", soup.getField() = " + soup.getField());
    Stew sub = new Stew();
    System.out.println("sub.field = " +
      sub.field + ", sub.getField() = " +
      sub.getField() +
      ", sub.getSuperField() = " +
      sub.getSuperField());
  }
} /* Output:
soup.field = 0, soup.getField() = 1
sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0
*///:~
```