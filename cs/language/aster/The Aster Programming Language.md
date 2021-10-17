# The Aster Programming Language

The **Aster** programming language is a *general-purposed*, *static*, and *strong-typed* language. I draft this language in the light of existing programming languages. **Aster** has a similar grammar with modern **C++**, a similar type system with **Haskell**, and a similar object management strategy like **Rust**. **Garbage Collection (GC)** widely used in many languages like **Java** and **Python** is welcomed to be standardized, but I will *not* enforce it.

Basically, I'm fascinated by **C++**'s support for flexible paradigms and powerful meta-programming with templates, but sometimes annoyed by the over-complicated language features, redundant grammars, and strange corner cases due to backward compatibility. That's why I want to design a new language, not as good as **C++** in terms of runtime performance and low-level correspondence, but much better with respect to: 1. easy to write safe codes; 2. easy to predict program behaviors (no *Undefined Behavior (UB)* of course); 3. more natural transitions between different paradigms.

In this article (probably will evolve into a tiny guide book after several iteration), I will try to use a casual tone and share all my thoughts and choices made during the consideration of language features. Occasionally, I will refer to some features appeared or proposed in other languages, like the ones I mentioned before.

Let's begin now. Have fun!

[TOC]



## Basics

We will have to be familiar with the following terms and concepts before moving foward, since they will be frequently used. Important words are *italicized*.

- A *source file* is a file that contains **Aster** *source code* to be *compiled* into a file of a *target language*, e.g. **C++**.
- A *module* is a collection of source files managed by a *header file* that contains *declarations* of *identifiers* as well as specifications of their visibility to other modules.
- **Aster** preserves ceratin permutations of characters, called *keywords*, to structure the source code. Some other patterns of character sequences are recognized as *literals* that represent *objects* of *integers*, *strings*, etc.
- All other words are *identifiers* that can be bound to **Aster** *entities*, including *objects*, *types*, *classes*, *templates*, *namespaces*, *macros*, *labels*, and *argument packs*. 
- The action of introducing *identifiers* is known as a *declaration*. The process of binding them to *entities* is called a *definition*. An identifier can only be bound to one entity within the same *scope level*, which rule is called the *One Definition Rule (ODR)*.
- Objects that are bound to identifiers are called *variables*. As a convention, we also call the identifiers they bound to variables. This could cause ambiguity sometimes, especially when we talk about the *constness* of variables, in which case we actually refer to the identifier, not the object.
- A *scope* is either a *file scope*, restricted to a source file, or *block scope*, usually confined within a pair of braces, `{}`. Identifiers defined in a scope could be "redefined" in another scope, and cannot be referred to at outside of the scope where their definitions lie.
- Objects are *evaluable* entities that can be resolved to bound *values*. Some of them are known as *callables*, which can run *subroutines* if one or more *arguments* are provided.
- There is a special subroutine called *main function* that is by default configured as the entry of every program.
- A *subroutine* consists of commands, called *statements*, that are executed in sequence after compilation to executable files. Each statement involves *operations* that perform calculations or modifications on objects. The operations together with the operands are called *expressions*. An expression is also evaluable, just like an object.
- All *objects* and *expressions* are associated with a *storage type*, which reveals the underlying structure of the values they are evaluated to. A *type*, on the other hand, is an *alias* of a storage type that can concretize the same value with different meanings. *Member functions* are callables that are connected with bound *types*.
- A *type class* or simply *class* claims properties of an unknown group of *types*. A type *satisfies* a class if all claimed properties are met, and we also call it an *instance* of the class.

Besides, it should be helpful if I introduce the grammar to define a variable in **Aster**:

```cpp
auto i = 10;			// i : int
auto s = "abc";			// s : [const char]
auto arr = [ 1, 2, 3 ];	// arr : [int]
```

That's it. If you are familiar with **C++**, you can treat it as the auto type deduction (with minor differences we don't care so far). `i` is an identifier bound to an integer object whose value is `10`; `s` is an identifier bound to an array of constant characters who make up of a string `"abc"`; `arr` is an identifier bound to an array of integers whose values are `1`, `2`, and `3`.

Finally, let's get to know the abbreviations of some terms, which will be frequently used in illustration of grammar. Note that `@` appears when the following name should be interpreted via some structures, some of which names are listed below:

| **Abbreviation** | **Meaning** | **Abbreviation** | **Meaning** | **Abbreviation** | **Meaning** |
| ---------------- | ----------- | ---------------- | ----------- | ---------------- | ----------- |
| `@id`            | identifier  | `@expr`          | expression  | `@stmt`          | statement   |
| `@qual`          | qualifier   | `@spec`          | specifier   | `@mod`           | modifier    |
| `@attr`          | attribute   | `@patt`          | pattern     | `@cls`           | clause      |
| `@list`          | list        | `?`              | optional    | `!`              | strict      |
| `@ent`           | entity      |                  |             |                  |             |

Some of the terms above needs further specification for syntax, let's view some of the most frequetly used:

- `@id`: Identifiers or scope-specified identifiers. e.g. `name`, `ns::ms::ks::name`. This could be any valid identifiers.
- `@type-id`: Identifiers or scope-specified identifiers that have *type semantics*. e.g. `MyType`, `ns::MyType`. Note that identifiers for *templates* are also put into this category.
- `@class-id`: Identifiers or scope-specified identifiers that have *class semantics*. e.g. `trait`, `ns::trait`.
- `@list`: All structures containing comma-separated entities are called `@list`.
- `@arr-list`: Lists that enclosed by a pair of brackets. e.g. `[10, a, c + d]`.
- `@tup-list`: Lists that enclosed by a pair of parentheses. e.g. `(1, "abc", secret)`.
- `@patt`: Patterns are like value expressions, but each element could be a `@cstr-chain` or `@id : @cstr-chain`, which is also known as `@patt-pair`.

## A Crash Course of Aster

### Statements

I wound like to start with the *statements* in **Aster**. It's rather similar to **C++**:

- Labeled Statements: A label structure followed by `:` or `=>`, and a statement.

  - `@id : @stmt`: A target for jump statements.
  - `?@attr @patt => @stmt`: A case in the switch or match statement.

  ```cpp
  auto main = () -> do {
  begin:
      auto i : ?int;
      input &i;
      match (i) {
          i : int => print i;
      	else    => goto begin;
      }
  };
  ```

- Expression Statements: The trivial statements made up of an expression.

  - `?@attr @expr;`: An operation (that may contain nested operations) whose result is discarded.

  ```cpp
  auto main = () -> do {
      auto i = 10;
      i += 42;
      print (let j = 10 in i + j);
  }
  ```

- Compound Statements: A collection of statements put inside a block.

  - `{ ?@stmt... }`: 

  ```cpp
  auto main = () -> do {
      auto i = 42;
      {
          auto i = 10;
      }
      print i;
  }
  ```

- Branch Statements: Select different paths to go based on conditions or patterns.

  - `if (@expr) @stmt`: If the `@expr` is evaluated *non-false*, the `@stmt` will be run.
  - `if (@expr) @stmt else @stmt`: Same as the last one, but the `@stmt` after the `else` keyword will be run if the `@expr` is evaluated *false*.
  - `switch (@expr) @stmt`: Dispatch the control flow by jump tables. The `@expr` must has integral type.
  - `match (@expr) @stmt`: Dispatch the control flow by pattern matching. Basically a syntax sugar for if statements.

  ```cpp
  auto main = () -> do {
      auto i : int;
      input &i;
      if (i >= 10) {
       	print "Too big!" ;  
      }
      else if (i < 0) {
       	print "Too small";   
      }
      switch (i) {
       	[0..5] => "Good!";
          [5..10] => "Bad!";
      }
  }
  ```

- Loop Statements: Repeat the execution of a statement under (optional) conditions.

  - `?@attr while (@expr) @stmt`: Loop the `<stmt>` as long as the `@expr` is evauated *non-false* each time the control flow reaches the start of the loop.
  - `?@attr loop @stmt`: Loop the `@stmt` unconditionally.
  - `?@attr loop @stmt when (@expr)`: Same as the while loop.
  - `?@attr for (@patt in @expr) @stmt`: The *range-for* loop. Iterate over elements in a range that match the given pattern.

  ```cpp
  auto main = () -> do {
      auto i = 10;
      while (i > 0) print i;
      i = 10;
      loop { print i; } when (i > 0);
      
      auto arr = [1, 2, 3];
      for (x : (> 0) in arr) print x;
  }
  ```

- Jump Statements: 

  - `goto @id;`: Jump to the given label unconditionally.
  - `break ?@id;`: Break the loop specified by the given label; break the most inner loop if no label is provided.
  - `continue ?@id;`: Go to the next iteration of the loop specified by the given label; iterate over the most inner loop if no label is provided.
  - `return ?@expr;`: Return from the current do expression with an optional return value.
  - `return ?@expr for @id;`: Return a value from a labeled `do` expression.

  ```cpp
  auto main = () -> do {
      [[@name: outer]]
      while (true) {
       	[[@name: inner]]
          while (true) {
           	auto i : int;
              input &i;
              if (i > 0) break outer;
              else if (i < 0) break inner;
              else continue;
          }
      }
  }
  ```

- Declaration Statements:

  - `@decl`: Declaration of identifiers.



### Expressions

Recall that an *expression* is a sequence of operations consisting of *operators* and *operands*. All expressions are *evaluable*, i.e. there is a result produced by the expression. For example, `1+1` is an expression having a single operation, whose operator is `+` and operands are both `1`. The result of this operation is `2`. 

All expressions can be categorized into types as follows:

- Value expressions: Expressions that are themselves values. This includes all *literals* and *variables*. No operation is involved.
- Function Application Expressions: Expressions that make function calls. 
- Infix Operation Expressions: 
- Subscript Expressions:
- Borrow Expressions: 
- Member Access Expressions:
- Let Expressions: Temporary identifier binding in a sub-expression.
  - `let @patt <- @expr ?,... in @expr`: The `@expr` after `in` keyword will be evaluated with the augmented context configured by a list of temporary binding.
- Do Expressions: A *subroutine* structure.
  - `do <stmt>`: The result of this is given by the return statement. This is a lazy expression that will not be evaluated until there is a forced-evaluation semantics.
- If Expressions: A concise version of the if statements.
  - `if @expr then @expr else @expr`

### Clauses

**Aster** has a couple of *clauses* that work as syntax sugar.

- Where Clauses: Add temporary variables to the context.
  - `where @id = @expr ?,...`: This has a similar effect with let expressions, but they're used in different places. Where clauses are used after any expressions, but they have to qualify the outmost expression and appear only once in a statement. 
  - `where @patt <- @expr ?,...`: 
  - `where @func-decl ?,...`: Declare, but doesn't define some identifiers.
- When Clauses: Add conditions to statements to be executed.
  - `when (@expr)`: Only if the `@expr` is evaluated `true` will the preceding statement be executed.
- Requires Clauses: Add constraints to statements to be successfully compiled.
  - `requires (@static-expr)`: Only if the `@static-expr` is evaluated `true` will the preceding statement pass the compilation.
  - `requires (@func-decl ?,...) @stmt`: Only when the `@stmt` has no semantic error as well as grammatic and syntactic error will the preceding statement pass the compilation. Often used inside a type class declaration with a where clause.



### Declarations

**Aster** has a concise grammar for *declarations*, unlike the bizzare one in **C++**. Declarations of entities are almost homogeneous across different *entity categories*, like *objects*, *types* and *classes*. Within each category, the declaration grammar is **identical**.

- Object Declarations:

  - `@decl-key @id : @type-id|@class-id;`: Declare a variable unbound to any object, but its type is specified. The `@decl-key` could be any of `auto`, `const`, and `mutable`.
  - `@decl-key @id = @expr;`: Declare a variable bound to a value obtained from the `@expr`. The type of the variable will be the same as the storage type of the *right-hand side (RHS)* of the equation.
  - `@decl-key @id : @type-id|@class-id = @expr;`: The combination of the two cases above.
  - `@decl-key @patt <- @expr;`: Declare one or more variables embedded in a pattern trying to match the RHS. The statement won't complie on match failures.
  - `@decl-key @patt : @type-id|@class-id = @expr;`: Add explicit type specification for the pattern matching declaration.

  ```cpp
  // Definition of the main function
  auto main = () -> {
      auto x : int;
      auto y = 10;
      auto z : [char] = "abc";
      auto [first, rest...] <- z;
      auto [second, last] : [char] = [rest...];
  }
  ```

- Type Declarations:

  - `type @id : @class-id;`: Declare a type alias unbound to any type, but the type class it conforms to is specified.
  - `type @id = ?@type-mod @type-id;`: Declare a type alias bound to a type. The `@type-mod` could be any of `?` and `!`.
  - `type @id : @class-id = @type-id;`: The combination of the previous two grammar.
  - `@id ?: ?@class-op-list

  ```cpp
  type mytup_t : std::eq;
  type mytup_t = ?(int, [char]);
  type mytup2_t : std::ord = (int, [char])
  ```

- Template Delcarations:

  - `@decl-key @id<<id> ?: ?@class-op-list ?,...> = <static-expr>`: Declare a template variable with a static value.
  - `@decl-key @id<<id> ?: ?@class-op-list ?,...>`: Declare a template variable.
  - `@decl-key 

- Type Class Declarations:

  - `class @id <<id> : @class-op-list ?,...>;`: Declare a type class that inherits all constraints required in the RHS class generated by the list of classes.

  ```cpp
  class ord<T : std::eq & std::serial> where
      (<) : T -> T -> bool;
  ```

  

### Functions

**Aster** *functions* are identifiers that bound to *closure objects*, which are essentially objects of types that *overload* the `()` operator (called "functors" in **C++**, but well, this is not a proper name). The RHS of a function declarations are as follows:

- `[@id = @expr ?,...] (?@func-decl ?,...) -> @expr`: This closure object has several *members* declared in the *capture list*, specified by a pair of brackets. The *parameter list* are specified by a pair of  

```cpp
[](a) -> [](b) -> [](c) -> [](d) -> a + b + c + d
```



