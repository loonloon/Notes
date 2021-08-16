#### Strategy Pattern ####

##### Initial Design #####
* New requiments: Need the duck to fly

![image](https://user-images.githubusercontent.com/5309726/128587171-e35a8a0f-ad77-4593-9ae0-0553a20ed5df.png)

---

##### 1st Design: Inheritance #####

![image](https://user-images.githubusercontent.com/5309726/128587140-feb191c7-45f8-4ad2-ba60-0a1869eaf135.png)

* Problems:
  * Not all subclasses of Duck should fly
  * Changes can unintentionally affect other ducks

---

##### 2nd Design: Apply interface #####

![image](https://user-images.githubusercontent.com/5309726/128587417-0603c027-065e-4883-b7b9-b171a25a18e8.png)

* Problems:
  * Code is duplicated across subclasses
  * Hard to gain knowledge of all duck behaviors
 
---

##### Final Design #####
* Separating what changes from what stays the same
* Program to interface, not implementations

![image](https://user-images.githubusercontent.com/5309726/128587029-1b20fa82-889f-42d1-9643-ba85b18274ca.png)

---

#### Observer Pattern ####

##### Initial Design #####
* New requiments: Can add or remove as many display elements as they want to the application

![image](https://user-images.githubusercontent.com/5309726/128588314-be17ca6f-ad67-4cce-906a-64214eb224a7.png)

```
public class WeatherData
{
    public void MeasurementsChanged()
    {
        var temp = GetTemperate();
        var humidity = GetHumidity();
        var pressure = GetPressure();

        /*
         * By coding to concrete implementations, we have no way to add or remove other display elements
         * without making changes to the program
         */
        currentConditionsDisplay.Update(temp, humidity, pressure);
        statisticsDisplay.Update(temp, humidity, pressure);
        forecaseDisplay.Update(temp, humidity, pressure);
    }
}
```

---

##### Final Design #####
* Define 1 to many relationship between a set of objects

![image](https://user-images.githubusercontent.com/5309726/128801048-4c880fce-da3e-4668-a96d-0a51a8f7ed5c.png)

```
class Program
{
    static void Main(string[] args)
    {
        var weatherData = new WeatherData();

        var currentConditions = new CurrentConditionsDisplay();
        currentConditions.Subscribe(weatherData);

        var forecastDisplay = new ForecastDisplay();
        forecastDisplay.Subscribe(weatherData);

        weatherData.SetMeasurements(80, 65, 30.4f);
        weatherData.SetMeasurements(82, 70, 29.2f);
        weatherData.SetMeasurements(78, 90, 29.2f);
    }
}

public interface IDisplayElement
{
    void Display();
}

public class CurrentConditionsDisplay : IDisplayElement, IObserver<WeatherData>
{
    private float _temperature;
    private float _humidity;
    private IDisposable _cancellation;
    public void Subscribe(WeatherData provider)
    {
        _cancellation = provider.Subscribe(this);
    }

    public void Display()
    {
        Console.WriteLine($"Current conditions: {_temperature}F degrees and {_humidity}% humidity");
    }

    public void OnCompleted()
    {
    }

    public void OnError(Exception error)
    {
    }

    public void OnNext(WeatherData value)
    {
        _temperature = value.Temperature;
        _humidity = value.Humidity;
        Display();
    }
}

public class ForecastDisplay : IDisplayElement, IObserver<WeatherData>
{
    private float _currentPressure = 29.92f;
    private float _lastPressure; 
    private IDisposable _cancellation;

    public void Subscribe(WeatherData provider)
    {
        _cancellation = provider.Subscribe(this);
    }

    public void Display()
    {
        Console.WriteLine("Forecast: ");

        if (_currentPressure > _lastPressure)
        {
            Console.WriteLine("Improving weather on the way!");
        }
        else if (_currentPressure == _lastPressure)
        {
            Console.WriteLine("More of the same");
        }
        else if (_currentPressure < _lastPressure)
        {
            Console.WriteLine("Watch out for cooler, rainy weather");
        }
    }

    public void OnCompleted()
    {
    }

    public void OnError(Exception error)
    {
    }

    public void OnNext(WeatherData value)
    {
        _lastPressure = _currentPressure;
        _currentPressure = value.Pressure;
        Display();
    }
}

public class WeatherData : IObservable<WeatherData>
{
    private readonly List<IObserver<WeatherData>> _observers;
    public float Temperature { private set; get; }
    public float Humidity { private set; get; }
    public float Pressure { private set; get; }

    public WeatherData()
    {
        _observers = new List<IObserver<WeatherData>>();
    }

    public IDisposable Subscribe(IObserver<WeatherData> observer)
    {
        _observers.Add(observer);
        observer.OnNext(this);
        return new Unsubscriber<WeatherData>(_observers, observer);
    }

    public void SetMeasurements(float temperature, float humidity, float pressure)
    {
        Temperature = temperature;
        Humidity = humidity;
        Pressure = pressure;
        MeasurementsChanged();
    }

    public void MeasurementsChanged()
    {
        foreach (var observer in _observers)
        {
            observer.OnNext(this);
        }
    }
}

public class Unsubscriber<WeatherData> : IDisposable
{
    private readonly List<IObserver<WeatherData>> _observers;
    private readonly IObserver<WeatherData> _observer;

    internal Unsubscriber(List<IObserver<WeatherData>> observers, IObserver<WeatherData> observer)
    {
        _observers = observers;
        _observer = observer;
    }

    public void Dispose()
    {
        if (_observers.Contains(_observer))
        {
            _observers.Remove(_observer);
        }
    }
}
```

---

#### Decorator Pattern ####

##### Initial Design #####
* New requiments: In addition to your coffee, you can also ask for several condiments like steamed milk, soy and etc.

![image](https://user-images.githubusercontent.com/5309726/128802668-29a50c29-efac-4da5-9800-aa664896ca00.png)

---

##### 1st Design #####

![image](https://user-images.githubusercontent.com/5309726/128803249-843e39e0-363d-4c63-80c3-1040e7a1a54a.png)

* Problems:
  * Class explosion

---

##### 2nd Design #####

![image](https://user-images.githubusercontent.com/5309726/128803380-8ddd6753-4471-4a61-b813-0e28e9444ede.png)

* Problems:
  * New condiments will force to add new methods and alter the cost method in the superclass
  * May have new beverage. For some of these beverage (Iced Tea), the condiments may not be appropriate, yet the Tea subclass will still inherit methods like `hasWhip()`

---

##### Final Design #####
* Open Closed Princple
* Attach additional responsibilities to an object dynamically

![image](https://user-images.githubusercontent.com/5309726/128821342-856cafb6-a247-4d9a-beca-2c95ed2564bd.png)

![image](https://user-images.githubusercontent.com/5309726/128821609-235c30f2-c997-4111-bf25-22dca15dce82.png)

```
static void Main(string[] args)
{
    Beverage beverage = new Espresso();
    Console.WriteLine($"{beverage} ${beverage.Cost()}");

    beverage = new Mocha(beverage);
    Console.WriteLine($"{beverage} ${beverage.Cost()}");
}

public enum Size
{
    Tall,
    Grande,
    Venti
}

public abstract class Beverage
{
    public abstract double Cost();
    public override string ToString() => "Unknown Beverage";
}

public abstract class CondimentDecorator : Beverage
{
    protected Beverage Beverage;
}

public class Espresso : Beverage
{
    public override double Cost() => 1.99;
    public override string ToString() => "Espresso";
}

public class HouseBlend : Beverage
{
    public override double Cost() => 0.89;
    public override string ToString() => "House Blend Coffee";
}

public class Mocha : CondimentDecorator
{
    public Mocha(Beverage beverage)
    {
        Beverage = beverage;
    }

    public override double Cost() => Beverage.Cost() + 0.2;
    public override string ToString() => $"{Beverage}, Mocha";
}

public class Soy : CondimentDecorator
{
    public Soy(Beverage beverage)
    {
        Beverage = beverage;
    }

    public override double Cost()
    {
        var cost = Beverage.Cost();

        cost += Beverage.Size switch
        {
            Size.Tall => 0.1,
            Size.Grande => 0.15,
            _ => 0.2
        };

        return cost;
    }
}
```

---

#### Factory Pattern ####

##### Initial Design #####

![image](https://user-images.githubusercontent.com/5309726/128848857-9dd62714-9103-4acb-8951-bb4a46f7321b.png)

![image](https://user-images.githubusercontent.com/5309726/129537798-bdeebe5b-b7cf-4739-8e92-24d821b9f253.png)

---

##### 1st Design (Simple Factory) #####

![image](https://user-images.githubusercontent.com/5309726/128848314-7bd21671-ac88-43dc-9cf8-56b0a4c0a404.png)

---

##### 2nd Design (Factory Method) #####
* Defines an interface for creating an object, but lets subclasses decide which class to instantiate (through inheritance)

![image](https://user-images.githubusercontent.com/5309726/129320702-cd6314d5-91a5-4a08-a28e-81fd6680a113.png)

![image](https://user-images.githubusercontent.com/5309726/129322173-3d2e64eb-ae28-4b8f-98d7-5132dbe37a8e.png)

![image](https://user-images.githubusercontent.com/5309726/129537321-b9979bcb-54a9-4e9b-b53c-87fb9d495438.png)

---

##### Final Design (Abstract Factory) #####
* Dependency Inversion Principle
  * No variable should hold a reference to a concrete class
  * No class should derive from a concrete class
  * No method should override an implemented method of any of its base classes
* Provides an interface for creating families of related or dependent objects without specifying their concrete classes (through composition)

![image](https://user-images.githubusercontent.com/5309726/129539149-202df6ca-7e5d-4a11-9fdf-bfd8a99f4afe.png)

![image](https://user-images.githubusercontent.com/5309726/129538550-5cf9cc61-341a-4730-a28c-401b11d58730.png)

---
