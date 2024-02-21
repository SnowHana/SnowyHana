---
title: 1. Introduction and basics
date: 2023-02-20
---
[[GPlan-2]]

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
2. Then, iterate through worker, and randomly swap two shifts between a single worker (ie. Horizontal Swap)
3. After finishing swap, *fix* the shift whenever it's broken. 
	1. For example, D N O B D N O B was swapped to  D N N O B D O B , and it should be fixed to D N O B N O B D.
4. Calculate weight scores of before_swap, after_swap and discard the one with higher score.
5. Repeat this process for certain iteration.
Note that, the fixing process is done after swap process is done for all the workers. 

### Code
```python
def swap_shift(base_shift: pd.DataFrame) -> pd.DataFrame:

"""Swap shifts between workers or swap one's shift with himself

to generate a *better* shift

  

Args:

shifts_df (pd.DataFrame): _description_

  

Returns:

pd.DataFrame: _description_

"""

df = base_shift.copy(deep=True)

rows, cols = df.shape


for worker in df.index.tolist():

mask_D = df.loc[worker] == "D"

mask_N = df.loc[worker] == "N"

  

# We want to randomly swap Day and Night

  

# Assumption: every person takes equal amount of night and day shift... (Number of night, day shift is fixed)

# Swap shifts. btw person (ie. Horizontal Random Swap between day and night shift)


day_data = get_shift_type_columns("D", df)

night_data = get_shift_type_columns("N", df)

# Iterate through workers

swapped_df = df.copy(deep=True)

  

for worker in swapped_df.index.tolist():

# Randomly swap two shifts

random_day = random.choice(day_data[worker])

random_night = random.choice(night_data[worker])

# D N O B -> N O B D

swapped_df = swap_two_shifts(

swapped_df,

random_day,

random_night,

worker,

)

# And then add O and B, except last days....  

# Not valid smh, N D , N N -> Push back everything insert O B?

swapped_df = fix_night_shifts(swapped_df)

assert is_valid_night(swapped_df), "Uhm night shift is still not valid,..."

assert is_valid_shift(

swapped_df

), f"Shift is not valid (at least one person for night and day shift\n {swapped_df}"
  

return swapped_df
```
### Benefits
This algorithm is better for few reasons.

First, it's **guaranteed** to work! ~~(Later on, I found out that this is not always true....)~~ This is because we start from a possible, most basic shift and fix it whenever it's broken. So no matter which shift we swap, it is guaranteed to work.

