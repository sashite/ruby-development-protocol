# Ruby Development Protocol

## 1. Object Structure and State

1.1 Instance variables must only be assigned values in the `initialize` method.
1.2 All objects must be frozen at the end of initialization by calling `freeze`.
1.3 Use `attr_reader` for accessing instance variables; never use `attr_writer` or `attr_accessor`.
1.4 Instance collections (arrays, hashes) may be mutated only by methods ending with `!`.
1.5 Instance variables must never be reassigned after initialization.
1.6 Classes should implement immutable value semantics whenever possible.

## 2. Fundamental Principles

2.1 Code must follow functional programming principles over imperative approaches.
2.2 Object state mutation is prohibited except in strictly controlled circumstances.
2.3 Immutability must be enforced for all non-collection objects.
2.4 Side effects must be explicitly marked and minimized.
2.5 Pure functions must be preferred wherever possible.
2.6 Threads must not be used unless absolutely necessary and approved by code review.

## 3. Parameter Passing

3.1 Keyword arguments must be used instead of hash parameters.
3.2 Arrays must not be used as parameters when elements have distinct meanings.
3.3 All optional parameters must have explicit default values.
3.4 The `**options` pattern must be used for optional parameters or to replace hashes.
3.5 The `*args` pattern must be used for lists rather than array parameters.

## 4. Collection Management

4.1 Higher-order functions (`map`, `select`, `reduce`) must be used instead of iterative loops.
4.2 Non-collection objects must never be mutated.
4.3 Enumerable API methods must be used for collection manipulation.
4.4 The `each` method with external variable mutation is prohibited.
4.5 Objects must be duplicated with `.dup` before modification.

## 5. Return Values

5.1 Methods ending with `!` must return `nil` or raise an exception in case of failure.
5.2 Methods ending with `?` must return a boolean value.
5.3 Return types must be consistent within method families.
5.4 Return values must not be ignored unless explicitly commented.

## 6. Security Considerations

6.1 All input parameters must be validated before use, with type checking and bounds checking.
6.2 Implement a circuit breaker pattern for emergency stops when anomalies are detected.
6.3 Use a formal verification framework to verify invariants when possible.
6.4 Classes with sensitive operations must implement an access control system.
6.5 Methods that interact with external systems must be isolated in dedicated modules.
6.6 Implement a contract-like pattern with preconditions and postconditions for critical methods.
6.7 All methods modifying state must verify state consistency before and after execution.

## 7. Types and Validation

7.1 Simple types must be used; mixins for type definitions are prohibited.
7.2 Expected types must be documented in comments.
7.3 All method arguments must be validated at the beginning of methods.
7.4 Exceptions must be raised for any argument anomalies.
7.5 Exceptions must be explicit and meaningful.
7.6 Implement fail-fast validation for all inputs and state changes.

## 8. Naming Conventions

8.1 All identifiers must use explicit, descriptive names.
8.2 Ruby naming conventions must be followed.
8.3 Boolean-returning methods must use a `?` suffix, not `is_` or `has_` prefixes.
8.4 Methods with side effects must end with `!`.
8.5 All methods ending with `!` must have a side effect.
8.6 Array variable names must end with 's'.

## 9. Code Organization

9.1 Modules must be used over classes for shared behavior.
9.2 Composition must be used instead of inheritance.
9.3 The use of `extend` and `include` must be limited and justified.
9.4 Dependencies must be explicitly declared.
9.5 Related functionality must be grouped logically.

## 10. Documentation

10.1 Documentation must explain intention, not implementation.
10.2 Parameter and return types must be specified.
10.3 Potential side effects must be documented.
10.4 All exceptions that might be raised must be documented.
10.5 Documentation must be written in complete, grammatically correct sentences.
