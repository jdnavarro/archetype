# Archetype IDL

Archetype is another attempt at IDL (interface description/definition language)
inspired by Dhall configuration language.  Dhall is used to represent AST
(Abstract Syntax Tree) generated by Archetype compiler.

Main ideas:

* Protocol/interface is described using types only.  RPC/REST calls are just
  types too.
* Dhall used as an intermediate language, and it can be used as a kind of macro
  language in Archetype itself.
* Code generators are explicit; there is no default code generator.  This is to
  prevent implementation to be tied to a specific representation too much, and
  it also encourages users to think of code generators as something they can
  provide them selves.


# Logo

TODO:
```
arch:Type
```

# Architecture

In principle Archetype compiler `ark` does this:

```
                                          ┌────────────────────┐
                ┌─────────┐              ┌────────────────────┐│
 Intefrace      │         │    Dhall     │                    ││        Native
description ───>│   ark   │─────────────>│   code generator   ││─────>   code
(Archetype)     │         │   via pipe   │                    │┘   (.hs, .psc, etc.)
                └─────────┘              └────────────────────┘
```

However, the implementation of code generator should behave as a pure function,
or at least as much as one can be approximated.  This allows us to safely
examine the output of code generator.

```
       Archetype
           │
           │
┌──────────│──────────┐
│ ark      ▼          │
│   ┌─────────────┐   │
│   │             │   │
│   │    front    │   │
│   │     end     │   │
│   │             │   │
│   └─────────────┘   │
│          │          │
│     AST  │          │
│          ▼          │
│   ┌─────────────┐   │  AST (Dhall)   ┌─────────────┐
│   │             │───────────────────>│             │
│   │  back end   │   │                │    code     │
│   │  executor   │   │  Files (Dhall) │  generator  │
│   │             │<───────────────────│             │
│   └─────────────┘   │                └─────────────┘
│          │          │
└──────────│──────────┘
           │
           ▼
       Generated
         Files
```
