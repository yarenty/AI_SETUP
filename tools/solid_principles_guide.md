# SOLID Principles Implementation Guide

> A practical guide for implementing SOLID principles in AI-assisted development across different programming languages and frameworks.

## Table of Contents
1. [Overview](#overview)
2. [Single Responsibility Principle (SRP)](#single-responsibility-principle-srp)
3. [Open/Closed Principle (OCP)](#openclosed-principle-ocp)
4. [Liskov Substitution Principle (LSP)](#liskov-substitution-principle-lsp)
5. [Interface Segregation Principle (ISP)](#interface-segregation-principle-isp)
6. [Dependency Inversion Principle (DIP)](#dependency-inversion-principle-dip)
7. [AI Development Guidelines](#ai-development-guidelines)
8. [Language-Specific Examples](#language-specific-examples)

## Overview

SOLID principles are fundamental guidelines for writing maintainable, scalable software. This guide provides practical implementation strategies and examples for AI-assisted development.

### Quick Reference
| Principle | Key Question | AI Guideline |
|-----------|--------------|--------------|
| **SRP** | Does this class have one reason to change? | Before adding functionality, ask "Does this belong here?" |
| **OCP** | Can I extend behavior without modifying existing code? | Extend through new classes/modules, not modifications |
| **LSP** | Can I substitute this subclass for its parent? | Ensure behavioral compatibility in inheritance |
| **ISP** | Are there unused methods in this interface? | Create focused, cohesive interfaces |
| **DIP** | Do high-level modules depend on low-level details? | Inject dependencies, depend on abstractions |

## Single Responsibility Principle (SRP)

> A class should have only one reason to change.

### Implementation Strategy
- **Identify responsibilities**: Each class should handle one specific concern
- **Separate concerns**: Business logic, data access, and presentation should be distinct
- **Small, focused classes**: Better than large, monolithic ones

### Good Example (TypeScript)
```typescript
// ❌ Violates SRP - multiple responsibilities
class UserManager {
  saveUser(user: User) { /* database logic */ }
  sendEmail(user: User) { /* email logic */ }
  validateUser(user: User) { /* validation logic */ }
}

// ✅ Follows SRP - single responsibilities
class UserRepository {
  save(user: User) { /* only database logic */ }
}

class EmailService {
  sendWelcomeEmail(user: User) { /* only email logic */ }
}

class UserValidator {
  validate(user: User): ValidationResult { /* only validation logic */ }
}
```

### AI Development Guidelines
- **Before adding a method**: Ask "Does this method belong in this class?"
- **When extending functionality**: Consider if a new class is needed
- **Code review prompt**: "What would cause this class to change?"

## Open/Closed Principle (OCP)

> Software entities should be open for extension, closed for modification.

### Implementation Strategy
- **Use abstractions**: Interfaces and abstract classes enable extension
- **Strategy pattern**: For varying algorithms
- **Dependency injection**: To swap implementations
- **Plugin architecture**: For extensible systems

### Good Example (Python)
```python
# ❌ Violates OCP - requires modification for new shapes
class AreaCalculator:
    def calculate_area(self, shapes):
        total_area = 0
        for shape in shapes:
            if shape.type == "circle":
                total_area += 3.14 * shape.radius ** 2
            elif shape.type == "rectangle":
                total_area += shape.width * shape.height
            # Adding new shape requires modifying this method
        return total_area

# ✅ Follows OCP - extensible without modification
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> float:
        pass

class Circle(Shape):
    def __init__(self, radius: float):
        self.radius = radius
    
    def area(self) -> float:
        return 3.14 * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width: float, height: float):
        self.width = width
        self.height = height
    
    def area(self) -> float:
        return self.width * self.height

class AreaCalculator:
    def calculate_area(self, shapes: list[Shape]) -> float:
        return sum(shape.area() for shape in shapes)

# New shapes can be added without modifying existing code
class Triangle(Shape):
    def __init__(self, base: float, height: float):
        self.base = base
        self.height = height
    
    def area(self) -> float:
        return 0.5 * self.base * self.height
```

### AI Development Guidelines
- **When adding new behavior**: Create new classes/modules instead of modifying existing ones
- **Design for extension**: Use interfaces and abstract base classes
- **Avoid switch statements**: Use polymorphism instead

## Liskov Substitution Principle (LSP)

> Objects of a superclass should be replaceable with objects of its subclasses without breaking functionality.

### Implementation Strategy
- **Behavioral compatibility**: Subclasses must honor the contract of their parent
- **Preconditions cannot be strengthened**: Subclasses can't be more restrictive
- **Postconditions cannot be weakened**: Subclasses must provide at least the same guarantees

### Good Example (Java)
```java
// ❌ Violates LSP - Square changes Rectangle behavior
class Rectangle {
    protected double width;
    protected double height;
    
    public void setWidth(double width) { this.width = width; }
    public void setHeight(double height) { this.height = height; }
    public double getArea() { return width * height; }
}

class Square extends Rectangle {
    @Override
    public void setWidth(double width) {
        this.width = width;
        this.height = width; // Breaks LSP - unexpected behavior
    }
}

// ✅ Follows LSP - proper abstraction
abstract class Shape {
    abstract double getArea();
}

class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    public double getArea() { return width * height; }
}

class Square extends Shape {
    private double side;
    
    public Square(double side) {
        this.side = side;
    }
    
    public double getArea() { return side * side; }
}
```

### AI Development Guidelines
- **Test substitutability**: Can you replace parent with child in all contexts?
- **Honor contracts**: Subclasses must fulfill the same promises as their parents
- **Avoid breaking changes**: Don't change expected behavior in inheritance

## Interface Segregation Principle (ISP)

> Clients should not be forced to depend on interfaces they do not use.

### Implementation Strategy
- **Small, focused interfaces**: Better than large, monolithic ones
- **Role-based interfaces**: Design around client needs
- **Composition over inheritance**: Combine multiple small interfaces

### Good Example (C#)
```csharp
// ❌ Violates ISP - fat interface forces unnecessary dependencies
interface IWorker 
{
    void Work();
    void Eat();
    void Sleep();
}

class Robot : IWorker 
{
    public void Work() { /* work logic */ }
    public void Eat() { throw new NotImplementedException(); } // Robot doesn't eat
    public void Sleep() { throw new NotImplementedException(); } // Robot doesn't sleep
}

// ✅ Follows ISP - segregated interfaces
interface IWorkable 
{
    void Work();
}

interface IFeedable 
{
    void Eat();
}

interface IRestable 
{
    void Sleep();
}

class Human : IWorkable, IFeedable, IRestable 
{
    public void Work() { /* work logic */ }
    public void Eat() { /* eat logic */ }
    public void Sleep() { /* sleep logic */ }
}

class Robot : IWorkable 
{
    public void Work() { /* work logic */ }
    // Only implements what it needs
}
```

### AI Development Guidelines
- **Design client-specific interfaces**: Focus on what clients actually need
- **Avoid God interfaces**: Split large interfaces into focused ones
- **Think in terms of roles**: What role does this client play?

## Dependency Inversion Principle (DIP)

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

### Implementation Strategy
- **Dependency injection**: Inject dependencies rather than creating them
- **Inversion of control containers**: Use IoC frameworks where appropriate
- **Abstract interfaces**: Define contracts that both layers can depend on

### Good Example (Rust)
```rust
// ❌ Violates DIP - high-level depends on low-level
struct FileLogger {
    file_path: String,
}

impl FileLogger {
    fn log(&self, message: &str) {
        // Write to file
    }
}

struct OrderService {
    logger: FileLogger, // Direct dependency on concrete implementation
}

impl OrderService {
    fn process_order(&self, order: Order) {
        // Process order
        self.logger.log("Order processed");
    }
}

// ✅ Follows DIP - both depend on abstraction
trait Logger {
    fn log(&self, message: &str);
}

struct FileLogger {
    file_path: String,
}

impl Logger for FileLogger {
    fn log(&self, message: &str) {
        // Write to file
    }
}

struct DatabaseLogger {
    connection: String,
}

impl Logger for DatabaseLogger {
    fn log(&self, message: &str) {
        // Write to database
    }
}

struct OrderService<T: Logger> {
    logger: T, // Depends on abstraction
}

impl<T: Logger> OrderService<T> {
    fn new(logger: T) -> Self {
        OrderService { logger }
    }
    
    fn process_order(&self, order: Order) {
        // Process order
        self.logger.log("Order processed");
    }
}
```

### AI Development Guidelines
- **Inject, don't create**: Pass dependencies as parameters
- **Define interfaces first**: Think about what you need, not how it's implemented
- **Test with mocks**: DIP makes testing much easier

## AI Development Guidelines

### Before Writing Code
1. **Identify the single responsibility** - What is this class/module supposed to do?
2. **Design for extension** - How might this need to change in the future?
3. **Define interfaces** - What contract should this fulfill?
4. **Consider dependencies** - What does this need from other components?

### During Implementation
1. **Keep methods small** - Each method should have one clear purpose
2. **Use descriptive names** - Make the responsibility obvious from the name
3. **Avoid deep inheritance** - Prefer composition over inheritance
4. **Inject dependencies** - Don't create dependencies inside classes

### Code Review Checklist
- [ ] Does each class have a single, clear responsibility?
- [ ] Can new behavior be added without modifying existing code?
- [ ] Are subclasses truly substitutable for their parents?
- [ ] Are interfaces focused and client-specific?
- [ ] Do high-level modules avoid depending on implementation details?

### Refactoring Signals
- **Large classes** → Split by responsibility (SRP)
- **Switch statements** → Use polymorphism (OCP)
- **Type checking in client code** → Fix inheritance hierarchy (LSP)
- **Empty method implementations** → Segregate interfaces (ISP)
- **Direct instantiation of dependencies** → Use dependency injection (DIP)

## Language-Specific Examples

### Python
- **SRP**: Use modules and classes appropriately
- **OCP**: Leverage duck typing and protocols
- **LSP**: Be careful with method overrides
- **ISP**: Use Protocol classes (Python 3.8+)
- **DIP**: Use dependency injection frameworks like `dependency-injector`

### TypeScript/JavaScript
- **SRP**: Use ES6 modules and classes
- **OCP**: Leverage interfaces and function composition
- **LSP**: Maintain behavioral contracts in inheritance
- **ISP**: Create focused interface definitions
- **DIP**: Use constructor injection and IoC containers

### Java
- **SRP**: Single-purpose classes and packages
- **OCP**: Abstract classes and interfaces
- **LSP**: Proper inheritance hierarchies
- **ISP**: Multiple small interfaces
- **DIP**: Spring Framework dependency injection

### C#
- **SRP**: Classes with single concerns
- **OCP**: Interfaces and virtual methods
- **LSP**: Proper inheritance design
- **ISP**: Role-based interfaces
- **DIP**: Built-in dependency injection container

### Rust
- **SRP**: Modules and structs with clear purposes
- **OCP**: Traits and generics
- **LSP**: Careful trait implementation
- **ISP**: Small, focused traits
- **DIP**: Trait objects and dependency injection

## Common Violations and Solutions

### SRP Violations
- **Problem**: God classes doing everything
- **Solution**: Extract related methods into separate classes

### OCP Violations
- **Problem**: Modifying existing code for new features
- **Solution**: Use strategy pattern or plugin architecture

### LSP Violations
- **Problem**: Subclasses throwing exceptions for inherited methods
- **Solution**: Redesign inheritance hierarchy or use composition

### ISP Violations
- **Problem**: Interfaces with many unrelated methods
- **Solution**: Break into smaller, role-specific interfaces

### DIP Violations
- **Problem**: Classes creating their own dependencies
- **Solution**: Use constructor injection or factory patterns

## Conclusion

SOLID principles are not just theoretical concepts—they're practical tools for creating maintainable, testable, and extensible software. When working with AI assistants:

1. **Start with principles in mind**: Design following SOLID from the beginning
2. **Refactor incrementally**: Apply principles gradually to existing code
3. **Use principles as review criteria**: Check each principle during code review
4. **Document architectural decisions**: Record why you chose specific patterns

Remember: These principles guide good design but shouldn't be followed blindly. Use judgment to apply them appropriately to your specific context and requirements.