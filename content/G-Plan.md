---
title: G-Plan
tags:
  - project
---

[[Gplan w1]]

# Motivation
Yep...

---
# How it started...

---
## 1. No requirement at all?
This is, the *ideal* case for the schedule builder, because he doesn't have to consider anything.
This case can be resolved extremely easily by simply allocating Day, Night, Off, Break to every worker.
### 1 - 1. Test
Python test cases...

## Constraint.
### Hard constraint
1. For every day and night shift, at least one worker is needed. (Assume that we already have more than minimum number of workers required.)
2. A person can't have 2 shifts on a same day. (Day shift and Night shift on a same day is impossible)
3. After Night shift, a person should take a day off, and a break. (So N - O - B is fixed)
### Soft constraint
1. Preferably, at least 2 workers for a night shift.
2. Maximise similarlity with preference.

## 2. Preferences
Assume we have $N$ workers, and each worker fill in 28 slots which marks their preference, by D N O B F(Fill). 
Basic feasible solution would be cyclic D N O B...

We can adapt neighbour search algorithm discussed.

A, B can be excluded.

