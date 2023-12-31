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

            // Named parameter
            builder.RegisterType<SmsLog>().As<ILog>().WithParameter("phoneNumber", "12345678");

            // Type Parameter
            builder.RegisterType<SmsLog>().As<ILog>().WithParameter(new TypedParameter(typeof(string), "12345678"));

            // Resolved Parameter
            builder.RegisterType<SmsLog>().As<ILog>()
                .WithParameter(new ResolvedParameter((pi, ctx) => 
                pi.ParameterType == typeof(string) && pi.Name == "phoneNumber", (pi, ctx) => "12345678"));

            builder.Register((c, p) => new SmsLog(p.Named<string>("phoneNumber")));

            var container = builder.Build();
            var car = container.Resolve<Car>();
            car.Go();

            var list = container.Resolve<IList<int>>();
            Console.WriteLine(list.GetType());

            var smsLog = container.Resolve<ILog>(new NamedParameter("phoneNumber", "12345678"));
            smsLog.Write("Testing");
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

#### Section 3: Advanced Registration Concepts ####

```csharp
namespace TestAutoFac
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var containerBuilder = new ContainerBuilder();
            containerBuilder.RegisterType<Service>();
            containerBuilder.RegisterType<DomainObject>();

            var c = containerBuilder.Build();

            // Bad
            c.Resolve<DomainObject>(new PositionalParameter(1, 42));

            // Good
            var factory = c.Resolve<DomainObject.Factory>();
            Console.WriteLine(factory(42));
        }
    }

    public class Service
    {
        public string DoSomething(int value)
        {
            return $"I have {value}";
        }
    }

    public class DomainObject
    {
        private readonly Service service;
        private readonly int value;
        // Delegate Factories
        public delegate DomainObject Factory(int value);

        public DomainObject(Service service, int value)
        {
            this.service = service;
            this.value = value;
        }

        public override string ToString()
        {
            return service.DoSomething(value);
        }
    }
}
```

```csharp
namespace TestAutoFac
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var containerBuilder = new ContainerBuilder();
            containerBuilder.RegisterType<Entity>().InstancePerDependency();
            containerBuilder.RegisterType<ViewModel>();

            var c = containerBuilder.Build();
            var vm = c.Resolve<ViewModel>();
            vm.Method();
        }
    }

    public class Entity
    {
        private static readonly Random random = new();
        private readonly int number;
        public delegate Entity Factory();

        public Entity()
        {
            number = random.Next();
        }

        public override string ToString()
        {
            return $"test {number}";
        }
    }

    public class ViewModel
    {
        private Entity.Factory entityFactory { get; }

        public ViewModel(Entity.Factory entityFactory)
        {
            this.entityFactory = entityFactory;
        }

        // Bad
        //public ViewModel(IContainer container)
        //{
        //}

        public void Method()
        {
            // Objects on demand
            Console.WriteLine(entityFactory());
        }
    }
}
```
