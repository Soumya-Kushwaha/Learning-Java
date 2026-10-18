# Core Architecture Components
- JVM (Java Virtual Machine)
  - Executes Java bytecode (.class).
  - Provides platform independence: same bytecode can run on any OS with a compatible JVM.
  - Uses Interpreter + JIT Compiler for execution and optimization.
  - Manages memory, garbage collection, and runtime security.
- JRE (Java Runtime Environment)
  JVM + core Java libraries required to run Java applications.
  Used for running Java programs, not developing them.
- JDK (Java Development Kit)
  JRE + development tools.
  Includes tools such as javac (compiler), debugger, documentation tools, etc.
  Used to develop, compile, debug, and run Java applications.
  JDK → JRE → JVM

# Execution Lifecycle
- Source Code
  - .java file containing Java source code.
- Compilation
  - javac compiles source code into platform-independent bytecode.
  - Output: .class file.
- Runtime Execution
  - JVM loads and verifies the bytecode.
  - Interpreter executes bytecode.
  - JIT (Just-In-Time) Compiler identifies frequently executed code and compiles it into native machine code for better performance.

# Flow:
.java → javac → .class (Bytecode) → JVM → Interpreter / JIT → Machine Code

# Java Editions
- Java SE (Standard Edition)
  - Core Java platform.
  - Provides fundamental APIs and features such as OOP, Collections, I/O, Multithreading, JDBC, Networking, etc.
  - Used for general-purpose Java applications.
- Java EE (Enterprise Edition)
  - Built on top of Java SE for enterprise/server-side applications.
  - Provides specifications/APIs for things like web applications, REST services, transactions, security, persistence, etc.
  - Now known as Jakarta EE.
- Java ME (Micro Edition)
  - Designed for resource-constrained devices such as embedded systems and older mobile/IoT devices.
  - Provides a smaller Java runtime and API set compared to Java SE.

# Quick distinction

| Edition | Purpose |
|---|---|
| **Java SE** | Core/general-purpose Java |
| **Java EE / Jakarta EE** | Enterprise & server-side applications |
| **Java ME** | Embedded/resource-constrained devices |
