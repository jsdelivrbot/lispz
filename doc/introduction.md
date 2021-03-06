# Why another *&^% language?
**For Fun:**
It is fun to create something new - even when you are following paths well trodden by others for decades. By making your own decisions and learning from them you get a better understanding of the how and why of other languages.

**Extensibility:**
Few languages macros integrated in the language - where macros are expressed in the language itself. There is no difference between built-ins, libraries and code created by the end-user.

**Simplicity:**
Many languages and frameworks are overloaded with features - generating a huge learning curve.

# Overcoming the fear of change
Lispz has different expression ordering, lots of parentheses and function programming overtones. If you have a fear of change and, like me, had decades of OO and Imperative software development then Lispz looks strange, behaves strangely and requires a different way of thinking.

And yet, Lispz is only a thin veneer over JavaScript.

    Javascript: console.log("message:", msg)
    Lispz:      (console.log "message:" msg)

If you move the parenthenis pairs around and replace brace with parenthesis then the surface similarities become more obvious.

The first major difference is not what has been added, but what has been taken away. Lisp(z) has a lot less syntax. Only

    (fn params)
    [list]
    [[array]]
    {dict}

form the core syntax. Everything else is either a function or a macro. We won't talk more about macros yet - in case paranoia sets in.

# The benefits of lisp
Having only parenthesis, bracket or brace to deal with reduces ambiguity - when used with appropriate white-space. In many cases the functional style can be clearer:

    (or value default1 default2 12)
    (+ a b 12)

although not always

    (/ (* value percent) 100)

While our practiced eye finds this harder to understand than "a * percent / 100" it is easier to decipher. Take the 'standard' syntax. Are these the same:

    value * percent / 100
    (value * percent) / 100

You win if you said 'no'. In most languages operator precedence is such that the first sample will do the divide before the multiply. With real numbers the change in order can cause a different result. For languages without auto-conversion, the first will return zero (assuming percent is less than 100). With auto-conversion and all integers, the first will cause two floating point operations while the second only one.

Back to

    (/ (* value percent) 100)

With the understanding that everything appears to be a function, it becomes easier to read and there are no ambiguities. The word is 'appears' as Lispz expands binaries in-line, such that the code run as

    ((value * percent) / 100)

# Where functional programming comes in
Shhh! Don't tell the Haskellers. JavaScript is already a functional language in that it provides functions as first-class operators, closures and bindings. There are other aspects that it does not support - the major ones being immutability and static types. I think of JavaScript as a modern assembler, making it the responsibility of the higher (:) level language to fill in the gaps.

Lispz is way more function than JavaScript. Where JavaScript has statements, (almost) everything in Lispz returns a value. It is best to write Lispz as you would write Haskell where a lambda is one equation that does one thing. Use composition for larger tasks. With practice this becomes a powerful way to build up a system one step at a time. Modules and closures allow decomposition in a controlled manner.

Lispz is too lightweight to do anything about static types.

Immutability is a moving target. For a functional language, this means if a function is called with the same parameters it will always return the same result. Another definition states "no side-effects". A third suggest it means all data on the stack - meaning function call parameters. In the extreme it means that there are no variables, only constants - data once allocated never changes.

Lispz takes a pragmatic approach leaving it up to the developer. A reference (created with _ref_) can be shadowed by using the same name - but only in the same function. In an inner function the outer reference name can be accessed, but any shadowing only effects the immediate (inner) function. Because immutability is such a hard task master in an imperative world (such as the dom), Lisp does include a _stateful_ macro. Unlike assignment, _stateful_ is painful enough to remind the developer to limit it's use. Putting a bang after any exported function that includes mutability provides a good hint to rule-breaking. It is up to the developer to ensure than any function exported from a module honors the immutability and repeatability rules - and to flag the method (with a bang) when this is not possible.
