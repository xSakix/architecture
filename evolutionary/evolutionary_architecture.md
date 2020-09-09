# Evolutionary architecture

Classic/Standard architecture is 'static'. Done at the beginning with the hope of reflecting future changes in requirements. But the requirements change. So should the architecture. So how to evolve the architecture to reflect new requirements?

## Fitness functions

Term used from evolutionary computing. Fitness functions guide the architecture to a specifically chosen architecture attribute or [metric](https://docs.google.com/spreadsheets/d/1nnJvPuQN8SKYJr9bJz1Hl8mLzDYKCofdvec8qntxhGQ/edit?usp=sharing). One can imagine them as a unit test, that tests wheter some modules are coupled or not. If it finds out they are coupled, then an architecture metric called coupling isn't satisfied. 

So each metric which is important for our architecture has one or more fitness functions which guide this specific goal.


## In my words

So you start with some architecture which is aligned with your metrics. And to check whether those metric hold on each build/deploy/whatever you make 'fitness' function which either validate it, or enumerate at some degree. 
So if you want to change the metric/metrics because of some new requirements in some way you can do it this way by changing the fitness functions. 

## Source

* [Building Evolutionary Architectures: Support Constant Change](https://www.goodreads.com/en/book/show/35755822)
