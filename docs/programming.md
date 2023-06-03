# Programming

## Organizational Skills Beat Algorithmic Wizardry
[Link](https://prog21.dadgum.com/177.html)
When it comes to writing code, the number one most important skill is how to keep a tangle of features from collapsing under the weight of its own complexity.

## On optimizng code  
[Link](https://news.ycombinator.com/item?id=11042400)
I try to optimize my code around reducing state, coupling, complexity and code, in that order. I'm willing to add increased coupling if it makes my code more stateless. I'm willing to make it more complex if it reduces coupling. And I'm willing to duplicate code if it makes the code less complex. Only if it doesn't increase state, coupling or complexity do I dedup code.
The reason I put stateless code as the highest priority is it's the easiest to reason about. Stateless logic functions the same whether run normally, in parallel or distributed. It's the easiest to test, since it requires very little setup code. And it's the easiest to scale up, since you just run another copy of it. Once you introduce state, your life gets significantly harder.
