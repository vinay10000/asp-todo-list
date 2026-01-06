Hi


giv me c# all methods, syntax , data structures... allll

Perfect! Now I'll create your final comprehensive report.

***

# C# Complete Reference Guide: Methods, Syntax, Data Structures

## Executive Summary

This comprehensive guide covers every essential aspect of C# programming: method definitions, syntax features, data structures, operators, keywords, and advanced language features. Whether you're preparing for technical interviews, job applications, or deepening your C# expertise, this reference provides authoritative, code-rich examples covering C# through version 14.

***

## Part 1: Methods – The Foundation of C# Programming

### Method Fundamentals

A method is a reusable block of code that performs specific tasks.[1] Every C# program executes within methods, making them the fundamental unit of organization in object-oriented programming.

**Method Signature Structure:**
```csharp
[access_modifier] [modifiers] return_type method_name([parameters])
{
    // method body
}
```

**Signature Components Explained:[1]**

- **Access Modifier:** Controls visibility—`public` (accessible anywhere), `private` (class-only, default), `protected` (derived classes), `internal` (assembly-only), or `protected internal` (union of both)
- **Modifiers:** Include `static` (belongs to type), `virtual` (overridable), `abstract` (must be implemented), `async` (asynchronous), `sealed` (non-overridable)
- **Return Type:** The data type the method returns (`void` for no return value)
- **Method Name:** Uses PascalCase convention
- **Parameter List:** Optional input parameters with types

**Practical Example:**
```csharp
public static int CalculateSum(int num1, int num2)
{
    return num1 + num2;
}

// Instance method
private void DisplayMessage(string message)
{
    Console.WriteLine(message);
}

// Expression-bodied member (concise syntax)
public int Square(int x) => x * x;

// Async method
public async Task<string> FetchDataAsync()
{
    await Task.Delay(100);
    return "Data";
}
```
### Method Invocation Patterns

**Instance Methods** require object instantiation:
```csharp
MyClass obj = new MyClass();
obj.InstanceMethod();
```

**Static Methods** called via type name:
```csharp
ClassName.StaticMethod();
```

**Named Arguments** provide clarity and flexibility:
```csharp
int result = moto.Drive(speed: 60, miles: 170);
// Arguments can be in any order
```

**Method Overloading** allows multiple methods with the same name but different parameter signatures:
```csharp
public void Display(int x)           // Overload 1
public void Display(string x)        // Overload 2
public void Display(int x, string y) // Overload 3
```

The compiler determines which overload to call based on argument types and count.[1]

***

## Part 2: Parameters – How Methods Receive Data

### Parameter Types and Passing Mechanisms[1]

**Value Parameters** (default behavior):
```csharp
static void Modify(int x)
{
    x = 30;  // Changes local copy only
}

int value = 20;
Modify(value);
// value remains 20 - changes to parameter don't affect original
```

**Reference Parameters (`ref`)** pass actual references:
```csharp
static void Swap(ref int x, ref int y) => 
    (y, x) = (x, y);

int a = 2, b = 3;
Swap(ref a, ref b);
// a is now 3, b is now 2
```

**Output Parameters (`out`)** for methods that need to return multiple values through parameters:
```csharp
static void GetValues(out int x, out string y)
{
    x = 10;
    y = "Hello";
}

GetValues(out int result, out string message);
// Both variables assigned by method
```

**Input Parameters (`in`, C# 7.2+)** prevent copying while ensuring read-only access:
```csharp
static void Display(in Person person)
{
    Console.WriteLine(person.Name);  // Can read
    // person = new Person();        // Error: cannot modify
}
```

**Parameter Collections (`params`)** enable variable-length argument lists:
```csharp
static string GetVowels(params IEnumerable<string>? input)
{
    char[] vowels = ['A', 'E', 'I', 'O', 'U'];
    return string.Concat(
        input.SelectMany(
            word => word.Where(letter => vowels.Contains(char.ToUpper(letter)))));
}

string result = GetVowels("apple", "banana", "pear");
// Can pass individual arguments, arrays, or nothing
```

### Optional Parameters and Default Values

Methods can specify default values, allowing callers to omit arguments:[1]

```csharp
public void ExampleMethod(int required, int optionalInt = 0, 
                          string description = "N/A")
{
    var msg = $"{description}: {required} + {optionalInt} = {required + optionalInt}";
    Console.WriteLine(msg);
}

// All valid:
ExampleMethod(10);
ExampleMethod(10, 5);
ExampleMethod(10, 5, "Sum");
ExampleMethod(12, description: "Addition");

// Output examples:
// N/A: 10 + 0 = 10
// N/A: 10 + 5 = 15
// Sum: 10 + 5 = 15
// Addition: 12 + 0 = 12
```

Optional parameters must follow required parameters in the parameter list.

***

## Part 3: Return Values – Sending Data Out

### Single Value Returns

The simplest return pattern:
```csharp
public int CalculateSum(int a, int b)
{
    return a + b;
}

// Expression-bodied alternative
public int Multiply(int x, int y) => x * y;
```

### Multiple Return Values with Tuples[1]

Modern C# supports returning multiple values via tuples:

```csharp
// Unnamed tuple
public (string, string, string, int) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}

var person = GetPersonalInfo("111111111");
Console.WriteLine($"{person.Item1} {person.Item3}: age = {person.Item4}");

// Named tuple (preferred)
public (string FName, string MName, string LName, int Age) GetPersonalInfoNamed(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}

var info = GetPersonalInfoNamed("111111111");
Console.WriteLine($"{info.FName} {info.LName}: age = {info.Age}");

// Deconstruction
(string first, string middle, string last, int age) = GetPersonalInfoNamed("111");
```

### Early Exit with return

The `return` keyword stops method execution:
```csharp
public void ProcessData(List<int> items)
{
    if (items == null || items.Count == 0)
        return;  // Exit early if empty
    
    foreach (var item in items)
        Console.WriteLine(item);
}
```

***

## Part 4: Data Structures and Collections

Data structures organize and manage data efficiently. C# provides extensive built-in collections through `System.Collections` and `System.Collections.Generic` namespaces.[2]
### Linear Collections

**Arrays** – Fixed-size, index-based collections:
```csharp
// Declaration
int[] numbers = new int[5];
int[] initialized = { 1, 2, 3, 4, 5 };
int[] expressionBodied = [1, 2, 3];  // C# 12+

// Properties
numbers.Length      // Get array size
numbers[0]          // Access element
Array.Sort(numbers);
Array.Reverse(numbers);
```

**List<T>** – Dynamic, generic, indexed collection (most common):[3]
```csharp
List<int> numbers = new() { 1, 2, 3 };
List<string> names = ["Alice", "Bob"];  // C# 12+

// Essential Methods
numbers.Add(4);              // Add one element
numbers.AddRange([5, 6]);    // Add multiple
numbers.Insert(0, 0);        // Insert at index
numbers.Remove(5);           // Remove by value
numbers.RemoveAt(2);         // Remove by index
numbers.Clear();             // Remove all
numbers.Contains(3);         // Check existence
numbers.IndexOf(3);          // Find index
numbers.Count;               // Total elements
numbers.Sort();              // Sort ascending
numbers.FindAll(x => x > 2); // Find matching

// Iteration
foreach (var num in numbers) Console.WriteLine(num);
for (int i = 0; i < numbers.Count; i++) Console.WriteLine(numbers[i]);
```

**Stack<T>** – LIFO (Last In, First Out) access pattern:
```csharp
Stack<int> stack = new();
stack.Push(1);      // Add to top
stack.Pop();        // Remove and return top
stack.Peek();       // View without removing
```

**Queue<T>** – FIFO (First In, First Out) access pattern:
```csharp
Queue<string> queue = new();
queue.Enqueue("First");   // Add to back
queue.Dequeue();          // Remove from front
queue.Peek();             // View front
```

**LinkedList<T>** – Node-based, efficient insertion/deletion:
```csharp
LinkedList<int> list = new();
list.AddFirst(1);      // Add to beginning
list.AddLast(3);       // Add to end
list.First.Value;      // Access first node
list.Remove(node);     // Remove node
```

### Key-Value Collections

**Dictionary<TKey, TValue>** – Fast O(1) lookup by key:[2]
```csharp
Dictionary<string, int> ages = new()
{
    { "Alice", 25 },
    { "Bob", 30 }
};

Dictionary<int, string> map = new()
{
    [1] = "One",
    [2] = "Two"
};

// Operations
ages.Add("Charlie", 35);
ages["Dave"] = 28;           // Add or update
ages.Remove("Alice");        // Remove by key
ages.ContainsKey("Bob");     // Check key
ages.TryGetValue("Eve", out var age);
ages.Count;
ages.Keys;
ages.Values;

// Iteration
foreach (var kvp in ages)
    Console.WriteLine($"{kvp.Key}: {kvp.Value}");
```

**SortedList<TKey, TValue>** – Automatically maintains key sort order:
```csharp
SortedList<int, string> sorted = new();
sorted.Add(3, "Three");
sorted.Add(1, "One");
sorted.Add(2, "Two");
// Automatically ordered: 1→One, 2→Two, 3→Three
```

### Set-Based Collections

**HashSet<T>** – Unordered unique elements with O(1) lookup:
```csharp
HashSet<int> set = new() { 1, 2, 3 };
set.Add(4);
set.Remove(2);
set.Contains(3);

// Set operations
set.UnionWith(other);        // Combine
set.IntersectWith(other);    // Common elements
set.ExceptWith(other);       // Remove common
set.SymmetricExceptWith(other);  // Unique elements
```

**SortedSet<T>** – Sorted unique elements:
```csharp
SortedSet<int> sorted = new() { 3, 1, 2 };
// Maintains ascending order automatically
```

### Specialized Collections

**Tuple & ValueTuple** – Immutable groupings:[2]
```csharp
// ValueTuple (value type, preferred)
var person = ("John", 30, "Engineer");
var named = (Name: "John", Age: 30, Title: "Engineer");

// Access
Console.WriteLine(person.Item1);  // "John"
Console.WriteLine(named.Name);     // "John"

// Deconstruction
var (name, age, title) = named;
```

### Collection Selection Guide

| Use Case | Recommended | Rationale |
|----------|------------|-----------|
| Fixed-size sequential access | `Array` | Best performance for fixed size |
| Dynamic indexed access | `List<T>` | Flexible resizing, fast indexing |
| LIFO processing | `Stack<T>` | Perfect for undo stacks, DFS |
| FIFO processing | `Queue<T>` | Ideal for task queues, BFS |
| Unique items only | `HashSet<T>` | O(1) membership testing |
| Fast key-value lookup | `Dictionary<K,V>` | O(1) average retrieval |
| Ordered key-value pairs | `SortedList<K,V>` | Maintains key order |
| Linked sequential access | `LinkedList<T>` | Efficient insertion/deletion |
| Return multiple values | `ValueTuple` | Lightweight, immutable |

***

## Part 5: Keywords – The Language's Reserved Words

C# contains 77 reserved keywords with predefined meanings that cannot be used as identifiers (unless prefixed with `@`).[4]

### Reserved Type Keywords

```csharp
// Primitive Types
bool, byte, char, decimal, double, float, int, long, 
sbyte, short, string, struct, uint, ulong, ushort

// Reference Types
class, interface, enum, object, void, null
```

### Access Modifiers

- **`public`** – Accessible anywhere in any assembly
- **`private`** – Accessible only within the same class (default)
- **`protected`** – Accessible in class and derived classes
- **`internal`** – Accessible within same assembly only
- **`protected internal`** – Union: accessible in same assembly OR derived classes

### Method & Class Modifiers

```csharp
abstract        // Must be implemented by derived classes
sealed          // Cannot be inherited or overridden
static          // Belongs to type, not instance
readonly        // Cannot be reassigned after initialization
const           // Compile-time constant
volatile        // May change unexpectedly (threading)
virtual         // Can be overridden by derived classes
override        // Implements virtual member from base
explicit        // Explicit type conversion required
implicit        // Implicit type conversion allowed
async           // Asynchronous execution
await           // Wait for async operation
```

### Control Flow Keywords

```csharp
if, else                    // Conditional execution
switch, case, default       // Multi-way selection
for, foreach, while, do     // Loop constructs
break, continue             // Loop control
return                      // Exit method
goto                        // Unconditional jump (rarely used)
throw, try, catch, finally  // Exception handling
checked, unchecked          // Overflow checking
fixed, lock                 // Synchronization
```

### Contextual Keywords (Special Meaning in Specific Contexts)

```csharp
var             // Implicit type declaration
dynamic         // Runtime type resolution
async, await    // Asynchronous programming
where           // Generic constraints OR LINQ filtering
select, from, let, join, orderby, group, into  // LINQ
yield           // Iterator implementation
record          // Immutable reference type
init            // Init-only property setter
required        // Required member initialization
with            // Non-destructive mutation
partial         // Partial class/method definitions
nameof          // Get member name as string
```

***

## Part 6: Operators – Computational Tools

### Arithmetic Operators[5]

```csharp
+    // Addition:       a + b
-    // Subtraction:    a - b
*    // Multiplication: a * b
/    // Division:       a / b (integer or float)
%    // Modulus:        a % b (remainder)
++   // Increment:      a++ (post) or ++a (pre)
--   // Decrement:      a-- (post) or --a (pre)

// Examples
int a = 10, b = 3;
a + b   // 13
a - b   // 7
a * b   // 30
a / b   // 3
a % b   // 1
```

### Comparison Operators[5]

```csharp
==   // Equal:          a == b
!=   // Not equal:      a != b
<    // Less than:      a < b
>    // Greater than:   a > b
<=   // Less or equal:  a <= b
>=   // Greater or equal: a >= b

// Examples
5 == 5      // true
5 != 3      // true
5 > 3       // true
3 <= 3      // true
```

### Logical & Bitwise Operators[5]

```csharp
&&   // Logical AND (short-circuit):  a && b
||   // Logical OR (short-circuit):   a || b
!    // Logical NOT:                  !a
&    // Bitwise AND:                  a & b
|    // Bitwise OR:                   a | b
^    // Bitwise XOR:                  a ^ b
~    // Bitwise NOT:                  ~a
<<   // Left shift:                   a << n
>>   // Right shift:                  a >> n
>>>  // Unsigned right shift:         a >>> n
```

### Assignment Operators

```csharp
=    // Simple assignment:      x = 5
+=   // Add and assign:         x += 5      (x = x + 5)
-=   // Subtract and assign:    x -= 3      (x = x - 3)
*=   // Multiply and assign:    x *= 2      (x = x * 2)
/=   // Divide and assign:      x /= 2      (x = x / 2)
%=   // Modulus and assign:     x %= 3      (x = x % 3)
&=   // Bitwise AND and assign: x &= y
|=   // Bitwise OR and assign:  x |= y
^=   // Bitwise XOR and assign: x ^= y
<<=  // Left shift and assign:  x <<= 2
>>=  // Right shift and assign: x >>= 2
??=  // Null coalesce and assign: x ??= default
```

### Special Operators[5]

**Null-Coalescing (`??`)** – Default value if null:
```csharp
string name = userInput ?? "Unknown";
// Returns userInput if not null, else "Unknown"

int? age = null;
int actualAge = age ?? 0;  // 0
```

**Null-Conditional (`?.` and `?[]`)** – Safe navigation:
```csharp
int? length = person?.Name?.Length;
// Returns length if person and Name are not null, else null

object? value = list?[0];
// Returns element if list is not null, else null
```

**Conditional/Ternary (`?:`)** – Inline if-else:
```csharp
string status = (age >= 18) ? "Adult" : "Minor";
int category = score > 80 ? 1 : score > 60 ? 2 : 3;
```

**Type-Related Operators:**
```csharp
is          // Type check:     if (obj is string s)
as          // Safe cast:      string s = obj as string;
typeof      // Get type:       Type t = typeof(int);
new         // Create instance: new ClassName();
nameof      // Get name:       nameof(variable)
checked     // Check overflow: checked(a + b)
unchecked   // Skip overflow:  unchecked(a + b)
```

### Operator Precedence (High to Low)[5]

Operators with higher precedence evaluate before lower precedence. Use parentheses to override:

| Priority | Operators | Category |
|----------|-----------|----------|
| 1 | `x.y`, `f(x)`, `a[i]`, `x?.y`, `x?[y]`, `x++`, `x--`, `x!`, `new`, `typeof`, `checked`, `unchecked`, `default`, `nameof`, `delegate`, `sizeof`, `stackalloc`, `x->y` | Primary |
| 2 | `+x`, `-x`, `!x`, `~x`, `++x`, `--x`, `^x`, `(T)x`, `await`, `&x`, `*x`, `true`, `false` | Unary |
| 3 | `x..y` | Range |
| 4 | `x * y`, `x / y`, `x % y` | Multiplicative |
| 5 | `x + y`, `x – y` | Additive |
| 6 | `x << y`, `x >> y`, `x >>> y` | Shift |
| 7 | `x < y`, `x > y`, `x <= y`, `x >= y`, `is`, `as` | Relational |
| 8 | `x == y`, `x != y` | Equality |
| 9 | `x & y` | AND |
| 10 | `x ^ y` | XOR |
| 11 | `x | y` | OR |
| 12 | `x && y` | Conditional AND |
| 13 | `x || y` | Conditional OR |
| 14 | `x ?? y` | Null-coalescing |
| 15 | `c ? t : f` | Conditional |
| 16 | Assignment, lambda | Assignment |

**Associativity:**
- **Left-associative** (most operators): `a - b + c` evaluates as `(a - b) + c`
- **Right-associative** (assignment, null coalescing, conditional): `x = y = z` evaluates as `x = (y = z)`

***

## Part 7: Advanced Language Features

### Generic Methods & Constraints[6]

Generics enable type-safe, reusable code:

```csharp
// Generic method
public T GetFirst<T>(List<T> items)
{
    return items.Count > 0 ? items[0] : default;
}

// Generic class
public class Container<T>
{
    private T value;
    public void Set(T val) => value = val;
    public T Get() => value;
}

// Usage
int first = GetFirst(new List<int> { 1, 2, 3 });
var container = new Container<string>();
container.Set("Hello");
```

**Generic Constraints** restrict type parameters:[6][7]

```csharp
// Reference type constraint
public class RefContainer<T> where T : class
{
    // T must be a reference type (class, interface, delegate, array)
}

// Value type constraint
public class ValueContainer<T> where T : struct
{
    // T must be a value type (int, double, bool, etc., but not nullable)
}

// Interface constraint
public void Process<T>(T item) where T : IEnumerable
{
    // T must implement IEnumerable interface
}

// Base class constraint
public class Derived<T> where T : BaseClass
{
    // T must be BaseClass or derive from it
}

// Parameterless constructor constraint
public T Create<T>() where T : new()
{
    return new T();  // T must have public parameterless constructor
}

// Multiple constraints
public class Multi<T, U> 
    where T : class, IComparable, new()
    where U : struct
{
    // T: reference type AND implements IComparable AND has parameterless constructor
    // U: value type
}
```

### Lambda Expressions & Delegates

Concise anonymous functions enabling functional programming:

```csharp
// Single expression lambda
Func<int, int> square = x => x * x;
int result = square(5);  // 25

// Multiple parameters
Func<int, int, int> add = (x, y) => x + y;

// Statement body
Func<int, string> classify = x =>
{
    if (x > 0) return "Positive";
    if (x < 0) return "Negative";
    return "Zero";
};

// No parameters
Action greet = () => Console.WriteLine("Hello!");

// With LINQ
List<int> numbers = [1, 2, 3, 4, 5];
var evenNumbers = numbers.Where(n => n % 2 == 0);
var doubled = numbers.Select(n => n * 2).ToList();
var total = numbers.Sum(n => n);
```

### LINQ – Language Integrated Query[2]

Query collections with SQL-like syntax:

```csharp
List<int> nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Method syntax
var evens = nums.Where(n => n % 2 == 0).ToList();
var squared = nums.Select(n => n * n).ToList();
var sorted = nums.OrderByDescending(n => n).ToList();

// Query syntax
var query = from n in nums
            where n > 5
            orderby n descending
            select n * 2;

// Common LINQ methods
nums.Where(n => n > 5);         // Filter
nums.Select(n => n * 2);        // Transform
nums.OrderBy(n => n);           // Sort ascending
nums.OrderByDescending(n => n); // Sort descending
nums.GroupBy(n => n % 2);       // Group
nums.Distinct();                // Remove duplicates
nums.Take(3);                   // First 3
nums.Skip(2);                   // Skip first 2
nums.First();                   // First element
nums.FirstOrDefault();          // First or null
nums.Count();                   // Total count
nums.Sum();                     // Sum all
nums.Average();                 // Average value
nums.Any(n => n > 5);           // Any match?
nums.All(n => n < 100);         // All match?
```

### Asynchronous Programming[1]

Non-blocking operations essential for I/O and responsiveness:

```csharp
// Async method returning Task<T>
public async Task<int> FetchDataAsync()
{
    await Task.Delay(1000);  // Simulate delay
    return 42;
}

// Usage
public async Task Main()
{
    int result = await FetchDataAsync();
    Console.WriteLine(result);
}

// Multiple async operations
Task<int> task1 = FetchDataAsync();
Task<int> task2 = FetchDataAsync();
int[] results = await Task.WhenAll(task1, task2);
// Wait for all to complete

Task<int> completed = await Task.WhenAny(task1, task2);
// Wait for first to complete

// Async void (only for event handlers)
public async void Button_Click(object sender, EventArgs e)
{
    await ProcessAsync();
}
```

### Pattern Matching[1]

Powerful, expressive conditional logic:

```csharp
// Type pattern
if (obj is string s)
{
    Console.WriteLine(s);
}

// Relational pattern
if (age is >= 18 and < 65)
{
    // Working age adult
}

// Property pattern
if (person is { Name: "John", Age: > 30 })
{
    // John over 30
}

// Switch expression (C# 8+)
string status = age switch
{
    < 13 => "Child",
    < 18 => "Teen",
    < 65 => "Adult",
    _ => "Senior"
};

// List pattern (C# 11+)
if (numbers is [1, 2, ..])
{
    // Starts with 1, 2
}
```

### Exception Handling

Robust error management:

```csharp
try
{
    int result = 10 / int.Parse("0");
}
catch (DivideByZeroException ex)
{
    Console.WriteLine("Cannot divide by zero");
}
catch (FormatException ex)
{
    Console.WriteLine("Invalid format");
}
catch (Exception ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
finally
{
    Console.WriteLine("Cleanup code always runs");
}

// Throwing exceptions
if (age < 0)
    throw new ArgumentException("Age cannot be negative");

// Re-throw preserving stack trace
catch (Exception ex)
{
    throw;
}
```

***

## Part 8: Practical Best Practices & Patterns

### Code Style Conventions

```csharp
// PascalCase for public types, methods, properties
public class UserAccount { }
public void ProcessData() { }
public string FullName { get; set; }

// camelCase for parameters and local variables
public void Calculate(int baseValue, decimal rate)
{
    int calculatedResult = baseValue * 10;
}

// UPPER_CASE for constants
private const double PI = 3.14159;
public static readonly string DefaultName = "User";
```

### Null Safety Patterns

```csharp
// Explicit nullable types
int? age = null;
if (age.HasValue)
    Console.WriteLine(age.Value);

// Null coalescing
string name = userName ?? "Unknown";
int finalAge = age ?? 0;

// Null-conditional operators
int? length = person?.Name?.Length;
var value = list?[0];

// Pattern matching
if (person is not null)
{
    Console.WriteLine(person.Name);
}
```

### Resource Management

```csharp
// Using statement ensures disposal
using (StreamReader reader = new StreamReader("file.txt"))
{
    string content = reader.ReadToEnd();
}  // reader.Dispose() called automatically

// Using declaration (C# 8+)
using StreamReader reader = new("file.txt");
string content = reader.ReadToEnd();
// Disposed at end of scope
```

***

## Comprehensive Reference Summary

This guide encompasses:

1. **Methods** – Complete syntax, signatures, invocation patterns, and parameter types
2. **Data Structures** – All major collections with complexity analysis and use cases
3. **Keywords** – 77 reserved keywords organized by function plus contextual keywords
4. **Operators** – All operator categories with precedence tables and associativity rules
5. **Advanced Features** – Generics, lambdas, LINQ, async/await, pattern matching
6. **Best Practices** – Style conventions, null safety, resource management

For deeper exploration of specific topics, refer to Microsoft's official C# language reference[8][9] and the authoritative C# language specification.[10]

This reference serves as both a learning tool for career development and a quick-lookup guide for active development—ideal preparation for technical interviews, job applications, and professional C# development.

***

**Citations:**

[1] https://learn.microsoft.com/en-us/dotnet/csharp/methods

[11] https://www.w3schools.com/cs/cs_methods.php

[12] https://www.geeksforgeeks.org/c-sharp/c-sharp-data-structures/

[3] https://uomustansiriyah.edu.iq/media/lectures/6/6_2024_11_23!10_11_58_AM.pdf

[8] https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/

[2] https://learn.microsoft.com/en-us/dotnet/standard/collections/

[10] https://www.ecma-international.org/wp-content/uploads/ECMA-334_1st_edition_december_2001.pdf

[9] https://learn.microsoft.com/en-us/dotnet/csharp/methods

[7] https://www.telerik.com/blogs/constraining-generics-in-c

[5] https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/

[6] https://dotnettutorials.net/lesson/generic-constraints-in-csharp/

[4] https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/

 Image reference for method anatomy diagram

Citations:
[1] C# Methods - W3Schools https://www.w3schools.com/cs/cs_methods.php
[2] Collections and Data Structures - .NET https://learn.microsoft.com/en-us/dotnet/standard/collections/
[3] L3- Lists Data Structure 2024-2025 D https://uomustansiriyah.edu.iq/media/lectures/6/6_2024_11_23!10_11_58_AM.pdf
[4] C# Keywords and contextual keywords - C# reference https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/
[5] List all operators and expression - C# reference https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/
[6] Generic Constraints in C# with Examples https://dotnettutorials.net/lesson/generic-constraints-in-csharp/
[7] Constrain by Class https://www.telerik.com/blogs/constraining-generics-in-c
[8] C# language reference https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/
[9] Overview of methods - C# | Microsoft Learn https://learn.microsoft.com/en-us/dotnet/csharp/methods
[10] C# Language Specification https://www.ecma-international.org/wp-content/uploads/ECMA-334_1st_edition_december_2001.pdf
[11] C# Data Structures https://www.geeksforgeeks.org/c-sharp/c-sharp-data-structures/
[12] Method in C#: Learn How to Use Methods in C# https://www.scholarhat.com/tutorial/csharp/methods-in-csharp
[13] C# Guide - .NET managed language https://learn.microsoft.com/en-us/dotnet/csharp/
[14] C# (C Sharp) | Methods - Codecademy https://www.codecademy.com/resources/docs/c-sharp/methods
[15] A Comprehensive Guide with Examples for Data Structures ... https://www.cloudthat.com/resources/blog/a-comprehensive-guide-with-examples-for-data-structures-in-c
[16] ECMA-334 https://ecma-international.org/publications-and-standards/standards/ecma-334/
[17] Methods in C# - GeeksforGeeks https://www.geeksforgeeks.org/c-sharp/methods-in-c-sharp/
[18] Net Data structures: ArrayList, List, HashTable, Dictionary, ... https://stackoverflow.com/questions/128636/net-data-structures-arraylist-list-hashtable-dictionary-sortedlist-sorted
[19] C Sharp (programming language) https://en.wikipedia.org/wiki/C_Sharp_(programming_language)
[20] C# Methods Tutorial - .NET Methods explained in easy words https://tutorials.eu/methods-in-net-7-0/
[21] Learn All About Methods in C# (With Examples) | Simplilearn https://www.simplilearn.com/tutorials/csharp-tutorial/c-sharp-methods
[22] Data Structures in C# Part 2: Lists <T> https://dev.to/rasheedmozaffar/data-structures-in-c-part-2-lists-e76
[23] Essential C# https://essentialcsharp.com
[24] Data Structures In C#: A Comprehensive Guide https://www.nilebits.com/blog/2024/02/data-structures-in-csharp/
[25] C# - Operators Precedence https://www.tutorialspoint.com/csharp/csharp_operators_precedence.htm
[26] List C# keywords https://stackoverflow.com/questions/44728159/list-c-sharp-keywords
[27] Generic method multiple (OR) type constraint https://stackoverflow.com/questions/10833918/generic-method-multiple-or-type-constraint
[28] Operators and expressions - List all operators and expression - C# reference https://learn.microsoft.com/hi-in/dotnet/csharp/language-reference/operators/
[29] C# Keywords https://www.geeksforgeeks.org/c-sharp/c-sharp-keywords/
[30] C# Keywords and Identifiers https://www.programiz.com/csharp-programming/keywords-identifiers
[31] C# Operator Precedence and Associativity - Programiz https://www.programiz.com/csharp-programming/operator-precedence-associativity
[32] Keywords in C#: Types of Keywords with Examples https://www.scholarhat.com/tutorial/csharp/keywords-in-csharp
[33] Generic Constraints in C# - GeeksforGeeks https://www.geeksforgeeks.org/c-sharp/generic-constraints-in-c-sharp/
[34] Operator Precedence in Programming https://www.geeksforgeeks.org/dsa/operator-precedence-in-programming/
[35] C# keywords https://www.tutorialsteacher.com/csharp/csharp-keywords
[36] Generic method with type constraints or base class parameter https://stackoverflow.com/questions/48748604/generic-method-with-type-constraints-or-base-class-parameter
[37] C# Operator Precedence and Logic Order https://stackhub.net/manuals/c-operator-precedence-and-logic-order
[38] C# Generic Constraints Explained https://www.youtube.com/watch?v=u3a_7Ii3axE
[39] Operator Precedence and Associativity in C# Programming https://www.programtopia.net/csharp/docs/operator-precedence-and-associativity
[40] C# Keywords https://www.c-sharpcorner.com/blogs/c-sharp-keywords
