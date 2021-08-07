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
         * //By coding to concrete implementations, we have no way to add or remove other display elements
         * without making changes to the program
         */
        currentConditionsDisplay.Update(temp, humidity, pressure);
        statisticsDisplay.Update(temp, humidity, pressure);
        forecaseDisplay.Update(temp, humidity, pressure);
    }
}
```

##### Final Design #####
* Define 1 to many relationship between a set of objects

![image](https://user-images.githubusercontent.com/5309726/128589395-a804e9c8-bcea-477c-a701-ec25f25c71f0.png)

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
