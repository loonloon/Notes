#### Overview ####
![image](https://user-images.githubusercontent.com/5309726/101895492-9269ff80-3be2-11eb-9cb4-bf8c3d05eed3.png)

#### The SOLID Design Principles ####
* Single Responsibility Principle

```
//bad
public class Journal
{
    private readonly List<string> entries = new List<string>();
    private static int _count;

    public int AddEntry(string text)
    {
        entries.Add($"{++_count}: {text}");
        return _count; // memento pattern!
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
    private static int _count;

    public int AddEntry(string text)
    {
        entries.Add($"{++_count}: {text}");
        return _count; // memento pattern!
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
    public string Name { get; }
    public Color Color { get; }
    public Size Size { get; }

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
    private readonly Color color;

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
    private readonly Size size;

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
    private readonly ISpecification<T> first;
    private readonly ISpecification<T> second;

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
        set => base.Width = base.Height = value;
    }

    public new int Height
    {
        set => base.Width = base.Height = value;
    }
}

public class Demo
{
    public static int Area(Rectangle r) => r.Width * r.Height;

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
        set => base.Width = base.Height = value;
    }

    public override int Height
    {
        set => base.Width = base.Height = value;
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
    private readonly double x;
    private readonly double y;

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
    private readonly double x;
    private readonly double y;

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
    private readonly double x;
    private readonly double y;

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
    private readonly double x;
    private readonly double y;

    // factory property
    public static Point Origin { get; } = new Point(0, 0);

    // singleton field
    public static Point Origin2 { get; } = new Point(0, 0);

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

    private readonly Dictionary<AvailableDrink, IHotDrinkFactory> factories =
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
    private readonly List<Tuple<string, IHotDrinkFactory>> namedFactories =
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
            Console.WriteLine($@"{index}: {tuple.Item1}");
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
    public string[] Names { get; }
    public Address Address { get; }

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
    public string StreetName { get; }
    public int HouseNumber { get; }

    public Address(string streetName, int houseNumber)
    {
        StreetName = streetName;
        HouseNumber = houseNumber;
    }

    public Address(Address other)
    {
        StreetName = other.StreetName;
        HouseNumber = other.HouseNumber;
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
public interface IPrototype<T>
{
    T DeepCopy();
}

public class Person : IPrototype<Person>
{
    public string[] Names { get; }
    public Address Address { get; }

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
    public string StreetName { get; }
    public int HouseNumber { get; }

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
        var stream = new MemoryStream();
        var formatter = new BinaryFormatter();
        formatter.Serialize(stream, self);
        stream.Seek(0, SeekOrigin.Begin);
        var copy = formatter.Deserialize(stream);
        stream.Close();
        return (T)copy;
    }

    public static T DeepCopyXml<T>(this T self)
    {
        XmlSerializer s = new XmlSerializer(typeof(T));
        using (var ms = new MemoryStream())
        {
            s.Serialize(ms, self);
            ms.Position = 0;
            return (T)s.Deserialize(ms);
        }
    }
}
```

---

#### Singleton ####

```
public interface IDatabase
{
    int GetPopulation(string name);
}

public class SingletonDatabase : IDatabase
{
    private readonly Dictionary<string, int> capitals;
    // laziness + thread safety
    private static Lazy<SingletonDatabase> _instance = new Lazy<SingletonDatabase>(() =>
    {
        Count++;
        return new SingletonDatabase();
    });

    public static int Count { get; private set; }
    public static IDatabase Instance => _instance.Value;

    private SingletonDatabase()
    {
        Console.WriteLine("Initializing database");

        capitals = File.ReadAllLines(Path.Combine(new FileInfo(typeof(IDatabase).Assembly.Location).DirectoryName, "capitals.txt"))
            .Batch(2)
            .ToDictionary(list => list.ElementAt(0).Trim(), list => int.Parse(list.ElementAt(1)));
    }

    public int GetPopulation(string name)
    {
        return capitals[name];
    }
}
```

---

* Testability Issues

```
public class SingletonRecordFinder
{
    public int TotalPopulation(IEnumerable<string> names)
    {
        // hardcoded reference, unable to subsitute something
        return names.Aggregate(0, (current, name) => current + SingletonDatabase.Instance.GetPopulation(name));
    }
}
```

---

* Singleton in Dependency Injection

```
public class ConfigurableRecordFinder
{
    private IDatabase database;

    public ConfigurableRecordFinder(IDatabase database)
    {
        this.database = database;
    }

    public int GetTotalPopulation(IEnumerable<string> names)
    {
        return names.Aggregate(0, (current, name) => current + database.GetPopulation(name));
    }
}
```

----

* Monostate
  * Allows create many objects with shared state

```
public class ChiefExecutiveOfficer
{
    private static string _name;
    private static int _age;

    public string Name
    {
        get => _name;
        set => _name = value;
    }

    public int Age
    {
        get => _age;
        set => _age = value;
    }

    public override string ToString()
    {
        return $"{nameof(Name)}: {Name}, {nameof(Age)}: {Age}";
    }
}

public class Demo
{
    static void Main(string[] args)
    {
        var ceo = new ChiefExecutiveOfficer { Name = "Adam Smith", Age = 55 };

        var ceo2 = new ChiefExecutiveOfficer();
        Console.WriteLine(ceo2);
    }
}
```

---

* Adapter
A construct which adapts an existing interface X to conform (符合) to the required interface Y

```
//problem
public class Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public override string ToString()
    {
        return $"{nameof(X)}: {X}, {nameof(Y)}: {Y}";
    }
}

public class Line
{
    public Point Start { get; set; }
    public Point End { get; set; }

    public Line(Point start, Point end)
    {
        Start = start;
        End = end;
    }
}

public class VectorObject : Collection<Line>
{
}

public class VectorRectangle : VectorObject
{
    public VectorRectangle(int x, int y, int width, int height)
    {
        Add(new Line(new Point(x, y), new Point(x + width, y)));
        Add(new Line(new Point(x + width, y), new Point(x + width, y + height)));
        Add(new Line(new Point(x, y), new Point(x, y + height)));
        Add(new Line(new Point(x, y + height), new Point(x + width, y + height)));
    }
}

public class Demo
{
    private static readonly List<VectorObject> vectorObjects = new List<VectorObject>
    {
        new VectorRectangle(1, 1, 10, 10),
        new VectorRectangle(3, 3, 6, 6)
    };

    // the interface we have
    public static void DrawPoint(Point p)
    {
        Console.Write("...");
    }

    static void Main(string[] args)
    {
        // if called twice, will regenerate again
        Draw();
        Draw();
    }
}

//fix
public class LineToPointAdapter : Collection<Point>
{
    private static int _count;

    public LineToPointAdapter(Line line)
    {
        Console.WriteLine($@"{++_count}: Generating points for line [{line.Start.X},{line.Start.Y}]-[{line.End.X},{line.End.Y}] (no caching)");

        var left = Math.Min(line.Start.X, line.End.X);
        var right = Math.Max(line.Start.X, line.End.X);
        var top = Math.Min(line.Start.Y, line.End.Y);
        var bottom = Math.Max(line.Start.Y, line.End.Y);
        var dx = right - left;
        var dy = line.End.Y - line.Start.Y;

        if (dx == 0)
        {
            for (var y = top; y <= bottom; ++y)
            {
                Add(new Point(left, y));
            }
        }
        else if (dy == 0)
        {
            for (var x = left; x <= right; ++x)
            {
                Add(new Point(x, top));
            }
        }
    }
}

private static void Draw()
{
    foreach (var vo in vectorObjects)
    {
        foreach (var line in vo)
        {
            var adapter = new LineToPointAdapter(line);
            adapter.ForEach(DrawPoint);
        }
    }
}
```

---

* Adapter Caching

```
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected bool Equals(Point other)
    {
        return X == other.X && Y == other.Y;
    }

    public override bool Equals(object obj)
    {
        if (obj is null)
        {
            return false;
        }

        if (ReferenceEquals(this, obj))
        {
            return true;
        }

        return obj.GetType() == GetType() && Equals((Point)obj);
    }

    public override int GetHashCode()
    {
        unchecked
        {
            return (X * 397) ^ Y;
        }
    }

    public override string ToString()
    {
        return $"({X}, {Y})";
    }
}

public class Line
{
    public Point Start { get; }
    public Point End { get; }

    public Line(Point start, Point end)
    {
        Start = start;
        End = end;
    }

    protected bool Equals(Line other)
    {
        return Equals(Start, other.Start) && Equals(End, other.End);
    }

    public override bool Equals(object obj)
    {
        if (obj is null)
        {
            return false;
        }

        if (ReferenceEquals(this, obj))
        {
            return true;
        }

        return obj.GetType() == GetType() && Equals((Line)obj);
    }

    public override int GetHashCode()
    {
        unchecked
        {
            return ((Start != null ? Start.GetHashCode() : 0) * 397) ^ (End != null ? End.GetHashCode() : 0);
        }
    }
}

public abstract class VectorObject : Collection<Line>
{
}

public class VectorRectangle : VectorObject
{
    public VectorRectangle(int x, int y, int width, int height)
    {
        Add(new Line(new Point(x, y), new Point(x + width, y)));
        Add(new Line(new Point(x + width, y), new Point(x + width, y + height)));
        Add(new Line(new Point(x, y), new Point(x, y + height)));
        Add(new Line(new Point(x, y + height), new Point(x + width, y + height)));
    }
}

public class LineToPointAdapter : IEnumerable<Point>
{
    private static int _count;
    private static readonly Dictionary<int, List<Point>> cache = new Dictionary<int, List<Point>>();
    private readonly int hash;

    public LineToPointAdapter(Line line)
    {
        hash = line.GetHashCode();

        if (cache.ContainsKey(hash))
        {
            return;
        }

        Console.WriteLine($@"{++_count}: Generating points for line [{line.Start.X},{line.Start.Y}]-[{line.End.X},{line.End.Y}] (with caching)");

        var points = new List<Point>();
        var left = Math.Min(line.Start.X, line.End.X);
        var right = Math.Max(line.Start.X, line.End.X);
        var top = Math.Min(line.Start.Y, line.End.Y);
        var bottom = Math.Max(line.Start.Y, line.End.Y);
        var dx = right - left;
        var dy = line.End.Y - line.Start.Y;

        if (dx == 0)
        {
            for (var y = top; y <= bottom; ++y)
            {
                points.Add(new Point(left, y));
            }
        }
        else if (dy == 0)
        {
            for (var x = left; x <= right; ++x)
            {
                points.Add(new Point(x, top));
            }
        }

        cache.Add(hash, points);
    }

    public IEnumerator<Point> GetEnumerator()
    {
        return cache[hash].GetEnumerator();
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}


public class Demo
{
    private static readonly List<VectorObject> VectorObjects = new List<VectorObject>
    {
      new VectorRectangle(1, 1, 10, 10),
      new VectorRectangle(3, 3, 6, 6)
    };

    // the interface we have
    public static void DrawPoint(Point p)
    {
        Console.Write(".");
    }

    static void Main(string[] args)
    {
        Draw();
        Draw();
    }

    private static void Draw()
    {
        foreach (var vo in VectorObjects)
        {
            foreach (var line in vo)
            {
                var adapter = new LineToPointAdapter(line);
                adapter.ForEach(DrawPoint);
            }
        }
    }
}
```

---

#### Bridge ####
  * A mechanism that decouples an interface (hierarchy) from an implementation (hierarchy)
  * Prevents a 'Cartesian product' complexity explosion
    * Base class ThreadScheduler, can be preemptive / cooperative, can run on Windows / Unix. End up with a 2 x 2 scenario
    
Before

![image](https://user-images.githubusercontent.com/5309726/102063349-e2d69c80-3e30-11eb-9959-74bf0f4d837f.png)

After

![image](https://user-images.githubusercontent.com/5309726/102063762-66908900-3e31-11eb-9339-4f10e822a2ea.png)

```
public interface IRenderer
{
    void RenderCircle(float radius);
}

public class VectorRenderer : IRenderer
{
    public void RenderCircle(float radius)
    {
        Console.WriteLine($@"Drawing a circle of radius {radius}");
    }
}

public class RasterRenderer : IRenderer
{
    public void RenderCircle(float radius)
    {
        Console.WriteLine($@"Drawing pixels for circle of radius {radius}");
    }
}

public abstract class Shape
{
    protected IRenderer renderer;

    // a bridge between the shape that's being drawn an
    // the component which actually draws it
    protected Shape(IRenderer renderer)
    {
        this.renderer = renderer;
    }

    public abstract void Draw();
    public abstract void Resize(float factor);
}

public class Circle : Shape
{
    private float radius;

    public Circle(IRenderer renderer, float radius) : base(renderer)
    {
        this.radius = radius;
    }

    public override void Draw()
    {
        renderer.RenderCircle(radius);
    }

    public override void Resize(float factor)
    {
        radius *= factor;
    }
}

public class Demo
{
    static void Main(string[] args)
    {
        var raster = new RasterRenderer();
        var vector = new VectorRenderer();
        var circle = new Circle(vector, 5, 5, 5);

        circle.Draw();
        circle.Resize(2);
        circle.Draw();
    }
}
```

---

#### Decorator ####
Facilitates the addition of behaviors to individual objects without inheriting from them

```
public class CodeBuilder
{
    private readonly StringBuilder _builder = new StringBuilder();

    public override string ToString()
    {
        return _builder.ToString();
    }

    public void GetObjectData(SerializationInfo info, StreamingContext context)
    {
        ((ISerializable)_builder).GetObjectData(info, context);
    }

    public int EnsureCapacity(int capacity)
    {
        return _builder.EnsureCapacity(capacity);
    }

    public string ToString(int startIndex, int length)
    {
        return _builder.ToString(startIndex, length);
    }

    public CodeBuilder Clear()
    {
        _builder.Clear();
        return this;
    }
    
    // etc
}

public class Demo
{
    static void Main(string[] args)
    {
        var cb = new CodeBuilder();
        cb.AppendLine("class Foo")
          .AppendLine("{")
          .AppendLine("}");
        Console.WriteLine(cb);
    }
}
```

---

* Adapter Decorator

```
public class MyStringBuilder
{
    private readonly StringBuilder _sb = new StringBuilder();

    //=============================================

    public static implicit operator MyStringBuilder(string s)
    {
        var msb = new MyStringBuilder();
        msb._sb.Append(s);
        return msb;
    }

    public static MyStringBuilder operator +(MyStringBuilder msb, string s)
    {
        msb.Append(s);
        return msb;
    }

    public override string ToString()
    {
        return _sb.ToString();
    }

    //=============================================

    public void GetObjectData(SerializationInfo info, StreamingContext context)
    {
        ((ISerializable)_sb).GetObjectData(info, context);
    }

    public int EnsureCapacity(int capacity)
    {
        return _sb.EnsureCapacity(capacity);
    }

    public string ToString(int startIndex, int length)
    {
        return _sb.ToString(startIndex, length);
    }

    public StringBuilder Clear()
    {
        return _sb.Clear();
    }
    
    // etc
}

public class Demo
{
    static void Main(string[] args)
    {
        MyStringBuilder s = "hello ";
        s += "world"; // will work even without op+ in MyStringBuilder
                      // why? you figure it out!
        Console.WriteLine(s);
    }
}
```

---

* Multiple Inheritance

```
public class Bird
{
    public void Fly()
    {
    }
}

public class Lizard
{
    public void Crawl()
    {
    }
}

public class Dragon // no multiple inheritance
{
    private readonly Bird _bird;
    private readonly Lizard _lizard;

    public Dragon(Bird bird, Lizard lizard)
    {
        _bird = bird ?? throw new ArgumentNullException(paramName: nameof(bird));
        _lizard = lizard ?? throw new ArgumentNullException(paramName: nameof(lizard));
    }

    public void Crawl()
    {
        _lizard.Crawl();
    }

    public void Fly()
    {
        _bird.Fly();
    }
}
```

---

* Dynamic Decorator

```
public abstract class Shape
{
    public virtual string AsString() => string.Empty;
}

public class Circle : Shape
{
    private float radius;

    public Circle() : this(0)
    {
    }

    public Circle(float radius)
    {
        this.radius = radius;
    }

    public void Resize(float factor)
    {
        radius *= factor;
    }

    public override string AsString() => $"A circle of radius {radius}";
}

public class Square : Shape
{
    private readonly float side;

    public Square() : this(0)
    {
    }

    public Square(float side)
    {
        this.side = side;
    }

    public override string AsString() => $"A square with side {side}";
}

public class ColoredShape : Shape
{
    private readonly Shape shape;
    private readonly string color;

    public ColoredShape(Shape shape, string color)
    {
        this.shape = shape ?? throw new ArgumentNullException(paramName: nameof(shape));
        this.color = color ?? throw new ArgumentNullException(paramName: nameof(color));
    }

    public override string AsString() => $"{shape.AsString()} has the color {color}";
}

public class TransparentShape : Shape
{
    private readonly Shape shape;
    private readonly float transparency;

    public TransparentShape(Shape shape, float transparency)
    {
        this.shape = shape ?? throw new ArgumentNullException(paramName: nameof(shape));
        this.transparency = transparency;
    }

    public override string AsString() => $"{shape.AsString()} has {transparency * 100.0f} transparency";
}

public class Demo
{
    static void Main(string[] args)
    {
        var square = new Square(1.23f);
        Console.WriteLine(square.AsString());

        var redSquare = new ColoredShape(square, "red");
        Console.WriteLine(redSquare.AsString());

        var redHalfTransparentSquare = new TransparentShape(redSquare, 0.5f);
        Console.WriteLine(redHalfTransparentSquare.AsString());
    }
}
```

---

* Static Decorator Composition

```
public abstract class Shape
{
    public virtual string AsString() => string.Empty;
}

public class Circle : Shape
{
    private float radius;

    public Circle() : this(0)
    {
    }

    public Circle(float radius)
    {
        this.radius = radius;
    }

    public void Resize(float factor)
    {
        radius *= factor;
    }

    public override string AsString() => $"A circle of radius {radius}";
}

public class Square : Shape
{
    private readonly float side;

    public Square() : this(0)
    {
    }

    public Square(float side)
    {
        this.side = side;
    }

    public override string AsString() => $"A square with side {side}";
}

public class ColoredShape<T> : Shape where T : Shape, new()
{
    private readonly string color;
    private readonly T shape = new T();

    public ColoredShape() : this("black")
    {
    }

    public ColoredShape(string color) // no constructor forwarding
    {
        this.color = color ?? throw new ArgumentNullException(paramName: nameof(color));
    }

    public override string AsString()
    {
        return $"{shape.AsString()} has the color {color}";
    }
}

public class TransparentShape<T> : Shape where T : Shape, new()
{
    private readonly float transparency;
    private readonly T shape = new T();

    public TransparentShape(float transparency)
    {
        this.transparency = transparency;
    }

    public override string AsString()
    {
        return $"{shape.AsString()} has transparency {transparency * 100.0f}";
    }
}

public class Demo
{
    static void Main(string[] args)
    {
        var square = new Square(1.23f);
        Console.WriteLine(square.AsString());

        var blueCircle = new ColoredShape<Circle>("blue");
        Console.WriteLine(blueCircle.AsString());

        // unable to set property, not working in C#
        var blackHalfSquare = new TransparentShape<ColoredShape<Square>>(0.4f);
        Console.WriteLine(blackHalfSquare.AsString());
    }
}
```

---

#### Facade ####
Provides a simple, easy to understand / user interface over a large and sophisticated body of code

---

#### Flyweight ####

---
