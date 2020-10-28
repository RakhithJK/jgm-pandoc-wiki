Eine Schnittstelle definiert Eigenschaften oder Funktionen einer Komponente oder eines Moduls, die von anderen Software-Entitäten verwendet werden können. Schnittstellen sind immer öffentlich.
## C#
~~~cs
public interface MyInterface
{
    string Property { get; set; }

    void DoSomething();
}