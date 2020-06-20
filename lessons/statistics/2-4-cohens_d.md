[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

# Are first babies lighter/heavier?

After reading in the `preg` data, the first we need to establish the variables 
by filtering the for successful birth outcomes.

```python
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]
```
Then we establish variables for our `firsts` and `others` groups

```python
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]
```

After which we find the mean `totalwgt_lb` of each groups

```python 
firsts_mean_weight = firsts.totalwgt_lb.mean()
```
= 7.201094430437772

```python 
others_mean_weight = others.totalwgt_lb.mean()
```
= 7.325855614973262

The difference is < 0.2 lbs

To check the effect size using Cohen's d
```python
d = (firsts_mean_weight - others_mean_weight) / np.std(live.totalwgt_lb)
```
d = -0.08859523385261187

This d is very small, meaning **birth order does not have a significant impact
on birth weight.**

Birth order has slightly greater impact on birth weight than it does on
pregnancy length (d = 0.029), but *neither result is clinically significant*. 
