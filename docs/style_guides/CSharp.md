
# C#



## Ссылки на сторонние Code Style 
- [C# Coding Conventions by Microsoft](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [C# at Google Style Guide](https://google.github.io/styleguide/csharp-style.html)

## Соглашения об именах
- Имена интерфейсов начинаются с заглавной буквы I.
- Типы атрибутов заканчиваются словом Attribute.
- Типы перечисления используют единственное число для объектов, не являющихся флагами, и множественное число для флагов.
- Идентификаторы не должны содержать два последовательных символа подчеркивания ( _ ). Эти имена зарезервированы для создаваемых компилятором идентификаторов.

### Регистр Pascal
При именовании class , record, interface или struct Используйте регистр Pascal ("PascalCasing").

``` csharp
public class DataService
{

}

public record PhysicalAddress(
    string Street,
    string City,
    string StateOrProvince,
    string ZipCode);

public interface IWorkerQueue
{
}
```
При именовании членов типов доступных вне класса (public, protected и internal), таких как поля, свойства, события, методы и локальные функции, используйте регистр Pascal.

```csharp
public class ExampleEvents
{
    // A public field, these should be used sparingly
    public bool IsValid;

    // An init-only property
    public IWorkerQueue WorkerQueue { get; init; }

    // An event
    public event Action EventProcessing;

    // Method
    public void StartEventProcessing()
    {
        // Local function
        static int CountQueueItems() => WorkerQueue.Count;
        // ...
    }
}
```
Для **параметров** record используйте регистр языка Pascal, так как это открытые свойства записи.
``` csharp
public record PhysicalAddress(
    string Street,
    string City,
    string StateOrProvince,
    string ZipCode);
```
### Регистр camelCase
При именовании private или internal полях следует использовать регистр в стиле Camel ("camelCasing") и добавить к ним префикс _.
``` csharp
public class DataService
{
    private IWorkerQueue _workerQueue;
}
```
При написании параметров метода используйте регистр в стиле Camel.
``` csharp
public T SomeMethod<T>(int someNumber, bool isValid)
{
}
```

### Соглашения о структуре
Чтобы выделить структуру кода и облегчить чтение кода, в хорошем макете используется форматирование. Примеры и образцы корпорации Майкрософт соответствуют следующим соглашениям.

- Запись только одного оператора в строке.
- Запись только одного объявления в строке.
- Если отступ для дополнительных строк не ставится автоматически, необходимо сделать для них отступ на одну позицию табуляции (четыре пробела).
- Добавление по крайней мере одной пустой строки между определениями методов и свойств.
- Использование скобок для ясности предложений в выражениях, как показано в следующем коде.
``` csharp
if ((val1 > val2) && (val1 > val3))
{
    // Take appropriate action.
}
```
- в многострочных конструкциях LINQ располагайте каждый последующий вызов в новой строке начинающейся с точки.
``` csharp
x => x.Lists.Include(l => l.Title)
     .Where(l => l.Title != String.Empty)
     .Where(l => l.InternalName != String.Empty
```
- **Рекомендация** В многострочных логических конструкциях начинайте строку с оператора сравнения.
``` csharp
if (value1 == comparision1
    && value2 != comparision2
    && !comparision3.Contains(value3))
{
    // stuff
}
```
- TODO: IF - выносить сложные конструкции в переменные
``` csharp
if ( (value1 == comparision1)
    && (longValue2 != longComparision2 || A) 
    && (!comparision3.Contains(value3) || C) )
{
    // stuff
}

```
- В **многострочном** тернарном операторе каждую строку начинать с ? или :.
``` csharp
return SomeLongCondition() 
    ? someObject.GetOneResult()
    : anotherObject.GetAnotherResult();
```

### Порядок объявления членов класса

**Это рекомендация** 

`Исключение 1. Допускается располагать блок Properties перед блоком конструкторов. Но важно соблюдать единообразие расположения блоков. В рамках одного проекта необходимо стремиться чтобы блоки были расположены всегда одинаково. `

`Исключение 2. Внутри Interface implementations блоки группируются по интерфейсам.`

Порядок внутри класса по типам:
- Constant Fields
- Fields
- Constructors
- Finalizers(Destructors)
- Delegates
- Events
- Enums
- Interface implementations
- Properties
- Indexers
- Methods
- Inner structs
- Inner classes

Внутри каждой из вышеперечисленных групп порядок по уровню доступа
- public
- internal
- protected internal
- protected
- private

Внутри каждой из групп по уровню доступа:
- static
- non-static

Внутри каждой из static/non-static групп
- readonly
- non-readonly

### Выход из метода
**Рекомендация**
Рекомендуется писать функцию так, чтобы основной поток выполнения шел строго сверху вниз. Точки возврата блокирующие основной поток выполнения и приводящие к досрочному выходу из метода необходимо располагать в начале метода (GuardClause). Возврат значения из метода располагать в его конце, возврат значений в середине метода запрещен.
Исключение - Когда метод простой и/или состоит только из множества return
- Guard clause в начале метода.
``` csharp
  public Foo merge (Foo a, Foo b) {
    if (a == null) return b;
    if (b == null) return a;

    // complicated merge code goes here.
  }
```
Когда метод простой и/или состоит только из множества return
``` csharp
switch(foo)
{
   case "A":
     return "Foo";
   case "B":
     return "Bar";
   case "C":
     return "Bar";
   // and many more 
   default:
     throw new NotSupportedException();
}
```

### Соглашения о комментариях
- Комментарий размещается на отдельной строке, а не в конце строки кода.
- Текст комментария начинается с заглавной буквы.
- Текст комментария завершается точкой.
- Между разделителем комментария (//) и текстом комментария вставляется один пробел, как показано в следующем примере.
``` csharp
// The following declaration creates a query. It does not run
// the query.
```
- Закоментированный код должен присутствовать в основной ветке только с указанием причины по которой он здесь присутствует. Закоментированный код без объяснений должен удалятся.
- Между разделителем комментария и закомментированным кодом пробел не ставится. 
``` csharp
//Console.WriteLine("Hello");
```
- Не создавайте форматированные блоки из звездочек вокруг комментариев.
- Убедитесь, что все открытые члены имеют необходимые комментарии XML, обеспечивая соответствующие описания их поведения.

## Рекомендации по использованию языка
В следующих подразделах описаны методики, которыми руководствуется команда C# для подготовки примеров и образцов кода.
### Строковый тип данных
- Для сцепления коротких строк рекомендуется использовать интерполяцию строк, как показано в следующем коде.
``` csharp
string displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
```
- Для добавления строк в циклах, особенно при работе с текстами больших размеров, используйте объект StringBuilder.
``` csharp
var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
var manyPhrases = new StringBuilder();
for (var i = 0; i < 10000; i++)
{
    manyPhrases.Append(phrase);
}
//Console.WriteLine("tra" + manyPhrases);
```

### Беззнаковые типы данных
Как правило, рекомендуется использовать int вместо беззнаковых типов. В C# обычно используется int. Использование int упрощает взаимодействие с другими библиотеками.

### Массивы
При инициализации массивов в строке объявления рекомендуется использовать сокращенный синтаксис. В следующем примере видно, что var нельзя использовать вместо string[].
``` csharp
string[] vowels1 = { "a", "e", "i", "o", "u" };
```
Если экземпляр создается явно, можно использовать var.
``` csharp
var vowels2 = new string[] { "a", "e", "i", "o", "u" };
```
Если вы указали размер массива, нужно поочередно инициализировать все элементы.
``` csharp
var vowels3 = new string[5];
vowels3[0] = "a";
vowels3[1] = "e";
// And so on.
```

### Операторы `try-catch` и `using` при обработке исключений
**Рекомендация**
В C# 8 и более поздних версиях используйте новый [синтаксис using](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/using-statement), в котором не требуются скобки:

### Оператор `new`
**Рекомендация**
- Используйте одну из сокращенных форм создания экземпляров объектов, как показано в следующих объявлениях. Во втором примере используется синтаксис, который появился в версии C# 9.
``` csharp
var instance1 = new ExampleClass();
```
``` csharp
ExampleClass instance2 = new();
```
- Используйте инициализаторы объектов, чтобы упростить создание объектов, как показано в следующем примере.
``` csharp
var instance3 = new ExampleClass
{
    Name = "Desktop",
    ID = 37414,
    Location = "Redmond",
    Age = 2.3 
};
```
### Оператор nameof
- Используйте оператор nameof(...) там, где это возможно.
``` csharp
public void SomeMethod(string requiredParameterChanged)
{
    if (requiredParameter == null)
        throw new ArgumentException(nameof(requiredParameter));
    ...
}
```

### Async/Await

- В названиях публичных (и по желанию в приватных) асинхронных методов (== методы, которые возвращает Task) использовать постфикс `Async`
``` csharp
interface IRenderer
{
    Task<RenderResult> RenderAsync();
}
```
- При вызове асинхронных методов с использованием await в коде библиотек всегда использовать вызов `ConfigureAwait(false)`.
[ConfigureAwait FAQ](https://devblogs.microsoft.com/dotnet/configureawait-faq/) by Stephen Toub.
``` csharp
await renderer.RenderAsync().ConfigureAwait(false);
```

### Оператор `if`
- **Не использовать** запись `if` в одну строку.
``` csharp
if (source == null) throw new ArgumentNullException("source");
```
- Скобки в конструкции `if`/`else if`/`else` всегда допустимы. И необходимы во всех блоках, если хотя бы один из блоков их использует.
``` csharp
if (conditionOne)
{
    single statement;
}
else if (conditionTwo)
{
    first statement;
    second statement;
}
else
{
    single statement;
}
```
- Скобки можно не использовать, только в случае если каждый блок состоит из однострочного выражения.
``` csharp
if (conditionOne)
  DoSomething();
else if (anotherCondition)
  DoSomethingElse();
else
  DoAnotherThing();
```

### Разбиение кода на файлы
- Для каждого класса предпочтительно использовать отдельный файл. Это упрощает поиск класса среди файлов. И отслеживание изменений через систему контроля версий.