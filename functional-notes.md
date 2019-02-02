# Functional programming general concepts
- Functions are first class citizens and the core module of reuse not classes and objects
- immutability is desired and we focus NOT mutating state where ever it is possible.

- most functions are 'Pure'
    - a pure function does NOT have any side effects
    - side effects can be any of the following:
        - mutating any state that is not local to the function
        - mutating arguments
        - throw exceptions
        - performing IO
        - anything that affects non-local scope

    - a pure function is therefore a function which in this sense does not ever have side effects and depend only and entirely on their arguments

# Functions Terminology
- Arity - the number of arguments a function has
- Special names for different arities:
    - Nullary - no arguments
    - Unary - one argument
    - Binary - 2 args
    - Ternary - 3 args
    - etc...

- All functions can be represented as a unary function which takes a tuple containing all arguments

- Higher order functions
    - higher order functions are functions that take functions as arguments or return them as return values
    - HOF can be used to reduce code duplication

# Functional programming benefits
    - cleaner code which is easier to maintain
    - Better support for concurrency

    - Pure functions can easily be:
        - reasoned about
        - ran in parallel
        - have cached results(memoization)
        - lazily evaluated when needed instead of automatically
        - trivially tested
    
# Function signature notations

Arrow notation is generally used to represent a function's signature
a function f ( int x, int y) that returns a string, I would represent it like:

f: int -> int -> string

the function f could be written in pseudo code as 

f(x) {
    return g(y) { return "";}
}

Functions should have a signature have signatures which tell you what they do exactly in non-ambiguous terms. You can use a type which represents a specific kind of input in order to do that:

f: int -> string
vs
f: PersonAge -> InsuranceRisk

ideally a function should describe a complete mapping of a domain to a range and function signature should tell the programmer about everything they need to know.

# Functions should always return values
functions should return a value even if there's no reason to.

If you ave nothing to actually return a "Unit" value, that is a domain which has only one possible value.

"Unit" is used as a return type in FP where other return types don't make sense.
void is not a return type it is effectively a useless "I give you nothing as an answer"

Instead use a value to represent unit (like an empty tuple) and return unit instead of void.

a 'void returning' function is simply 100% side cause/effect 

# functions can be divided into Total and Partial functions
- Total functions map exactly one input to exactly one output for all it's possible input values
- partial functions map some inputs to an output.

# partial functions can be represented with options
An option can be 'some' or 'none' where if there is a value it is considered some and no value is considered none and is an alternative to using null for the absence of value.

Typed as:
Option<T> Some | None

option is simply container for a value not a value itself, an abstraction that wraps the effect of absence or presence of a value.

Calling code then just needs to call functions which transform or operate on the container and/or its inner value (T) and needs to execute code depending on the absence or presence of a value.

our first function on an option will be 'match' and returns a value from the inner value

match: option<T> -> ( () -> R ) -> (T->R) -> R

match( none:()=> "", some: t => "yup it has a value")

options map perfectly to partial functions

# Functors
a container abstraction for which a HOF called map with the following signature can be defined in a 'reasonable way' and with no side effects:

map: (C<T>, (T->R)) -> C<R> , this .Select() from C# linq

if such a map function can be reasonably defined for a type C<T> then it is a functor

# Monads
if we have a Monadic value M<T> then these two functions can be defined:

return: T -> M<T> , aka lift
bind: (M<T>, (T->M<R>)) -> M<R> , .SelectMany() from C# linq

if a bind and return function can be implemented for a type C<T> then it is called a Monad

# Regular vs Elevated values
Types can then be divided into regular and elevated values, so called "Worlds of types"

"Worlds of types are just types and some are abstractions that use a type as an "inner value"

+== Regular | Abstraction/Elevated|
+-- T       | A<T>                |
+-- string  | IEnumerable<string> | 
+-- int     | Option<int>         |

abstractions add effects to a type

Option adds the effect of possibility of a T 
IEnumerable adds the effect of aggregation of Ts
Func<T> adds the effect of lazy computation in obtaining a T
Task<T> adds the effect of asyncrony in obtaining a T