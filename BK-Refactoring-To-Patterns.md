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
