# MEML Syntax Standard

This provides implementation details for the meml syntax. For an introduction to creating a language please see [crafting interpreters](https://craftinginterpreters.com/). A large part of this language standard guide is based off this book.

## Language grammar

Again, the language grammer here is based off of that for lox. Here page is a program. It contains everything in a file.

```ts
// This is the entry point into the program (a single file).
//
// DISCUSSION: How should scripting work? Should it be JS
//             passthrough, should it be limited to inside
//             custom tags, should it be LISP or should it
//             be something more c-like (or python-like)
page        → statement* EOF;

// This will contain all of the major logic, for the moment
// it only accepts meml statements, but that will change once
// scripting is being planned
statement   → memlStmt;

// This is what a meml tag will look like. Note that there
// can be as many expressions as is
memlStmt    → '(' IDENTIFIER memlProp* exprOrMeml* ')';

// This is the layout for the properties of a meml tag. It
// can either be a key-value pair or a key pair (for example
// `disabled`)
memlProp    →  IDENTIFIER
            | IDENTIFIER '=' expression;

// Expression statements and meml can be used interchangeably
// within a tag
exprOrMeml  → memlStmt
            | expression;

// Doing math, boolean logic or combining strings and such
expression  → literal
            | unary
            | binary
            | grouping;

// Contains all of the raw data types.
literal     → NUMBER
            | STRING
            | 'true'
            | 'false'
            | 'null';

// Used to negate or invert a value
unary       → ('-' | '!') expression;

// This is your standard something + something or a boolean
// comparison. Please note the order of operations with
// * and / having priority over + and -
binary      → expression operator expression;

// Everything inside of the grouping takes precedence to
// everything outside
grouping    → '(' expression ')';

// All of the different operators that you should want
//
// DISCUSSION: Should we include === or does that just
//             need to die
operator    → "==" | "!=" | "<" | "<=" | ">" | ">="
            | "+"  | "-"  | "*" | "/" ;
```

## Appendix 1: Grammar reference

The grammar "pseudo-code" that is used in this document follows the basic structure defined below:

```
Identifier → What that identifier consists of
```

Here is a nice cheat sheet for all of the symbols:

- `→`: Separator
- `|`: Or, the item that comes first takes precedence
- `;`: End of this line
- `*`: None, one, or multiple
- `(...)`: Grouping, everything inside is calculated to form one answer outside

Additionally, here is a cheat sheet of types and tokens. Typescript types are used because they are the limitation we are bound with when working with web technologies.

- `NUMBER`
- `STRING`: Text
- `EOF`: End of file token
- `IDENTIFIER`: The name of a defined value, for example tag or variable. like a string, just for a compiler