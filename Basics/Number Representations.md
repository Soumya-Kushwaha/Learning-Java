# Number Representation in Java

## 1. Negative Integer Representation — 2's Complement

- Java uses **2's complement** to represent signed integer values.
- Calculation: invert all bits (1's complement), then add `1`.
- The MSB indicates the sign: `0` for non-negative, `1` for negative values.
- Provides a single representation of zero.
- For an `n`-bit signed integer, range: \(-2^{n-1}\) to \(2^{n-1}-1\).

Example (8-bit `5` and `-5`):

```text
 5  = 00000101
Invert: 11111010
Add 1: 11111011  = -5
```

## 2. Floating-Point Representation — IEEE 754

Java `float` and `double` use IEEE 754 binary floating-point representation.

| Component | `float` (32-bit) | `double` (64-bit) |
|---|---:|---:|
| Sign | 1 bit | 1 bit |
| Exponent | 8 bits | 11 bits |
| Fraction | 23 bits | 52 bits |
| Exponent bias | 127 | 1023 |

- **Sign:** Positive or negative.
- **Exponent:** Represents the scale of the number using a biased exponent.
- **Fraction (significand):** Represents the significant binary digits.
- **Normalization:** Normalized values are represented in binary scientific notation: \(1.xxxx_2 \times 2^e\).
- **Bias:** Stored exponent = actual exponent + bias, for normalized values.

## 3. Floating-Point Precision

- Many decimal fractions, such as `0.1` and `0.7`, have no finite binary representation.
- `float` and `double` store approximations, which can cause rounding errors.
- `double` generally provides more precision than `float`, but neither guarantees exact decimal arithmetic.

```java
System.out.println(0.1 + 0.2); // 0.30000000000000004
```

### `BigDecimal`

Use `BigDecimal` when exact decimal arithmetic is required, such as financial calculations.

```java
BigDecimal a = new BigDecimal("0.1");
BigDecimal b = new BigDecimal("0.2");

System.out.println(a.add(b)); // 0.3
```

**Note:** Prefer `BigDecimal` constructed from strings; `new BigDecimal(0.1)` preserves the binary floating-point approximation of `0.1`.
