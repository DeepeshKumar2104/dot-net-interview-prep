# 🎓 Complete C# Interview Guide - All in One
## Linkwell Telesystems Pvt Ltd (VISIONTEK)

**Last Updated:** April 2, 2026  
**Purpose:** Complete interview preparation with concepts, real examples, and answers  
**Target:** Linkwell Telesystems C# Developer Interview

---

## 📑 Table of Contents

1. [OOP - Complete Guide](#1-oop---complete-guide)
2. [Value vs Reference Types](#2-value-vs-reference-types)
3. [String vs StringBuilder](#3-string-vs-stringbuilder)
4. [var vs dynamic](#4-var-vs-dynamic)
5. [Exception Handling](#5-exception-handling)
6. [Async/Await](#6-asyncawait)
7. [Delegates](#7-delegates)
8. [Practice Questions & Answers](#8-practice-questions--answers)

---

# 1. OOP - Complete Guide

## What is OOP?

**Definition:**  
Object-Oriented Programming is a programming paradigm that organizes code into "objects" containing both data (properties) and behavior (methods). It models real-world entities.

**Simple Analogy:**  
Think of a Car object:
- **Data (Properties):** Color, Speed, FuelLevel, Type
- **Behavior (Methods):** Accelerate(), Brake(), Honk()

```csharp
// Real-world OOP example
public class Car
{
    // Data - Properties
    public string Color { get; set; }
    public int Speed { get; set; }
    public double FuelLevel { get; set; }
    
    // Behavior - Methods
    public void Accelerate()
    {
        Speed += 10;
        FuelLevel -= 1;
    }
    
    public void Brake()
    {
        Speed -= 10;
    }
}

// Usage
Car mycar = new Car { Color = "Red", Speed = 0, FuelLevel = 50 };
mycar.Accelerate();  // Speed = 10, Fuel = 49
```

## Why We Study OOP?

| Reason | Explanation |
|--------|------------|
| **Code Organization** | Related data and methods grouped together |
| **Reusability** | Write once, use many times (inheritance) |
| **Maintainability** | Easy to find and fix bugs |
| **Scalability** | Build large systems without chaos |
| **Real-World Mapping** | Code matches how we think about real entities |
| **Team Collaboration** | Clear structure makes team work easier |

---

## 4 Pillars of OOP

### A) ENCAPSULATION (डेटा छुपाना)

**What is it?**  
Bundling data (properties) and methods together, hiding internal details from outside world.

**Why study?**
- Protects sensitive data from unauthorized access
- Allows controlled access to object state
- Can change internal implementation without affecting external code

**Real-World Example: Bank Account**

```csharp
public class BankAccount
{
    // ❌ DON'T expose balance directly
    // public decimal balance;  // Anyone can change this!
    
    // ✅ DO encapsulate with access control
    private decimal balance = 0;  // Hidden from outside
    
    public decimal GetBalance()
    {
        // Controlled read access
        return balance;
    }
    
    public void Deposit(decimal amount)
    {
        // Validation before deposit
        if (amount <= 0)
            throw new ArgumentException("Amount must be positive");
        
        balance += amount;
        Console.WriteLine($"Deposited: {amount}. New balance: {balance}");
    }
    
    public void Withdraw(decimal amount)
    {
        // Check before withdrawal
        if (amount > balance)
            throw new InvalidOperationException("Insufficient funds");
        
        balance -= amount;
        Console.WriteLine($"Withdrew: {amount}. New balance: {balance}");
    }
}

// Usage
BankAccount account = new BankAccount();
account.Deposit(1000);      // ✅ Works
account.Withdraw(500);      // ✅ Works
account.Withdraw(600);      // ✗ Throws error - insufficient funds
// account.balance = -9999;  // ✗ Can't access private field
```

**Real scenario:**  
In Linkwell's banking systems (they work with banks), account balance MUST be protected. Without encapsulation, anyone could set balance to any value!

---

### B) INHERITANCE (विरासत)

**What is it?**  
A class derives properties and methods from a parent class, enabling code reuse and establishing hierarchical relationships.

**Why study?**
- Avoid code duplication
- Establish clear relationships
- Extend functionality easily

**Real-World Example: Employee Hierarchy**

```csharp
// Parent class
public class Employee
{
    public string Name { get; set; }
    public int Id { get; set; }
    public decimal Salary { get; set; }
    
    public virtual void GetDetails()
    {
        Console.WriteLine($"Name: {Name}, ID: {Id}, Salary: {Salary}");
    }
}

// Child class 1 - inherits from Employee
public class Manager : Employee
{
    public int TeamSize { get; set; }
    
    public override void GetDetails()
    {
        base.GetDetails();  // Call parent method
        Console.WriteLine($"Manages {TeamSize} people");
    }
}

// Child class 2 - inherits from Employee
public class Developer : Employee
{
    public string ProgrammingLanguage { get; set; }
    
    public override void GetDetails()
    {
        base.GetDetails();
        Console.WriteLine($"Expert in: {ProgrammingLanguage}");
    }
}

// Usage
Employee emp1 = new Manager 
{ 
    Name = "John", 
    Id = 1, 
    Salary = 80000, 
    TeamSize = 5 
};

Employee emp2 = new Developer 
{ 
    Name = "Jane", 
    Id = 2, 
    Salary = 60000, 
    ProgrammingLanguage = "C#" 
};

emp1.GetDetails();
// Output:
// Name: John, ID: 1, Salary: 80000
// Manages 5 people

emp2.GetDetails();
// Output:
// Name: Jane, ID: 2, Salary: 60000
// Expert in: C#
```

**Real scenario:**  
Linkwell has multiple product types (POS terminals, Energy meters, Vehicle tracking systems). All share common properties (serialNumber, firmwareVersion, lastUpdate) but differ in specific features. Inheritance prevents duplicating common code.

---

### C) POLYMORPHISM (बहुरूपता)

**What is it?**  
Same method name, different behavior based on the object type. "Many forms."

**Why study?**
- Write flexible, extensible code
- Add new types without changing existing code
- Handle different objects uniformly

**Real-World Example: Payment Processing**

```csharp
// Base class/interface
public abstract class PaymentMethod
{
    public abstract decimal CalculateFee(decimal amount);
    public abstract void ProcessPayment(decimal amount);
}

// Different payment methods
public class CreditCard : PaymentMethod
{
    public override decimal CalculateFee(decimal amount)
    {
        return amount * 0.02m;  // 2% fee
    }
    
    public override void ProcessPayment(decimal amount)
    {
        decimal total = amount + CalculateFee(amount);
        Console.WriteLine($"Processed credit card: ${total}");
    }
}

public class UPI : PaymentMethod
{
    public override decimal CalculateFee(decimal amount)
    {
        return amount * 0.005m;  // 0.5% fee
    }
    
    public override void ProcessPayment(decimal amount)
    {
        decimal total = amount + CalculateFee(amount);
        Console.WriteLine($"Processed UPI: ${total}");
    }
}

public class NetBanking : PaymentMethod
{
    public override decimal CalculateFee(decimal amount)
    {
        return 0;  // No fee
    }
    
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processed net banking: ${amount}");
    }
}

// Generic processor - works with ANY payment method
public class PaymentProcessor
{
    public void Process(PaymentMethod method, decimal amount)
    {
        method.ProcessPayment(amount);
    }
}

// Usage
PaymentProcessor processor = new PaymentProcessor();

PaymentMethod payment1 = new CreditCard();
PaymentMethod payment2 = new UPI();
PaymentMethod payment3 = new NetBanking();

processor.Process(payment1, 1000);  // Credit card processing
processor.Process(payment2, 1000);  // UPI processing
processor.Process(payment3, 1000);  // Net banking processing

// Tomorrow: Add new payment method (Apple Pay)
// No changes needed to PaymentProcessor!
```

**Key Benefit:**  
You can add new payment methods without changing PaymentProcessor code. This is **Open-Closed Principle** - open for extension, closed for modification.

**Real scenario:**  
Linkwell's POS terminals need to support multiple payment methods (Card, UPI, Digital Wallets, BNPL). Polymorphism lets them add new methods without rewriting the entire system.

---

### D) ABSTRACTION (सारांश)

**What is it?**  
Hiding complex implementation details, showing only what's necessary.

**Why study?**
- Reduces complexity for users
- Lets you change internal implementation safely
- Focuses on "what" not "how"

**Real-World Example: ATM Machine**

```csharp
// Abstraction - user doesn't need to know internal details
public abstract class ATM
{
    // User interacts with these methods only
    public abstract bool ValidatePin(int pin);
    public abstract void DisplayBalance();
    public abstract void Withdraw(decimal amount);
    public abstract void Deposit(decimal amount);
    
    // Internal complex logic hidden
    protected abstract decimal GetBalance();
    protected abstract void UpdateDatabase();
}

public class SBIAtm : ATM
{
    private decimal balance = 5000;
    
    public override bool ValidatePin(int pin)
    {
        // Complex PIN validation with encryption
        // User doesn't need to know how this works
        return pin == 1234;  // Simplified for example
    }
    
    public override void DisplayBalance()
    {
        Console.WriteLine($"Your balance: ${GetBalance()}");
    }
    
    public override void Withdraw(decimal amount)
    {
        if (amount > balance)
        {
            Console.WriteLine("Insufficient balance");
            return;
        }
        
        balance -= amount;
        Console.WriteLine($"Withdrawal successful. New balance: ${balance}");
        UpdateDatabase();
    }
    
    public override void Deposit(decimal amount)
    {
        balance += amount;
        Console.WriteLine($"Deposit successful. New balance: ${balance}");
        UpdateDatabase();
    }
    
    protected override decimal GetBalance()
    {
        // Complex logic to get balance
        return balance;
    }
    
    protected override void UpdateDatabase()
    {
        // Complex DB update logic
        Console.WriteLine("Database updated");
    }
}

// User's interaction is simple
ATM atm = new SBIAtm();
if (atm.ValidatePin(1234))
{
    atm.DisplayBalance();
    atm.Withdraw(500);
}
// User doesn't know: how PIN is encrypted, how DB works, etc.
```

**Real scenario:**  
When you use an ATM, you don't know:
- How it connects to the bank's database
- How it encrypts your PIN
- How it manages physical cash
You just interact with simple buttons and screens. This is abstraction.

---

## Question: What's the Difference Between Abstract Class and Interface?

| Feature | Abstract Class | Interface |
|---------|---|---|
| **Purpose** | Base class for related objects (IS-A) | Contract for behavior (CAN-DO) |
| **Methods** | Can have implementation | Only signature (before C# 8.0) |
| **Fields** | Can have fields and state | Cannot have fields |
| **Constructor** | Can have constructor | Cannot have constructor |
| **Access Modifiers** | public, private, protected | public only |
| **Inheritance** | Single class inheritance | Multiple interface implementation |
| **Example** | `Animal` base class | `IFlyable`, `ISwimmable` |

**Real Code Example:**

```csharp
// ❌ WRONG - Interface can't have state
public interface IPaymentGateway
{
    decimal processingFee = 0.02m;  // ✗ Can't do this
}

// ✅ CORRECT - Abstract class for related objects
public abstract class PaymentGateway
{
    protected decimal processingFee = 0.02m;  // Can have state
    
    public abstract void Process();
}

// ✅ CORRECT - Interface for unrelated behaviors
public interface IRetundable
{
    void Refund(string transactionId);
}

public interface INotifiable
{
    void SendNotification();
}

// A class can implement multiple interfaces
public class StripeGateway : PaymentGateway, IRefundable, INotifiable
{
    public override void Process()
    {
        // Process payment
    }
    
    public void Refund(string id)
    {
        // Refund logic
    }
    
    public void SendNotification()
    {
        // Notification logic
    }
}
```

**When to use:**
- **Abstract Class:** Employee, Vehicle, BankAccount (related hierarchy)
- **Interface:** IPayable, ISerializable, IDisposable (behavior contract)

---

# 2. Value vs Reference Types

## What is Value Type?

**Definition:**  
Value types store their actual value on the **stack** memory. When you assign them to another variable, a **copy** is made.

**Real-world analogy:**  
Like copying a recipe. If you give your friend a recipe, you both have your own copy. Changes to your copy don't affect their copy.

```csharp
// Value types
int x = 5;
int y = x;  // y gets a COPY of 5
y = 10;     // Changing y doesn't affect x

Console.WriteLine(x);  // 5 (unchanged)
Console.WriteLine(y);  // 10 (changed)

// Both have separate values in memory
```

## What is Reference Type?

**Definition:**  
Reference types store a **reference/address** to data on the **heap**. Multiple variables can point to the same object.

**Real-world analogy:**  
Like sharing a house address. Multiple people can visit the same house. Changes to the house affect everyone visiting it.

```csharp
// Reference types
class Person
{
    public string Name { get; set; }
}

Person person1 = new Person { Name = "John" };
Person person2 = person1;  // person2 points to SAME object

person2.Name = "Jane";  // Changes the object

Console.WriteLine(person1.Name);  // Jane (changed!)
Console.WriteLine(person2.Name);  // Jane

// Both point to same object in memory
```

## Why Study This?

| Reason | Impact |
|--------|--------|
| **Memory efficiency** | Know where data lives (stack vs heap) |
| **Bug prevention** | Unexpected changes when you don't expect them |
| **Performance** | Stack allocation is faster than heap |
| **Understanding passing** | Know what happens when you pass parameters |

## Real-World Example: Banking System

```csharp
// VALUE TYPE - int (salary)
int salary1 = 50000;
int salary2 = salary1;
salary2 += 5000;

Console.WriteLine($"Salary 1: {salary1}");  // 50000 (unchanged)
Console.WriteLine($"Salary 2: {salary2}");  // 55000 (changed)

// Each person has their own copy of salary value

// REFERENCE TYPE - Employee object
public class Employee
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
}

Employee emp1 = new Employee { Name = "John", Salary = 50000 };
Employee emp2 = emp1;  // emp2 points to SAME employee

emp2.Salary = 55000;  // Modifying same object

Console.WriteLine($"Emp1 Salary: {emp1.Salary}");  // 55000 (changed!)
Console.WriteLine($"Emp2 Salary: {emp2.Salary}");  // 55000 (same object)

// Problem: emp1 and emp2 point to same person!
// If you meant to create separate employee, you'd do:
Employee emp3 = new Employee { Name = "John", Salary = 50000 };
Employee emp4 = new Employee { Name = "John", Salary = 50000 };
// Now emp3 and emp4 are separate objects
```

## Value Types List

```csharp
int age = 25;              // Value type
double height = 5.9;       // Value type
bool isActive = true;      // Value type
decimal salary = 50000.50m; // Value type - used for money!
struct Point                // Value type
{
    public int X;
    public int Y;
}
```

## Reference Types List

```csharp
string name = "John";      // Reference type
class Employee { }         // Reference type
List<int> numbers = new(); // Reference type
int[] array = new int[10]; // Reference type
Employee emp = new();      // Reference type
```

---

# 3. String vs StringBuilder

## What is String?

**Definition:**  
String is a reference type representing a sequence of characters. **Immutable** - cannot be changed once created.

```csharp
string name = "John";
name = name + " Doe";  // Creates NEW string object
// Old "John" object is discarded (garbage collected)
```

## What is StringBuilder?

**Definition:**  
StringBuilder is a **mutable** reference type. More efficient for multiple string operations because it reuses the same buffer.

## Why Study This?

Imagine building a long sentence word by word. Each time you add a word with String, you create a new sentence and throw away the old one. With StringBuilder, you just add to existing structure.

## Real-World Example: Building a Log Message

```csharp
// ❌ STRING - VERY SLOW
public string BuildLogMessage_String()
{
    string log = "";  // Create empty string
    
    for (int i = 0; i < 10000; i++)
    {
        log += $"[{DateTime.Now}] Message {i}\n";
        // Creates NEW string each iteration!
        // Old string is discarded
    }
    
    return log;
}
// Creates 10,000+ string objects in memory!
// Time: ~5000ms (5 seconds)

// ✅ STRINGBUILDER - VERY FAST
public string BuildLogMessage_StringBuilder()
{
    StringBuilder sb = new StringBuilder();
    
    for (int i = 0; i < 10000; i++)
    {
        sb.Append($"[{DateTime.Now}] Message {i}\n");
        // Appends to same buffer
    }
    
    return sb.ToString();
}
// Creates only 1 string builder object
// Time: ~10ms (0.01 seconds)
```

## Performance Comparison

```csharp
using System.Diagnostics;
using System.Text;

// Test with 1,000,000 concatenations
const int iterations = 1000000;

// Test 1: String concatenation
var sw = Stopwatch.StartNew();
string result = "";
for (int i = 0; i < iterations; i++)
{
    result += i.ToString();
}
sw.Stop();
Console.WriteLine($"String: {sw.ElapsedMilliseconds}ms");
// Output: ~2500ms

// Test 2: StringBuilder
sw = Stopwatch.StartNew();
StringBuilder sb = new StringBuilder();
for (int i = 0; i < iterations; i++)
{
    sb.Append(i.ToString());
}
string result2 = sb.ToString();
sw.Stop();
Console.WriteLine($"StringBuilder: {sw.ElapsedMilliseconds}ms");
// Output: ~5ms

// StringBuilder is 500x FASTER!
```

## When to Use Each

```csharp
// Use STRING for:
// - Single concatenation
// - Few operations (< 10)
string greeting = "Hello" + " " + "World";
string fullName = firstName + " " + lastName;

// Use STRINGBUILDER for:
// - Loops
// - Generating queries
// - Building large strings
StringBuilder query = new StringBuilder();
query.Append("SELECT * FROM Users WHERE ");
for (int i = 0; i < 100; i++)
{
    query.Append($"Status = 'Active' OR ");
}
query.Length -= 4;  // Remove last " OR "
```

## Real-World Scenario: Linkwell Transaction Log

```csharp
// In banking/POS systems, you often build transaction logs
public class TransactionLogger
{
    // ❌ BAD - Using string concatenation
    public string GenerateReport_Bad(List<Transaction> transactions)
    {
        string report = "=== TRANSACTION REPORT ===\n";
        
        foreach (var transaction in transactions)  // 1000s of transactions
        {
            report += $"ID: {transaction.Id}\n";
            report += $"Amount: ${transaction.Amount}\n";
            report += $"Status: {transaction.Status}\n";
            report += "---\n";
        }
        
        return report;  // Creates thousands of string objects!
    }
    
    // ✅ GOOD - Using StringBuilder
    public string GenerateReport_Good(List<Transaction> transactions)
    {
        StringBuilder report = new StringBuilder();
        report.AppendLine("=== TRANSACTION REPORT ===");
        
        foreach (var transaction in transactions)
        {
            report.AppendLine($"ID: {transaction.Id}");
            report.AppendLine($"Amount: ${transaction.Amount}");
            report.AppendLine($"Status: {transaction.Status}");
            report.AppendLine("---");
        }
        
        return report.ToString();  // Creates 1 string!
    }
}
```

---

# 4. var vs dynamic

## What is var?

**Definition:**  
`var` is a keyword that tells the compiler to infer the type from the right side of assignment. **Compile-time** type checking.

```csharp
var name = "John";        // Compiler knows it's string
var age = 25;             // Compiler knows it's int
var salary = 50000.50m;   // Compiler knows it's decimal

// These are EXACTLY the same as:
string name2 = "John";
int age2 = 25;
decimal salary2 = 50000.50m;
```

## What is dynamic?

**Definition:**  
`dynamic` tells the compiler to skip type checking. **Runtime** type checking using reflection.

```csharp
dynamic x = 5;
x = "Hello";      // Can change type!
x = new object(); // Can be anything

// You can call ANY method on dynamic
x.UnknownMethod();  // Compiles fine
x.RandomProperty;   // Compiles fine
// But crashes at runtime if method doesn't exist!
```

## Why Study This?

| Aspect | var | dynamic |
|--------|-----|---------|
| **Type Checking** | Compile-time (safe) | Runtime (risky) |
| **IntelliSense** | Full support (IDE helps) | Limited/none |
| **Performance** | No overhead | Reflection overhead |
| **Error Detection** | Early (compile-time) | Late (runtime) |
| **Use Case** | Regular C# code | COM interop, dynamic languages |

## Real-World Example: LINQ Query

```csharp
// PERFECT use of var - type is complex
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int Quantity { get; set; }
}

List<Product> products = new()
{
    new Product { Id = 1, Name = "Laptop", Price = 50000, Quantity = 10 },
    new Product { Id = 2, Name = "Mouse", Price = 500, Quantity = 100 },
    new Product { Id = 3, Name = "Keyboard", Price = 1500, Quantity = 50 }
};

// ✅ var is perfect here - type is inferred as IEnumerable<T>
var lowStockProducts = products
    .Where(p => p.Quantity < 20)
    .Select(p => new { p.Name, p.Price })
    .OrderBy(p => p.Price);

// Without var, you'd write:
IEnumerable<dynamic> lowStockProducts2 = products
    .Where(p => p.Quantity < 20)
    .Select(p => new { p.Name, p.Price })
    .OrderBy(p => p.Price);

// var knows exact type. dynamic doesn't.
// var gives IntelliSense help. dynamic doesn't.
```

## Real-World Example: JSON Parsing (dynamic needed)

```csharp
// When working with external JSON data, you might not know structure
public class ExternalDataProcessor
{
    // Using dynamic when JSON structure is unknown
    public void ProcessJsonData(string jsonString)
    {
        // Parsing JSON without knowing exact structure
        dynamic data = JsonConvert.DeserializeObject(jsonString);
        
        // Can access properties that might exist
        string name = data.user.name;           // Might work or fail
        string email = data.user.email;         // Depends on JSON
        int id = data.id;                       // Might not exist
        
        // This compiles fine, but crashes if properties don't exist
    }
    
    // Better approach - use typed classes
    public class UserData
    {
        public User user { get; set; }
        public int id { get; set; }
    }
    
    public class User
    {
        public string name { get; set; }
        public string email { get; set; }
    }
    
    public void ProcessJsonDataTyped(string jsonString)
    {
        // var with typed class - safe and checked at compile time
        var data = JsonConvert.DeserializeObject<UserData>(jsonString);
        
        string name = data.user.name;   // ✅ Compiler knows this exists
        string email = data.user.email; // ✅ Compiler knows this exists
    }
}
```

## Quick Decision Guide

```csharp
// Use VAR when:
var x = 5;                    // Obvious what type is
var person = new Person();    // Constructor tells type
var result = GetData();       // Return type is clear
var list = numbers.Where(...); // LINQ result type

// Use DYNAMIC when:
dynamic obj = GetFromComObject();    // COM interop
dynamic parsed = JsonParse(json);    // Unknown JSON structure
dynamic config = ReadConfigFile();   // Unknown config structure

// AVOID DYNAMIC for:
dynamic x = "hello";                 // Just use string!
dynamic age = 25;                    // Just use int!
dynamic salary = 50000.50m;         // Just use decimal!
```

---

# 5. Exception Handling

## What is Exception?

**Definition:**  
An exception is an error that occurs during program execution. Exception handling allows graceful error management instead of program crash.

```csharp
// ❌ Without exception handling - program crashes
int result = 10 / int.Parse("0");  // Crashes! DivideByZeroException

// ✅ With exception handling - graceful error
try
{
    int result = 10 / int.Parse("0");
}
catch (DivideByZeroException)
{
    Console.WriteLine("Cannot divide by zero");
}
// Program continues
```

## Why Study Exception Handling?

| Reason | Real Impact |
|--------|------------|
| **Program Stability** | App doesn't crash on errors |
| **User Experience** | Show friendly messages instead of crashes |
| **Debugging** | Know what went wrong |
| **Data Integrity** | Rollback transactions on error |
| **Logging** | Record errors for monitoring |

## Complete Exception Handling Structure

```csharp
try
{
    // Code that might throw exception
    int age = int.Parse("invalid");
}
catch (FormatException ex)
{
    // Handle specific exception
    Console.WriteLine($"Invalid format: {ex.Message}");
    Logger.Error("Format error", ex);
}
catch (OverflowException ex)
{
    // Handle another specific exception
    Console.WriteLine($"Number too large: {ex.Message}");
}
catch (Exception ex)
{
    // Catch ANY other exception
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
finally
{
    // Always runs - cleanup code
    Console.WriteLine("Operation complete");
    // Close connections, release resources, etc.
}
```

## Real-World Example: Linkwell Banking Transaction

```csharp
public class BankingService
{
    private IDatabase database;
    private ILogger logger;
    
    public bool TransferMoney(int fromAccountId, int toAccountId, decimal amount)
    {
        using (var transaction = database.BeginTransaction())
        {
            try
            {
                // Step 1: Validate amount
                if (amount <= 0)
                    throw new ArgumentException("Amount must be positive");
                
                // Step 2: Check balance
                var fromAccount = database.GetAccount(fromAccountId);
                if (fromAccount.Balance < amount)
                    throw new InsufficientFundsException(
                        $"Balance {fromAccount.Balance} is less than {amount}"
                    );
                
                // Step 3: Debit from sender
                fromAccount.Balance -= amount;
                database.UpdateAccount(fromAccount);
                
                // Step 4: Credit to receiver
                var toAccount = database.GetAccount(toAccountId);
                toAccount.Balance += amount;
                database.UpdateAccount(toAccount);
                
                // Step 5: Commit transaction
                transaction.Commit();
                logger.Info($"Transfer successful: {amount} from {fromAccountId} to {toAccountId}");
                return true;
            }
            catch (InsufficientFundsException ex)
            {
                // Specific error - inform user
                transaction.Rollback();
                logger.Warn($"Insufficient funds: {ex.Message}");
                Console.WriteLine("You have insufficient balance");
                return false;
            }
            catch (DatabaseException ex)
            {
                // Database issue - critical error
                transaction.Rollback();
                logger.Error("Database error during transfer", ex);
                Console.WriteLine("System error. Please try later");
                return false;
            }
            catch (Exception ex)
            {
                // Unexpected error
                transaction.Rollback();
                logger.Error("Unexpected error in transfer", ex);
                Console.WriteLine("An unexpected error occurred");
                return false;
            }
            finally
            {
                // Always cleanup
                Console.WriteLine("Transaction attempt completed");
            }
        }
    }
}

// Custom exceptions for domain-specific errors
public class InsufficientFundsException : Exception
{
    public InsufficientFundsException(string message) : base(message) { }
}
```

## Common Exception Types

```csharp
ArgumentException      // Invalid argument passed
ArgumentNullException  // Null argument passed
FormatException        // Invalid format
OverflowException      // Number too large
NullReferenceException // Accessing null object
IndexOutOfRangeException // Array index out of bounds
InvalidOperationException // Operation not valid in current state
NotImplementedException // Method not implemented yet
TimeoutException       // Operation timed out
IOException           // File/IO operation failed
```

---

# 6. Async/Await

## What is Async/Await?

**Definition:**  
`async/await` allows writing non-blocking code that can pause execution while waiting for long operations, freeing the thread to do other work.

```csharp
// ❌ SYNCHRONOUS (BLOCKING) - Thread waits and does nothing
public string FetchData()
{
    // Thread is BLOCKED here for 5 seconds
    var result = HTTPClient.GetString("https://api.example.com/data");
    return result;
}

// ✅ ASYNCHRONOUS (NON-BLOCKING) - Thread continues
public async Task<string> FetchDataAsync()
{
    // Thread is NOT blocked, it can do other work
    var result = await HTTPClient.GetStringAsync("https://api.example.com/data");
    return result;
}
```

## Why Study Async/Await?

| Reason | Impact |
|--------|--------|
| **UI Responsiveness** | UI doesn't freeze during operations |
| **Server Scalability** | Server can handle more concurrent requests |
| **Better UX** | User can continue interacting |
| **Performance** | Same hardware serves more users |
| **Modern requirement** | Industry standard for web apps |

## Real-World Example: Bank Statement Download

```csharp
public class BankStatementService
{
    private HttpClient httpClient;
    
    // ❌ BLOCKING - UI freezes for 3+ seconds
    public void DownloadStatement_Bad(string accountId)
    {
        Console.WriteLine("Starting download...");
        
        // Thread BLOCKS here - UI is frozen!
        string statement = httpClient.GetStringAsync(
            $"https://bank.example.com/statement/{accountId}"
        ).Result;  // .Result blocks the thread
        
        // 3+ seconds pass, user can't click anything
        
        SaveStatement(statement);
        Console.WriteLine("Download complete");
    }
    
    // ✅ NON-BLOCKING - UI stays responsive
    public async Task DownloadStatementAsync(string accountId)
    {
        Console.WriteLine("Starting download...");
        
        // Thread does NOT block here!
        string statement = await httpClient.GetStringAsync(
            $"https://bank.example.com/statement/{accountId}"
        );
        
        // During these 3+ seconds, thread can handle other requests
        // UI can respond to user clicks
        
        await SaveStatementAsync(statement);
        Console.WriteLine("Download complete");
    }
    
    private async Task SaveStatementAsync(string statement)
    {
        // Simulate file saving
        await Task.Delay(1000);
        File.WriteAllText("statement.txt", statement);
    }
}

// Usage in UI
public class BankingUI
{
    private BankStatementService service;
    
    // ❌ BAD - Button click is slow
    public void DownloadButton_Click()
    {
        service.DownloadStatement_Bad("ACC123");
        // UI is frozen for 3+ seconds!
    }
    
    // ✅ GOOD - Button click is instant
    public async void DownloadButton_Click()
    {
        await service.DownloadStatementAsync("ACC123");
        // UI stays responsive during download!
    }
}
```

## Async/Await Pattern

```csharp
// Simple async/await pattern
public async Task<int> GetDataAsync()
{
    // await pauses here, thread returns
    int result = await FetchFromServerAsync();
    return result;
}

private async Task<int> FetchFromServerAsync()
{
    // Simulate network delay
    await Task.Delay(3000);  // 3 seconds
    return 42;
}

// Usage
Task<int> task = GetDataAsync();  // Starts immediately
Console.WriteLine("Waiting...");
int result = await task;           // Waits for result
Console.WriteLine($"Result: {result}");
```

## Parallel Async Operations

```csharp
// Download multiple statements in parallel
public async Task DownloadAllStatementsAsync(List<string> accountIds)
{
    // ❌ SLOW - Download one by one
    foreach (var accountId in accountIds)
    {
        await DownloadStatementAsync(accountId);
        // Waits for each to complete
    }
    
    // ✅ FAST - Download all simultaneously
    var tasks = accountIds.Select(id => DownloadStatementAsync(id)).ToList();
    await Task.WhenAll(tasks);  // Wait for ALL to complete
    // All downloads happen in parallel!
}
```

## Common Mistakes

```csharp
// ❌ MISTAKE 1: Using .Result (can cause deadlock)
public void BadAsync()
{
    var result = FetchDataAsync().Result;  // ✗ DON'T DO THIS
}

// ✅ CORRECT: Use await
public async Task GoodAsync()
{
    var result = await FetchDataAsync();   // ✅ DO THIS
}

// ❌ MISTAKE 2: async void (hard to track completion)
public async void DownloadData()  // ✗ Avoid async void
{
    await FetchDataAsync();
}

// ✅ CORRECT: async Task
public async Task DownloadDataAsync()  // ✅ Use async Task
{
    await FetchDataAsync();
}
```

---

# 7. Delegates

## What is a Delegate?

**Definition:**  
A delegate is a type-safe function pointer. It's a template for method signature, allowing you to pass methods as parameters or store them as variables.

```csharp
// Define delegate - it's a METHOD TEMPLATE
public delegate void LogHandler(string message);

// Now you can:
1. Create methods matching this signature
2. Store them in LogHandler variable
3. Invoke them through the delegate
```

## Why Study Delegates?

| Reason | Use Case |
|--------|----------|
| **Callbacks** | Pass method as parameter |
| **Event handling** | React to user actions |
| **Flexibility** | Different implementations at runtime |
| **LINQ foundation** | LINQ uses delegates heavily |
| **Functional programming** | Treat methods as first-class objects |

## Real-World Example: Logging System

```csharp
// Define delegate - method signature template
public delegate void LoggerDelegate(string message);

public class Application
{
    // Store delegate references
    public LoggerDelegate Logger { get; set; }
    
    public void ProcessData(string data)
    {
        Logger?.Invoke("Processing started");  // Call delegate
        
        // Do work...
        
        Logger?.Invoke("Processing completed");
    }
}

// Different logger implementations
public class ConsoleLogger
{
    public void Log(string message)
    {
        Console.WriteLine($"[CONSOLE] {message}");
    }
}

public class FileLogger
{
    public void Log(string message)
    {
        File.AppendAllText("app.log", $"[FILE] {message}\n");
    }
}

public class DatabaseLogger
{
    public void Log(string message)
    {
        // Save to database
        Console.WriteLine($"[DATABASE] {message}");
    }
}

// Usage
var app = new Application();

// Can use ANY logger
app.Logger = new ConsoleLogger().Log;
app.ProcessData("data1");

app.Logger = new FileLogger().Log;
app.ProcessData("data2");

app.Logger = new DatabaseLogger().Log;
app.ProcessData("data3");

// Can combine multiple loggers (multicast delegate)
var console = new ConsoleLogger();
var file = new FileLogger();

app.Logger = console.Log + file.Log;  // Both execute!
app.ProcessData("data4");
// Output:
// [CONSOLE] Processing started
// [FILE] Processing started
// [CONSOLE] Processing completed
// [FILE] Processing completed
```

## Built-in Delegates

```csharp
// 1. Action - No return value
Action<string> greet = (name) => Console.WriteLine($"Hello {name}");
greet("John");

// 2. Func - Returns value
Func<int, int, int> add = (a, b) => a + b;
int sum = add(5, 3);  // 8

// 3. Predicate - Returns bool
Predicate<int> isEven = (x) => x % 2 == 0;
bool result = isEven(4);  // true

// Practical usage in LINQ
List<int> numbers = new() { 1, 2, 3, 4, 5, 6 };

// Using Func
var doubled = numbers.Select(x => x * 2);  // x * 2 is Func

// Using Predicate
var evens = numbers.Where(x => x % 2 == 0);  // x % 2 == 0 is Predicate
```

---

# 8. Practice Questions & Answers

## OOP Questions

### Q1: What is OOP? Give a Real Example.

**Answer:**  
OOP is a programming paradigm that organizes code into objects containing data (properties) and behavior (methods). It models real-world entities.

**Real Example - E-Commerce Product:**
```csharp
public class Product
{
    // Data - Properties
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int StockQuantity { get; set; }
    
    // Behavior - Methods
    public bool IsInStock() => StockQuantity > 0;
    public decimal ApplyDiscount(decimal discount) => Price - discount;
    public void ReduceStock(int quantity)
    {
        if (quantity <= StockQuantity)
            StockQuantity -= quantity;
    }
}

// Usage
var laptop = new Product 
{ 
    Id = 1, 
    Name = "Laptop", 
    Price = 50000, 
    StockQuantity = 10 
};

Console.WriteLine(laptop.IsInStock());  // true
laptop.ReduceStock(3);
Console.WriteLine(laptop.StockQuantity);  // 7
```

---

### Q2: What is the Difference Between Abstract Class and Interface?

**Answer:**

| Aspect | Abstract Class | Interface |
|--------|---|---|
| **Purpose** | Base for related classes | Contract/Behavior |
| **Methods** | Can have implementation | Only signature (before C# 8.0) |
| **State** | Can have fields | Cannot have state |
| **Inheritance** | Single only | Multiple |
| **When to use** | IS-A relationship | CAN-DO relationship |

**Example:**
```csharp
// Abstract class - for related entities
public abstract class Shape
{
    public string Color { get; set; }
    
    public abstract double GetArea();
    
    public void Print()
    {
        Console.WriteLine($"Shape color: {Color}");
    }
}

// Interface - for unrelated behaviors
public interface IDrawable
{
    void Draw();
}

public class Circle : Shape, IDrawable
{
    public double Radius { get; set; }
    
    public override double GetArea() => Math.PI * Radius * Radius;
    
    public void Draw()
    {
        Console.WriteLine("Drawing circle");
    }
}
```

---

### Q3: Explain the 4 OOP Pillars with Real Examples.

**Answer:**  
Already covered in detail above. See sections on Encapsulation, Inheritance, Polymorphism, Abstraction.

---

## Memory & Type Questions

### Q4: What is Boxing and Unboxing? Explain with Example.

**Answer:**

**Boxing:** Converting value type to reference type (object)  
**Unboxing:** Converting reference type back to value type

```csharp
// Boxing - value to object
int age = 25;
object boxed = age;  // Boxes int into object
Console.WriteLine(boxed);  // 25

// Unboxing - object back to value
int unboxed = (int)boxed;
Console.WriteLine(unboxed);  // 25

// ❌ WRONG UNBOXING - InvalidCastException
string wrongUnbox = (string)boxed;  // ✗ Crash!

// Performance impact
for (int i = 0; i < 1000000; i++)
{
    object x = i;      // Boxing - slow
    int y = (int)x;    // Unboxing - slow
}
// Avoid boxing/unboxing in loops!
```

---

### Q5: Difference Between Value and Reference Types?

**Answer:**

**Value Types:**
- Stored on stack
- Passing creates a copy
- Examples: int, double, bool, struct

**Reference Types:**
- Stored on heap
- Passing shares reference
- Examples: class, string, array, list

```csharp
// Value type
int x = 5;
int y = x;
y = 10;
Console.WriteLine(x);  // 5 (unchanged)

// Reference type
class Person { public string Name { get; set; } }
Person p1 = new Person { Name = "John" };
Person p2 = p1;
p2.Name = "Jane";
Console.WriteLine(p1.Name);  // Jane (changed!)
```

---

## String & Performance Questions

### Q6: Why is StringBuilder Faster Than String Concatenation?

**Answer:**

String creates new object for each concatenation (O(n²) complexity).  
StringBuilder reuses buffer (O(n) complexity).

**Performance:**
```
String concatenation (10,000 times): ~2500ms
StringBuilder (10,000 times): ~5ms
Speed difference: 500x FASTER!
```

**Example:**
```csharp
// ❌ SLOW
string result = "";
for (int i = 0; i < 10000; i++)
{
    result += i.ToString();  // Creates new string each time
}

// ✅ FAST
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++)
{
    sb.Append(i.ToString());  // Reuses buffer
}
string result = sb.ToString();
```

---

## async/await Questions

### Q7: What is Async/Await? How Does It Help?

**Answer:**

`async/await` allows writing non-blocking code. The thread doesn't wait idly; it can do other work while a long operation completes.

**Synchronous (Blocking):**
```csharp
public void DownloadFile()
{
    var data = webClient.Download("https://...");  // Thread waits here
    ProcessData(data);  // Blocked until download completes
}
// UI freezes for 3+ seconds
```

**Asynchronous (Non-Blocking):**
```csharp
public async Task DownloadFileAsync()
{
    var data = await webClient.DownloadAsync("https://...");  // Thread returns
    ProcessData(data);
}
// UI stays responsive during download
```

**Benefits:**
- UI remains responsive
- Server handles more concurrent requests
- Better resource utilization
- Modern standard

---

### Q8: What's Wrong with Using .Result in Async Code?

**Answer:**

`.Result` blocks the thread, defeating the purpose of async. Can cause deadlock.

```csharp
// ❌ WRONG
public void BadCode()
{
    string result = FetchDataAsync().Result;  // Blocks thread
}

// ✅ CORRECT
public async Task GoodCode()
{
    string result = await FetchDataAsync();  // Doesn't block thread
}
```

---

## Delegates Questions

### Q9: What are Delegates and How Do You Use Them?

**Answer:**

Delegate is a type-safe function pointer - a template for method signature.

```csharp
// Define delegate
public delegate void NotifyHandler(string message);

// Implement methods
void EmailNotify(string msg) => Console.WriteLine($"Email: {msg}");
void SMSNotify(string msg) => Console.WriteLine($"SMS: {msg}");

// Use delegates
NotifyHandler notifier = EmailNotify;
notifier("Order confirmed");  // Calls EmailNotify

notifier = SMSNotify;
notifier("Order confirmed");  // Calls SMSNotify

// Multicast - call multiple methods
notifier = EmailNotify + SMSNotify;
notifier("Order confirmed");  // Calls both
```

---

### Q10: Difference Between delegate, Action, and Func?

**Answer:**

```csharp
// Custom delegate
public delegate void MyDelegate(string msg);

// Action - no return
Action<string> log = (msg) => Console.WriteLine(msg);

// Func - returns value
Func<int, int, int> add = (a, b) => a + b;

// Predicate - returns bool
Predicate<int> isPositive = (x) => x > 0;

// Action & Func are built-in; custom delegates allow more control
```

---

## Company-Specific Questions

### Q11: How Would You Design a Payment System for Linkwell?

**Answer:**

See detailed example in OOP Polymorphism section above. Key points:
- Use abstract base class for PaymentMethod
- Implement different payment types (Card, UPI, NetBanking)
- Calculate fees polymorphically
- Handle exceptions for each method
- Use async for payment processing
- Log all transactions

---

### Q12: How Do You Ensure Code Quality in Large Systems?

**Answer:**

1. **SOLID Principles** - Maintainable code
2. **Exception Handling** - Graceful error management
3. **Async/Await** - Non-blocking operations
4. **Unit Testing** - Verify behavior
5. **Code Reviews** - Peer feedback
6. **Logging** - Monitor issues
7. **Documentation** - Help others understand

---

### Q13: What's Your Approach to Debugging Production Issues?

**Answer:**

1. **Logs** - Check application logs
2. **Exception Stack Trace** - Know exactly where error occurred
3. **Reproduce Locally** - Try to replicate issue
4. **Isolate Problem** - Binary search through code
5. **Fix** - Make minimal change
6. **Test** - Verify fix works
7. **Monitor** - Ensure no regression

---

## Summary Checklist

- ✅ OOP - All 4 pillars with real examples
- ✅ Memory management - Value vs reference types
- ✅ String performance - When to use StringBuilder
- ✅ Type safety - var vs dynamic
- ✅ Error handling - try-catch-finally patterns
- ✅ Async patterns - Non-blocking operations
- ✅ Delegates - Method passing and events
- ✅ Real examples - Banking, POS, Payment systems
- ✅ Practice questions - 13 core interview questions

---

## Final Interview Tips

**DO:**
- ✅ Explain concepts clearly with examples
- ✅ Ask clarifying questions
- ✅ Discuss trade-offs
- ✅ Show knowledge of SOLID principles
- ✅ Mention error handling
- ✅ Talk about performance
- ✅ Show enthusiasm

**DON'T:**
- ❌ Memorize without understanding
- ❌ Ignore edge cases
- ❌ Dismiss questions you don't know
- ❌ Over-complicate answers
- ❌ Forget about testing
- ❌ Ignore code quality
- ❌ Rush through explanations

---

## Company Focus - Linkwell

**Remember:**
- They build products deployed in 50+ countries
- Reliability and quality are critical
- Code must maintain for 30+ years
- Work with banking, telecom, government systems
- Performance matters (millions of devices)
- Security is important

**Focus on:**
- Clean, maintainable code
- Proper error handling
- Performance optimization
- Real-world scenarios
- Code quality
- Scalability

---

# 🎯 YOU'RE READY!

This document contains everything you need:
- ✅ What: Concept definitions
- ✅ Why: Reasons to study
- ✅ Real examples: Practical scenarios
- ✅ Code: Working implementations
- ✅ Questions: Interview practice
- ✅ Tips: Success strategies

**Go ace that Linkwell interview! 🚀**

---

**Document Version:** 1.0  
**Last Updated:** April 2, 2026  
**Created for:** Linkwell Telesystems Interview Preparation  
**Status:** Complete & Comprehensive ✅
