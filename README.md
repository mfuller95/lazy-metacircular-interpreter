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
	- **a. `and` expression: ` (and <test> ...) `**:
The `<test>` expressions are evaluated from left to right, and the value of the first expression that evaluates to a false value is returned. Any remaining expressions are not evaluated. If all the expressions evaluate to true values, the value of the last expression is returned. If there are no expressions then #t is returned.
Examples: 
