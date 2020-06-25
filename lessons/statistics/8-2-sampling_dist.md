[Think Stats Chapter 8 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77) (scoring)

# Exponential sample distribution

**Simulate an experiment 1000 times where we draw a sample with size n=10 from an exponential distribution with lambda=2. Compute stardard error of the estimate and the 90% confidence interval.**

First let's take a look at the RMSE and mean error of the mean and median to see which measure of central tendency is more robust.

```python
def Estimate4(n=10, iters=1000):
    lam = 2

    means = []
    medians = []
    for _ in range(iters):
        xs = np.random.exponential(1.0/lam, n)
        L = 1 / np.mean(xs)
        Lm = np.log(2) / thinkstats2.Median(xs)
        means.append(L)
        medians.append(Lm)

    print('rmse L', RMSE(means, lam))
    print('rmse Lm', RMSE(medians, lam))
    print('mean error L', MeanError(means, lam))
    print('mean error Lm', MeanError(medians, lam))

Estimate4()
```
rmse L 0.8916641239189094
rmse Lm 1.8468958176827193
mean error L 0.26828513393086467
mean error Lm 0.7354311897380287

Mean has lower RMSE than median so we will use that. 

Now let's make a list of sample means for each sample in our experiment (1000 total). 

```python
def simulate_sample2(n, iters=1000):
    lam = 2

    means = []
    for _ in range(iters):
        xs = np.random.exponential(1.0/lam, n)
        L = 1 / np.mean(xs)
        means.append(L)
        
    return means

n10_means = simulate_sample2(10)
```

Now we can plot a CDF of the sample.

```python
cdf = thinkstats2.Cdf(n10_means)
thinkplot.Cdf(cdf)
thinkplot.Config(xlabel='Sample mean',
                 ylabel='CDF')
```

The mean of these sample means will stand in for our best approximation of the actual mean of the distribution (mu). 

```python
mu = np.mean(n10_means)
```

2.2116285967014293

Now we'll calculate 90% confidence interval. 

```python
ci = cdf.Percentile(5), cdf.Percentile(95)
ci
```

And finally we can calculate the Standard Error (SE). 

```python
stderr = RMSE(n10_means, np.mean(n10_means))
```

Lastly, let's repeat process for other sample sizes to examine the relationship between n and SE.

```python
import matplotlib.pyplot as plt


stderrs = []
n_range = range(5, 100, 5)

for n in n_range:
    means = simulate_sample2(n)
    
    cdf = thinkstats2.Cdf(means)
    
    stderr = RMSE(means, np.mean(means))

    stderrs.append(stderr)


plt.plot(n_range, stderrs)
plt.xlabel('Sample Size')
plt.ylabel('Standard Error');
```

We can see from this visualization that SE decreases as sample size increases, slowly approaching zero. 

