# LINQ Aggregate Operators

Aggregate operators in LINQ perform calculations on a sequence and return a single value. They use immediate execution, meaning the query is evaluated as soon as the operator is called.

## Common Aggregate Operators

1. Count
2. Max
3. Min
4. Sum 
5. Average 

### Count Operator

The Count operator returns the number of elements in a sequence.

#### Basic Usage

```csharp
var result = ProductList.Count();  // LINQ Operator 
var result = ProductList.Count;    // List Property
```

#### Count with Predicate

```csharp
var result = ProductList.Count(p => p.UnitsInStock == 0);
Console.WriteLine(result);
```

### Max Operator

The Max operator returns the maximum value in a sequence.

#### Basic Usage

```csharp
var result = ProductList.Max();
Console.WriteLine(result);
```

Note: This usage requires the Product class to implement IComparable<Product>.

#### Max with Selector

```csharp
var result = ProductList.Max(p => p.ProductName.Length);
```

### Min Operator

The Min operator returns the minimum value in a sequence.

#### Basic Usage

```csharp
var result = ProductList.Min();
```

Note: This usage requires the Product class to implement IComparable<Product>.

#### Min with Selector

```csharp
var result = ProductList.Min(p => p.ProductName.Length);
```

### Implementing IComparable<Product>

For the basic usage of Max and Min, the Product class must implement IComparable<Product>:

```csharp
public class Product : IComparable<Product>
{
    // ... other properties ...

    public int CompareTo(Product? other)
    {
        return this.UnitPrice.CompareTo(other?.UnitPrice);
    }
}
```

### Combining Aggregate Operators

You can combine aggregate operators with other LINQ methods for more complex queries:

```csharp
var minLength = ProductList.Min(p => p.ProductName.Length);

var result = (from p in ProductList
              where p.ProductName.Length == minLength
              select p).FirstOrDefault();

Console.WriteLine(result);
```

## Comparison: Basic Usage vs. Selector Usage

| Aspect | Basic Usage (e.g., Max()) | Selector Usage (e.g., Max(p => p.Property)) |
|--------|---------------------------|---------------------------------------------|
| IComparable Requirement | Yes | No |
| Flexibility | Less (compares whole objects) | More (can compare specific properties) |
| Ease of Use | Simple, but requires class modification | More verbose, but doesn't require class modification |

## Best Practices

1. Implement IComparable<T> in your class if you frequently need to compare whole objects.
2. Use selector overloads (e.g., Max(p => p.Property)) when you need to compare based on specific properties or when you can't modify the class.
3. Combine aggregate operators with other LINQ methods for more complex queries.

```mermaid
graph TD
    A[Aggregate Operation] --> B{Need to compare whole objects?}
    B -->|Yes| C{Can modify class?}
    C -->|Yes| D[Implement IComparable]
    C -->|No| E[Use selector overload]
    B -->|No| E
    D --> F[Use basic overload e.g. Max()]
    E --> G[Use selector overload e.g. Max(p => p.Property)]
```

This diagram illustrates the decision process for choosing between basic and selector overloads of aggregate operators.
