---
layout: post
title: Trying out factor
category: articles
---

I have been trying out a few new programming languages lately: Factor and Erlang.
I don't expect to be using them for any real programming, but I want to learn enough about them that some things will stick. 

I did the same last year for Haskell, and a few things managed to stick:

- how to separate the generic part of an algorithm from the concrete data structure it is working on,
- how static typing and haskell's type system allows you to easily formulate data structures
- how monads and combinators and types building upon those allow you to easily thread programs together

It also makes you realize that there is a concise and elegant world out there, outside of c++.

Of course, these are short lived insights, being able to "think" in a new programming paradigm takes writing a non-trivial program, which takes time. For now, I am happy with just picking for insights here and there.

## Factor

Factor is a concatenative programming language, where every word you define is a function, and you compose these functions by writing these next to each other. It is stack based, uses reverse polish notation like Forth, but allows you to define anonymous words using `[` and `]`. These anonymous words (called quotations) can be further composed using combinators. It is a very functional way of writing programs, and saves a lot on the stack manipulation words I knew from Forth.

The main lesson I took from my one day of playing with Factor is that you build up quotations (functions, really), and then finally apply them. For example, instead of having an if expression, and a true and false branch, you build up the quotation for the predicate, build up the quotation for the if branch, and for the false branch, and compose those.

Because you can easily extract every seemingly trivial piece of code out to create new functions, your program is decomposed into minimal atoms that provide insight into the underlying structure of what you are doing. The choice of naming and decomposition both is guided and guides you towards understanding your problem.

Here is a probably very naive attempt to search for the first value matching in a list, with my first attempt at writing things out explicitly, and my second and third attempts where I build up a predicate to check for equality of the first list element. This still minimally uses Factor's combinators.

This is me writing c++ in Factor.

	: is-first ( seq quot: ( elt -- ? ) -- ? ) [ first ] dip call ; inline
	: find-first ( seq quot: ( elt -- ? ) -- elt )
	    [ 2dup is-first ] ! check head
	    [ [ rest ] dip ] ! drop head
	    until
	    drop first ! return first element
	    ; inline

This is me cutting down on stack manipulation and creating a predicate quotation.

	: find-first2 ( seq quot: ( elt -- ? ) -- elt )
	    [ dup first ] prepose ! create pred for until
	    [ rest ]
	    until
	    first
	    ; inline

The marginally less naive version, which is much longer because it handles special cases:

	: check-first ( seq quot -- ? )
	    [ dup ] dip ! duplicate seq
	    [ first ] prepose ! turn quot into [ first quot ]
	    [ t ] swap
	    if-empty
	    ; inline
	
	: find-first3 ( seq quot: ( elt -- ? ) -- elt )
	    [ check-first ] curry ! create a predicate to check first or empty
	    [ rest ]
	    until
	    ?first
	    ; inline
