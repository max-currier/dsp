[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

# Visualizing PMFs and CDFs with random distribution
Create a Series of 1000 random numbers betwen 0-1

```python
rand_nums = pd.Series(np.random.random(size=1000))
```

Calculate PMF data

```python
rand_nums_pmf = rand_nums.value_counts() / rand_nums.count()
```

Visualize PMF

The line is straight and horizontal, indicating uniform distribution. 
```Python
plt.plot(rand_series_pmf)
plt.suptitle("PMD of np.random.random Values")
plt.xlabel("Value")
plt.ylabel("Frequency");
```

Calculate CDF data

```python
def num_to_cdf(value, array):
    count = 0.0
    for i in array:
        if i <= value:
            count += 1
            
    precentile_rank = count / len(array)
    
    return precentile_rank

cdf = dict()

for num in rand_nums:
    cdf[num] = num_to_cdf(num, rand_nums)
```

Visualize CDFs

The line is straight and diagonal (L->R, Down->Up), also indicating uniform distribution.
```python
plt.plot(list(cdf.keys()), list(cdf.values()))
plt.suptitle("CDF of np.random.random Values")
plt.xlabel("Value")
plt.ylabel("Cumulitive Probability");
```

