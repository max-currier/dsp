[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

# Biased vs Unbiased Data
## Calculations
1. Import modules and read Data

```python
from matplotlib import pyplot as plt
import pandas as pd

resp = nsfg.ReadFemResp()
```

2. Examine distribution of unbiased data

* Unbiased histogram

```python
unbiased_hist = resp.numkdhh.value_counts().sort_index()

plt.hist(resp.numkdhh, align= 'left')
plt.xlabel("Number of children per family")
plt.ylabel("Freq")
plt.suptitle("Unbiased Child Count");
```
* Unbiased PMF

```python
unbiased_pmf = unbiased_hist / resp.numkdhh.count()

plt.plot(unbiased_pmf)
plt.suptitle('PMF Children/Family')
plt.xlabel("Number of children in family")
plt.ylabel("Probability");
plt.suptitle("Unbiased Child Count");
```

3. Examine distribution of biased data

* Biased PMF

```python
def biased_pmf_maker(unbiased_pmf_series):
    """
    unbiased_pmf_series: pd.Series of unbiased PMF 
    
    Returns pd.Series of normalized biased PFM
    """
    biased_pmf_series = pd.Series(unbiased_pmf_series)
    for idx, prob in biased_pmf_series.iteritems():
        new_prob = prob * idx
        biased_pmf_series[idx] = new_prob
    norm = biased_pmf_series / biased_pmf_series.sum()
    return norm

biased_pmf = biased_pmf_maker(unbiased_pmf)
if biased_pmf.sum() == 1:
    print(biased_pmf)

plt.plot(biased_pmf)
plt.xlabel("Number of children in family")
plt.ylabel("Probability");
plt.suptitle("Biased Child Count");

```

4. Plot difference between bias and unbiased biased PMF

```python
bias_diff = (biased_pmf - unbiased_pmf) * 100
plt.bar(bias_diff.index, bias_diff)
plt.suptitle("Biased vs. Unbiased Child Count")
plt.xlabel("Number of children in family")
plt.ylabel("Difference in probability (%)");
```

5. Calculate mean of biased and unbiased biased PMFs

```python
def pmf_mean(pmf_series):
    """
    pmf_series: pd.Series of PMF
    
    Returns flot of mean value from PMF
    """
    mean = float(0)
    for idx, prob in pmf_series.iteritems():
        mean += (idx * prob)
    return mean
```
## Results
```python
print("The unbiased mean # children/family is", pmf_mean(unbiased_pmf))
print("The biased mean # children/family is", pmf_mean(biased_pmf))
print("\nThis demonstrates that if you only ask children how many chilren/family,")
print("the data will show that there are twice as many children. ")
```

The unbiased mean # children/family is 1.024205155043831
The biased mean # children/family is 2.403679100664282

This demonstrates that if you only ask children how many chilren/family,
the data will show that there are twice as many children. 

