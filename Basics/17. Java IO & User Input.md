# Java I/O & User Input

## Standard I/O Streams
- `System.in`: Standard input, typically keyboard input; type `InputStream`.
- `System.out`: Standard output, typically console output; type `PrintStream`.
- `System.err`: Standard error output, typically for errors and diagnostics; type `PrintStream`.

```java
System.out.println("Success");
System.err.println("Something went wrong");
```

`System.err` is a separate output stream, but it is not guaranteed to be displayed separately or immediately in every environment.

## Java I/O Stream Hierarchy

Java I/O commonly processes data through streams.

### Byte Streams
- `InputStream`: Base class for reading bytes.
  - `FileInputStream`
  - `ByteArrayInputStream`
  - `BufferedInputStream`
  - `DataInputStream`
- `OutputStream`: Base class for writing bytes.
  - `FileOutputStream`
  - `ByteArrayOutputStream`
  - `BufferedOutputStream`
  - `DataOutputStream`
- Suitable for binary data such as images and files.

### Character Streams
- `Reader`: Base class for reading characters.
- `Writer`: Base class for writing characters.
- Designed for text and character encoding.

`InputStreamReader` bridges byte input to character input by decoding bytes into characters using a charset.

## Reading with `System.in.read()`
- Reads one byte at a time and returns an `int`.
- Returns `-1` when the stream reaches end-of-file.
- May throw `IOException`.
- The returned `int` allows `-1` to represent EOF separately from byte values.

```java
int value = System.in.read();

if (value != -1) {
    System.out.println((char) value);
}
```

This reads one byte, not an entire line or a complete number.

## Using `BufferedReader`
Wraps a character reader and buffers input for efficient reading.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(
            new InputStreamReader(System.in)
        );

        String line = br.readLine();
        System.out.println(line);
    }
}
```

- `readLine()` reads a complete line without the line terminator.
- Returns `null` at EOF.
- `BufferedReader` reads text; it does not automatically parse numeric values.

Parse explicitly when needed:

```java
int age = Integer.parseInt(br.readLine());
double price = Double.parseDouble(br.readLine());
```

## Using `Scanner`
Provides convenient tokenization and parsing of primitive types and strings.

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);

int age = sc.nextInt();
double salary = sc.nextDouble();
sc.nextLine(); // Consume the remaining line ending
String name = sc.nextLine();
```

Common methods:
- `nextInt()`, `nextDouble()`: Read the next token as a number.
- `next()`: Read the next token.
- `nextLine()`: Read the remainder of the current line.

### Common Gotcha: `nextInt()` and `nextLine()`
After `nextInt()`, the line terminator may remain in the input. Calling `nextLine()` immediately can consume the remainder of that line rather than the next full line. Consume the remainder first when appropriate.

Invalid input can cause `InputMismatchException`.

## Scanner vs. BufferedReader

| `Scanner` | `BufferedReader` |
|---|---|
| Convenient token and primitive parsing | Reads text as lines or characters |
| Built-in tokenization and parsing | Parsing is explicit |
| Generally slower for heavy input | Generally faster for large input |
| Parsing errors can raise `InputMismatchException` | Parsing errors depend on the parser used |

For competitive programming or very large input, `BufferedReader` with explicit parsing is often preferred. For small programs and interactive input, `Scanner` is usually simpler.

## Key Gotchas
- Byte streams and character streams are not interchangeable concepts.
- Use an explicit charset when decoding text where portability matters, e.g. `new InputStreamReader(System.in, StandardCharsets.UTF_8)`.
- `read()` returning an `int` does not mean it always returns a meaningful numeric value from the user's input.
- Closing a `Scanner` or reader wrapping `System.in` also closes the underlying standard input stream; avoid closing it if other parts of the program still need it.
- Handle checked `IOException` where required.
