# Ruby Development Protocol

**Version**: 1.0.0
**Author**: [Sashité](https://sashite.com/)
**Published**: May 3, 2025
**License**: MIT License

---

## 1. Core Principles

1. Code must follow functional programming principles.
2. Object state mutation is prohibited.
3. Pure functions must be preferred wherever possible.
4. Threads are prohibited.
5. One file per constant, per class, per module.
6. Strict methods must be preferred over lax methods (e.g., use `#fetch` instead of `#[]` on collections).

## 2. Object Structure

1. Instance variables must only be assigned values in the `initialize` method.
2. All objects must be frozen at the end of initialization by calling `freeze`.
3. Use `attr_reader` for accessing instance variables; `attr_writer` and `attr_accessor` are prohibited.
4. Methods that manage internal collections (e.g., arrays or hashes) must end with `!`.
5. Classes should implement immutable value semantics whenever possible.

### Explicit Internal State Changes

Objects are frozen at the end of initialization to prevent reassignment of instance variables. However, this does **not** prevent internal mutation of referenced objects such as arrays or hashes.

This is **intentional**: internal collections may be altered after initialization — this does not violate the immutability principle, as long as the mutation is **explicitly marked**.

#### Protocol Rule:

- All methods that modify internal state (such as adding or removing elements in a collection) must end with `!`.
- Such methods must be documented as performing side effects.
- Direct reassignment of instance variables is still strictly prohibited after initialization.

This approach allows controlled and visible evolution of an object’s internal state, while maintaining a predictable and disciplined design.

## 3. Parameters and Arguments

1. Keyword arguments must be used instead of hash parameters to ensure local isolation.
2. Flatten arguments (multiple discrete arguments) must be used instead of array parameters.
3. All optional parameters must have explicit default values.
4. All method arguments must be validated at the beginning of each method.

## 4. Import/Export Protocol (`from_params` / `to_params`)

### Objective

This protocol defines a standardized interface for initializing Ruby class instances from parameters and exporting the current instance state into parameters.

### Interface

- `self.from_params(**params) → instance`
  - Class method.
  - Accepts a keyword parameter hash.
  - Returns a new instance initialized with the provided parameters.

- `to_params() → Hash`
  - Instance method.
  - Returns a hash representing the current state of the object.

### Core Principles

1. **Reversible Serialization**: For any valid instance `obj`, the expression `obj.class.from_params(**obj.to_params)` must return a functionally equivalent instance.
2. **Dynamic State**: The result of `to_params` must reflect the *current* state of the object, even if it has changed since initialization.
3. **Completeness**: `to_params` must include all parameters necessary to recreate the object in its current state.
4. **History Independence**: `to_params` output is not required to match the original input to `from_params`, only the current logical state.

### Acceptable Behaviors

- `to_params` may include default or computed values not provided at initialization.
- Transformed or normalized values are allowed in the output hash.

### Invariants

- If no mutation occurs, `from_params(**obj.to_params)` should return an equivalent object.
- If `a.to_params == b.to_params`, then `a` and `b` must be logically equivalent.
- `from_params` must be a pure function.

### Use Cases

This protocol facilitates:

- Persistence and state restoration
- Serialization for network transmission
- Deep cloning
- Test reproducibility
- Cross-environment interoperability

## 5. References and Constants

1. Root-level constants must be referenced using the `::` prefix (e.g., `::String`, `::ArgumentError`).
2. Dependencies must be explicitly declared.
3. Only simple types must be used; type mixins are prohibited.

## 6. Method Conventions

1. Methods ending with `!` must perform a side effect and return `nil`.
2. Methods ending with `?` must return a boolean result.
3. Return types must be consistent within a method family.
4. Methods with the same name across modules/classes must return the same type.
5. Boolean-returning methods must not use `is_` or `has_` prefixes.
6. Methods must return a single type (or `nil`). Returning different types based on conditions is prohibited.

## 7. Naming Conventions

1. All identifiers must use explicit and descriptive names.
2. Array variable names must end with an `s`.
3. All names must be in technical English.
4. The default primary method name of an object must be `call`.
5. Constant names must use capitalized (CamelCase) identifiers.

## 8. Error Handling and Validation

1. All input parameters must be validated before use.
2. Exceptions must be raised for any parameter anomalies.
3. Exceptions must be explicit and meaningful.
4. Expected types must be documented in comments.
5. Input/state validation must follow a fail-fast approach.
6. Methods that may raise exceptions must document them clearly.

## 9. Security and Safety

1. Methods interacting with external systems must be isolated in dedicated classes or methods.

## 10. Documentation

1. Documentation must explain *intention*, not *implementation*.
2. Parameter and return types must be specified.
3. Potential side effects must be documented.
4. All documentation must be written in technical English.

---

Copyright © 2011 [Sashité](https://sashite.com/).
