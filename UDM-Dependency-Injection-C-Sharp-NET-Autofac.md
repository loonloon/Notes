#### Section 2 Registration Concepts ####

```csharp
namespace AutoFac
{
    internal class Program
    {
        static void Main(string[] args)
        {
            // Scenario (Without DI)
            var log = new ConsoleLog();
            var engine = new Engine(log);
            var car = new Car(log, engine);
            car.Go();

            var builder = new ContainerBuilder();
            builder.RegisterType<ConsoleLog>().As<ILog>();
            builder.RegisterType<Engine>();
            builder.RegisterType<Car>();

            var container = builder.Build();
            var car = container.Resolve<Car>();
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
