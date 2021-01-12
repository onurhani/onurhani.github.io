---
layout: post
title: "Have you heard about the Collatz Conjecture?"
excerpt: "..and how to create the Collatz the sequence with any initial value using Python?"
categories: [python]
comments: true
---

Hello, world! It took couple of weeks to recover from cumulative stress of thesis writing and defending but I am finally blogging! The first post is going to be hopefully a good example of how I want to associate programming or data science concepts that I am advancing with real life problems. During the time I spent on my thesis project, I used Python almost everyday. Honestly,  I've never followed a curriculum to learn this general-purpose programming language. I used Python whenever I need to solve a problem that can be quickly solved with this great language. I've used scikit-learn for desigining and running ML models on a Jupyter Notebook for my thesis research and ad-hoc analysis in Casper, but never programmed complex functions to optimise my analysis processes. Especially after thesis studies I've decided to practice more Python to become more fluent with the language. Therefore, I googled most appreciated books for learning Python for all skill levels and found [Automate the Boring Stuff with Python](https://www.amazon.com/Automate-Boring-Stuff-Python-Programming/dp/1593275994). As humorous cover and title reveal, book promises applications of Python to solve real life problems. For now, I'm walking through first chapters and each chapter is understandible for ultimate beginners. Here is an example from first chapters: Creating a Collatz sequence. Let's remember one of the most popular unsolved problems of maths[^1]]:

> The **Collatz sequence**, also called the **Hailstone sequence**, is a sequence of numbers relevant to the [Collatz conjecture](http://en.wikipedia.org/wiki/Collatz_sequence), which theorizes that any number using this algorithm will eventually be reduced to 1. The conjecture's truth is supported by calculations, but it hasn't yet been proved that no number can indefinitely stay above 1.

Based on the definition of Collatz cojecture, here is the pseudocode of how Collatz sequence is formed:

```pseudocode
 function collatz(n)
 while n > 1
   show n
   if n is odd then
     set n = 3n + 1
   else
     set n = n / 2
   endif
 endwhile
 show n
```

Briefly, we start with a number. If the number is even, it is divided by two. If it is odd, then we multiply it by three and add one to obtain the new value. We keep doing the same operation until the number becomes one. If you try to prove this phenomena with different numbers without today's computing tech, that would be a quite time-consuming initiative. Below Python function puts the above pseudocode for creating a Collatz sequence into practice. Enter any number to the console and see how sequence ends with the same value no mather how you initialize it.

<iframe src="https://trinket.io/embed/python/01f7884c66?start=result" width="100%" height="356" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

It's one of the simplest examples from book. There are many similar exercises with increasing complexity. I also recommend you to check [this](https://automatetheboringstuff.com) website dedicated to the book. I'm going to reference more Python practices from this book in the upcoming days. Please don't hesitate to share your favourite resources for learning Python by then!

[^1]: Esolang: Collatz Sequence https://esolangs.org/wiki/Collatz_sequence
