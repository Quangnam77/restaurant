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


#👨👩 Do males give more tips?
For this, I also do the same and get to details to compare males and females
Code: 
## Male
```
male_df = df[df['sex']=='Male']
male_tip_min=male_df['tip'].min()
male_tip_max= male_df['tip'].max()
male_tip_mean= male_df['tip'].mean()
male_tip_median= male_df['tip'].median()
# YOUR CODE
male_values = [male_tip_min, male_tip_max, male_tip_mean, male_tip_median]
# Round all the values to 4 decimal places
male_values = map(lambda x: round(x, 4), male_values)
# Make a dataframe from the list
male_mct = pd.DataFrame(male_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
male_mct
```

## Female
```
female_df = df[df['sex']=='Female']
female_tip_min=female_df['tip'].min()
female_tip_max= female_df['tip'].max()
female_tip_mean= female_df['tip'].mean()
female_tip_median= female_df['tip'].median()
# YOUR CODE
female_values = [female_tip_min, female_tip_max, female_tip_mean, female_tip_median]
# Round all the values to 4 decimal places
female_values = map(lambda x: round(x, 4), female_values)
# Make a dataframe from the list
female_mct = pd.DataFrame(female_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
female_mct
```

### Comparision data
```
all_vals_dict1 = {
    'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
    'Male': {'min': male_tip_min, 'max': male_tip_max, 'mean': male_tip_mean, 'median': male_tip_median},
    'Female': {'min': female_tip_min, 'max': female_tip_max, 'mean': female_tip_mean, 'median': female_tip_median}
}
# Make a dataframe
all_mct = pd.DataFrame(all_vals_dict1)
# Output the dataframe
all_mct
```
Result comparision
<img width="275" height="154" alt="image" src="https://github.com/user-attachments/assets/6285d273-5c8f-4d75-a9a7-b97bfbfd2a1f" />

Histogram distribution of male and female:
```
fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 1st chart: Whole dataset
axes[0].hist(df.tip, color='#74b9ff')
axes[0].set_xlabel('Tip value')
axes[0].set_ylabel('Frequency')
axes[0].set_title('Whole dataset tip values')
axes[0].grid(True)

# 2nd chart: Male
axes[1].hist(male_df.tip, color='#ff7675')
axes[1].set_xlabel('Tip value')
axes[1].set_ylabel('Frequency')
axes[1].set_title('Male tip values')
axes[1].grid(True)

# 3rd chart: Female
axes[2].hist(female_df.tip, color='#55efc4')
axes[2].set_xlabel('Tip value')
axes[2].set_ylabel('Frequency')
axes[2].set_title('Female tip values')
axes[2].grid(True)

plt.tight_layout()
plt.show()
```

Result
<img width="892" height="253" alt="image" src="https://github.com/user-attachments/assets/4b36a9be-761c-4645-8574-9eb47de0115b" />

Insight: 
Males: More likely to leave extreme high tips (≥7). Their tipping shows greater spread/variability.
Females: More consistent, most tips cluster below 4.
Central tendency: Both genders center around ~2, but males have a longer right tail (raising the mean slightly).
Interpretation: If you’re measuring average tip size, males may appear slightly higher due to outliers, but if looking at typical behavior (median), males and females are similar.

👉 Takeaway:
Male tippers → more variable, occasionally very generous.
Female tippers → more consistent, rarely extreme.

📆 Do weekends bring more tips?
For this, I also do the same and get to details to compare weekend and weekdays
Code:
```
df['weekend'] = df['day'].isin(['Sun','Sat']).map({True: 'Yes', False: 'No'})
df['weekend'].head(5)
```
Weekend details:
```
Weekend_df = df[df['weekend']=='Yes']

Weekend_tip_min=Weekend_df['tip'].min()
Weekend_tip_max= Weekend_df['tip'].max()
Weekend_tip_mean= Weekend_df['tip'].mean()
Weekend_tip_median= Weekend_df['tip'].median()
# YOUR CODE
Weekend_values = [Weekend_tip_min, Weekend_tip_max, Weekend_tip_mean, Weekend_tip_median]
# Round all the values to 4 decimal places
Weekend_values = map(lambda x: round(x, 4), Weekend_values)

# Make a dataframe from the list
Weekend_mct = pd.DataFrame(Weekend_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
Weekend_mct
```

Weekday details:
```
Weekday_df = df[df['weekend']=='No']
Weekday_tip_min=Weekday_df['tip'].min()
Weekday_tip_max= Weekday_df['tip'].max()
Weekday_tip_mean= Weekday_df['tip'].mean()
Weekday_tip_median= Weekday_df['tip'].median()
# YOUR CODE
Weekday_values = [Weekday_tip_min, Weekday_tip_max, Weekday_tip_mean, Weekday_tip_median]
# Round all the values to 4 decimal places
Weekday_values = map(lambda x: round(x, 4), Weekday_values)
# Make a dataframe from the list
Weekday_mct = pd.DataFrame(Weekday_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
Weekday_mct
```

Comparision data: 
```
all_vals_dict2 = {
    'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
    'Weekend': {'min': Weekend_tip_min, 'max': Weekend_tip_max, 'mean': Weekend_tip_mean, 'median': Weekend_tip_median},
    'Weekday': {'min': Weekday_tip_min, 'max': Weekday_tip_max, 'mean': Weekday_tip_mean, 'median': Weekday_tip_median}
}

# Make a dataframe
all_mct = pd.DataFrame(all_vals_dict2)
# Output the dataframe
all_mct
```

<img width="274" height="150" alt="image" src="https://github.com/user-attachments/assets/62e24df3-3484-43a4-90ed-ae7e8a71b262" />

Histogram to show the data distribution:
```
fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 1st chart: Whole dataset
axes[0].hist(df.tip, color='#74b9ff')
axes[0].set_xlabel('Tip value')
axes[0].set_ylabel('Frequency')
axes[0].set_title('Whole dataset tip values')
axes[0].grid(True)

# 2nd chart: Weekend
axes[1].hist(Weekend_df.tip, color='#ff7675')
axes[1].set_xlabel('Tip value')
axes[1].set_ylabel('Frequency')
axes[1].set_title('Weekend tip values')
axes[1].grid(True)

# 3rd chart: Weekday
axes[2].hist(Weekday_df.tip, color='#55efc4')
axes[2].set_xlabel('Tip value')
axes[2].set_ylabel('Frequency')
axes[2].set_title('Weekday tip values')
axes[2].grid(True)

plt.tight_layout()
plt.show()
```
<img width="909" height="249" alt="image" src="https://github.com/user-attachments/assets/4fb1991d-778c-4d7d-8523-f1f8183c232e" />

Insight: 
Weekends: More consistent tipping around 2–3, with higher volume of data points (more diners).
Weekdays: Fewer customers, but tips are more spread out → some tip very low (~1), some higher (~5–6).
Central tendency: Both weekends and weekdays cluster around 2–3, but weekends are tighter and more predictable.
Extreme tips: Appear in both, but slightly higher ceiling on weekends.

👉 Takeaway:
On weekends, customers tend to tip more consistently and in higher volume, clustering around 2–3.
On weekdays, tipping is less predictable, with both smaller and larger tips relative to weekends.

🕑 Do dinners bring more tips?
Code:
```
df['time'].unique()
```
Dinner details;
```
dinner_df = df[df['time']=='Dinner']
dinner_tip_min=dinner_df['tip'].min()
dinner_tip_max= dinner_df['tip'].max()
dinner_tip_mean= dinner_df['tip'].mean()
dinner_tip_median= dinner_df['tip'].median()
# YOUR CODE
dinner_values = [dinner_tip_min, dinner_tip_max, dinner_tip_mean, dinner_tip_median]
# Round all the values to 4 decimal places
dinner_values = map(lambda x: round(x, 4), dinner_values)
# Make a dataframe from the list
dinner_mct = pd.DataFrame(dinner_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
dinner_mct
```

Lunch details:
```
lunch_df = df[df['time']=='Lunch']
lunch_tip_min=lunch_df['tip'].min()
lunch_tip_max= lunch_df['tip'].max()
lunch_tip_mean= lunch_df['tip'].mean()
lunch_tip_median= lunch_df['tip'].median()
# YOUR CODE
lunch_values = [lunch_tip_min, lunch_tip_max, lunch_tip_mean, lunch_tip_median]
# Round all the values to 4 decimal places
lunch_values = map(lambda x: round(x, 4), lunch_values)
# Make a dataframe from the list
lunch_mct = pd.DataFrame(lunch_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
lunch_mct
```

Comparision data: 
```

all_vals_dict3 = {
    'Dinner': {'min': dinner_tip_min, 'max': dinner_tip_max, 'mean': dinner_tip_mean, 'median': dinner_tip_median},
    'Weekday': {'min': lunch_tip_min, 'max': lunch_tip_max, 'mean': lunch_tip_mean, 'median': lunch_tip_median}
}

# Make a dataframe
all_mct = pd.DataFrame(all_vals_dict3)
# Output the dataframe
all_mct
```
Result:
<img width="199" height="147" alt="image" src="https://github.com/user-attachments/assets/42c72ec3-c246-4420-b1e3-bb5c16d9cba6" />

Histogram show the distribution:
```

fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 1st chart: Whole dataset
axes[0].hist(df.tip, color='#74b9ff')
axes[0].set_xlabel('Tip value')
axes[0].set_ylabel('Frequency')
axes[0].set_title('Whole dataset tip values')
axes[0].grid(True)

# 2nd chart: Dinner
axes[1].hist(dinner_df.tip, color='#ff7675')
axes[1].set_xlabel('Tip value')
axes[1].set_ylabel('Frequency')
axes[1].set_title('Dinner tip values')
axes[1].grid(True)

# 3rd chart: Lunch
axes[2].hist(lunch_df.tip, color='#55efc4')
axes[2].set_xlabel('Tip value')
axes[2].set_ylabel('Frequency')
axes[2].set_title('Lunch tip values')
axes[2].grid(True)

plt.tight_layout()
plt.show()
```
Result:
<img width="909" height="252" alt="image" src="https://github.com/user-attachments/assets/eb248908-4bc1-484c-886a-fc9aed44144a" />

Insight:
Dinner: More customers, tips cluster consistently around 2–4, but some customers leave very high tips (≥7).
Lunch: Fewer customers, tips are more variable, but without extreme highs.
Central tendency: Dinner tips slightly higher (peak ~3), lunch peaks lower (~2).
Extreme values: Dinner allows for very large tips, lunch does not.

👉 Takeaway:
Dinner: Larger customer base, higher & more consistent tipping, with the possibility of big outliers.
Lunch: Smaller customer base, more scattered tipping, and generally lower maximum values.
