# restaurant
🍽️ Restaurant Tips Analysis
<img width="1000" height="563" alt="image" src="https://github.com/user-attachments/assets/800df8a4-1e81-4cb7-a551-066b3432df32" />

This project aims to use the restaurant tips dataset to practice creating composition plots and visualizations. We will examine the relationship between different variables and the tips given.
The dataset consists of information from 244 restaurant bills, collected in the US in 1987.
It includes details about the tips given to restaurant staff, such as the total bill, tip amount, gender of the person paying, smoking status, day of the week, time of day, and party size.
## Import some important library to clean data
Import pandas library and matplotlib
```
import pandas as pd
import matplotlib.pyplot as plt
```
## Import restaurant source data 
```
df = pd.read_csv('https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv')
```
## Data exploration
### Checking sample
```
df.head(5)
```
Result: 
<img width="385" height="180" alt="image" src="https://github.com/user-attachments/assets/9d14e8e2-3260-46b3-afb6-96055b1f2d8d" />

### Checking column type and fix errors
```
df.info()
```
<img width="330" height="211" alt="image" src="https://github.com/user-attachments/assets/26a2ea3a-531f-42f0-8987-d63b1e52e73c" />

So that I need to convert some objects type to string
```
df = df.convert_dtypes()
df.info()
```
<img width="299" height="232" alt="image" src="https://github.com/user-attachments/assets/253435e6-d546-41db-b9b4-80d12ae7eced" />

### Analysis statistic data
```
df.describe()
```
<img width="329" height="282" alt="image" src="https://github.com/user-attachments/assets/34aa5217-f73d-45b3-99d3-f311e7d626d7" />


*After checking, exploring and summarizing data, this source need to analyse details which is influnce and some categories affect the data
💸 Tip value influencers
🚬 Do people who smoke give more tips?
Let's figure out the difference between smokers and non-smokers in terms of their behavior and purchasing habits in public catering establishments.

#Separate smokers and non-smokers
```
smokers_df = df[df['smoker']=='Yes']
non_smokers_df = df[df['smoker']=='No']
smokers_df.head(5)
non_smokers_df.head(4)
```

For Smokers:
<img width="387" height="181" alt="image" src="https://github.com/user-attachments/assets/d28b2f04-51a3-4e10-a061-9504418ee26c" />


For Non-smokers:
<img width="403" height="181" alt="image" src="https://github.com/user-attachments/assets/442f2221-63be-43df-bc1e-4939f668acbf" />
#Compare their measures of central tendency
As we know, measures of central tendency is one of the basic tools, that allow us to compare different datasets as it shows the most typical values.
1/ Tips
Calculate them for the 'tip' column through the whole dataset and save them into the following variables:
```
common_tip_min=df['tip'].min()
common_tip_max=df['tip'].max()
common_tip_mean=df['tip'].mean()
common_tip_median=df['tip'].median()
# Make a list of values
common_values = [common_tip_min, common_tip_max, common_tip_mean, common_tip_median]
# Round all the values to 4 decimal places
common_values = map(lambda x: round(x, 4), common_values)
# Make a dataframe from the list
common_mct = pd.DataFrame(common_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
common_mct
```
2/ 🚬 Smokers
```
smokers_tip_min=smokers_df['tip'].min()
smokers_tip_max= smokers_df['tip'].max()
smokers_tip_mean= smokers_df['tip'].mean()
smokers_tip_median= smokers_df['tip'].median()
smokers_values = [smokers_tip_min, smokers_tip_max, smokers_tip_mean, smokers_tip_median]
# Round all the values to 4 decimal places
smokers_values = map(lambda x: round(x, 4), smokers_values)
# Make a dataframe from the list
smokers_mct = pd.DataFrame(smokers_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
smokers_mct
```
3/ 🚭 Non-smokers
```
non_smokers_tip_min=non_smokers_df['tip'].min()
non_smokers_tip_max= non_smokers_df['tip'].max()
non_smokers_tip_mean= non_smokers_df['tip'].mean()
non_smokers_tip_median= non_smokers_df['tip'].median()
non_smokers_values = [non_smokers_tip_min, non_smokers_tip_max, non_smokers_tip_mean, non_smokers_tip_median]
# Round all the values to 4 decimal places
non_smokers_values = map(lambda x: round(x, 4), non_smokers_values)
# Make a dataframe from the list
non_smokers_mct = pd.DataFrame(non_smokers_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
non_smokers_mct
```
📝 Conclusion vs Compare 
```
all_vals_dict = {
    'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
    'Smokers': {'min': smokers_tip_min, 'max': smokers_tip_max, 'mean': smokers_tip_mean, 'median': smokers_tip_median},
    'Non-smokers': {'min': non_smokers_tip_min, 'max': non_smokers_tip_max, 'mean': non_smokers_tip_mean, 'median': non_smokers_tip_median}
}
# Make a dataframe
all_mct = pd.DataFrame(all_vals_dict)
# Output the dataframe
all_mct
```
<img width="302" height="151" alt="image" src="https://github.com/user-attachments/assets/dec55058-162c-432e-94c9-68d3ae45e829" />

Insights:

1/Range (spread of data)
Both smokers and non-smokers have the same minimum (1), but smokers can reach a maximum of 10, while non-smokers peak at 9.
Suggests smokers might give slightly higher extreme values.
2/ Average behavior
The mean values are very close (~3).
Smokers’ mean (3.01) is just a bit higher than non-smokers (2.99), but the difference is tiny → not statistically significant unless tested further.
3/ Median difference
Smokers’ median = 3.0, non-smokers’ median = 2.74.
This gap shows non-smokers tend to cluster slightly lower than smokers.
Suggests distribution may be slightly skewed.
4/Overall (Common)
Aggregated mean and median sit in between the two groups, as expected.

To more understand and get more details, the histogram is applied to make it clear
```
fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 1st chart: Whole dataset
axes[0].hist(df.tip, color='#74b9ff')
axes[0].set_xlabel('Tip value')
axes[0].set_ylabel('Frequency')
axes[0].set_title('Whole dataset tip values')
axes[0].grid(True)

# 2nd chart: Smokers
axes[1].hist(smokers_df.tip, color='#ff7675')
axes[1].set_xlabel('Tip value')
axes[1].set_ylabel('Frequency')
axes[1].set_title('Smokers tip values')
axes[1].grid(True)

# 3rd chart: Non-Smokers
axes[2].hist(non_smokers_df.tip, color='#55efc4')
axes[2].set_xlabel('Tip value')
axes[2].set_ylabel('Frequency')
axes[2].set_title('Non-Smokers tip values')
axes[2].grid(True)

plt.tight_layout()
plt.show()
```
<img width="906" height="260" alt="image" src="https://github.com/user-attachments/assets/c73ca6f9-2b42-4fcf-968c-b82d864bc009" />
Insight: Comparison Insights

Median gap confirmed: Smokers’ distribution shifts slightly higher (more around 3), while non-smokers lean lower (around 2).
Extreme values: Smokers are more likely to leave unusually high tips (≥7), which pulls their mean up slightly.
Consistency: Non-smokers show more clustering at lower values, suggesting less variability.
👉 Takeaway:
Smokers tend to give slightly higher tips and more extreme outliers, while non-smokers are more conservative and clustered at the lower end.
