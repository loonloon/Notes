#### Section 2 Registration Concepts ####

```csharp
namespace TestAutoFac
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var builder = new ContainerBuilder();

            // Use it when dealing with singletons, integrating with external libraries, etc
            var log = new ConsoleLog();
            builder.RegisterInstance(log).As<ILog>();

            builder.RegisterType<ConsoleLog>().As<ILog>().As<IConsole>().AsSelf();

            // Use PreserveExistingDefaults without overwriting the statements above.
            builder.RegisterType<EmailLog>().As<ILog>().PreserveExistingDefaults();
            builder.RegisterType<Engine>();

            builder.Register((IComponentContext c) => new Engine(c.Resolve<ILog>()));
            builder.RegisterGeneric(typeof(List<>)).As(typeof(IList<>));

            // By default, it will select the constructor with the most parameters
            builder.RegisterType<Car>().UsingConstructor(typeof(Engine));

            var container = builder.Build();
            var car = container.Resolve<Car>();
            car.Go();

            var list = container.Resolve<IList<int>>();
            Console.WriteLine(list.GetType());
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

    public class Car
    {
        private readonly ILog log;
        private readonly Engine engine;

        public Car(Engine engine)
        {
            this.engine = engine;
            log = new EmailLog();
        }

        public Car(ILog log, Engine engine)
        {
            this.log = log;
            this.engine = engine;
        }

        public void Go()
        {
            engine.Ahead(100);
            log.Write("Car going forward...");
        }
    }
}
```

---
