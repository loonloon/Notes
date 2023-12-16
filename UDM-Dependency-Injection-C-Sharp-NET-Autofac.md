#### Section 2 Registration Concepts ####

```csharp
namespace TestAutoFac
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var builder = new ContainerBuilder();
            builder.RegisterType<ConsoleLog>().As<ILog>().As<IConsole>().AsSelf();
            builder.RegisterType<EmailLog>().As<ILog>().PreserveExistingDefaults();
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

    public interface IConsole
    {
    }

    public class ConsoleLog : ILog, IConsole
    {
        public ConsoleLog()
        {
        }

        public void Write(string message)
        {
            Console.WriteLine(message);
        }
    }

    public class EmailLog : ILog
    {
        private const string adminEmail = "admin@foo.com";

        public EmailLog()
        {
        }

        public void Write(string message)
        {
            Console.WriteLine($"Email sent to {adminEmail} : {message}");
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
