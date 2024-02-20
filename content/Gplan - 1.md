---
title: 1. Introduction
date: 2023-02-20
---
- Make a weight function (To measure how *optimal* our schedule is)
- Make a graph to visualise the weight function

Basically, make a function that can evaluate our algorithm. 
Then, next week we can test different algorithms.

# Introduction
Initially, allocating Day, Night, Off, Break shifts throughout workers so that maximum number of *requirements* are met seemed not so enormously challenging. 
"**Greedy algorithm** looks like a feasible solution?", I thought, which was obviously a wrong assumption.

----
## Greedy Algorithm Approach
Seemingly feasible algorithm for finding optimal shift schedule would be like this...
1. Write down how many requirements exist per worker.
2. Starting from a worker with maximum requirement, randomly choose and satisfy one of his requirement and move on to a worker with next-highest requirement. 
3. If meeting a certain requirement result in breaking our rule, discard the requirement (ie. go back) and choose some other requirement among his.
4. If we are stuck (ie. We can't satisfy any other requirement nor can we allocate any more shift because it breaks our core rule.), reset and start over.
### Issues
As you can see, this solution had number of issues
First of all, we are not guaranteed to find a working solution! Yes, finding a optimal solution is important, but more importantly, we (me, my boss and my coworkers) want a **working solution**. Above greedy algorithm doesn't guarantee a working solution.

Secondly, there is no proof that this algorithm propagates to a *better* direction. That is, we can't guarantee that as we iterate over our algorithm, it will find a better solution. 

Lastly, this algorithm relies heavily on randomness so we can't really prove that our solution found is optimal.

### Reality...
After some research, I found out that this problem is actually [[NP-Hard]].
This problem is a variation of well-known problem called Nurse Scheduling Problem (also known as NSP). 
Thus, I should probably give up on designing a beautiful algorithm that runs on a polynomial time and find an optimal solution. Rather, I read this article on google (Maximizing the nurses’ preferences in nurse scheduling problem:  
mathematical modeling and a meta-heuristic algorithm  
Hamed Jafari1 •Nasser Salmasi), and decided to adapt a random-swap method.

----

## Random-Swap Algorithm
1. Start with an obvious feasible solution, by allocating DNOB repeatedly starting from first worker.
2. Then, randomly swap two shifts between a worker (ie. Horizontal Swap)
3. After finishing swap, *fix* the shift whenever it's broken. 
	1. For example, D N O B D N O B was swapped to  D N N O B D O B , and it should be fixed to D N O B N O B D.
4. Calculate weight scores of before_swap, after_swap and discard the one with higher score.
5. Repeat this process for certain iteration.

### Benefits
This algorithm is better for few reasons.

First, it's **guaranteed** to work! This is because we start from a possible, most basic shift and fix it whenever it's broken. So no matter which shift we swap, it is guaranteed to work.