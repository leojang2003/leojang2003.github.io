---
layout: post
title:  "Compiler Theory"
date:   2019-07-26 10:28:00 +0800
categories: JavaScript
tags: You-Don't-Know-JS
---

In a traditional compiled-language process, a chunk of source code, your program, will undergo typically three steps before it is executed,
roughly called "compilation":

傳統上編譯語言執行前會經過三個步驟。
  
Tokenizing/Lexing: breaking up a string of characters into meaningful (to the language) chunks, called tokens. For instance, consider the program: var a = 2;. This program would likely be broken up into the following tokens: var, a, =, 2, and ;. 
Tokenizing/Lexing 將字串拆成有意義的區塊，叫做 token。舉例來說 var a = 2 會拆成 var, a, =, 2 與 ;。

Parsing: taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an "AST" (Abstract Syntax Tree).
Parsing 會將多個區塊轉為 Abstract Syntax Tree。

The tree for var a = 2; might start with a top-level node called VariableDeclaration, with a child node called Identifier (whose value is a), and another child called AssignmentExpression which itself has a child called NumericLiteral (whose value is 2).
以 var a = 2 的例子而言，樹的最上層是叫做 VariableDeclaration 的節點，分別有兩個子節點。第一個子節點為 Identifier，其值為 a，第二個子節點為 AssignmentExpression，該節點有一個子節點 NumericLiteral，其值為 2。

Code-Generation: the process of taking an AST and turning it into executable code. This part varies greatly depending on the language, the platform it's targeting, etc.
Code-Generation 將 AST 轉為可執行的程式碼 executable code




