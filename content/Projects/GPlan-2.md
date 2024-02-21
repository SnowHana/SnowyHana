---
title: "GPlan : Random Swap Algorithm"
tags:
  - project
  - gplan
---


[[Gplan - 1]]
# Issues...
So random swap algorithm has issues. Most obvious one right now is that our final solution can be not feasible!

![[Screenshot 2024-02-20 at 4.54.21 PM.png]]
As you can see, on day 2, there is no worker assigned for a Day shift. This can be fixed in following ways.
1. Discard *swap* if it breaks our requirement (ie. Breaking min 1 worker per shift rule.)
2. Instead of horizontal swap, swap between workers (Vertical Swap).
3. Add an auto-correction functionality

#### Soln 1. Discard swap if it breaks our req.
This is the most obvious, and easiest solution, but it can be problematic. We might be stuck, and this significantly reduces our *searching space* as well....
##### Possible Issues
This might severely reduce our *searching space*

#### Soln 2. Instead of vertical swap, swap between workers.
This approach is the approach that will guarantee our result always being feasible, most-balanced, because initially, every shift will have approximately equal number of workers.
##### Possible Issues
- Optimal shift might not have equal number of workers for every shift. 
	- ie) If it is *better* to have a single worker on a specific shift and have three workers on other shift, then we should choose that way.
#### Soln 3. Auto-correction functionality.



---
### How about all 3?
1. Implement a vertical random swap 
	- Randomly choose 2 worker (Azir, Kaisa)
	- Swap them if two are different
2. Horizontal Swap
	- Discard the swap if it breaks our hard req (at least one worker for every shift)

This is better algorithm because it is (finally) guaranteed to produce us a valid shift. It also fixes our concern of adopting soln 1 (Limiting a searching space) up to some point, thanks to step 1. There still is an underlying issue that our searching space might be limited.

Plus, we can increase the searching space dramatically by instead of discarding the swap if it worsens the weight, we can store them in a temporary list, and after every $n$ th iteration, only choose the *best* few shifts, and resume on them.
Also, instead of manually setting the number of iteration, we can automatically decide when to stop, by setting a threshold rate.

----
# To - Do
1. Implement a vertical swap function
2. Modify horizontal swap (Instead of iterating through workers list and using swap on every worker, randomly choose a worker!)
3. Implement an automatic threshold functionality....