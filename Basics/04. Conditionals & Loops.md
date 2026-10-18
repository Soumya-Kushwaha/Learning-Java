# Java Decision-Making

## If-Else
- `if`, `if-else`, nested `if`, and `else-if` ladder for conditional execution.
- Conditions must evaluate to `boolean`.

## Switch
- Alternative to `if-else` chains when comparing one expression against multiple cases.
- Traditional `switch` supports `byte`, `short`, `char`, `int`, their wrappers, `String`, and enums.
- `break` prevents fall-through in traditional switch statements.
- Without `break`, execution continues into subsequent cases until a `break` or the end of the switch.
- Supports nested switches.

```java
switch (status) {
    case 1:
        System.out.println("Active");
        break;
    default:
        System.out.println("Unknown");
}
```

### Switch Expressions (Java 14+)
- Use `->` syntax; no fall-through.
- Can return a value.

```java
String result = switch (status) {
    case 1 -> "Active";
    case 2 -> "Inactive";
    default -> "Unknown";
};
```

### Internal Implementation
- Traditional integer switches may compile to `tableswitch` (dense keys) or `lookupswitch` (sparse keys).
- Actual bytecode selection depends on the compiler; `switch` is not inherently faster than `if-else` in every case.

# Java Loops

## Loop Types
- **`while`:** Checks condition before each iteration; may execute zero times.
- **`do-while`:** Checks condition after execution; executes at least once.
- **`for`:** Combines initialization, condition, and update in one construct.
- **Enhanced `for`:** Iterates over arrays and `Iterable` collections.

## Nested Loops
- A loop inside another loop.
- Commonly used for multidimensional data and pattern traversal.
- Inner loop completes its iterations for each outer-loop iteration.

## Jump Statements
- **`break`:** Exits the nearest enclosing loop or switch.
- **`continue`:** Skips the rest of the current loop iteration.
- **Labeled `break` / `continue`:** Targets a specific enclosing labeled statement; useful for nested loops.

```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) continue outer;
    }
}
```

