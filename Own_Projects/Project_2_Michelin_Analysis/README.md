# Analysis of Michelin Restaurants in 2025 using Python

Welcome to my analysis of the Michelin Restaurants in 2025. This porject was created to find out which country and city have the most number of Michelin Restaurant. It looks into the number of Michelin restaurants around the world and which cuisine has the most number of Michelin recognition. 

The data soured from [Kaggle](https://www.kaggle.com/datasets/ngshiheng/michelin-guide-restaurants-2021) provides the database for my analysis, containing detailed information on the restaurant names, locations, Michelin award level, etc.. Through a series of Python scropts, I explored key questions such as the countries and cities with the most Michelin restaurants and Starred restaurants. 

# The Questions
Below are the questions I want to answer in my project:
1. What is the proportion of awards for each award level in the Michelin Guide? 
2. Which country has the most number of Michelin restaurants?
3. Which city has the most number of Michelin restaurants?
4. Which cuisine is most likely to get a Michelin recognition?

# Tools I used
For my analysis, I use several key tools:
- **Python**: This is the programming language I used to perform the data analyses. Within Python, I also used the libraries below for their respective fuctions:
    - **Pandas** for data analysis
    - **Matplotlib** for data visualisation
    - **Seaborn** for advanced customisation on visuals
- **Jupyter Notebook**: I use this to run my Python scripts which let me easily include my notes and analysis. 
- **Visual Studio Code**: For executing my Python scripts.
- **Git & GitHub**: Essential for version control and sharing my Python code analysis. 
- **ChatGPT**: For trouble shooting when codes run into error and transform manually writted dataset into code that is repetitive.

# Data Preparation and Cleanup
This sections outlines the steps taken to prepare the data for analysis, exusring accuracy and usability. 

## Import Data
``` 
# Importing Libraries
import pandas as pd
import matplotlib.pyplot as plt 
import seaborn as sns

#Load datase to df
df = pd.read_csv('michelin_my_maps.csv')
```

## Data Cleanup

### Splity ```Location``` into ```City``` and ```Country``` 
```
#Split city and country into its own columns
df[['City', 'Country']] = df['Location'].str.split(', ', n=1, expand = True)

#Find out what cities have null country
null_country = df[df['Country'].isnull()].index.tolist()
df.loc[null_country, 'City'].unique().tolist()

# ['Singapore', 'Hong Kong', 'Macau', 'Dubai', 'Abu Dhabi', 'Luxembourg']

#Since these are either city states or small countries, The country is filled with the city name. 
#Fill null Country with City 
df['Country'] = df['Country'].fillna(df['City'])
```
### Use the primary cuisine as the sole cuisine in ```Cuisine```
```
df['Cuisine'] = df['Cuisine'].str.split(',')

def replace_multiple_cuisines(cuisine):
    cuisines = cuisine.split(", ")  # Assuming cuisines are separated by commas
    if len(cuisines) > 1:
        return cuisines[0]  # Return the first cuisine
    return cuisine  # Return the original if only one cuisine

# Apply the function to the 'Cuisine' column
df_cuisine_grouped = df.copy()
df_cuisine_grouped['Cuisine'] = df_cuisine_grouped['Cuisine'].apply(replace_multiple_cuisines)
```

### Categorise ```Cuisine``` into ```Summarised Cuisine``` for more generalised analysis
```
#This code was created by ChatGPT
category_mapping = {
    'Afghan': ['Afghan'],
    'African': ['African', 'Ethiopian', 'North African'],
    'American': ['American', 'American Contemporary', 'Californian', 'Creole', 'Southern', 'Tex-Mex'],
    ...
    'Vegan & Vegetarian': ['Vegan', 'Vegetarian'],
    'Venezuelan': ['Venezuelan'],
    'Vietnamese': ['Vietnamese', 'Vietnamese Contemporary']
}

# Create a reversed mapping from original cuisine to summarised cuisine
reverse_category_mapping = {orig: summarised for summarised, originals in category_mapping.items() for orig in originals}

# Function to categorize cuisines
def categorise_cuisine(cuisine):
    return reverse_category_mapping.get(cuisine, "Other")

df_cuisine_grouped['Summarised Cuisine'] = df_cuisine_grouped['Cuisine'].apply(categorise_cuisine)
```

# The Analysis

## 1. What is the proportion of awards for each award level in the Michelin Guide? 

To find the proportion of award for each award level in the Michelin Guide, I categorised the number of award in each award level and plot both a bar chart and a pie chart. 
```
# Bar Chart
award = df.groupby('Award').size().sort_values(ascending=False).reset_index(name = 'Count')

sns.barplot(data= award, x = 'Award', y = 'Count', hue = 'Award', palette = 'rocket')
```
![Number of Michelin Award for each Award level in 2025](Own_Projects\Project_2_Michelin_Analysis\images\bar_restaurants by award.png)
*Bar Chart on the Number of Michelin Award for each Award Level in 2025*
```
#Pie Chart
award_prop = df.groupby('Award').size().reset_index(name = 'award_count')

colours = sns.set_palette('rocket_r', n_colors=len(awards))
fig, ax = plt.subplots(figsize=(8, 8))
ax.pie(award_counts, labels=awards, colors=colours, startangle=144,autopct='%1.1f%%', labeldistance=1.2, pctdistance=1.1)
```
![Proportion of Michelin Award for each Award level in 2025](Own_Projects\Project_2_Michelin_Analysis\images\pie_restaurants by award.png)
*Pie Chart on the Number of Michelin Award for each Award Level in 2025*

