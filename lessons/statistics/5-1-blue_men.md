[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

# Modeling height using CDF

**Question: What percentage of men are between 5'10" and 6'1"?**

First, since the distribution of heights is roughly normal, to simplifiy things 
we can create a model of normal distribution using mean and STD that correspond 
to data about male height

```python
import scipy.stats

mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)
type(dist)
```

Then convert heights from in to cm to be consistent with model

```pythyon
def in_to_cm(inches):
    """
    inches: int or float, number of inches you want to convert to cm
    returns float of length in centimeters
    """
    cm = inches * 2.54
    return cm

min_height_in = (5 * 12) + 10
max_height_in = (6 * 12) + 1

min_height_cm = in_to_cm(min_height_in)
max_height_cm = in_to_cm(max_height_in)
```

Finally, calculate difference in cumulative probability of max height and 
cumulative probability of min height. 

```pythyon
target_heightrange_prob = dist.cdf(max_height_cm) - dist.cdf(min_height_cm)
```

This yields 0.34274683763147457, which means about **34% of men are in between 
these heights.**
