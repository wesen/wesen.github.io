---
layout: post
title: Maximum flow - minimum cut
category: articles
---

<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<script>
MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre']
      }
    });
    
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax(), i;
        for(i=0; i < all.length; i += 1) {
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });
</script>

The maximum flow / minimum cut is a graph problem 
that has a surprising amount of applications.
Some of them are non-obvious, but their formulation as
maximum flow problems are very elegant.

## Formulation of maximum flow

Imagine having an undirected graph $$$G$$$, with vertices $$$V$$$ and edges $$$E$$$.
Each edge has a capacity $$$C_E$$$

\\[\begin{aligned}
\dot{x} & = \sigma(y-x) \\\
\dot{y} & = \rho x – y – xz \\\
\dot{z} & = -\beta z + xy
\end{aligned}
\\]

## How I came upon max flow

I encountered maximum flow as part of the Algorithm II lecture
by Robert Sedgewick on coursera.
I love the programming assignment of that course, 
but they are usually limited to applying known algorithms
(often already implemented in the java library provided with the course).
This article is about my own exploration of maximum flow,
and a discussion of different algorithms to solve it.

While looking for more in-depth information about the problem,
I stumbled across the book "Algorithm Design" by Jon Kleinberg and
Eva Tardos, which seems to be a great book about applied algorithm design.
Every chapter consists of multiple design problems, 
and how they are solved.
Instead of just showing a problem, 
algorithms and data structures to solve it, 
and possible applications,
it goes deeply into the design of the algorithm, 
including possible dead-ends, and other challenges.