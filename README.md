<h1 id="lazy-metacircular-interpreter">Lazy Metacircular Interpreter</h1>

<p>In this project, I modified the <strong>metacircular interpreter</strong> from SICP, available <a href="http://mitpress.mit.edu/sicp/code/ch4-mceval.scm">here</a>.</p>



<h2 id="working-with-the-interpreter">Working With The Interpreter</h2>

<p>Start <code>mit-scheme</code> with the metacircular interpreter loaded by executing the following command at the prompt (the prompt below is assumed to be <code>$</code>):</p>

<p><code>mit-scheme --load lazymceval.scm</code></p>

<p>There are two ways to test the evaluator:</p>

<ol>
    <li>Execute `(driver-loop)` from the `mit-scheme` REPL. The driver loop will read in Scheme expressions and pass them to your interpreter for evaluation.</li>
    <li>Call the `eval` function with an expression and a global environment from the `mit-scheme` REPL. For example:</li>
</ol>

<pre><code>(eval '(+ 2 3) the-global-environment)
</code></pre>



<h2 id="original-features">Original Features</h2>

<p>See chapter 4 in <a href="https://sicpebook.files.wordpress.com/2011/06/sicp.pdf">SICP</a>.</p>



<h2 id="extending-features">Extending Features</h2>

<ol>
<li><strong>Adding Primitives</strong> <br>
<ul><li>Add the following primitive operations:  <code>+</code>, <code>*</code>, <code>-</code>, <code>/</code>, <code>&lt;</code>, <code>&lt;=</code>, <code>=</code>, <code>&gt;=</code>, <code>&gt;</code>, and <code>error</code>.</li>
<li>The <code>error</code> primitive takes no arguments and abort the interpreter with the message “Metacircular Interpreter Aborted”.</li></ul></li>
<li><p><strong>Implementing <code>and</code> and <code>or</code></strong></p>

<ul><li><p><strong><code>and</code> expression: <code>(and &lt;test&gt; ...)</code></strong>: <br>
The <code>&lt;test&gt;</code> expressions are evaluated from left to right, and the value of the first expression that evaluates to a false value is returned. Any remaining expressions are not evaluated. If all the expressions evaluate to true values, the value of the last expression is returned. If there are no expressions then #t is returned. <br>
Examples: </p>

<pre class="prettyprint"><code class=" hljs php">(<span class="hljs-keyword">and</span> (= <span class="hljs-number">2</span> <span class="hljs-number">2</span>) (&gt; <span class="hljs-number">2</span> <span class="hljs-number">1</span>))                   ===&gt;  <span class="hljs-comment">#t</span>
(<span class="hljs-keyword">and</span> (= <span class="hljs-number">2</span> <span class="hljs-number">2</span>) (&lt; <span class="hljs-number">2</span> <span class="hljs-number">1</span>))                   ===&gt;  <span class="hljs-comment">#f</span>
(<span class="hljs-keyword">and</span> <span class="hljs-number">1</span> <span class="hljs-number">2</span> <span class="hljs-string">'c '</span>(f g))                     ===&gt;  (f g)
(<span class="hljs-keyword">and</span>)                                   ===&gt;  <span class="hljs-comment">#t</span></code></pre></li>
<li><p><strong><code>or</code> expression: <code>(or &lt;test&gt; ...)</code></strong>: <br>
The  expressions are evaluated from left to right, and the value of the first expression that evaluates to a true value (see section 6.3.1) is returned. Any remaining expressions are not evaluated. If all expressions evaluate to false values, the value of the last expression is returned. If there are no expressions then #f is returned. <br>
Examples:</p>

<pre class="prettyprint"><code class=" hljs php">(<span class="hljs-keyword">or</span> (= <span class="hljs-number">2</span> <span class="hljs-number">2</span>) (&gt; <span class="hljs-number">2</span> <span class="hljs-number">1</span>))                    ===&gt;  <span class="hljs-comment">#t</span>
(<span class="hljs-keyword">or</span> (= <span class="hljs-number">2</span> <span class="hljs-number">2</span>) (&lt; <span class="hljs-number">2</span> <span class="hljs-number">1</span>))                    ===&gt;  <span class="hljs-comment">#t</span>
(<span class="hljs-keyword">or</span> <span class="hljs-comment">#f #f #f)         ===&gt;  #f</span>
(<span class="hljs-keyword">or</span> (memq <span class="hljs-string">'b '</span>(a b c)) 
    (/ <span class="hljs-number">3</span> <span class="hljs-number">0</span>))                            ===&gt;  (b c)</code></pre></li></ul></li>
<li><p><strong>Implementing <code>let</code></strong></p>

<ul><li>This is exercise 4.6 in <a href="http://mitpress.mit.edu/sicp/code/ch4-mceval.scm">SICP</a>.</li>
<li><p><code>let</code> expression is derived expression, because:</p>

<pre class="prettyprint"><code class=" hljs r">(let ((&lt;var1&gt; &lt;exp1&gt;) <span class="hljs-keyword">...</span> (&lt;varn&gt; &lt;expn&gt;))
    &lt;body&gt;) </code></pre>

<p>is equivalent to</p>

<pre class="prettyprint"><code class=" hljs r">((lambda (&lt;var1&gt; <span class="hljs-keyword">...</span> &lt;varn&gt;) 
    &lt;body&gt;)
&lt;exp1&gt;
<span class="hljs-keyword">...</span>
&lt;expn&gt;)</code></pre></li>
<li><p>Implemented a syntactic transformation <code>let-&gt;combination</code> that reduces evaluating <code>let</code> expressions to evaluating combinations of the type shown above, and added the appropriate clause to <code>eval</code> to handle <code>let</code> expressions.</p></li></ul></li>
<li><p><strong>Make the evaluator Lazy</strong></p>

<ul><li>Stratergy: Use thunks to implement call-by-need and applicative-order-evaluation <code>force</code> and <code>delay</code>.</li>
<li><code>force</code> is implemented as a special form.</li>
<li><code>thunk</code> and <code>evaluated-thunk</code> tags are introduced to support <code>force</code> and <code>delay</code>. Since thunks, whether or not they are evaluated, can only legally appear as arguments to <code>force</code>. </li></ul></li>
<li><p><strong>Implementing Streams</strong> <br>
Add suport for the following stream functions to the interpreter, also uses call-by-need implementation of <code>force</code> and <code>delay</code>, as oppose to call-by-name version of <code>force</code> and <code>delay</code> in the <a href="http://mitpress.mit.edu/sicp/code/ch4-mceval.scm">SICP</a>.</p>

<ul><li><code>con-stream</code>: similar to <code>cons</code> in list, accepts any two values and joins them into a pair.</li>
<li><code>the-empty-stream</code>: similar to <code>empty</code> in list, it returns an empty stream.</li>
<li><code>stream-null?</code>: return true if the stream is empty, false otherwise.</li>
<li><code>stream-car</code>: similar to <code>car</code> in list, it returns the first element of the stream.</li>
<li><code>stream-cdr</code>: similar to <code>cdr</code> in list, it returns the original stream without the first element.</li></ul></li>
</ol>
