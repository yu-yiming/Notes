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
- **Aster** preserves certain permutations of characters, called *keywords*, to structure the source code. Some other patterns of character sequences are recognized as *literals* that represent *objects* of *integers*, *strings*, etc.
- All other words are *identifiers* that can be bound to **Aster** *entities*, including *objects*, *types*, *classes*, *templates*, *namespaces*, *macros*, *labels*, and *parameter packs*. 
- The action of introducing *identifiers* is known as a *declaration*. The process of binding them to *entities* is called a *definition*. An identifier can only be bound to one entity within the same *scope level*, which rule is called the *One Definition Rule (ODR)*.
- Objects that are bound to identifiers are called *variables*. As a convention, we also call the identifiers they bound to variables. This could cause ambiguity sometimes, especially when we talk about the *constness* of variables, in which case we actually refer to the identifier, not the object.
- A *scope* is either a *file scope*, restricted to a source file, or *block scope*, usually confined within a pair of braces, `{}`. Identifiers defined in a scope could be "redefined" in another scope, and cannot be referred to at outside of the scope where their definitions lie.
- Objects are *evaluable* entities that can be resolved to bound *values*. Some of them are known as *invokables*, which can run *subroutines* if one or more *arguments* are provided.
- There is a special subroutine called *main function* that is by default configured as the entry of every program.
- A *subroutine* consists of commands, called *statements*, that are executed in sequence after compilation to executable files. Each statement involves *operations* that perform calculations or modifications on objects. The operations together with the operands are called *expressions*. An expression is also evaluable, just like an object.
- All *objects* and *expressions* are associated with a *storage type*, which reveals the underlying structure of the values they are evaluated to. A *type*, on the other hand, is an *alias* of a storage type that can concretize the same value with different meanings. *Member functions* are invokables that are connected with bound *types*.
- A *type class* or simply *class* claims properties of an unknown group of *types*. A type *satisfies* a class if all claimed properties are met, and we also call it an *instance* of the class.

Besides, it should be helpful if I introduce the grammar to define a variable in **Aster**:

```cpp
auto i = 10;			// i : int
auto s = "abc";			// s : [const char]
auto arr = [ 1, 2, 3 ];	// arr : [int]
```

That's it. If you are familiar with **C++**, you can treat it as the auto type deduction (with minor differences we don't care so far). `i` is an identifier bound to an integer object whose value is `10`; `s` is an identifier bound to an array of constant characters who make up of a string `"abc"`; `arr` is an identifier bound to an array of integers whose values are `1`, `2`, and `3`.

Finally, let's get to know the abbreviations of some terms, which will be frequently used in illustration of grammar. Note that `@` appears when the following name should be interpreted via some structures, some of which names are listed below:

| **Abbreviation** | **Meaning**                        | **Abbreviation** | **Meaning**               | **Abbreviation** | **Meaning**               |
| ---------------- | ---------------------------------- | ---------------- | ------------------------- | ---------------- | ------------------------- |
| `@src`           | source file                        | `@modul`         | module                    | `@head`          | header file               |
| `@key`           | keyword                            | `@lit`           | literal                   | `@id`            | identifier                |
| `@decl`          | declaration                        | `@def`           | definition                | `@spec`          | specification             |
| `@stmt`          | statement                          | `@expr`          | expression                | `@cls`           | clause                    |
| `@block`         | block                              | `@var`           | variable                  | `@val`           | value                     |
| `@qual`          | qualifier                          | `@spcf`          | specifier                 | `@mod`           | modifier                  |
| `@ctrl`          | control                            | `@conj`          | conjunction               | `@sym`           | symbol                    |
| `@ent`           | entity                             | `@obj`           | object                    | `@type`          | type                      |
| `@class`         | type class                         | `@tmpl`          | template                  | `@ns`            | namespace                 |
| `@macro`         | macro                              | `@lbl`           | label                     | `@parpack`       | parameter pack            |
| `@func`          | function                           | `@clos`          | closure                   | `@invok`         | invokable                 |
| `@keyval`        | colon separated pair               | `@bind`          | equal sign separated pair | `@match`         | Left arrow separated pair |
| `@seq`           | space separated sequence           | `@list`          | comma separated sequence  | `@proj`          | arrow separated sequence  |
| `@scoped`        | double colon separated sequence    | `@lval`          | left value                | `@rval`          | right value               |
|                  |                                    |                  |                           |                  |                           |
| `@attr`          | attribute                          | `@patt`          | pattern                   |                  |                           |
| `@cstr`          | `&` or `|` separated types/classes |                  |                           |                  |                           |
| `?`              | optional                           | `*`              | match none or once        | `**`             | match once or more        |
| `<>`             | syntactic brackets                 | `<or>`           | one or two of             | `<!or>`          | only one of               |
| `()`             | syntactic brackets for optional    |                  |                           |                  |                           |

Some of the terms above needs further specification for syntax, let's view some of the most frequetly used:

- `@id`: Identifiers or scope-specified identifiers. e.g. `name`, `ns::ms::ks::name`. This could be any valid identifiers.
- `@pureid`: Identifiers only.
- `@type-id`: Identifiers or scope-specified identifiers that have *type semantics*. e.g. `MyType`, `ns::MyType`. Note that identifiers for *templates* are also put into this category.
- `@class-id`: Identifiers or scope-specified identifiers that have *class semantics*. e.g. `trait`, `ns::trait`.
- `@decl`: Declaration of entities differ a lot depending on specific circumstance, but `auto` is usually an available `@decl-key` for most declarations.
- `@arr-list`: Comma separated sequence enclosed by a pair of brackets. e.g. `[10, a, c + d]`.
- `@tup-list`: Comma separated sequence enclosed by a pair of parentheses. e.g. `(1, "abc", secret)`.
- `@link-proj`: Arrow separated sequence enclosed by a pair of brackets. e.g. `[1 -> 2 -> a -> e + f + g]`.
- `@attr`: Comma separated identifiers or name bindings enclosed by a pair of double-brackets. The syntax is `[[<@id <!or> @bind>-list]]`.  e.g. `[[name = "my attribute", std::run_once]]`.
- `@patt`: Key-value pairs whose left-hand side is an expression of identifiers, and right-hand side is an expression. The syntax is `@id-expr : @expr`. e.g. `i : int`, `(i, { name, id }) : (int, std::serializable)`.
- `@cstr`: Expressions that contains conjunctions and disjunctions of types and type classes. The syntax is `*<@type-id <!or> @class-id>(& or |)`. e.g. `int | ([char], bool) | std::convertible_to<int>`.

## A Crash Course of Aster

### Statements

I wound like to start with the *statements* in **Aster**. It's rather similar to **C++**:

- Labeled Statements: An identifier or pattern structure followed by `:` or `=>`, and a statement.

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

  - `{ *@stmt }`: 

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
  - `return ?@expr from @id;`: Return a value from a labeled `do` expression.

  ```cpp
  auto main = () -> do {
      [[name = "outer"]]
      while (true) {
       	[[name = "inner"]]
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

  - `@decl`: Declaration of identifiers. See [declarations](# Declarations) for details.
  
- Constraint Specification Statements:

  - `@patt`: Pure declaration of identifiers



### Expressions

Recall that an *expression* is a sequence of operations consisting of *operators* and *operands*. All expressions are *evaluable*, i.e. there is a result produced by the expression. For example, `1+1` is an expression having a single operation, whose operator is `+` and operands are both `1`. The result of this operation is `2`. 

All expressions can be categorized into types as follows:

- Value expressions: Expressions that are themselves values. This includes all *literals* and *variables*. No operation is involved.
- Function Application Expressions: Expressions that make function calls. 
  - `@func-id @arg-seq`: Left-associative operation that **cannot** be overloaded. A explicit equivalent is the `$` operator, which **can** be overloaded.
- Infix Operation Expressions: Call an Invokable that has no less than 2 parameters can through their infix version.
  - `@expr @op @expr`:  A syntax sugar for `operator @op @expr @expr` or `(@op) @expr @expr`.
  - `@expr <@func-id> @expr`: A syntax sugar for `@func-id @expr @expr`.
- Subscript Expressions: Call the overloaded `operator []` function.
  - `@expr[@expr]`: A syntax sugar for `@expr.operator [] @expr`. Usually implemented as dereferencing after offsetting.
- Borrow Expressions: Borrow the ownership of an object.
  - `&@expr`:  The `@expr` **must** be evaluated to be an *lvalue*. Unary operation **cannot** be overloaded.
- Member Access Expressions: Access the inside of an object.
  - `@expr.@id`: A syntax sugar for `@expr.prototype::@id`, so this is not exactly an operation, so it **cannot** be overloaded.
- Let Expressions: Temporary identifier binding in a sub-expression.
  - `let @patt <- @expr ?,... in @expr`: The `@expr` after `in` keyword will be evaluated with the augmented context configured by a list of temporary binding.
- Do Expressions: A *subroutine* structure.
  - `do <stmt>`: The result of this is given by the return statement. This is a lazy expression that will not be evaluated until there is a forced-evaluation semantics.
- If Expressions: A concise version of the if statements.
  - `if @expr then @expr else @expr`

### Clauses

**Aster** has a couple of *clauses* that work as syntax sugar.

- Where Clauses: Add temporary variables to the context.
  - `where @where-item-list`: Declare variables that appears in a statement. It's similar to let expressions, but the where clauses **must** qualify the outmost expression in a statement and only appear **once** in that statement. The followings are cases of `@where-item`:
    - `@bind`: Define a variable **without** the declaration specifier.
    - `@match`: Define variables that match certain patterns **without** the declaration specifier.
    - `@func-decl`: Declare a variable but not necessarily define it, **without** the declaration specifier.
- When Clauses: Add conditions to statements to be executed.
  - `when (@expr)`: Only if the `@expr` is evaluated `true` will the preceding statement be executed.
- Requires Clauses: Add constraints to statements to be successfully compiled.
  - `requires (@static-expr)`: Only if the `@static-expr` is evaluated `true` will the preceding statement pass the compilation.
  - `requires (@pure-decl-list) @stmt`: Only when the `@stmt` has no semantic error as well as grammatic and syntactic error will the preceding statement pass the compilation. Often used inside a type class declaration with a where clause.
- With Clauses: Expand the scope of the given entities, like namespaces or compound objects.
  - `with @ns-id`:  All identifiers within the given namespace will be injected to the context of the current statement.
  - `with @expr`: The members in the result object will be exposed to the current statement.
- As Clauses: Define alias of identifiers. Commonly used in import statements and with clauses.
  - `as @pureid`: Define an alias for the preceding entity.



### Declarations

**Aster** has a concise grammar for *declarations*, unlike the bizarre ones in **C++**. Declarations of identifiers are almost homogeneous across different *entity categories*, like *objects*, *types* and *classes*. Within each category, the declaration grammar is **identical**.

- Object Declarations:

  - `@decl-key @pureid : @type-id <!or> @class-id;`: Declare a variable bound to an object of default value of type that is specified. The `@decl-key` could be any of `auto`, `const`, and `mutable`.
  - `@decl-key @pureid = @expr;`: Declare a variable bound to a value obtained from the `@expr`. The type of the variable will be the same as the storage type of the *right-hand side (RHS)* of the equation.
  - `@decl-key @pureid : @type-id <!or> @class-id = @expr;`: The combination of the two cases above.
  - `@decl-key @patt <- @expr;`: Declare one or more variables embedded in a pattern trying to match the RHS. The statement won't complie on match failures.
  - `@decl-key @patt : @type-id <!or> @class-id <- @expr;`: Add explicit type specification for the pattern matching declaration.

  ```cpp
  // Definition of the main function
  auto main = () -> do {
      auto x : int;
      auto y = 10;
      auto z : [char] = "abc";
      auto [first, rest...] <- z;
      auto [second, last] : [char] <- [rest...];
  }
  ```

- Type Declarations:

  - `@type-decl-key @id : @class-id;`: Declare a type alias unbound to any type, but the type class it conforms to is specified. The `@type-decl-key` could be either of `auto`, `type`.
  - `@type-decl-key @id = ?@type-mod @type-id;`: Declare a type alias bound to a type. The `@type-mod` could be any of `?` and `!`.
  - `@type-decl-key @id : @class-id = @type-id;`: The combination of the previous two grammar.

  ```cpp
  type mytup_t : std::eq;
  type mytup_t = ?(int, [char]);
  type mytup2_t : std::ord = (int, [char])
  ```

- Template Declarations:

  - `@decl-key @id<@id ?: ?@class-op-list ?,...> = <static-expr>`: Declare a template variable with a static value.
  - `@decl-key @id<<id> ?: ?@class-op-list ?,...>`: Declare a template variable.

- Type Class Declarations:

  - `class @id <<id> : @class-op-list ?,...>;`: Declare a type class that inherits all constraints required in the RHS class generated by the list of classes.

  ```cpp
  class ord T where
      T : std::eq & std::serial
      (<) : T -> T -> bool;
  ```
  
  

### Functions

**Aster** *functions* are identifiers that bound to *closure objects*, which are essentially objects of types that *overload* the `$` operator (called "functors" in **C++**, but well, this is not a proper name). A function declarations is as follows:

- `[*@bind] (*@decl) -> @expr`: This closure object has several *members* declared in the *capture list*, specified by a pair of brackets. The *parameter list* are specified by a pair of  


## Aster Core
### Evaluables and Invokables
There are two important categories in **Aster**, *evaluables* and *invokables*. We will spare some space to talk about them.
- *Evaluables* are any code segment that can be evaluated to a *value*. In **Aster**, evaluables includes *expressions* and *constraints*.
- *Invokables* are any code segment that can be followed by a `@seq`, i.e. it can be viewed as a function. In **Aster**, invokables includes *functions*, objects that overload `operator $`, and *templates*.

#### Evaluation of Expressions

In **Aster**, expressions are evaluated lazily, from left to right, following the precedence rules. The followings are some examples to explain the idea:

```cpp
1 + 2 + 3         // (1 + 2) + 3
1 + 2 * 3		  // 1 + (2 * 3)
foo 1 2           // (foo 1) 2
add 1 2 + 3       // ((add 1) 2) + 2 => (1 + 2) + 2
bar $ fst (1, 2)  // bar $ (fst (1, 2)) => bar $ 1 => bar 1
```

The precedence and associativity of operators differ. The following is a complete table of operators in **Aster**:

| Operator Name or Category                    | Symbol       | Precedence | Associativity | Overloadable       |
| -------------------------------------------- | ------------ | ---------- | ------------- | ------------------ |
| Implicit Function Application (Infixed Only) | space        | 10         | Left          | -                  |
| Function Composition                         | `<.>`        | 20         | Right         | -                  |
| Subscript Operation                          | `[]`         | 30         | -             | :heavy_check_mark: |
| Prefixed Unary Operation                     | e.g. `&`     | 40         | -             | -                  |
| Suffixed Unary Operation                     | e.g. `!`     | 50         | -             | -                  |
| Infixed Power                                | `^`          | 60         | Left          | :heavy_check_mark: |
| Infixed Multiplication/Division              | `*` and `/`  | 70         | Left          | :heavy_check_mark: |
| Infixed List Concatenation                   | `#`          | 80         | Right         | :heavy_check_mark: |
| Other Non-Assignment Built-in Infixed Op     | e.g. `+`     | 90         | Left          | :heavy_check_mark: |
| Infixed Function                             | e.g. `<add>` | 100        | Left          | :heavy_check_mark: |
| Assignment Operation                         | e.g. `=`     | 110        | Right         | :heavy_check_mark: |

Aside from these operations, we have other important symbols that have precedence. Special note that they are **not** operators, so they don't necessarily produce a value. Also, of course, none of them could be overloaded.

| Symbol Name or Category | Symbol       | Precedence | Associativity |
| ----------------------- | ------------ | ---------- | ------------- |
| Scope/Member Accessor   | `::` and `.` | 0          | Left          |
| Right Arrow             | `->`         | 970        | Right         |
| Class Combinator        | `&` and `|`  | 980        | Left          |
| All Other Symbols       |              | 990        | -             |

#### Operators

All the operators can be called like a function, which can take the form of `operator @` or `(@)`, where `@` is an the operator. Here are some examples:

```cpp
auto add_1 = operator +;
auto add_2 = (+);
auto res = operator + 1 2;
```

Here the reserved word `operator` is a built-in invokable that seems to take an operator and returns a function that execute the very operation. In fact, the `operator @` as a whole will be recognized as something like an identifier. The compiler will treat it as the prefix version of the operation. 

Like in **Haskell**, binary operators in **Aster** can be written in the form of `(@ x)` or `(x @)` , which are called the *operator sections*. Their definitions are as follows:

- `(@ x) = y -> y @ x` 
- `(x @) = y -> x @ y`, which is identical to the partial application of `@`, i.e. `(@) x`.

Functions can also be used like operators (i.e. be infixed), as long as they are enclosed by a pair of angle brackets `<>`:

```cpp
auto foo = a -> b -> a + b;
print $ a <foo> b
```

Operators can be overloaded if the operand types don't coincide with any built-in definition (when it's defined in the file scope). Other than unary operators, function composition operator and implicit function operator, all operators (in the table [here](# Evaluation of Expressions)) are overloadable. To declare a overloaded operator, just treat it as a function declaration:

```cpp
auto operator * : int -> [char] -> [char]
auto operator * = (n : int) -> (str : [char]) -> aux n str ""
    where aux 0 str res = res,
          aux n str res = aux (n - 1) str (str + res);
print $ 3 * "abc"
```

In **Aster**, we can define customized operators. They can be composed of (and only composed of) the following characters:

- `~@#$%^*-+=|<>/`: e.g. `<<+*`.
- `!&:.?`: These must appear inside a pair of angle brackets, e.g. `<!!!>`, `++<&><=:>`.

We can therefore create arbitrary sequence of symbols that represents an operator. An operator can only be bound to a two-parameter closure (yes there are counter cases, but they are all built-in operators). Since they are but special "identifier" that bound to entities, we call operators together with identifiers *general identifiers*. Later, when I mention "identifiers", operators will be included.

#### Invokable Objects

We already know that *closures* are invokable. There is another case where objects are invokable: when the object overloads the `operator $`.  Examples are as follows:

```cpp
auto Greeting = ();
auto operator $ : Greeting -> ();
auto operator $ _ = print "Hello, world!";
auto operator $ : Greeting -> [char] -> ();
auto operator $ _ msg = print msg;
auto gt : Greeting = ();
gt;
gt "Salutations!";
```

In **C++**, lambdas are just objects with hidden classes that overloads the `operator ()`; so are closures in **Aster**. The big difference is that **Aster** closures are explicitly typed. We can always obtain the type of an operator by the `typeof` operator.

```cpp
auto add : int -> int -> int;
auto add a b = a + b;
auto subtract : typeof add;
auto subtract a b = a - b;
```

#### Other Invokable Entities

