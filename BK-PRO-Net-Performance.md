### Chapter 3: Type Internals ###

#### Value Type Internals ####
* `System.ValueType` overrides the `Equals` and `GetHashCode` virtual methods inherited from `System.Object`
* `ValueType` ensures that value types are compared based on their content, whereas original Equals method in `System.Object` compares only the object references (identity)

#### Avoiding Boxing on Value Types with the `Equals` Method ####

```
public struct Point2D : IEquatable<Point2D>
{
    public int X;
    public int Y;
    public bool Equals(Point2D other) ... //as before
}

Example 1
Point2D point = ...;
IEquatable<Point2D> equatable = point; //boxing occurs here

Point2D point = ..., anotherPoint = ...;
point.Equals(anotherPoint); //no boxing occurs here, Point2D.Equals(Point2D) is invoked

Example 2
Point2D point = new Point2D { X = 5, Y = 7 };
Point2D anotherPoint = new Point2D { X = 6, Y = 7 };

IEquatable<Point2D> equatable = point; //boxing occurs here
equatable.Equals(anotherPoint); //returns false

point.X = 6;

point.Equals(anotherPoint); //returns true
equatable.Equals(anotherPoint); //returns false, the box was not modified!

Example 3
List<Address> addresses = new List<Address>
{
    new Address { ... }
};

int index = addresses.IndexOf(new Address { ... });
```

* Example 3 it will not work (i.e. index will be -1) if you have neither overridden `Equals(object)` nor implemented `IEquatable<T>`. `List<T>.IndexOf` won't call your `Equals` overload.
* This is one ground for the common recommendation to make value types immutable, and allow modification only by making more copies. (Consider the`System.DateTime AP`I for an example of a well-designed
immutable value type.)

#### The `GetHashCode` Method ####
The performance guarantees of hash tables rely strongly upon the hash function used by the hash table implementation, but require also several properties from the `GetHashCode` method
1. If two objects are equal, their hash codes are equal
2. If two objects are not equal, it should be unlikely that their hash codes are equal
3. GetHashCode should be fast (although often it is linear in the size of the object)
4. An object’s hash code should not change

```
public class Employee
{
    public string Name { get; set; }
    public override int GetHashCode() => Name.GetHashCode();
}

var employees = new HashSet<Employee>();
var kate = new Employee { Name = "Kate Jones" };
employees.Add(kate);

kate.Name = "Kate Jones-Smith";
employees.Contains(kate); //returns false!

```

* The object’s hash code has changed because its contents have changed, and we can no longer find the object in the hash table

#### Best Practices for Using Value Types ####
* Use value types if your objects are small and you intend to create a great many of them.
* Use value types if you require high-density memory collections.
* Override Equals, overload Equals, implement IEquatable<T>, overload operator ==, and overload operator != on your value types.
* Override GetHashCode on your value types.
* Consider making your value types immutable.

---
