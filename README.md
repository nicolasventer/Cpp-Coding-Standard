# Coding standard

The main goal of the coding rules is **code consistency** _(i.e. regardless of who writes the code)_.  
To achieve this, the rules should be written so that **it minimizes the number of ways to write the code**.  
<b>However, these rules are not absolute. <u>Exceptions can be made if justified.</u></b>

Many of the rules below are enforced or assisted by the tooling files in this repository: [.clang-format](.clang-format), [.clang-tidy](.clang-tidy) and [compile_flags.txt](compile_flags.txt).

<details open><summary><font size="5" id="coding-rules">Coding rules</font></summary>

Rules that describe the way to implement.

- General
  - Friendship: **only when declaring operators** (i.e. `<`, `==`, `<<`, ...)
  - Shared pointers: **only for instances of interfaces**
  - Dynamic allocation: **do not use** (i.e. no use of `new`)
  - Exceptions: **do not use**
  - Cast: if possible, **use `static_cast`**, else `reinterpret_cast`. **Do not use `dynamic_cast` or `C style cast`**
  - Comments: **for everything**
  - Asserts: **for all implicit conditions required for correct execution**
  - Aliases: **use the `using` keyword instead of the `typedef` keyword**
  - Lambda: **use `std::function` as the type but declare it with a `lambda function`** (or even better, declared with custom concept `Callable`)
  - Inheritance: **do not use inheritance** except for interfaces. Prefer composition over inheritance
  - Dead code: **do not use** (no what-if code)
  - Files: **only one class per file, except for header-only libraries**
  - Libraries:
    - Implementation: **should expose only the public interface** (i.e. may require PIMPL pattern)
    - Publication: **should be published as a header-only library** whose implementation is in `#ifdef IMPLEMENTATION` section
- Headers
  - Header inclusion: **no forward declaration**
  - Header guard: `#pragma once`
    - Declaration order
      - Accessibility order:
        1. public
        2. protected
        3. private
      - Categories order:
        1. Sub-classes
        2. Static methods
        3. Static members
        4. Non-static methods
        5. Non-static members
- Sources
  - Definition order: **same order as declared**
    - Include order
      In alphabetical order for each category:
      1. Corresponding header
      2. Standard library headers
      3. External dependencies headers
      4. Project headers
- Variables
  - Declaration
    - Instruction: **only one declaration per line**. This rule also applies for class members
    - Initialization: **if possible, initialize variable on declaration**. This rule also applies for class members
  - Scope: **declare the variable inside a method, a class, or a namespace**
  - Inline variables: **only for constant** or singleton (should be avoided)
  - Pointer/Reference: **use references instead of pointers when the value is not optional**, unless the object is contained in a data structure
  - Constness: **maximize the constness of variables**
  - Primitive type: **use only strong primitive types** _(e.g. `uint32_t`, `size_t`)_
  - Type: **use the most accurate types** _(e.g. `istringstream` instead of `stringstream`)_
  - `auto` keyword: **use only for lambda, loop-variable and types that are longer than 15 characters**
- Functions
  - Inline functions: **only for template specialization**. Other inline functions should be avoided
  - Inline methods: **only for setters and getters**
  - Side effect: **avoid side effect functions**  
     Exception valid for: data structures, in-out parameters, multiple outputs
  - `return` instruction: **immediately exit a function when a required condition is not fulfilled**
  - Parameters
    - Default value: **allowed**, rule TBD
    - Passing: **pass primitive types by value, pass any other types by reference**
  - Global scope functions: **do not use**, functions should be placed within a namespace or a class
  - Static functions: **do not use**. Prefer inline functions over static functions
- Structs
  - Usage: use structs for **mainly public attributes and methods**  
    Other rules in `Classes`
- Classes
  - Constructor: **do not use implicit conversions**, use the `explicit` keyword
  - Virtual: **declare the destructor as a virtual for classes that contain virtual methods**
  - Concept: **prefer concepts over interfaces**
  - Interface:
    - Usage: use interfaces **when polymorphism is required within a collection** (like array, set, map, ...)
    - Implementation: **should be implemented as a class with only virtual methods**
  - Sub-classes: **only for PIMPL pattern**
  - Accessibility: **minimize the accessibility of the class members**
  - Constness: **maximize the constness of the member functions**
  - Static: **only for utility functions with private implementation**, use namespaces if no private implementation
- Namespaces
  - Usage: use namespaces **for utility functions, for libraries, for converters of enumerators, and for enumerators as flag**
  - `using` instruction: **`using namespace` instruction should be avoided** or used in a function scope or namespace scope (not in global scope)
- Enumerators
  - Declaration: should be declared with `enum class` except for flags where it should be declared with `namespace E<enum_name> { enum Type { ... } }`
  - Flags: all values should be defined with `1 << N`, where `N` is the index of the value
  - Converters: **all functions should be defined in the namespace corresponding to the enumerator** as `static constexpr` functions with `switch case` statements
- Unions: **use variants instead of unions**
- Macros:
  - Usage: use macros **when using `__FUNCTION__` or `__LINE__`**
  - Implementation:
    - public macros should be surrounded by `do { ... } while (0)`
    - macros with parentheses should be called with `;` (i.e. no `;` at the end of the macro)
    </details>

<details open><summary><font size="5" id="naming-rules">Naming rules</font></summary>

Rules that describe the way to name.

- General
  - Language: **English** only
  - Word `number`: **do not use `number`. Use `count`, `id` or `index`** _(e.g. carId instead of carNumber)_
  - Plural: **no plural, describe as a container** _(e.g. carList instead of cars)_
  - File name: **same as declared/defined class**, PascalCase for class, PascalCase without leading `E` for enumerator, snake_case for namespace
  - Folder name: **denomination of the content** as PascalCase for classes and snake_case for namespaces
- Headers
  - Extension: `.h` only if c compatible, else `.hpp`
  - Content: only declarations and inline functions and template functions
- Sources
  - Extension: `.cpp`
- Variables
  - Iteration index: **`i, j, k...` are reserved name for iteration index**
  - Case: **camelCase**
  - Naming: **same name as type in camelCase** _(e.g. FileWatcher fileWatcher)_
  - Boolean: **boolean variables must start with `b`, `is` or `has`**
  - Container: **container variables must end with the type of the container** _(e.g. std::map<string, int> nameAgeMap)_
  - Modifiers _(as class member)_
    - Private/Protected/Public: **do not use any marker**
    - Constant: **SCREAMING_SNAKE_CASE**
    - Static: **do not use any marker**
- Functions
  - Overloading: **function names must be unique**, except for templated functions
  - Case: **camelCase**
  - Boolean: **boolean functions must start with `b`, `is` or `has`**
  - Modifiers _(as method member)_
    - Private/Protected/Public: **do not use any marker**
    - Static: **do not use any marker**
- Enumerators
  - Case: **PascalCase with leading `E`** If enum is a flag, use namespace E<enum_name> { enum Type { ... } }
  - Enum values case: **PascalCase**
  - Namespace: **same name as enum without leading `E`**
- Classes
  - Naming
    - Modifiers
      - Interface _(only virtual pure methods)_: **PascalCase with leading `I` and trailing `able`** _(e.g. IPlayable)_
      - Abstract: **PascalCase with leading `A`**
      - Static _(only static methods)_: **do not use any marker**
      - Templated: **do not use any marker**
    - Order:
      1. Data involved _(e.g. Car, Shoe, Key)_ _(still no plural)_
      2. Data processing method _(e.g. Watcher, Cleaner, Filter)_
    - Extension: **the child class name must end with its parent class name**  
       Abbreviate the parent name if its length is greater than `15`  
       Add a comment just above the parent class to define the used abbreviation
      Other rules in `Types`
- Methods
  - Case: **camelCase**. Should start with `get` only if no computation is involved, should return a reference or a pointer.
- Concepts
  - Case: **PascalCase with trailing `able`**
- Namespaces
  - Case: **snake_case**
- Types
  - Case: **PascalCase**
- Macros
  - Case: **SCREAMING_SNAKE_CASE** with leading `P_` for private macros
  </details>

<details open><summary><font size="5" id="format-rules">Format rules</font></summary>

All of the format rules to apply are defined in the [clang-format file](.clang-format).

</details>
<br>

# License

MIT License. See [LICENSE file](LICENSE).
Please refer me with:

    Copyright (c) Nicolas VENTER All rights reserved.
