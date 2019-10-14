https://industriallogic.com/xp/refactoring/chainConstructors.html

#### Chain Constructors ####

* Motivation
  * Code that’s duplicated across two or more of a class's constructors is an invitation for trouble.

* Example
```
//Before
public Loan(float notional, float outstanding, int rating, DateTime expiry)
{
    _strategy = new TermROC();
    _notional = notional;
    _outstanding = outstanding;
    _rating = rating;
    _expiry = expiry;
}

public Loan(float notional, float outstanding, int rating, DateTime expiry, DateTime maturity)
{
    _strategy = new RevolvingTermROC();
    _notional = notional;
    _outstanding = outstanding;
    _rating = rating;
    _expiry = expiry;
    _maturity = maturity;
}

public Loan(CapitalStrategy strategy, float notional, float outstanding, int rating,
    DateTime expiry, DateTime? maturity)
{
    _strategy = strategy;
    _notional = notional;
    _outstanding = outstanding;
    _rating = rating;
    _expiry = expiry;
    _maturity = maturity;
}

//After
public Loan(float notional, float outstanding, int rating, DateTime expiry)
    : this(new TermROC(), notional, outstanding, rating, expiry, null)
{
}

public Loan(float notional, float outstanding, int rating, DateTime expiry, DateTime maturity)
    : this(new RevolvingTermROC(), notional, outstanding, rating, expiry, maturity)
{
}

public Loan(CapitalStrategy strategy, float notional, float outstanding, int rating,
    DateTime expiry, DateTime? maturity)
{
    _strategy = strategy;
    _notional = notional;
    _outstanding = outstanding;
    _rating = rating;
    _expiry = expiry;
    _maturity = maturity;
}
```

#### Replace Multiple Constructors with Creation Methods ####
* Motivation
  * If you have only one constructor, this isn’t a problem. But if you have multiple constructors, programmers will have to choose which constructor to call by studying which parameters are expected and/or poking around at the constructor code.
  
* Example
```
//Before
public class Loan
{
    private const string TermLoan = "TL";
    private const string Revolver = "RC";
    private const string Rctl = "RCTL";
    private string _type;
    private CapitalStrategy _strategy;
    private float _notional;
    private float _outstanding;
    private int _rating;
    private DateTime _expiry;
    private DateTime? _maturity;

    public Loan(float notional, float outstanding, int customerRating, DateTime expiry) : this(TermLoan,
        new TermROC(), notional, outstanding,
        customerRating, expiry, null)
    {
    }

    public Loan(float notional, float outstanding, int customerRating, DateTime expiry, DateTime maturity) : this(
        Rctl, new RevolvingTermROC(), notional, outstanding, customerRating,
        expiry, maturity)
    {
    }

    public Loan(CapitalStrategy strategy, float notional, float outstanding,
        int customerRating, DateTime expiry, DateTime maturity) : this(Rctl, strategy, notional, outstanding,
        customerRating,
        expiry, maturity)
    {
    }

    public Loan(string type, CapitalStrategy strategy, float notional,
        float outstanding, int customerRating, DateTime expiry) : this(type, strategy, notional, outstanding,
        customerRating, expiry, null)
    {
    }

    public Loan(string type, CapitalStrategy strategy, float notional,
        float outstanding, int customerRating, DateTime expiry, DateTime? maturity)
    {
        _type = type;
        _strategy = strategy;
        _notional = notional;
        _outstanding = outstanding;
        _expiry = expiry;

        if (Rctl.Equals(type))
        {
            _maturity = maturity;
        }
    }
}

//After
public class Loan
{
    private const string TermLoan = "TL";
    private const string Revolver = "RC";
    private const string Rctl = "RCTL";
    private string _type;
    private CapitalStrategy _strategy;
    private float _notional;
    private float _outstanding;
    private int _rating;
    private DateTime _expiry;
    private DateTime? _maturity;

    public Loan(string type, CapitalStrategy strategy, float notional, float outstanding, int customerRating,
        DateTime expiry, DateTime? maturity)
    {
        _type = type;
        _strategy = strategy;
        _notional = notional;
        _outstanding = outstanding;
        _expiry = expiry;

        if (Rctl.Equals(type)) _maturity = maturity;
    }

    private static Loan NewTermLoan(float notional, float outstanding, int customerRating, DateTime expiry)
    {
        return new Loan(TermLoan, new TermROC(), notional, outstanding, customerRating, expiry, null);
    }

    private static Loan NewTermWithStrategy(CapitalStrategy strategy, float notional, float outstanding,
        int customerRating, DateTime expiry)
    {
        return new Loan(TermLoan, strategy, notional, outstanding, customerRating, expiry, null);
    }

    private static Loan NewRevolver(float notional, float outstanding, int customerRating, DateTime expiry)
    {
        return new Loan(Revolver, new RevolverROC(), notional, outstanding, customerRating, expiry, null);
    }

    private static Loan NewRevolverWithStrategy(CapitalStrategy strategy, float notional, float outstanding,
        int customerRating, DateTime expiry)
    {
        return new Loan(Revolver, strategy, notional, outstanding, customerRating, expiry, null);
    }

    private static Loan NewRctl(float notional, float outstanding, int customerRating, DateTime expiry,
        DateTime maturity)
    {
        return new Loan(Rctl, new RctlROC(), notional, outstanding, customerRating, expiry, maturity);
    }

    private static Loan NewRctlWithStrategy(CapitalStrategy strategy, float notional, float outstanding,
        int customerRating, DateTime expiry, DateTime maturity)
    {
        return new Loan(Rctl, strategy, notional, outstanding, customerRating, expiry, maturity);
    }
}
```

#### Encapsulate Subclasses with Creation Methods ####
* Motivation
  * To hide implementation details from those whodon’t need to know about them
  
  ![image-01](https://user-images.githubusercontent.com/5309726/66643442-496e9200-ec51-11e9-8db4-b361fd41385a.png)
* Example
```
//Before
public abstract class Loan
{
    ...
    protected Loan(...)
    {
    }
}

public class TermLoan : Loan
{
    public TermLoan(...)
        : base(...)
    {
    }
}

public class Revolver : Loan
{
    public Revolver(...) 
        : base(...)
    {
    }
}

public class RCTL : Loan
{
    public RCTL(...) 
        : base(...)
    {
    }
}

//After
public abstract class Loan
{
    public static Loan NewTermLoan(...)
    {
        return new TermLoan(...);
    }
}

public class TermLoan : Loan
{
    protected TermLoan(...)
        : base(...)
    {
    }
}

client code...
var termLoan = Loan.NewTermLoan(1000f, 250f, getCustRating(), expiryDate);
```

#### Extract Creation Class ####
* Motivation
  * There’s nothing wrong with a few Creation Methods on a class, but as the number of them grows, a class’s own primary responsibilities – it’s main purpose in life – can feel like it is obscured, outweighed by too much creational code. At such times, it’s best to restore the class’s identity by extracting all of its Creation Methods into their own home. We call such a home a Creation Class.

* Example
```
//Before
public class Loan
{
    private double _notional;
    private double _outstanding;
    private int _rating;
    private DateTime _start;
    private readonly CapitalStrategy _capitalStrategy;
    private DateTime? _expiry;
    private DateTime? _maturity;

    // . . . more instances variables not shown
    protected Loan(double notional, DateTime start, DateTime? expiry,
        DateTime? maturity, int riskRating, CapitalStrategy strategy)
    {
        _notional = notional;
        _start = start;
        _expiry = expiry;
        _maturity = maturity;
        _rating = riskRating;
        _capitalStrategy = strategy;
    }

    public double CalcCapital()
    {
        return _capitalStrategy.Calc(this);
    }

    public void SetOutstanding(double newOutstanding)
    {
        _outstanding = newOutstanding;
    }

    // ... more methods for dealing with the primary responsibilities of a Loan, not shown
    public static Loan NewAdvisor(double notional, DateTime start, DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewLetterOfCredit(double notional, DateTime start,
        DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewRctl(double notional, DateTime start,
        DateTime expiry, DateTime maturity, int rating)
    {
        return new Loan(notional, start, expiry, maturity, rating, new RCTLCapital());
    }

    public static Loan NewRevolver(double notional, DateTime start,
        DateTime expiry, int rating)
    {
        return new Loan(notional, start, expiry, null, rating, new RevolverCapital());
    }

    public static Loan NewSplc(double notional, DateTime start,
        DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewTermLoan(double notional, DateTime start,
        DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewVariableLoan(double notional, DateTime start,
        DateTime expiry, DateTime maturity, int rating)
    {
        return new Loan(notional, start, expiry, maturity, rating, new RCTLCapital());
    }
}

//After
public class Loan
{
    private double _notional;
    private double _outstanding;
    private int _rating;
    private DateTime _start;
    private readonly CapitalStrategy _capitalStrategy;
    private DateTime? _expiry;
    private DateTime? _maturity;

    // . . . more instances variables not shown
    protected internal Loan(double notional, DateTime start, DateTime? expiry,
        DateTime? maturity, int riskRating, CapitalStrategy strategy)
    {
        _notional = notional;
        _start = start;
        _expiry = expiry;
        _maturity = maturity;
        _rating = riskRating;
        _capitalStrategy = strategy;
    }

    public double CalcCapital()
    {
        return _capitalStrategy.Calc(this);
    }

    public void SetOutstanding(double newOutstanding)
    {
        _outstanding = newOutstanding;
    }
}

public class LoanCreator
{
    public static Loan NewAdvisor(double notional, DateTime start, DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewLetterOfCredit(double notional, DateTime start,
        DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewRctl(double notional, DateTime start,
        DateTime expiry, DateTime maturity, int rating)
    {
        return new Loan(notional, start, expiry, maturity, rating, new RCTLCapital());
    }

    public static Loan NewRevolver(double notional, DateTime start,
        DateTime expiry, int rating)
    {
        return new Loan(notional, start, expiry, null, rating, new RevolverCapital());
    }

    public static Loan NewSplc(double notional, DateTime start,
        DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewTermLoan(double notional, DateTime start,
        DateTime maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }

    public static Loan NewVariableLoan(double notional, DateTime start,
        DateTime expiry, DateTime maturity, int rating)
    {
        return new Loan(notional, start, expiry, maturity, rating, new RCTLCapital());
    }
}

var termLoan = LoanCreator.newTermLoan(…);
```

#### Replace Conditional Calculations with Strategy ####
![image](https://user-images.githubusercontent.com/5309726/66653802-66b05a00-ec6b-11e9-9bd0-3cf4039b1158.png)

* Motivation
  * A lot of condition logic can obscure any calculation, even a simple one. When that happens, your calculation can be misunderstood by others and harder to maintain, debug and extend
  
* Example
```
//Before
public class Loan
{
    private const int MillisPerDay = 86400000;
    private double _unusedPercentage;
    public double Notional { get; set; }
    public DateTime? Start { get; set; }
    public DateTime? Expiry { get; set; }
    public DateTime? Maturity { get; set; }
    public int Rating { get; set; }
    public double Outstanding { get; private set; }
    
    public void SetOutstanding(double newOutstanding)
    {
        Outstanding = newOutstanding;
    }

    public double CalcCapital()
    {
        return RiskAmount() * Duration() * RiskFactor.forRiskRating(Rating);
    }

    private double CalcUnusedRiskAmount()
    {
        return (Notional - Outstanding) * _unusedPercentage;
    }

    private double Duration()
    {
        if (Expiry == null)
        {
            return ((Maturity.Value.Subtract(Start.Value)).Milliseconds / MillisPerDay) / 365;
        }

        if (Maturity == null)
        {
            return ((Expiry.Value.Subtract(Start.Value).Milliseconds) / MillisPerDay) / 365;
        }

        long millisToExpiry = Expiry.Value.Subtract(Start.Value).Milliseconds;
        long millisFromExpiryToMaturity = Maturity.Value.Subtract(Expiry.Value).Milliseconds;
        double revolverDuration = (millisToExpiry / MillisPerDay) / 365;
        double termDuration = (millisFromExpiryToMaturity / MillisPerDay) / 365;
        return revolverDuration + termDuration;
    }

    private double RiskAmount()
    {
        return _unusedPercentage != 1.00 ? Outstanding + CalcUnusedRiskAmount() : Outstanding;
    }

    private void SetUnusedPercentage()
    {
        if (Expiry != null && Maturity != null)
        {
            _unusedPercentage = Rating > 4 ? 0.95 : 0.50;
        }
        else if (Maturity != null)
        {
            _unusedPercentage = 1.00;
        }
        else if (Expiry != null)
        {
            _unusedPercentage = Rating > 4 ? 0.75 : 0.25;
        }
    }
}

//After
public class Loan
{
    public double Notional { get; set; }
    public DateTime? Start { get; set; }
    public DateTime? Expiry { get; set; }
    public DateTime? Maturity { get; set; }
    public int Rating { get; set; }
    public CapitalStrategy CapitalStrategy { get; set; }
    public double Outstanding { get; set; }

    protected Loan(double notional, DateTime? start, DateTime? expiry, DateTime? maturity, int riskRating, CapitalStrategy strategy)
    {
        Notional = notional;
        Start = start;
        Expiry = expiry;
        Maturity = maturity;
        Rating = riskRating;
        CapitalStrategy = strategy;
    }
    
    //can apply Extract Creation Class
    public static Loan NewRctl(double notional, DateTime? start, DateTime? expiry, DateTime? maturity, int rating)
    {
        return new Loan(notional, start, expiry, maturity, rating, new RCTLCapital());
    }

    public static Loan NewRevolver(double notional, DateTime? start, DateTime? expiry, int rating)
    {
        return new Loan(notional, start, expiry, null, rating, new RevolverCapital());
    }

    public static Loan NewTermLoan(double notional, DateTime? start, DateTime? maturity, int rating)
    {
        return new Loan(notional, start, null, maturity, rating, new TermLoanCapital());
    }
}

public abstract class CapitalStrategy
{
    protected Loan Loan;
    protected const int MillisPerDay = 86400000;
    private const int DaysPerYear = 365;

    public double Calc(Loan loan)
    {
        Loan = loan;
        return RiskAmount() * Duration() * RiskFactor.forRiskRating(loan.Rating);
    }

    protected double CalcDuration(DateTime? start, DateTime? end)
    {
        return ((end.Value.Subtract(start.Value).Milliseconds / MillisPerDay) / DaysPerYear);
    }

    protected abstract double Duration();
    protected abstract double RiskAmount();
}

public class TermLoanCapital : CapitalStrategy
{
    protected override double Duration()
    {
        return CalcDuration(Loan.Maturity, Loan.Start);
    }

    protected override double RiskAmount()
    {
        return Loan.Outstanding;
    }
}

public class RevolverCapital : CapitalStrategy
{
    private double CalcUnusedPercentage()
    {
        return Loan.Rating > 4 ? 0.75 : 0.25;
    }

    private double CalcUnusedRiskAmount()
    {
        return (Loan.Notional - Loan.Outstanding) * CalcUnusedPercentage();
    }

    protected override double Duration()
    {
        return CalcDuration(Loan.Expiry, Loan.Start);
    }

    protected override double RiskAmount()
    {
        return Loan.Outstanding + CalcUnusedRiskAmount();
    }
}

public class RCTLCapital : CapitalStrategy
{
    private double CalcUnusedPercentage()
    {
        return Loan.Rating > 4 ? 0.95 : 0.50;
    }

    private double CalcUnusedRiskAmount()
    {
        return (Loan.Notional - Loan.Outstanding) * CalcUnusedPercentage();
    }

    protected override double Duration()
    {
        var revolverDuration = CalcDuration(Loan.Expiry, Loan.Start);
        var termDuration = CalcDuration(Loan.Maturity, Loan.Expiry);
        return revolverDuration + termDuration;
    }

    protected override double RiskAmount()
    {
        return Loan.Outstanding + CalcUnusedRiskAmount();
    }
}
```

#### Replace Implicit Tree with Composite* ####
* Motivation
  * One problem with implicit tree construction is the tight coupling between the code that builds the tree and how the tree is represented
  
* Example
```
//Before
var orders = "<orders>";
orders += "<order number='123'>";
orders += "<item number='x1786'>";
orders += "carDoor";
orders += "</item>";
orders += "</order>";
orders += "</orders>";

//After
var order = new TagNode("order");
order.AddAttribute("number", "123");

var item = new TagNode("item");
item.AddAttribute("number", "x1786");
item.AddValue("carDoor");

order.Add(item);

var orders = new TagNode("orders");
orders.Add(order);

public class TagNode
{
    private readonly string _tagName;
    private string _tagValue = "";
    private string _attributes = "";
    private readonly List<TagNode> _children = new List<TagNode>();

    public TagNode(string name)
    {
        _tagName = name;
    }

    public void Add(TagNode childNode)
    {
        _children.Add(childNode);
    }

    public void AddAttribute(string name, string value)
    {
        _attributes += $" {name}='{value}'";
    }

    public void AddValue(string value)
    {
        _tagValue = value;
    }

    public override string ToString()
    {
        var result = new StringBuilder();
        result.Append($"<{_tagName}{_attributes}>");

        for (var i = 0; i < _children.Count; i++)
        {
            result.Append(_children[i]);
        }

        if (!_tagValue.Equals(""))
        {
            result.Append(_tagValue);
        }

        result.Append($"</{_tagName}>");
        return result.ToString();
    }
}
```

#### Encapsulate Composite with Builder ####
![image](https://user-images.githubusercontent.com/5309726/66698192-6026ee80-ed0e-11e9-8c08-04fd5db2a76f.png)

* Motivation
  * I'm always interested in simplifying client code: I want it to read as clearly as English. So when it comes to creating really simple tree-construction code, I like the Builder pattern even better than the Composite pattern
  
* Example
```
//Before
var order = new TagNode("order");
order.AddAttribute("number", "123");

var item = new TagNode("item");
item.AddAttribute("number", "x1786");
item.AddValue("carDoor");

order.Add(item);

var orders = new TagNode("orders");
orders.Add(order);

//After
var orders = new XmlBuilder("orders");
orders.AddBelow("order");
orders.AddAttribute("number", "123");
orders.AddBelow("item");
orders.AddAttribute("number", "x1786");
orders.AddValue("carDoor");

public class XmlBuilder
{
    private readonly TagNode _root;
    private TagNode _current;
    private TagNode _parent;

    public XmlBuilder(string rootName)
    {
        _root = new TagNode(rootName);
        _current = _root;
        _parent = _root;
    }

    public void AddBelow(string child)
    {
        var childNode = new TagNode(child);
        _current.Add(childNode);
        _parent = _current;
        _current = childNode;
    }

    public void AddBeside(string sibling)
    {
        var siblingNode = new TagNode(sibling);
        _parent.Add(siblingNode);
        _current = siblingNode;
    }

    public void AddAttribute(string name, string value)
    {
        _current.AddAttribute(name, value);
    }

    public void AddValue(string value)
    {
        _current.AddValue(value);
    }

    public override string ToString()
    {
        return _root.ToString();
    }
}
```

#### Extract Special-Case Logic into Decorators ####
![image](https://user-images.githubusercontent.com/5309726/66699978-51483800-ed1e-11e9-9b19-7c4c6c13b6a8.png)

* Motivation
  * Consider an Invoice class that is responsible for keeping track of payment information for a customer invoice. Most invoices are simple - some dollar amount is owed, and all the Invoice object has to do is calculate the amount owed. But what happens when the amount
owed is overdue or if a special discount must be applied because the customer is a preferred customer? Those are two special conditions that the Invoice’s calcAmountOwed() method will have to deal with
  * But what happens when we add more special conditions to calcAmountOwed()? As more special conditions are added, the Invoice class gets more complex: it holds onto more instance variables, it supports more getter and setter methods for handling special conditions and its calculation logic gets longer and more involved

* Example
```
//Before
var m = new Model(10.0f, 50.0f, "Ford Taurus");

var r1 = new CarRental(m, 5);
Debug.Assert(r1.CalcPrice() == 250.0f);

var r2 = new CarRental(m, 5);
r2.SetInsurance(12.5f);
Debug.Assert(r2.CalcPrice() == 312.5f);

var r3 = new CarRental(m, 5);
r3.SetInsurance(12.5f);
r3.SetRefuelOnReturn(3.75f);
Debug.Assert(r3.CalcPrice() == 350.0f);

public class CarRental
{
    protected float FuelConsumed { get; private set; }
    protected int RentalDays { get; }
    protected Model Model { get; }
    protected float InsuranceRate { get; private set; }
    protected bool HasInsurance { get; private set; }
    protected bool HasRefuelOnReturn { get; private set; }
    protected float RefuelPrice { get; private set; }

    public CarRental(Model model, int rentalDays)
    {
        Model = model;
        RentalDays = rentalDays;
        HasInsurance = false;
        HasRefuelOnReturn = false;
    }

    public float CalcPrice()
    {
        var price = (Model.Price * RentalDays);

        if (HasInsurance)
        {
            price += InsuranceAmount();
        }

        if (HasRefuelOnReturn)
        {
            price += GetRefuelPrice();
        }

        return price;
    }

    public void SetFuelConsumed(float amount)
    {
        FuelConsumed = amount;
    }

    public void SetInsurance(float rate)
    {
        InsuranceRate = rate;
        HasInsurance = true;
    }

    public void SetRefuelOnReturn(float pricePerGallon)
    {
        RefuelPrice = pricePerGallon;
        HasRefuelOnReturn = true;
    }

    private float InsuranceAmount()
    {
        return InsuranceRate * RentalDays;
    }

    private float GetRefuelPrice()
    {
        return (Model.FuelCapacity - FuelConsumed) * RefuelPrice;
    }
}

public class Model
{
    public float FuelCapacity { get; }
    public float Price { get; }
    public string Name { get; }

    public Model(float fuelCapacity, float price, string name)
    {
        FuelCapacity = fuelCapacity;
        Price = price;
        Name = name;
    }
}

//After
var m = new Model(10.0f, 50.0f, "Ford Taurus");

var insuredFord = new Insurance(new CarRental(m, 5), 12.5f);
var refuelInsuredFord = new RefuelOnReturn(insuredFord, 3.75f);
var price1 = refuelInsuredFord.CalcPrice();
Debug.Assert(price1 == 350.0f);

var refuelFord = new RefuelOnReturn(new CarRental(m, 5), 3.75f);
var insuredRefuelFord = new Insurance(refuelFord, 12.5f);
var price2 = insuredRefuelFord.CalcPrice();
Debug.Assert(price2 == 350.0f);

public interface IRental
{
    float FuelConsumed { get; }
    int RentalDays { get; }
    Model Model { get; }
    float CalcPrice();
    void SetFuelConsumed(float amount);
}

public class CarRental : IRental
{
    public float FuelConsumed { get; set; }
    public int RentalDays { get; }
    public Model Model { get; }

    public CarRental(Model model, int rentalDays)
    {
        Model = model;
        RentalDays = rentalDays;
    }
    
    public float CalcPrice()
    { 
        return Model.Price * RentalDays;
    }

    public void SetFuelConsumed(float amount)
    {
        FuelConsumed = amount;
    }
}

public class CarRentalDecorator : IRental
{
    protected IRental Rental;
    public float FuelConsumed => Rental.FuelConsumed;
    public int RentalDays => Rental.RentalDays;
    public Model Model => Rental.Model;

    protected CarRentalDecorator(IRental rental)
    {
        Rental = rental;
    }

    public virtual float CalcPrice()
    {
        return Rental.CalcPrice();
    }

    public void SetFuelConsumed(float amount)
    {
        Rental.SetFuelConsumed(amount);
    }
}

public class Insurance : CarRentalDecorator
{
    protected float InsuranceRate;

    public Insurance(IRental rental, float insuranceRate)
        : base(rental)
    {
        InsuranceRate = insuranceRate;
    }

    private float InsuranceAmount()
    {
        return InsuranceRate * Rental.RentalDays;
    }
    
    public override float CalcPrice()
    {
        return Rental.CalcPrice() + InsuranceAmount();
    }
}

public class RefuelOnReturn : CarRentalDecorator
{
    private readonly float _refuelPrice;

    public RefuelOnReturn(IRental rental, float refuelPrice)
        : base(rental)
    {
        _refuelPrice = refuelPrice;
    }

    public override float CalcPrice()
    {
        return Rental.CalcPrice() + CalcRefuelPrice();
    }

    private float CalcRefuelPrice()
    {
        return (Rental.Model.FuelCapacity - Rental.FuelConsumed) * _refuelPrice;
    }
}
```

#### Replace Hard-Coded Notifications with Observer ####
![image](https://user-images.githubusercontent.com/5309726/66709943-f277d280-eda0-11e9-9c10-5a6edc6d0b7b.png)

* Motivation
  * The Observer pattern is popular. Many programmers know it well and use it often.
  
#### Move Accumulation to Collecting Parameter ####
![image](https://user-images.githubusercontent.com/5309726/66710518-cfebb680-edac-11e9-9252-0add4b2e6b9f.png)

* Motivation
  * A Collecting Parameter is an object that you pass to methods in order to collect information from those methods
  
* Example
```
//Before
public class TagNode
{
    private readonly string _tagName;
    private string _tagValue = "";
    private string _attributes = "";
    private readonly List<TagNode> _children = new List<TagNode>();

    public TagNode(string name)
    {
        _tagName = name;
    }

    public override string ToString()
    {
        var result = new StringBuilder();
        result.Append($"<{_tagName}{_attributes}>");

        for (var i = 0; i < _children.Count; i++)
        {
            result.Append(_children[i]);
        }

        if (!_tagValue.Equals(""))
        {
            result.Append(_tagValue);
        }

        result.Append($"</{_tagName}>");
        return result.ToString();
    }
}

//After
public class TagNode
{
    private readonly string _tagName;
    private string _tagValue = "";
    private string _attributes = "";
    private readonly List<TagNode> _children = new List<TagNode>();

    public override string ToString()
    {
        var result = new StringBuilder("");
        ToStringHelper(result);
        return result.ToString();
    }

    private void ToStringHelper(StringBuilder result)
    {
        WriteOpenTagTo(result);
        WriteChildrenTo(result);
        WriteValueTo(result);
        WriteEndTagTo(result);
    }

    private void WriteOpenTagTo(StringBuilder result)
    {
        result.Append("<");
        result.Append(_tagName);
        result.Append(_attributes);
        result.Append(">");
    }
    
    private void WriteChildrenTo(StringBuilder result)
    {
        for (var i = 0; i < _children.Count; i++)
        {
            _children[i].ToStringHelper(result);

        }
    }
    
    private void WriteValueTo(StringBuilder result)
    {
        if (!_tagValue.Equals(""))
        {
            result.Append(_tagValue);
        }
    }

    private void WriteEndTagTo(StringBuilder result)
    {
        result.Append("</");
        result.Append(_tagName);
        result.Append(">");
    }
}
```

#### Replace One/Many Distinctions with Composite ####
![image](https://user-images.githubusercontent.com/5309726/66717520-92b71100-ee0c-11e9-9197-a803a17fff8a.png)

* Motivation
  * A good reason to refactor to Composite is to get rid of code that distinguishes between single objects and collections of those objects
  
#### Compose Method ####
![image](https://user-images.githubusercontent.com/5309726/66717536-d3af2580-ee0c-11e9-8354-0b587faf2735.png)

* Motivation
  * Transform the logic into a small number of intention-revealing steps at the same level of detail
  
* Example
```
//Before
public bool Contains(Component c)
{
    var locX = c.Location.X;
    var locY = c.Location.Y;
    var completelyWithin =
        (locX >= coords[0] &&
         locY >= coords[1] &&
         (locX + CardComponent.Width) <= coords[2]) &&
        (locY + CardComponent.Height) <= coords[3];

    if (completelyWithin)
    {
        return true;
    }

    locX = locX + CardComponent.Width;
    locY = locY + CardComponent.Height;

    var partiallyWithin =
        (locX > coords[0] &&
         locY > coords[1] &&
         (locX < coords[2]) &&
         (locY < coords[3]));

    return partiallyWithin;
}

//After
public bool Contains(Component c)
{
    return CompletelyWithin(c) || PartiallyWithin(c);
}

private bool CompletelyWithin(Component c)
{
    return (c.Location.X >= coords[0] &&
            c.Location.Y >= coords[1] &&
            (c.Location.X + CardComponent.Width) <= coords[2]) &&
           (c.Location.Y + CardComponent.Height) <= coords[3];
}

private bool PartiallyWithin(Component c)
{
    return ((c.Location.X + CardComponent.Width) > coords[0] &&
            (c.Location.Y + CardComponent.Height) > coords[1] &&
            (c.Location.X + CardComponent.Width) < coords[2] &&
            (c.Location.Y + CardComponent.Height) < coords[3]);
}
```

#### Separate Versions with Adapters ####

* Motivation
  * While software must often support multiple versions of a component, library or API, code that handles these versions doesn’t have to be a confusing mess

* Example
```
//Before
public class Query
{
    private SDLogin _sdLogin;               // needed for SD version 5.1
    private SDSession _sdSession;           // needed for SD version 5.1
    private SDLoginSession _sdLoginSession; // needed for SD version 5.2
    private bool _sd52;                     // tells if we're running under SD 5.2
    private SDQuery _sdQuery;               // this is needed for SD versions 5.1 & 5.2

    // this is a login for SD 5.1
    // // NOTE: remove this when we convert all aplications to 5.2
    public void Login(string server, string user, string password)
    {
        _sd52 = false;

        try
        {
            _sdSession = _sdLogin.LoginSession(server, user, password);
        }
        catch (SDLoginFailedException lfe)
        {
            throw new QueryException(QueryException.LOGIN_FAILED, "Login failure\n" + lfe, lfe);
        }
        catch (SDSocketInitFailedException ife)
        {
            throw new QueryException(QueryException.LOGIN_FAILED, "Socket fail\n" + ife, ife);
        }
    }

    // 5.2 login
    public void Login(string server, string user, string password, string sdConfigFileName)
    {
        sd52 = true;
        sdLoginSession = new SDLoginSession(sdConfigFileName, false);

        try
        {
            _sdLoginSession.LoginSession(server, user, password);
        }
        catch (SDLoginFailedException lfe)
        {
            throw new QueryException(QueryException.LoginFailed, "Login failure\n" + lfe, lfe);
        }
        catch (SDSocketInitFailedException ife)
        {
            throw new QueryException(QueryException.LoginFailed, "Socket fail\n" + ife, ife);
        }
        catch (SDNotFoundException nfe)
        {
            throw new QueryException(QueryException.LoginFailed, "Not found exception\n" + nfe, nfe);
        }
    }

    public void DoQuery()
    {
        if (_sdQuery != null)
        {
            _sdQuery.ClearResultSet();
        }

        if (_sd52)
        {
            _sdQuery = _sdLoginSession.CreateQuery(SDQuery.OpenForQuery);
        }
        else
        {
            _sdQuery = _sdSession.CreateQuery(SDQuery.OpenForQuery);
        }

        ExecuteQuery();
    }
}

//After

//Client code
public void LoginToDatabase(string db, string user, string password)
{
    IQuery query = null;

    if (UsingSDVersion52())
    {
        query = new QuerySD52(GetSD52ConfigFileName());
    }
    else
    {
        query = new QuerySD51();

        try
        {
            query.Login(db, user, password);

        }
        catch (QueryException qe)
        {

        }
    }
}

public interface IQuery
{
    void Login(string server, string user, string password);
    void DoQuery();
}

public abstract class Query : IQuery
{
    protected SDQuery SdQuery;

    public virtual void Login(string server, string user, string password)
    {
    }

    public void DoQuery()
    {
        // a Template Method [GoF]
        if (SdQuery != null)
        {
            SdQuery.ClearResultSet();
        }

        SdQuery = CreateQuery(); // call to the Factory Method
        ExecuteQuery();
    }

    // a Factory Method [GoF]
    protected abstract SDQuery CreateQuery();
}

public class QuerySD51 : Query
{
    private SDLogin _sdLogin;
    private SDSession _sdSession;

    public override void Login(string server, string user, string password)
    {
        try
        {
            _sdSession = _sdLogin.LoginSession(server, user, password);
        }
        catch (SDLoginFailedException lfe)
        {
            throw new QueryException(QueryException.LOGIN_FAILED, "Login failure\n" + lfe, lfe);
        }
        catch (SDSocketInitFailedException ife)
        {
            throw new QueryException(QueryException.LOGIN_FAILED, "Socket fail\n" + ife, ife);
        }
    }

    protected override SDQuery CreateQuery()
    {
        return _sdSession.CreateQuery(SDQuery.OpenForQuery);
    }
}

public class QuerySD52 : Query
{
    private readonly string _sdConfigFileName;
    private SDLoginSession _sdLoginSession;

    public QuerySD52(string sdConfigFileName)
    {
        _sdConfigFileName = sdConfigFileName;
    }

    public override void Login(string server, string user, string password)
    {
        _sdLoginSession = new SDLoginSession(_sdConfigFileName, false);

        try
        {
            _sdLoginSession.LoginSession(server, user, password);
        }
        catch (SDLoginFailedException lfe)
        {
            throw new QueryException(QueryException.LoginFailed, "Login failure\n" + lfe, lfe);
        }
        catch (SDSocketInitFailedException ife)
        {
            throw new QueryException(QueryException.LoginFailed, "Socket fail\n" + ife, ife);
        }
        catch (SDNotFoundException nfe)
        {
            throw new QueryException(QueryException.LoginFailed, "Not found exception\n" + nfe, nfe);
        }
    }

    protected override SDQuery CreateQuery()
    {
        return _sdLoginSession.CreateQuery(SDQuery.OpenForQuery);
    }
}
```

#### Adapter Interface ####

* Motivation
  * Empty methods in concrete classes bother me. I often find that they’re there because a class needs to satisfy a contract by implementing an interface, but only really needs code for some of the interface’s methods
  
* Example
```

```
