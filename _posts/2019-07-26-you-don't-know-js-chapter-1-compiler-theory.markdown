---
layout: post
title:  "Compiler Theory"
date:   2019-07-26 10:28:00 +0800
categories: JavaScript
tags: You-Don't-Know-JS
---
<h3>0670</h3>
<main>
<div>
{% highlight html linenos %}  
In a traditional compiled-language process, a chunk of source code, your program, will undergo typically three steps before it is executed,
roughly called "compilation":
傳統上編譯語言執行前會經過三個步驟
  
Tokenizing/Lexing: breaking up a string of characters into meaningful (to the language) chunks, called tokens. For instance, consider the program: var a = 2;. This program would likely be broken up into the following tokens: var, a, =, 2, and ;. Whitespace may or may not be persisted as a token, depending on whether it's meaningful or not.

Note: The difference between tokenizing and lexing is subtle and academic, but it centers on whether or not these tokens are identified in a stateless or stateful way. Put simply, if the tokenizer were to invoke stateful parsing rules to figure out whether a should be considered a distinct token or just part of another token, that would be lexing.

Parsing: taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an "AST" (Abstract Syntax Tree).

The tree for var a = 2; might start with a top-level node called VariableDeclaration, with a child node called Identifier (whose value is a), and another child called AssignmentExpression which itself has a child called NumericLiteral (whose value is 2).

Code-Generation: the process of taking an AST and turning it into executable code. This part varies greatly depending on the language, the platform it's targeting, etc.
{% endhighlight html %}  
</div>
  
<div>

</div>

</main>

