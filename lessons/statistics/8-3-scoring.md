[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)

---
# Estimating scoring rate

The following function that takes a goal-scoring rate, lam, in goals per game, and simulates a game by generating the time between goals until the total time exceeds 1 game, then returns the number of goals scored. The output is the number of goals in one simulated game. 

```python
def SimulateGame(lam):
    """Simulates a game and returns the estimated goal-scoring rate.

    lam: actual goal scoring rate in goals per game
    """
    goals = 0
    t = 0
    while True:
        time_between_goals = random.expovariate(lam)
        t += time_between_goals
        if t > 1:
            break
        goals += 1

    # estimated goal-scoring rate is the actual number of goals scored
    L = goals
    return L


import math 
import numpy as np
```

This second function uses the previous function to simulate many games (```iters```), stores the estimates of ```lam```, then computes their mean error and RMSE.

```python
def exp_sample_rsme(lam, iters=1000):
    """Simulates n games and returns the RMSE based on estimated goal-scoring rate.

    lam: actual goal scoring rate in goals per game
    iters: number of games simulated
    """
    L_list = [] # list of scores from simulated games
    
    for i in range(iters):
        L = SimulateGame(lam) # simulates final score (aka rate) of one game at a time
        L_list.append(L) 
      
    mean_error = np.mean([(L - lam) for L in L_list])
    print("Mean error is ", mean_error)
    e2 = [(L - lam)**2 for L in L_list]
    mse = np.mean(e2)
    print("MSE is ", mse)
    rmse = math.sqrt(mse)
    print("RMSE is ", rmse)
    
    return rmse
```

Running ```exp_sample_rsme``` with ```lam = 4``` and ```iters``` set to the default 1000 yields the following results, the last int (RSME) being the output as an isolated float:

>Mean error is  -0.064  
MSE is  4.046  
RMSE is  2.0114671262538697

The non-zero mean error tells us that the sample is *slightly* biased toward preicting lower scores. 

The RSME of ~2 tells us that the estimated scores are different than the mean by about 2 goals in either direction on average.

---
