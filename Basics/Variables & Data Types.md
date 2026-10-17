# Java Variables & Data Types

## Variables

A variable is a named reference/storage used to hold a value.

### Types
- **Local:** declared inside method/block; no default value; must be initialized before use.
- **Instance:** non-static field; one copy per object; gets default value.
- **Static:** `static` field; one shared copy per class.

```java
class Employee {
    static String company = "ABC"; // static
    int id;                        // instance

    void work() {
        int hours = 8;             // local
    }
}
```

---

## Data Types

Java is **statically typed**.

### Primitive

`byte` → `short` → `int` → `long`  
`float` → `double`  
`char`  
`boolean`

| Type | Bits | Default | Notes |
|---|---:|---|---|
| `byte` | 8 | `0` | -128 to 127 |
| `short` | 16 | `0` | |
| `int` | 32 | `0` | Default integer literal |
| `long` | 64 | `0L` | `L` suffix |
| `float` | 32 | `0.0f` | `f` suffix |
| `double` | 64 | `0.0d` | Default decimal literal |
| `char` | 16 | `'\u0000'` | UTF-16 code unit |
| `boolean` | — | `false` | `true` / `false` |

### Reference

Hold references to objects.

`String`, arrays, classes, interfaces, enums, etc.

```java
String s = "Java";
int[] arr = {1, 2, 3};
```

---

## Identifiers

Names given to classes, methods, variables, etc.

**Conventions:**
- Class/interface → `PascalCase`
- Method/variable → `camelCase`
- Constant → `UPPER_SNAKE_CASE`

---

## Literals

Fixed values represented directly in source code.

```java
10          // integer
10L         // long
10.5        // double
10.5f       // float
'A'         // char
"Java"      // String
true        // boolean
null        // null reference
```

Integer bases:

```java
0b1010      // binary
012         // octal
0xFF        // hexadecimal
```

> `true`, `false`, and `null` are literals, not keywords.

---

## Keywords

Reserved words with predefined meaning; cannot be used as identifiers.

Examples:

`class`, `public`, `private`, `static`, `final`, `new`, `this`, `super`, `extends`, `implements`, `if`, `else`, `for`, `while`, `return`, `try`, `catch`, `throw`, `throws`

---

## `final`

Prevents reassignment of a variable.

```java
final int MAX = 100;
```

For references, `final` prevents changing the **reference**, not mutating the object.

```java
final List<Integer> list = new ArrayList<>();
list.add(10);       // ✓
list = new ArrayList<>(); // ✗
```

---

## Type Casting

### Widening
Implicit conversion to a compatible larger type.

```java
int x = 10;
long y = x;
```

`byte → short → int → long → float → double`

### Narrowing
Explicit conversion; possible data loss.

```java
double x = 10.5;
int y = (int) x; // 10
```

---

## `var`

Local variable type inference; type is determined at **compile time**.

```java
var count = 10;       // int
var name = "Java";    // String
```

- Local variables only
- Must be initialized
- Not dynamically typed
- Cannot be `null` without a target type
