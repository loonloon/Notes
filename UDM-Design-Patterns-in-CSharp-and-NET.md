#### Overview ####
![image](https://user-images.githubusercontent.com/5309726/101895492-9269ff80-3be2-11eb-9cb4-bf8c3d05eed3.png)

#### The SOLID Design Principles ####
* Single Responsibility Principle

```
//bad
public class Journal
{
    private readonly List<string> entries = new List<string>();
    private static int count = 0;

    public int AddEntry(string text)
    {
        entries.Add($"{++count}: {text}");
        return count; // memento pattern!
    }

    public void RemoveEntry(int index)
    {
        entries.RemoveAt(index);
    }

    public override string ToString()
    {
        return string.Join(Environment.NewLine, entries);
    }

    // breaks single responsibility principle
    public void Save(string filename, bool overwrite = false)
    {
        File.WriteAllText(filename, ToString());
    }

    public void Load(string filename)
    {
    }

    public void Load(Uri uri)
    {
    }
}

//good
public class Journal
{
    private readonly List<string> entries = new List<string>();
    private static int count = 0;

    public int AddEntry(string text)
    {
        entries.Add($"{++count}: {text}");
        return count; // memento pattern!
    }

    public void RemoveEntry(int index)
    {
        entries.RemoveAt(index);
    }

    public override string ToString()
    {
        return string.Join(Environment.NewLine, entries);
    }
}

public class Persistence
{
    public void SaveToFile(Journal journal, string filename, bool overwrite = false)
    {
        if (overwrite || !File.Exists(filename))
        {
            File.WriteAllText(filename, journal.ToString());
        }
    }

    public void Load(string filename)
    {
    }

    public void Load(Uri uri)
    {
    }
}
```

---

* Open Closed Principle

```
public enum Color
{
    Red,
    Green,
    Blue
}

public enum Size
{
    Small,
    Medium,
    Large,
    Huge
}

public class Product
{
    public string Name;
    public Color Color;
    public Size Size;

    public Product(string name, Color color, Size size)
    {
        Name = name ?? throw new ArgumentNullException(paramName: nameof(name));
        Color = color;
        Size = size;
    }
}

//bad
public class ProductFilter
{
    public IEnumerable<Product> FilterByColor(IEnumerable<Product> products, Color color)
    {
        foreach (var p in products)
        {
            if (p.Color == color)
            {
                yield return p;
            }
        }
    }

    public static IEnumerable<Product> FilterBySize(IEnumerable<Product> products, Size size)
    {
        foreach (var p in products)
        {
            if (p.Size == size)
            {
                yield return p;
            }
        }
    }

    public static IEnumerable<Product> FilterBySizeAndColor(IEnumerable<Product> products, Size size, Color color)
    {
        foreach (var p in products)
        {
            if (p.Size == size && p.Color == color)
            {
                yield return p;
            }
        }
    }
}

public class Demo
{
    static void Main(string[] args)
    {
        var apple = new Product("Apple", Color.Green, Size.Small);
        var tree = new Product("Tree", Color.Green, Size.Large);
        var house = new Product("House", Color.Blue, Size.Large);

        Product[] products = { apple, tree, house };

        var pf = new ProductFilter();
        Console.WriteLine("Green products (old):");

        foreach (var p in pf.FilterByColor(products, Color.Green))
        {
            Console.WriteLine($" - {p.Name} is green");
        }
    }
}

//good
public interface ISpecification<T>
{
    bool IsSatisfied(Product p);
}

public interface IFilter<T>
{
    IEnumerable<T> Filter(IEnumerable<T> items, ISpecification<T> spec);
}

public class ColorSpecification : ISpecification<Product>
{
    private Color color;

    public ColorSpecification(Color color)
    {
        this.color = color;
    }

    public bool IsSatisfied(Product p)
    {
        return p.Color == color;
    }
}

public class SizeSpecification : ISpecification<Product>
{
    private Size size;

    public SizeSpecification(Size size)
    {
        this.size = size;
    }

    public bool IsSatisfied(Product p)
    {
        return p.Size == size;
    }
}

public class AndSpecification<T> : ISpecification<T>
{
    private ISpecification<T> first, second;

    public AndSpecification(ISpecification<T> first, ISpecification<T> second)
    {
        this.first = first ?? throw new ArgumentNullException(paramName: nameof(first));
        this.second = second ?? throw new ArgumentNullException(paramName: nameof(second));
    }

    public bool IsSatisfied(Product p)
    {
        return first.IsSatisfied(p) && second.IsSatisfied(p);
    }
}

public class BetterFilter : IFilter<Product>
{
    public IEnumerable<Product> Filter(IEnumerable<Product> items, ISpecification<Product> spec)
    {
        foreach (var i in items)
        {
            if (spec.IsSatisfied(i))
            {
                yield return i;
            }
        }
    }
}

public class Demo
{
    static void Main(string[] args)
    {
        var apple = new Product("Apple", Color.Green, Size.Small);
        var tree = new Product("Tree", Color.Green, Size.Large);
        var house = new Product("House", Color.Blue, Size.Large);

        Product[] products = { apple, tree, house };

        var bf = new BetterFilter();

        Console.WriteLine("Green products (new):");

        foreach (var p in bf.Filter(products, new ColorSpecification(Color.Green)))
        {
            Console.WriteLine($" - {p.Name} is green");
        }

        System.Console.WriteLine("Large products");

        foreach (var p in bf.Filter(products, new SizeSpecification(Size.Large)))
        {
            Console.WriteLine($" - {p.Name} is large");
        }

        System.Console.WriteLine("Large blue items");

        foreach (var p in bf.Filter(products, 
            new AndSpecification<Product>(new ColorSpecification(Color.Blue), new SizeSpecification(Size.Large))))
        {
            Console.WriteLine($" - {p.Name} is big and blue");
        }
    }
}
```

---

* Liskov Substitution Principle
  * Should be always be up cast and the operation should be stil generally OK
  
```
//wrong
public class Rectangle
{
    public int Width { get; set; }
    public int Height { get; set; }

    public Rectangle()
    {
    }

    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }

    public override string ToString()
    {
        return $"{nameof(Width)}: {Width}, {nameof(Height)}: {Height}";
    }
}

public class Square : Rectangle
{
    public new int Width
    {
        set
        {
            base.Width = base.Height = value;
        }
    }

    public new int Height
    {
        set
        {
            base.Width = base.Height = value;
        }
    }
}

public class Demo
{
    static public int Area(Rectangle r) => r.Width * r.Height;

    static void Main(string[] args)
    {
        Rectangle rc = new Rectangle(2, 3);
        Console.WriteLine($"{rc} has area {Area(rc)}");

        Rectangle sq = new Square();
        sq.Width = 4;
        Console.WriteLine($"{sq} has area {Area(sq)}");
    }
}

//good
public class Rectangle
{
    /*
     * In simple you can imagine a base class with a virtual methodA will have a vtable that contains methodA.
     *
     * When derived class override methodA,
     * the vtable for methodA inherits from the parent class but
     * will now points to the methodA in the derived class rather than parent class.
     */
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public Rectangle()
    {

    }

    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }

    public override string ToString()
    {
        return $"{nameof(Width)}: {Width}, {nameof(Height)}: {Height}";
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = base.Height = value; }
    }
}
```

---

* Interface Segregation Principle

```
public class Document
{
}

//bad
public interface IMachine
{
    void Print(Document d);
    void Fax(Document d);
    void Scan(Document d);
}

public class MultiFunctionPrinter : IMachine
{
    public void Print(Document d)
    {
    }

    public void Fax(Document d)
    {
    }

    public void Scan(Document d)
    {
    }
}

public class OldFashionedPrinter : IMachine
{
    public void Print(Document d)
    {
    }

    public void Fax(Document d)
    {
        // unncessary
        throw new System.NotImplementedException();
    }

    public void Scan(Document d)
    {
        // unncessary
        throw new System.NotImplementedException();
    }
}

//good
public interface IPrinter
{
    void Print(Document d);
}

public interface IScanner
{
    void Scan(Document d);
}

public class Printer : IPrinter
{
    public void Print(Document d)
    {
    }
}

public class Photocopier : IPrinter, IScanner
{
    public void Print(Document d)
    {
    }

    public void Scan(Document d)
    {
    }
}

public interface IMultiFunctionDevice : IPrinter, IScanner
{
}

public struct MultiFunctionMachine : IMultiFunctionDevice
{
    //compose this out of several modules
    private IPrinter printer;
    private IScanner scanner;

    public MultiFunctionMachine(IPrinter printer, IScanner scanner)
    {
        if (printer == null)
        {
            throw new ArgumentNullException(paramName: nameof(printer));
        }

        if (scanner == null)
        {
            throw new ArgumentNullException(paramName: nameof(scanner));
        }

        this.printer = printer;
        this.scanner = scanner;
    }

    public void Print(Document d)
    {
        //decorator pattern
        printer.Print(d);
    }

    public void Scan(Document d)
    {
        scanner.Scan(d);
    }
}
```

---

* Dependency Inversion Principle
  * High-level modules should not depend on low-level; both should depend on abstractions
  * Abstractions should not depend on details; details should depend on abstractions
  
```
public enum Relationship
{
    Parent,
    Child,
    Sibling
}

public class Person
{
    public string Name;
    public DateTime DateOfBirth;
}

public class Relationships
{
    private List<(Person, Relationship, Person)> relations
        = new List<(Person, Relationship, Person)>();

    public void AddParentAndChild(Person parent, Person child)
    {
        relations.Add((parent, Relationship.Parent, child));
        relations.Add((child, Relationship.Child, parent));
    }

    public List<(Person, Relationship, Person)> Relations => relations;
}

//bad
public class Research
{
    public Research(Relationships relationships)
    {
        // high-level: find all of john's children
        var relations = relationships.Relations;

        foreach (var r in relations.Where(x => x.Item1.Name == "John" &&
                                               x.Item2 == Relationship.Parent))
        {
            Console.WriteLine($"John has a child called {r.Item3.Name}");
        }
    }
}

//good
public interface IRelationshipBrowser
{
    IEnumerable<Person> FindAllChildrenOf(string name);
}

// low-level module (e.g. client accesses DB)
public class Relationships : IRelationshipBrowser
{
    private List<(Person, Relationship, Person)> relations
        = new List<(Person, Relationship, Person)>();

    public void AddParentAndChild(Person parent, Person child)
    {
        relations.Add((parent, Relationship.Parent, child));
        relations.Add((child, Relationship.Child, parent));
    }
    
    public IEnumerable<Person> FindAllChildrenOf(string name)
    {
        return relations
            .Where(x => x.Item1.Name == name && x.Item2 == Relationship.Parent)
            .Select(r => r.Item3);
    }
}

public class Research
{
    public Research(IRelationshipBrowser browser)
    {
        foreach (var p in browser.FindAllChildrenOf("John"))
        {
            Console.WriteLine($"John has a child called {p.Name}");
        }
    }
}
```

---

#### Builder ####
When piecewise object construction is complicated, provide an API for doing it succinctly

```
//bad
var hello = "hello";
var sb = new StringBuilder();
sb.Append("<p>");
sb.Append(hello);
sb.Append("</p>");
Console.WriteLine(sb);


var words = new[] { "hello", "world" };
sb.Clear();
sb.Append("<ul>");

foreach (var word in words)
{
    sb.AppendFormat("<li>{0}</li>", word);
}

sb.Append("</ul>");
Console.WriteLine(sb);

//good
class HtmlElement
{
    public string Name, Text;
    public List<HtmlElement> Elements = new List<HtmlElement>();
    private const int indentSize = 2;

    public HtmlElement()
    {
    }

    public HtmlElement(string name, string text)
    {
        Name = name;
        Text = text;
    }

    private string ToStringImpl(int indent)
    {
        var sb = new StringBuilder();
        var i = new string(' ', indentSize * indent);
        sb.Append($"{i}<{Name}>\n");

        if (!string.IsNullOrWhiteSpace(Text))
        {
            sb.Append(new string(' ', indentSize * (indent + 1)));
            sb.Append(Text);
            sb.Append("\n");
        }

        foreach (var e in Elements)
        {
            sb.Append(e.ToStringImpl(indent + 1));
        }

        sb.Append($"{i}</{Name}>\n");
        return sb.ToString();
    }

    public override string ToString()
    {
        return ToStringImpl(0);
    }
}

class HtmlBuilder
{
    private readonly string rootName;
    HtmlElement root = new HtmlElement();

    public HtmlBuilder(string rootName)
    {
        this.rootName = rootName;
        root.Name = rootName;
    }

    // not fluent
    public void AddChild(string childName, string childText)
    {
        var e = new HtmlElement(childName, childText);
        root.Elements.Add(e);
    }

    public HtmlBuilder AddChildFluent(string childName, string childText)
    {
        var e = new HtmlElement(childName, childText);
        root.Elements.Add(e);
        return this;
    }

    public override string ToString()
    {
        return root.ToString();
    }

    public void Clear()
    {
        root = new HtmlElement { Name = rootName };
    }
}

public class Demo
{
    static void Main(string[] args)
    {
        // ordinary non-fluent builder
        var builder = new HtmlBuilder("ul");
        builder.AddChild("li", "hello");
        builder.AddChild("li", "world");
        WriteLine(builder.ToString());

        // fluent builder
        sb.Clear();
        builder.Clear();
        builder.AddChildFluent("li", "hello").AddChildFluent("li", "world");
        Console.WriteLine(builder);
    }
}
```

---

* Fluent Builder Inheritance with Recursive Generics

```
public class Person
{
    public string Name;

    public string Position;
    public static Builder New => new Builder();

    public override string ToString()
    {
        return $"{nameof(Name)}: {Name}, {nameof(Position)}: {Position}";
    }

    class Builder : PersonInfoBuilder<Builder>
    {
    }
}

abstract class PersonBuilder<T> where T : PersonBuilder<T>
{
    protected Person person = new Person();

    public Person Build()
    {
        return person;
    }
}

class PersonInfoBuilder<T> : PersonBuilder<PersonInfoBuilder<T>>
    where T : PersonInfoBuilder<T>
{
    public T Called(string name)
    {
        person.Name = name;
        return (T)this;
    }
}

class PersonJobBuilder<T> : PersonInfoBuilder<PersonJobBuilder<T>>
    where T : PersonJobBuilder<T>
{
    public T WorksAsA(string position)
    {
        person.Position = position;
        return (T)this;
    }
}

public class BuilderInheritanceDemo
{
    static void Main(string[] args)
    {
        var builder = new Builder();
        var person = builder
            .Called("dmitri")
            .WorksAsA("quant")
            .Build();
        Console.WriteLine(person);
    }
}
```

---

* Faceted Builder

```
public class Person
{
    public string StreetAddress, Postcode, City;
    public string CompanyName, Position;
    public int AnnualIncome;

    public override string ToString()
    {
        return $"{nameof(StreetAddress)}: {StreetAddress}, {nameof(Postcode)}: {Postcode}, " +
               $"{nameof(City)}: {City}, {nameof(CompanyName)}: {CompanyName}, " +
               $"{nameof(Position)}: {Position}, {nameof(AnnualIncome)}: {AnnualIncome}";
    }
}

// facade
public class PersonBuilder
{
    protected Person person = new Person();
    public PersonAddressBuilder Lives => new PersonAddressBuilder(person);
    public PersonJobBuilder Works => new PersonJobBuilder(person);

    public static implicit operator Person(PersonBuilder pb)
    {
        return pb.person;
    }
}

public class PersonJobBuilder : PersonBuilder
{
    public PersonJobBuilder(Person person)
    {
        this.person = person;
    }

    public PersonJobBuilder At(string companyName)
    {
        person.CompanyName = companyName;
        return this;
    }

    public PersonJobBuilder AsA(string position)
    {
        person.Position = position;
        return this;
    }

    public PersonJobBuilder Earning(int annualIncome)
    {
        person.AnnualIncome = annualIncome;
        return this;
    }
}

public class PersonAddressBuilder : PersonBuilder
{
    // might not work with a value type!
    public PersonAddressBuilder(Person person)
    {
        this.person = person;
    }

    public PersonAddressBuilder At(string streetAddress)
    {
        person.StreetAddress = streetAddress;
        return this;
    }

    public PersonAddressBuilder WithPostcode(string postcode)
    {
        person.Postcode = postcode;
        return this;
    }

    public PersonAddressBuilder In(string city)
    {
        person.City = city;
        return this;
    }
}

public class Demo
{
    static void Main(string[] args)
    {
        var pb = new PersonBuilder();
        Person person = pb
          .Lives
            .At("123 London Road")
            .In("London")
            .WithPostcode("SW12BC")
          .Works
            .At("Fabrikam")
            .AsA("Engineer")
            .Earning(123000);

        Console.WriteLine(person);
    }
}
```

---

#### Factories ####
A component responsible solely for the wholesale (not piecewise) creation of objects

* Factory method
  * overload with the same sets of arguments with different descriptive names
  * Violate Single Responsibility Principle since all methods in 1 class
  
```
//bad
public enum CoordinateSystem
{
    Cartesian,
    Polar
}

public class Point
{
    private double x, y;

    public Point(double a, double b, CoordinateSystem cs = CoordinateSystem.Cartesian)
    {
        switch (cs)
        {
            case CoordinateSystem.Polar:
                x = a * Math.Cos(b);
                y = a * Math.Sin(b);
                break;
            default:
                x = a;
                y = b;
                break;
        }
    }
}

//good
public class Point
{
    private double x, y;

    private Point(double x, double y)
    {
        this.x = x;
        this.y = y;
    }

    public static Point NewCartesianPoint(double x, double y)
    {
        return new Point(x, y);
    }

    public static Point NewPolarPoint(double rho, double theta)
    {
        return new Point(rho * Math.Cos(theta), rho * Math.Sin(theta));
    }
}
```

---

* Factory
  
```
public class Point
{
    private double x, y;
    
    // user still able to create point object
    public Point(double x, double y)
    {
        this.x = x;
        this.y = y;
    }
}

public static class PointFactory
{
    public static Point NewCartesianPoint(double x, double y)
    {
        return new Point(x, y);
    }

    public static Point NewPolarPoint(double rho, double theta)
    {
        return new Point(rho * Math.Cos(theta), rho * Math.Sin(theta));
    }
}
```

---

* Inner Factory
  * Only allows user create object through factory
  * E.g. Task.Factory.x

```
public class Point
{
    private double x, y;

    // factory property
    public static Point Origin => new Point(0, 0);

    // singleton field
    public static Point Origin2 = new Point(0, 0);

    private Point(double x, double y)
    {
        this.x = x;
        this.y = y;
    }

    public static class Factory
    {
        public static Point NewCartesianPoint(double x, double y)
        {
            return new Point(x, y);
        }

        public static Point NewPolarPoint(double rho, double theta)
        {
            return new Point(rho * Math.Cos(theta), rho * Math.Sin(theta));
        }
    }
}
```

---

* Abstract Factory

```
public interface IHotDrink
{
    void Consume();
}

internal class Tea : IHotDrink
{
    public void Consume()
    {
        Console.WriteLine("This tea is nice but I'd prefer it with milk.");
    }
}

internal class Coffee : IHotDrink
{
    public void Consume()
    {
        Console.WriteLine("This coffee is delicious!");
    }
}

public interface IHotDrinkFactory
{
    IHotDrink Prepare(int amount);
}

internal class TeaFactory : IHotDrinkFactory
{
    public IHotDrink Prepare(int amount)
    {
        Console.WriteLine($"Put in tea bag, boil water, pour {amount} ml, add lemon, enjoy!");
        return new Tea();
    }
}

internal class CoffeeFactory : IHotDrinkFactory
{
    public IHotDrink Prepare(int amount)
    {
        Console.WriteLine($"Grind some beans, boil water, pour {amount} ml, add cream and sugar, enjoy!");
        return new Coffee();
    }
}

//bad
public class HotDrinkMachine
{
    // violates open-closed
    public enum AvailableDrink
    {
        Coffee,
        Tea
    }

    private Dictionary<AvailableDrink, IHotDrinkFactory> factories =
      new Dictionary<AvailableDrink, IHotDrinkFactory>();

    public HotDrinkMachine()
    {
        foreach (AvailableDrink drink in Enum.GetValues(typeof(AvailableDrink)))
        {
            var factory = (IHotDrinkFactory)Activator.CreateInstance(
              Type.GetType(Enum.GetName(typeof(AvailableDrink), drink) + "Factory"));
            factories.Add(drink, factory);
        }
    }

    public IHotDrink MakeDrink(AvailableDrink drink, int amount)
    {
        return factories[drink].Prepare(amount);
    }
}

//good (with OOP)
public class HotDrinkMachine
{
    private List<Tuple<string, IHotDrinkFactory>> namedFactories =
        new List<Tuple<string, IHotDrinkFactory>>();

    public HotDrinkMachine()
    {
        foreach (var t in typeof(HotDrinkMachine).Assembly.GetTypes())
        {
            if (typeof(IHotDrinkFactory).IsAssignableFrom(t) && !t.IsInterface)
            {
                namedFactories.Add(Tuple.Create(
                    t.Name.Replace("Factory", string.Empty), 
                    (IHotDrinkFactory)Activator.CreateInstance(t)));
            }
        }
    }

    public IHotDrink MakeDrink()
    {
        Console.WriteLine("Available drinks");

        // index with name string for UI display 
        for (var index = 0; index < namedFactories.Count; index++)
        {
            var tuple = namedFactories[index];
            Console.WriteLine($"{index}: {tuple.Item1}");
        }
    }
}
```

---

#### Prototype ####
We make a copy (clone) the prototype and customize it (Requires "deep copy" support)

* ICloneable (Should not use for deep copy)
  * If check with Clone() in array, it will returns shallow copy of the object
  
```
public class Person : ICloneable
{
    public readonly string[] Names;
    public readonly Address Address;

    public Person(string[] names, Address address)
    {
        Names = names;
        Address = address;
    }

    public override string ToString()
    {
        return $"{nameof(Names)}: {string.Join(",", Names)}, {nameof(Address)}: {Address}";
    }
    
    // bad return object type
    public object Clone()
    {
        return new Person(Names, (Address)Address.Clone());
    }
}

public class Address : ICloneable
{
    public readonly string StreetName;
    public int HouseNumber;

    public Address(string streetName, int houseNumber)
    {
        StreetName = streetName;
        HouseNumber = houseNumber;
    }

    public override string ToString()
    {
        return $"{nameof(StreetName)}: {StreetName}, {nameof(HouseNumber)}: {HouseNumber}";
    }

    public object Clone()
    {
        return new Address(StreetName, HouseNumber);
    }
}
```

---

* Copy Constructor
  * Tedious when have a deep hierarchy of objects (10 different classes)
  
```
public class Person
{
    public readonly string[] Names;
    public readonly Address Address;

    public Person(string[] names, Address address)
    {
        Names = names;
        Address = address;
    }

    public Person(Person other)
    {
        Names = other.Names;
        Address = new Address(other.Address);
    }

    public override string ToString()
    {
        return $"{nameof(Names)}: {string.Join(",", Names)}, {nameof(Address)}: {Address}";
    }
}

public class Address
{
    public readonly string StreetName;
    public int HouseNumber;

    public Address(string streetName, int houseNumber)
    {
        StreetName = streetName;
        HouseNumber = houseNumber;
    }

    public Address(Address other)
    {
        StreetName = other.streetName;
        HouseNumber = other.houseNumber;
    }

    public override string ToString()
    {
        return $"{nameof(StreetName)}: {StreetName}, {nameof(HouseNumber)}: {HouseNumber}";
    }
}
```

---

* Explicit Deep Copy Interface
  * Tedious when have a deep hierarchy of objects (10 different classes)
  
```
ublic interface IPrototype<T>
{
    T DeepCopy();
}

public class Person : IPrototype<Person>
{
    public readonly string[] Names;
    public readonly Address Address;

    public Person(string[] names, Address address)
    {
        Names = names;
        Address = address;
    }

    public override string ToString()
    {
        return $"{nameof(Names)}: {string.Join(",", Names)}, {nameof(Address)}: {Address}";
    }

    public Person DeepCopy()
    {
        return new Person(Names, Address.DeepCopy());
    }
}

public class Address : IPrototype<Address>
{
    public readonly string StreetName;
    public int HouseNumber;

    public Address(string streetName, int houseNumber)
    {
        StreetName = streetName;
        HouseNumber = houseNumber;
    }

    public override string ToString()
    {
        return $"{nameof(StreetName)}: {StreetName}, {nameof(HouseNumber)}: {HouseNumber}";
    }

    public Address DeepCopy()
    {
        return new Address(StreetName, HouseNumber);
    }
}
```

* Copy Through Serialization

```
public static class ExtensionMethods
{
    public static T DeepCopy<T>(this T self)
    {
        MemoryStream stream = new MemoryStream();
        BinaryFormatter formatter = new BinaryFormatter();
        formatter.Serialize(stream, self);
        stream.Seek(0, SeekOrigin.Begin);
        object copy = formatter.Deserialize(stream);
        stream.Close();
        return (T)copy;
    }

    public static T DeepCopyXml<T>(this T self)
    {
        using (var ms = new MemoryStream())
        {
            XmlSerializer s = new XmlSerializer(typeof(T));
            s.Serialize(ms, self);
            ms.Position = 0;
            return (T)s.Deserialize(ms);
        }
    }
}
```

---

* Singleton

```
```

---
