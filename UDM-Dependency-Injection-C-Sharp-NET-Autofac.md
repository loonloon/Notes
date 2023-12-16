#### Section 2 Registration Concepts ####

```csharp
// Scenario (Without DI)
namespace AutoFac
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var log = new ConsoleLog();
            var engine = new Engine(log);
            var car = new Car(log, engine);
            car.Go();
        }
    }

    public interface ILog
    {
        void Write(string message);
    }

    public class ConsoleLog : ILog
    {
        public ConsoleLog()
        {
        }

        public void Write(string message)
        {
            Console.WriteLine(message);
        }
    }

    public class Engine(ILog log)
    {
        private readonly ILog log = log;
        private readonly int id = new Random().Next();

        public void Ahead(int power)
        {
            log.Write($"Engine [{id}] ahead {power}");
        }
    }

    public class Car(ILog log, Engine engine)
    {
        private readonly ILog log = log;
        private readonly Engine engine = engine;

        public void Go()
        {
            engine.Ahead(100);
            log.Write("Car going forward...");
        }
    }
}
```

---
