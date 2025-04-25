Lambda Calculus is a notion of function from computational perspective invented by
Alonzo Church. A function is black box - given inputs it produces an output.
It is a pure (mathematical function) with no internal state.

```scheme
; classic notation
; λx.x+1
; λx.λy.x+y
;(λx.x+1) 5

; scheme code
(λ (x) (+ x 1))
(λ (x y) (+ x y))
((λ (x) (+ x 1)) 5)
```

What is the point?
* Can encode any computation.
* Basis for functional programming languages.
* Now in most languages.

Lambda calculus has variables, a way of building functions, and a way of
applying functions. There are no built in data types, e.g. numbers and logic values.

How to encode logical values?
```scheme
; TRUE = λx.λy.x
; FALSE = λx.λy.y

(define TRUE
  (λ (x y) x))
(define FALSE
  (λ (x y) y))

; NOT = λb.b FALSE TRUE

(define NOT
  (λ (b) (b FALSE TRUE)))

; (NOT TRUE) => #<procedure:FALSE>
; (NOT FALSE) => #<procedure:TRUE>

; IF = λb.λx.λy.b x y

(define IF
  (λ (b x y) (b x y)))

(IF TRUE FALSE TRUE)
; (IF TRUE FALSE TRUE) => #<procedure:FALSE>

(IF FALSE TRUE FALSE)
; (IF FALSE TRUE FALSE) => #<procedure:FALSE>
```

Y combinator, invented by Haskell Curry, is the key to doing recursion in Lambda Calculus.

```scheme
#lang racket

; Y=λf.(λx.f (x x)) (λx.f (x x))
; (Here using the standard notations and conventions of lambda calculus: Y is a function that takes one argument
; f and returns the entire expression following the first period;
; the expression λx.f (x x) denotes a function that takes one argument x, thought of as a function,
; and returns the expression f (x x), where (x x) denotes x applied to itself.
; Juxtaposition of expressions denotes function application,
; is left-associative, and has higher precedence than the period.)

(define Y
  (λ (f)
    ((λ (x) (f (λ (v) ((x x) v))))
     (λ (x) (f (λ (v) ((x x) v)))))))

#|
The following is the classic untyped Y combinator:
```scheme
(define Y
  (λ (f)
    ((λ (x) (f (x x)))
     (λ (x) (f (x x))))))
```
This version doesn’t work directly in Scheme without adjustments — because Scheme is a strict (eager) language, not lazy. That means `(x x)` is evaluated immediately, which causes infinite recursion or a stack overflow, even before f gets a chance to run!

In lazy languages like Haskell, this form works fine because the evaluation of (x x) can be delayed.

In Scheme, you need to delay the recursive self-call so it doesn't blow up immediately. That's why we use the Z combinator or the "applicative-order Y combinator", which wraps the recursive call inside a lambda.
```
(define Y
  (lambda (f)
    ((lambda (x) (f (lambda (v) ((x x) v))))
     (lambda (x) (f (lambda (v) ((x x) v)))))))

(define fact-gen
  (lambda (f)
    (lambda (n)
      (if (= n 0)
          1
          (* n (f (- n 1)))))))

((Y fact-gen) 5)
```

Source:
* Lambda Calculus - Computerphile https://www.youtube.com/watch?v=eis11j_iGMs

A combinator is a function with no free variables. The fixed point of a function is that value when applied to it results in itself.
Basically, if you call a function f with fixpoint, the output should be equal to fixpoint.

```
f(fixpoint) = fixpoint
```

Y combinator works with any non-explicitly recursive function which takes a single argument.

Y is the applicative order fixpoint operator for functionals. It is one of the most interesting, most beautiful, and weiredest ideas.

source:
* https://lucasfcosta.com/2018/05/20/Y-The-Most-Beautiful-Idea-in-Computer-Science.html

