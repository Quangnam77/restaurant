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
![Uploading image.png…]()
