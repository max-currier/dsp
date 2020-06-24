[Think Stats Chapter 7 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2008.html#toc70) (weight vs. age)

>># Determining correlation
>>
>>What is the correlation between a mother's age and their child's birth weight?
>>
>>First, let's review the data frame to find data that supports our inquiry. 
>>
>>```python
>>import first
>>import matplotlib.pyplot as plt
>>import pandas as pd
>>import numpy as np
>>import math
>>
>>live, firsts, others = first.MakeFrames()
>>live = live.dropna(subset=['agepreg', >>'totalwgt_lb'])
>>
>>pd.options.display.max_columns = None
>>pd.options.display.max_rows = None
>>
>>live.head(3)
>>```
>>
>>Now that we've found columns for age and weight, let's set variables for them. 
>>
>>```python
>>weight = live.totalwgt_lb
>>age = live.agepreg
>>```
>>
>>Plot a scatterplot to initially visualize correlation. 
>>
>>```python
>>plt.scatter(age, weight, alpha = 0.2)
>>plt.xlabel("Mother's age, yrs")
>>plt.ylabel("Baby weight, lbs");
>>```
>>
>>The scatterplot doesn't appear to show strong correlation, so let's try to plot percentiles to paint a more detailed picture. 
>>
>>Group the data into bins based on age ranges. I chose 1 yr per bin. 
>>
>>```python
>>bins = np.arange(15, 42, 1)
>>indices = np.digitize(age, bins)
>>groups = live.groupby(indices)
>>```
>>
>>We can take a look at size of each bin to help us determine how many bins to divide the data into. 
>>
>>```python
>>for i, group in groups:
>>print(i, len(group))
>>```
>>0 58  
>>1 126  
>>2 237  
>>3 393  
>>4 539  
>>5 557  
>>6 631  
>>7 636  
>>8 549  
>>9 588  
>>10 558  
>>11 508  
>>12 512  
>>13 485  
>>14 439  
>>15 392  
>>16 392  
>>17 336  
>>18 276  
>>19 218  
>>20 171  
>>21 133  
>>22 98  
>>23 82  
>>24 54  
>>25 34  
>>26 19  
>>27 17  
>> 
>>Now we'll use a list comprehension to make a list of mean age of each bin.
>>
>>```python
>>mean_age = [group.agepreg.mean() for i, group in groups]
>>```
>>
>>Then we can assign variable to CDF of binned data so that we can determine percentiles.
>>
>>```python
>>cdfs = [thinkstats2.Cdf(group.totalwgt_lb) for i, group in groups]
>>```
>>>>
>>Finally, we can iterate through a list of desired percentile ranks to calculate and plot weight values at each specified percentile rank in each age bin.
>>
>>```python
>>
>>for percent in [75, 50, 25]:
>>    weight_percentiles = [cdf.Percentile(percent) for cdf in cdfs]
>>    label = '%dth' % percent + " Percentile"
>>    thinkplot.Plot(mean_age, weight_percentiles, label=label)
>>    
>>thinkplot.Config(xlabel='Mother Age (yrs)',
>>                 ylabel='Baby Weight (lbs)',
>>                 axis=[15, 41, 5, 10],
>>                 legend=True)
>>```
>>
>>This plot doesn't show very strong correlation. Let's do some calculations to quantify the exact correlation. 
>>
>>We'll start with Pearson's correlation.
>>
>>```python
>>cov = np.dot(age-age.mean(), weight-weight.mean()) / len(age)
>>
>>p = cov / math.sqrt(age.var() * weight.var())
>>```
>>
>>p = 0.06882635429188806
>>
>>And then we can calculate Spearman's correlation.
>>
>>```python
>>age.corr(weight, method='spearman')
>>```
>>which comes to 0.09461004109658226.
>>
>>
>>**Bottom line**:  
>>
>>Based off the above plots and very low Pearson's and Spearman's correlation coefficients, we can determine that there is *very low correlation* between a mother's age and their child's birth weight. 
