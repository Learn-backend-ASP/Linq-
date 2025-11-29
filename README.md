# LINQ Summary

> **Each section contains**  with:
>
> * **Description**
> * **Time Complexity**
> * **Method Syntax Example**
> * **Query Syntax Example**
> * **Output Comment**

---

# 0️⃣ Sample Data

```csharp
List<int> numbers = new List<int> { 5, 12, 4, 8, 1, 12, 18, 5 };
List<string> names = new List<string> { "Ahmed", "Mohamed", "Salama", "Nour", "Ahmed" };

public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
    public decimal Salary { get; set; }
}

List<Employee> employees = new List<Employee>
{
    new Employee { Id = 101, Name = "Ali", Department = "IT", Salary = 60000 },
    new Employee { Id = 102, Name = "Omar", Department = "HR", Salary = 45000 },
    new Employee { Id = 103, Name = "Noha", Department = "IT", Salary = 60000 },
    new Employee { Id = 104, Name = "Sami", Department = "Finance", Salary = 75000 }
};
```

---

# 1️⃣ Filtering & Projection

## **Where()**

* **Description:** Filters a sequence based on a condition.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = employees.Where(e => e.Salary >= 60000);
// Output: Ali, Noha, Sami
```

**Query Syntax:**

```csharp
var result = from e in employees
             where e.Salary >= 60000
             select e;
```

---

## **Select()**

* **Description:** Projects each element into a new form.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = employees.Select(e => e.Name);
// Output: Ali, Omar, Noha, Sami
```

**Query Syntax:**

```csharp
var result = from e in employees
             select e.Name;
```

---

## **SelectMany()**

* **Description:** Flattens nested collections.
* **Time Complexity:** O(n + inner)

**Method Syntax:**

```csharp
var result = departments.SelectMany(d => d.Employees);
// Output: all employees flattened
```

**Query Syntax:**

```csharp
var result = from d in departments
             from emp in d.Employees
             select emp;
```

---

# 2️⃣ Ordering

## **OrderBy()**

* **Description:** Ascending sort.
* **Time Complexity:** O(n log n)

**Method Syntax:**

```csharp
var result = numbers.OrderBy(n => n);
// Output: 1,4,5,5,8,12,12,18
```

**Query Syntax:**

```csharp
var result = from n in numbers
             orderby n ascending
             select n;
```

---

## **OrderByDescending()**

* **Description:** Descending sort.
* **Time Complexity:** O(n log n)

**Method Syntax:**

```csharp
var result = numbers.OrderByDescending(n => n);
// Output: 18,12,12,8,5,5,4,1
```

**Query Syntax:**

```csharp
var result = from n in numbers
             orderby n descending
             select n;
```

---

## **ThenBy() / ThenByDescending()**

* **Description:** Secondary sort.
* **Time Complexity:** O(n log n)

**Method Syntax:**

```csharp
var result = employees
    .OrderBy(e => e.Salary)
    .ThenBy(e => e.Name);
```

**Query Syntax:**

```csharp
var result = from e in employees
             orderby e.Salary, e.Name
             select e;
```

---

# 3️⃣ Partitioning

## **Take()**

* **Description:** Takes first N elements.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = numbers.Take(3);
// Output: 5,12,4
```

**Query Syntax:**

```csharp
var result = (from n in numbers select n).Take(3);
```

---

## **Skip()**

* **Description:** Skips first N elements.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = numbers.Skip(2);
// Output: 4,8,1,12,18,5
```

**Query Syntax:**

```csharp
var result = (from n in numbers select n).Skip(2);
```

---

## **TakeWhile() / SkipWhile()**

* **Description:** Conditional partitioning.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = numbers.TakeWhile(n => n < 10);
// Output: 5
```

```csharp
var result = numbers.SkipWhile(n => n < 10);
// Output: 12,4,8,1,12,18,5
```

Query syntax does **not** support TakeWhile/SkipWhile.

---

# 4️⃣ Grouping & Joining

## **GroupBy()**

* **Description:** Groups elements by a key.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = employees.GroupBy(e => e.Department);
```

**Query Syntax:**

```csharp
var result = from e in employees
             group e by e.Department;
```

---

## **Join()**

* **Description:** Inner join.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = employees.Join(departments,
    e => e.Department,
    d => d.Name,
    (e,d) => new { e.Name, d.Name });
```

**Query Syntax:**

```csharp
var result = from e in employees
             join d in departments
             on e.Department equals d.Name
             select new { e.Name, d.Name };
```

---

## **GroupJoin()**

* **Description:** Left join (grouped join).
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = departments.GroupJoin(employees,
    d => d.Name,
    e => e.Department,
    (d, emps) => new { d.Name, emps });
```

**Query Syntax:**

```csharp
var result = from d in departments
             join e in employees
             on d.Name equals e.Department into grp
             select new { d.Name, grp };
```

---

# 5️⃣ Aggregation

## **Count()**

* **Description:** Counts matching elements.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var result = numbers.Count(n => n > 10);
// Output: 3
```

**Query Syntax:**

```csharp
var result = (from n in numbers where n > 10 select n).Count();
```

---

## **Sum(), Min(), Max(), Average()**

* **Description:** Arithmetic aggregation.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var sum = numbers.Sum();
var max = numbers.Max();
var avg = numbers.Average();
```

**Query Syntax:**

```csharp
var sum = (from n in numbers select n).Sum();
```

---

# 6️⃣ Elements & Quantifiers

## **First(), FirstOrDefault()**

* **Description:** Gets first element.
* **Time Complexity:** O(1)

**Method Syntax:**

```csharp
var result = numbers.First(n => n > 10);
```

**Query Syntax:**

```csharp
var result = (from n in numbers where n > 10 select n).First();
```

---

## **Any(), All(), Contains()**

* **Description:** Boolean checks.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
bool result = numbers.Any(n => n > 20);
bool ok = numbers.All(n => n > 0);
bool exist = numbers.Contains(12);
```

**Query Syntax:**

```csharp
bool result = (from n in numbers where n > 20 select n).Any();
```

---

# 7️⃣ Set Operations

## **Distinct() / DistinctBy()**

* **Description:** Removes duplicates.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var unique = numbers.Distinct();
var uniqueDept = employees.DistinctBy(e => e.Department);
```

**Query Syntax:**

```csharp
var unique = (from n in numbers select n).Distinct();
```

---

## **Union() / UnionBy()**

* **Description:** Merges sequences & removes duplicates.
* **Time Complexity:** O(n+m)

---

## **Intersect() / IntersectBy()**

* **Description:** Common elements.
* **Time Complexity:** O(n+m)

---

## **Except() / ExceptBy()**

* **Description:** Items in first list only.
* **Time Complexity:** O(n+m)

---

# 8️⃣ Conversion

## **ToList(), ToDictionary(), ToHashSet()**

* **Description:** Converts IEnumerable.
* **Time Complexity:** O(n)

**Method Syntax:**

```csharp
var list = numbers.ToList();
var dict = employees.ToDictionary(e => e.Id);
var set = numbers.ToHashSet();
```

---

# 9️⃣ Misc

## **Reverse()**

* **Description:** Reverses sequence.
* **Time Complexity:** O(n)

```csharp
var reversed = numbers.Reverse();
```

---

## **Zip()**

* **Description:** Combines pairs.
* **Time Complexity:** O(min(n,m))

```csharp
var result = numbers.Zip(names, (a,b) => $"{a}:{b}");
```

---

## **Append() / Prepend()**

* **Description:** Adds element beginning/end.
* **Time Complexity:** O(1) lazy

```csharp
var r1 = numbers.Append(99);
var r2 = numbers.Prepend(0);
```

---

## **Chunk()**

* **Description:** Splits into chunks.
* **Time Complexity:** O(n)

```csharp
var chunks = numbers.Chunk(3);
```

