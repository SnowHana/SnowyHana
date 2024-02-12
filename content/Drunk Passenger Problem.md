---
title: "Drunk Passenger Problem : When alchy meets recursion"
tags: 
date: 2024-02-13
---
# Introduction


## Code Demo
Below is a very simple python code that prints a probability that 100th passenger taking his seat (ie. seat 100).

```
import numpy as np

  
  

def do_drunk(passenger: int, avail_seats: list):

if passenger in avail_seats:

# His seat is available.

avail_seats.remove(passenger)

# print(f"Passenger {passenger} took a seat {passenger}")

else:

# His seat is alr taken, choose a random seat

idx = np.random.randint(0, len(avail_seats))

seat = avail_seats.pop(idx)

# print(f"Passenger {passenger} took a seat {seat}")

# Choose random number

  

return avail_seats

  
  

def drunk():

# n = int(input("Enter how many passengers exist: "))

  

n = 100

  

avail_seats = list(range(1, n + 1))

  

# Passenger 1 takes a random seat

seat = np.random.randint(1, n + 1)

avail_seats.remove(seat)

# print(f"Passenger 1 took a seat {seat}")

  

for i in range(1, len(avail_seats)):

avail_seats = do_drunk(i + 1, avail_seats)

  

# Avail_seats will have only 1 remainng

return avail_seats[0]

  
  

iter = 10000

count = 0

for i in range(iter):

result = drunk()

if result == 100:

count += 1

  

print(f"Probability that 100th person taking his seat is {(count / iter) * 100 }%.")
```

Well...we got approximate 0.5. But how come?