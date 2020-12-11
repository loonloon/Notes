#### Overview ####
![image](https://user-images.githubusercontent.com/5309726/101895492-9269ff80-3be2-11eb-9cb4-bf8c3d05eed3.png)

#### The SOLID Design Principles ####
* Single Responsibility Principle

```
//bad
public class Journal
{
    private readonly List<string> entries = new List<string>();
    private static int count = 0;

    public int AddEntry(string text)
    {
        entries.Add($"{++count}: {text}");
        return count; // memento pattern!
    }

    public void RemoveEntry(int index)
    {
        entries.RemoveAt(index);
    }

    public override string ToString()
    {
        return string.Join(Environment.NewLine, entries);
    }

    // breaks single responsibility principle
    public void Save(string filename, bool overwrite = false)
    {
        File.WriteAllText(filename, ToString());
    }

    public void Load(string filename)
    {
    }

    public void Load(Uri uri)
    {
    }
}

//good
public class Journal
{
    private readonly List<string> entries = new List<string>();
    private static int count = 0;

    public int AddEntry(string text)
    {
        entries.Add($"{++count}: {text}");
        return count; // memento pattern!
    }

    public void RemoveEntry(int index)
    {
        entries.RemoveAt(index);
    }

    public override string ToString()
    {
        return string.Join(Environment.NewLine, entries);
    }

}

public class Persistence
{
    public void SaveToFile(Journal journal, string filename, bool overwrite = false)
    {
        if (overwrite || !File.Exists(filename))
        {
            File.WriteAllText(filename, journal.ToString());
        }
    }

    public void Load(string filename)
    {
    }

    public void Load(Uri uri)
    {
    }
}
```

* Open Closed Principle

```
```
