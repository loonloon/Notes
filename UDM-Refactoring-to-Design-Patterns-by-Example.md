* Refactoring - Changing the design without changing observable behavior
* Redesigning - Changing the design while observable behavior may change

#### Section 3 Decoupling Implementation with Strategies ####

```
//Initial

class Subtitles
{
    private IEnumerable<SubtitleLine> Lines { get; }

    public Subtitles(IEnumerable<SubtitleLine> lines)
    {
        Lines = lines.ToList();
    }

    public static Subtitles Parse(string[] text, TimeSpan clipDuration)
    {
        IEnumerable<string> lines = BreakLongLines(
            BreakIntoSentences(Cleanup(text)),
            95, 45).ToList();
        
        ...
    }

    private static IEnumerable<string> BreakLongLines(
        IEnumerable<string> text, int maxLineCharacters, int minBrokenLength)
    {
    }

    private static IEnumerable<string> BreakIntoSentences(IEnumerable<string> text)
    }
    }
    
    private static IEnumerable<string> Cleanup(string[] text)
    {
    }
}
```

```
//Final (Strategy Pattern)

class Subtitles
{
    private IEnumerable<SubtitleLine> Lines { get; }

    public Subtitles(IEnumerable<SubtitleLine> lines)
    {
        Lines = lines.ToList();
    }

    public static Subtitles Parse(string[] text, TimeSpan clipDuration)
    {
        ITextProcessor cleanup = new LinesTrimmer();
        ITextProcessor intoSentences = new SentencesBreaker();
        ITextProcessor intoShortLines = new LinesBreaker(95, 45);
        
        IEnumerable<string> lines = cleanup.Execute(text);
        lines = intoSentences.Execute(lines);
        lines = intoShortLines.Execute(lines).ToList();
        
        ...
    }
}

internal interface ITextProcessor
{
    IEnumerable<string> Execute(IEnumerable<string> text);
}

class LinesTrimmer : ITextProcessor
{
    public IEnumerable<string> Execute(IEnumerable<string> text)
    {
    }
}

class SentencesBreaker : ITextProcessor
{
    public IEnumerable<string> Execute(IEnumerable<string> text)
    {
    }
}

class LinesBreaker : ITextProcessor
{
    private int MaxLineLength { get; }
    private int MinLengthToBreakInto { get; }

    public LinesBreaker(int maxLineLength, int minLengthToBreakInto)
    {
        this.MaxLineLength = maxLineLength;
        this.MinLengthToBreakInto = minLengthToBreakInto;
    }
    
    public IEnumerable<string> Execute(IEnumerable<string> text)
    {
    }
}
```

---

#### Section 4 Chaining Implementation with Composite and Decorator ####

![image](https://user-images.githubusercontent.com/5309726/163710620-21631d42-fb35-42d9-862a-c438df896baa.png)

```
//Initial

class Subtitles
{
    private IEnumerable<SubtitleLine> Lines { get; }

    public Subtitles(IEnumerable<SubtitleLine> lines)
    {
        Lines = lines.ToList();
    }

    public static Subtitles Parse(string[] text, TimeSpan clipDuration)
    {
        ITextProcessor cleanup = new LinesTrimmer();
        ITextProcessor intoSentences = new SentencesBreaker();
        ITextProcessor intoShortLines = new LinesBreaker(95, 45);
        
        IEnumerable<string> lines = cleanup.Execute(text);
        lines = intoSentences.Execute(lines);
        lines = intoShortLines.Execute(lines).ToList();
        
        ...
    }
}
```

![image](https://user-images.githubusercontent.com/5309726/163710588-583f1e97-b3d1-441a-b23a-f5e382bb7f54.png)

```
//Final (Composite Pattern)

class Subtitles
{
    private IEnumerable<SubtitleLine> Lines { get; }

    public Subtitles(IEnumerable<SubtitleLine> lines)
    {
        Lines = lines.ToList();
    }

    public static Subtitles Parse(string[] text, TimeSpan clipDuration)
    {
        ITextProcessor parsing = new Pipeline(
            new LinesTrimmer(),
            new SentencesBreaker(),
            new LinesBreaker(95, 45));

        IEnumerable<string> lines = parsing.Execute(text).ToList();
        
        ...
    }
}

class Pipeline : ITextProcessor
{
    private IEnumerable<ITextProcessor> Processors { get; }

    public Pipeline(IEnumerable<ITextProcessor> processors)
    {
        this.Processors = processors.ToList();
    }

    public Pipeline(params ITextProcessor[] processors)
        : this((IEnumerable<ITextProcessor>)processors)
    {
    }

    public IEnumerable<string> Execute(IEnumerable<string> text) =>
        this.Processors.Aggregate(text, (current, processor) => processor.Execute(current));
}
```

Examples

![image](https://user-images.githubusercontent.com/5309726/163710713-689e7ca5-ee9f-4cb6-93e6-8826e15ca185.png)

Decorator Pattern

![image](https://user-images.githubusercontent.com/5309726/163711136-bb5dcfea-2bfa-4a1c-92bc-d7c678d3547e.png)

```
//Initial
class Subtitles
{
    private IEnumerable<SubtitleLine> Lines { get; }

    public Subtitles(IEnumerable<SubtitleLine> lines)
    {
        Lines = lines.ToList();
    }

    public static Subtitles Parse(string[] text, TimeSpan clipDuration)
    {
        ITextProcessor parsing = new Pipeline(
            new LinesTrimmer(),
            new SentencesBreaker(),
            new LinesBreaker(95, 45));

        IEnumerable<string> lines = parsing.Execute(text).ToList();
        
        ...
    }
}
```

```
//Final

class Subtitles
{
    private IEnumerable<SubtitleLine> Lines { get; }

    public Subtitles(IEnumerable<SubtitleLine> lines)
    {
        Lines = lines.ToList();
    }

    public static Subtitles Parse(string[] text, TimeSpan clipDuration)
    {
        ITextProcessor parsing =
            new LinesTrimmer()
                .Then(new SentencesBreaker())
                .Then(new LinesBreaker(95, 45));

        IEnumerable<string> lines = parsing.Execute(text).ToList();
        
        ...
    }
}

class ChainedProcessor : ITextProcessor
{
    private ITextProcessor Inner { get; }
    private ITextProcessor Next { get; }

    public ChainedProcessor(ITextProcessor inner, ITextProcessor next)
    {
        this.Inner = inner;
        this.Next = next;
    }

    public IEnumerable<string> Execute(IEnumerable<string> text) =>
        this.Next.Execute(this.Inner.Execute(text));
}

static class ChainConstruction
{
    public static ITextProcessor Then(this ITextProcessor first, ITextProcessor next) =>
        first is DoNothing ? next
        : next is DoNothing ? first
        : new ChainedProcessor(first, next);
}
```

---

#### Section 5 Constructing Complex Object Graphs with Builder ####

```
//Initial

class Program
{
    static void Process(FileInfo source, FileInfo destination, TimeSpan clipDuration)
    {
        try
        {
            string[] text = File.ReadAllLines(source.FullName);
            Subtitles subtitles = Subtitles.Parse(text, clipDuration);
            subtitles.SaveAsSrt(destination);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error processing text: {ex.Message}");
        }
    }
}

class Subtitles
{
    private IEnumerable<SubtitleLine> Lines { get; }

    public Subtitles(IEnumerable<SubtitleLine> lines)
    {
        Lines = lines.ToList();
    }

    public static Subtitles Parse(string[] text, TimeSpan clipDuration)
    {
        ITextProcessor parsing =
            new LinesTrimmer()
                .Then(new SentencesBreaker())
                .Then(new LinesBreaker(95, 45));

        IEnumerable<string> lines = parsing.Execute(text).ToList();
        
        ...
    }
}
```

```
//Final

static void Process(FileInfo source, FileInfo destination, TimeSpan clipDuration)
{
    try
    {
        string[] text = File.ReadAllLines(source.FullName);
        TimedText timed = new TimedText(text, clipDuration);

        Subtitles subtitles = new SubtitlesBuilder()
            .For(timed)
            .Using(new LinesTrimmer())
            .Using(new SentencesBreaker())
            .Using(new LinesBreaker(95, 45))
            .Build();

        subtitles.SaveAsSrt(destination);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error processing text: {ex.Message}");
    }
}

public class SubtitlesBuilder
{
    private TimedText Text { get; set; } = TimedText.Empty;
    private ITextProcessor Processing { get; set; } = new DoNothing();

    public SubtitlesBuilder For(TimedText text)
    {
        this.Text = text;
        return this;
    }

    public SubtitlesBuilder Using(ITextProcessor processor)
    {
        this.Processing = this.Processing.Then(processor);
        return this;
    }

    public Subtitles Build()
    {
        TimedText processed = this.Text.Apply(this.Processing);
        TextDurationMeter durationMeter = new TextDurationMeter(processed);
        IEnumerable<SubtitleLine> subtitles = durationMeter.MeasureLines();
        return new Subtitles(subtitles);
    }
}

public class TimedText
{
    public IEnumerable<string> Content { get; }
    public TimeSpan Duration { get; }

    public TimedText(IEnumerable<string> content, TimeSpan duration)
    {
        this.Content = content.ToList();
        this.Duration = duration;
    }

    public static TimedText Empty { get; } =
        new TimedText(Enumerable.Empty<string>(), TimeSpan.Zero);

    public TimedText Apply(ITextProcessor processor) =>
        new TimedText(processor.Execute(this.Content), this.Duration);
}

internal class TextDurationMeter
{
    private TimedText Text { get; }
    private double FullTextLength { get; }
    private TimeSpan FullTextDuration { get; }

    internal TextDurationMeter(TimedText text)
    {
        this.Text = text;
    }

    public IEnumerable<SubtitleLine> MeasureLines() =>
        this.MeasureLines(this.GetFullTextWeight());

    private IEnumerable<SubtitleLine> MeasureLines(double fullTextWeight)
    {
       ...
    }
}
```

---

#### Section 6 Modeling Low-level Concerns as Infrastructure ####

* The Infrastructure project depends on Domain project

![image](https://user-images.githubusercontent.com/5309726/163712268-a4042f5b-b4e4-42b8-b7fc-b22e251b557b.png)

![image](https://user-images.githubusercontent.com/5309726/163712376-f58a50df-ea45-48d5-9bdf-7f3b183ef41f.png)

---

#### Section 7 Factoring Domain Complexity Out with the Rules Pattern ####

![image](https://user-images.githubusercontent.com/5309726/163712521-77249f5c-c058-4383-a1b0-7732c65db019.png)

```
//Initial

public class SentencesBreaker : ITextProcessor
{
    private IEnumerable<(string pattern, string extract, string remove)> Rules { get; } = new[]
    {
            (@"^(?<remove>(?<extract>(\.\.\.|[^\.])+)\.)$", "${extract}", "${remove}"),
            (@"^(?<remove>(?<extract>[^\.]+),)$", "${extract}", "${remove}"),
            (@"^(?<remove>(?<extract>(\.\.\.|[^\.])+)\.)[^\.].*$", "${extract}", "${remove}"),
            (@"^(?<remove>(?<extract>[^:]+):).*$", "${extract}", "${remove}"),
            (@"^(?<extract>.+\?).*$", "${extract}", "${extract}"),
            (@"^(?<extract>.+\!).*$", "${extract}", "${extract}"),
        };

    public IEnumerable<string> Execute(IEnumerable<string> text) =>
        text.SelectMany(this.BreakSentences);

    private IEnumerable<string> BreakSentences(string text)
    {
        string remaining = text.Trim();
        while (remaining.Length > 0)
        {
            (string extracted, string rest) =
                this.FindShortestExtractionRule(this.Rules, remaining)
                    .Select(tuple => (
                        extracted: tuple.extracted,
                        removedLength: tuple.remove.Length))
                    .Select(tuple => (
                        extracted: tuple.extracted,
                        remaining: remaining.Substring(tuple.removedLength).Trim()))
                    .DefaultIfEmpty((extracted: remaining, remaining: string.Empty))
                    .First();

            yield return extracted;
            remaining = rest;
        }
    }

    private IEnumerable<(string extracted, string remove)> FindShortestExtractionRule(
        IEnumerable<(string pattern, string extractPattern, string removePattern)> rules,
        string text) =>
        rules
            .Select(rule => (
                pattern: new Regex(rule.pattern),
                extractPattern: rule.extractPattern,
                removePattern: rule.removePattern))
            .Select(rule => (
                pattern: rule.pattern,
                match: rule.pattern.Match(text),
                extractPattern: rule.extractPattern,
                removePattern: rule.removePattern))
            .Where(rule => rule.match.Success)
            .Select(rule => (
                extracted: rule.pattern.Replace(text, rule.extractPattern),
                remove: rule.pattern.Replace(text, rule.removePattern)))
            .WithMinimumOrEmpty(tuple => tuple.remove.Length);
}
```

```
//Final

public class SentencesBreaker : RuleBasedProcessor
{
    protected override IMultiwaySplitter Splitter { get; } = new[]
        {
                RegexSplitter.LeftAndRightExtractor(@"^(?<left> [^\?*]+\?)\s*(?<right>.*)$"),
                RegexSplitter.LeftAndRightExtractor(@"^(?<left>[^\!*]+\!)\s*(?<right>.*)$"),
                RegexSplitter.LeftAndRightExtractor(
                    @"^(?<left>(?:(?:\.\.\.)|[^\.])+)\.\s*(?<right>.*)$"),
                RegexSplitter.LeftAndRightExtractor(
                    @"(?<left>^.*\.\.\.)(?=(?:\s+\p{Lu})|(?:\s+\p{Lt})|\s*$)\s*(?<right>.*)$"),
                RegexSplitter.LeftExtractor(@"^(?<left>.*(?<!\.))\.$"),
                RegexSplitter.LeftExtractor(@"^(?<left>.*)(?:[\:\;\,]|\s+-\s*)$"),
            }
        .WithShortestLeft()
        .Repeat();
}

internal class RegexSplitter : ITwoWaySplitter
{
    private Regex Pattern { get; }
    private Func<Match, string> ExtractLeft { get; }
    private Func<Match, string> ExtractRight { get; }

    public RegexSplitter(
        Regex pattern,
        Func<Match, string> extractLeft,
        Func<Match, string> extractRight)
    {
        this.Pattern = pattern;
        this.ExtractLeft = extractLeft;
        this.ExtractRight = extractRight;
    }

    public static ITwoWaySplitter LeftAndRightExtractor(string pattern) =>
        new RegexSplitter(
            new Regex(pattern),
            match => match.Groups["left"].Value,
            match => match.Groups["right"].Value);

    public static ITwoWaySplitter LeftExtractor(string pattern) =>
        new RegexSplitter(
            new Regex(pattern),
            match => match.Groups["left"].Value,
            _ => "");

    public IEnumerable<(string left, string right)> ApplyTo(string line) =>
        this.Pattern
            .Matches(line)
            .Select(match => (this.ExtractLeft(match), this.ExtractRight(match)));
}

public abstract class RuleBasedProcessor : ITextProcessor
{
    protected abstract IMultiwaySplitter Splitter { get; }

    public IEnumerable<string> Execute(IEnumerable<string> text) =>
        text.SelectMany(this.Splitter.ApplyTo);
}

public interface IMultiwaySplitter
{
    IEnumerable<string> ApplyTo(string line);
}

public interface ITwoWaySplitter
{
    IEnumerable<(string left, string right)> ApplyTo(string line);
}

internal static class RuleComposition
{
    public static ITwoWaySplitter WithShortestLeft(
        this IEnumerable<ITwoWaySplitter> splitters) =>
        new ShortestLeftWins(splitters);

    public static IMultiwaySplitter Repeat(this ITwoWaySplitter splitter) =>
        new TwoWayRepeater(splitter);
}

internal class ShortestLeftWins : ITwoWaySplitter
{
    private IEnumerable<ITwoWaySplitter> Splitters { get; }

    public ShortestLeftWins(IEnumerable<ITwoWaySplitter> splitters)
    {
        this.Splitters = splitters;
    }

    public IEnumerable<(string left, string right)> ApplyTo(string line) =>
        this.Splitters
            .SelectMany(rule => rule.ApplyTo(line))
            .DefaultIfEmpty((left: line, right: string.Empty))
            .WithMinimumOrEmpty(tuple => tuple.left.Length);
}

class TwoWayRepeater : IMultiwaySplitter
{
    private ITwoWaySplitter Splitter { get; }

    public TwoWayRepeater(ITwoWaySplitter splitter)
    {
        this.Splitter = splitter;
    }

    public IEnumerable<string> ApplyTo(string line)
    {
        string remaining = line.Trim();
        while (remaining.Length > 0)
        {
            (string extracted, string rest) =
                this.Splitter.ApplyTo(remaining).First();

            yield return extracted;
            remaining = rest.Trim();
        }
    }
}
```

---

#### Section 8 Supporting Multiple Transform with the Visitor Pattern ####

![image](https://user-images.githubusercontent.com/5309726/163713727-44145e48-ef00-41b8-8946-add667800e61.png)
