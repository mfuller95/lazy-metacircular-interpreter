Lazy Metacircular Interpreter
===================
In this project, I modified the **metacircular interpreter** from SICP, available <a href="http://mitpress.mit.edu/sicp/code/ch4-mceval.scm">here</a>.
Working With The Interpreter
-------------------
Start `mit-scheme` with the metacircular interpreter loaded by executing the following command at the prompt (the prompt below is assumed to be `$`):

`mit-scheme --load lazymceval.scm`

There are two ways to test the evaluator:
<ol>
	<li>Execute `(driver-loop)` from the `mit-scheme` REPL. The driver loop will read in Scheme expressions and pass them to your interpreter for evaluation.</li>
	<li>Call the `eval` function with an expression and a global environment from the `mit-scheme` REPL. For example:</li>
</ol>

    (eval '(+ 2 3) the-global-environment)

Original Features
-------------------

See chapter 4 in <a href="https://sicpebook.files.wordpress.com/2011/06/sicp.pdf">SICP</a>.

Extending Features
-------------------

 1. **Adding Primitives**
	- Add the following primitive operations:  `+`, `*`, `-`, `/`, `<`, `<=`, `=`, `>=`, `>`, and `error`.
	- The `error` primitive takes no arguments and abort the interpreter with the message "Metacircular Interpreter Aborted".
 2. **Implementing `and` and `or`**
	- **`and` expression: ` (and <test> ...) `**:
The `<test>` expressions are evaluated from left to right, and the value of the first expression that evaluates to a false value is returned. Any remaining expressions are not evaluated. If all the expressions evaluate to true values, the value of the last expression is returned. If there are no expressions then #t is returned.
Examples: 

		```
		(and (= 2 2) (> 2 1))                   ===>  #t
		(and (= 2 2) (< 2 1))                   ===>  #f
		(and 1 2 'c '(f g))                     ===>  (f g)
		(and)                                   ===>  #t
		```
	- **`or` expression: `(or <test> ...) `**:
The <test> expressions are evaluated from left to right, and the value of the first expression that evaluates to a true value (see section 6.3.1) is returned. Any remaining expressions are not evaluated. If all expressions evaluate to false values, the value of the last expression is returned. If there are no expressions then #f is returned.
Examples:

		```
		(or (= 2 2) (> 2 1))                    ===>  #t
		(or (= 2 2) (< 2 1))                    ===>  #t
		(or #f #f #f)         ===>  #f
		(or (memq 'b '(a b c)) 
		    (/ 3 0))                            ===>  (b c)
		```

 3. **Implementing `let`**
	- This is exercise 4.6 in <a href="http://mitpress.mit.edu/sicp/code/ch4-mceval.scm">SICP</a>.
    - `let` expression is derived expression, because:
	```
	(let ((<var1> <exp1>) ... (<varn> <expn>))
	    <body>) 
	```  
		is equivalent to
        ```
		((lambda (<var1> ... <varn>) 
			<body>)
		<exp1>
		...
		<expn>)
        ```
     
- Implemented a syntactic transformation `let->combination` that reduces evaluating `let` expressions to evaluating combinations of the type shown above, and added the appropriate clause to `eval` to handle `let` expressions.

 5. **Make the evaluator Lazy**
	- Stratergy: Use thunks to implement call-by-need and applicative-order-evaluation `force` and `delay`.
	- `force` is implemented as a special form.
	- `thunk` and `evaluated-thunk` tags are introduced to support `force` and `delay`. Since thunks, whether or not they are evaluated, can only legally appear as arguments to `force`. 

 6. **Implementing Streams**
Add suport for the following stream functions to the interpreter, also uses call-by-need implementation of `force` and `delay`, as oppose to call-by-name version of `force` and `delay` in the <a href="http://mitpress.mit.edu/sicp/code/ch4-mceval.scm">SICP</a>.
	- `con-stream`: similar to `cons` in list, accepts any two values and joins them into a pair.
	- `the-empty-stream`: similar to `empty` in list, it returns an empty stream.
	-  `stream-null?`: return true if the stream is empty, false otherwise.
	- `stream-car`: similar to `car` in list, it returns the first element of the stream.
	- `stream-cdr`: similar to `cdr` in list, it returns the original stream without the first element.
