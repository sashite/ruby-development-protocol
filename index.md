# Ruby Development Protocol

**Version**: 1.1.0
**Author**: [Sashité](https://sashite.com/)
**Published**: May 5, 2025
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

1. Instance variables must only be assigned in `initialize`.
2. All objects must be frozen after initialization via `freeze`.
3. Use `attr_reader`; `attr_writer` and `attr_accessor` are prohibited.
4. Methods mutating internal collections must end with `!`.
5. Prefer immutable value semantics.

### Explicit Internal State Changes

- Internal collections (e.g., arrays, hashes) may be mutated post-initialization.
- Mutating methods must end with `!` and be documented as having side effects.
- Reassignment of instance variables after initialization is prohibited.

## 3. Parameters and Arguments

1. Use keyword arguments instead of hash parameters.
2. Use splat (`*args`) for ordered arguments instead of arrays.
3. Use double splat (`**kwargs`) for named arguments instead of hashes.
4. Optional parameters must have explicit default values.
5. All arguments must be validated at method entry.

**Examples:**

```ruby
# Recommended:
def add(*pieces)
  # ...
end

def load(name:, **options)
  # ...
end
````

## 4. Import/Export Protocol (`from_params` / `to_params`)

### Interface

* `self.from_params(**params) → instance`
* `to_params() → Hash`

### Principles

1. `from_params(**obj.to_params)` must reconstruct an equivalent object.
2. `to_params` reflects current state.
3. Output must include all reconstructable data.
4. Original input is not preserved — only state.

### Constraints

* `from_params` must be pure.
* Objects with identical `to_params` are logically equivalent.

## 5. References and Constants

1. Use `::` for top-level constants (e.g., `::String`).
2. Declare dependencies explicitly.
3. Avoid type mixins. Use only simple types.

## 6. Method Conventions

1. `!` methods perform side effects and return `nil`.
2. `?` methods return booleans.
3. Return types must be consistent within method families.
4. Identical method names across classes must return the same type.
5. Do not prefix booleans with `is_` or `has_`.
6. Methods must return a single type (or `nil`).

## 7. Naming Conventions

1. Names must be explicit and descriptive.
2. Arrays must have names ending in `s`.
3. All names must be in technical English.
4. Default method name should be `call`.
5. Constants use CamelCase.

## 8. Error Handling and Validation

1. Validate all inputs before use.
2. Raise explicit exceptions on anomalies.
3. Document expected types in comments.
4. Follow fail-fast design.
5. Clearly document exceptions.

## 9. Security and Safety

1. External system calls must be isolated in dedicated methods or classes.

## 10. Documentation

1. Document intention, not implementation.
2. Specify parameter and return types.
3. Document all side effects.
4. Write all documentation in technical English.

## 11. Type Coercion

Prefer Kernel methods over object methods for type conversion:

* Use `String(x)`, `Array(x)`, `Integer(x)` instead of `x.to_s`, `x.to_a`, `x.to_i`, etc.
* These conversions are safer, more explicit, and fail predictably on invalid input.

**Example:**

```ruby
# Recommended:
def format_name(input)
  name = String(input) # raises if input is invalid
  # ...
end
```

---

Copyright © 2011–2025 [Sashité](https://sashite.com/)
