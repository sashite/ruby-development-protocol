# Ruby Development Protocol

A minimal and opinionated protocol to industrialize and standardize Ruby development.

This specification defines strict conventions around functional programming, immutability, naming, error handling, and serialization — designed for maintainable, testable, and predictable Ruby codebases.

## 📘 Highlights

- Functional-first principles with controlled internal mutability
- Explicit side-effect signaling using method naming (`!`, `?`)
- Immutable object design with frozen state
- Standardized object import/export via `from_params` / `to_params`
- Enforced consistency across methods, constants, and naming

## 📄 Specification

Read the full protocol here:
[Ruby Development Protocol (v1.0.0)](./index.md)

## 🧪 Suggested Use

- As a reference for internal team guidelines
- As a checklist during code review
- As a foundation for linters and code analysis tools

## License

MIT © [Sashité](https://sashite.com/)
