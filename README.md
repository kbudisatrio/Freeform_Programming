# Freeform_Programming
The aim of "Life Expectancy Data Analysis.ipynb" is to analyse the Life Expectancy Dataset and make graphs, the program visualises the world life expectancy and as a sample, I have chosen the southeast asia region to be featured in the analysis. The program was run on Colab.
The Life Expectancy Dataset has 3 seperate datasets:
life_expectancy.csv
life_expectancy_different_ages.csv
life_expectancy_female_male.csv

## The Datasets
```
#Libraries/Packages
import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.pyplot as plt

#Load datasets into variables
life_expectancy=pd.read_csv('/content/drive/MyDrive/Freeform Programming/life_expectancy.csv')
life_expectancy_different_ages=pd.read_csv('/content/drive/MyDrive/Freeform Programming/life_expectancy_different_ages.csv')
life_expectancy_female_male=pd.read_csv('/content/drive/MyDrive/Freeform Programming/life_expectancy_female_male.csv')
```

## Generating line graph for World Life Expectancy
```
#selecting rows for world life expectancy data
world_le=life_expectancy.set_index('Code')
#sorting rows of data based on year in ascending manner,
#because data before the year 1950 is placed at the end
world_le=world_le.loc['OWID_WRL'].sort_values(by='Year',ascending=True)
display(world_le)

#generating a line graph of world life expectancy
world_le.plot(x='Year',y=['LifeExpectancy'], title='World', ylabel='Life Expectancy')
```

## Generate line plot for  world life expectancy by age group
```
#selecting rows for world life expectancy in different ages
world_le_age=life_expectancy_different_ages.set_index('Code')
world_le_age=world_le_age.loc['OWID_WRL']
#select starting year 1950 because there is no data prior to that for columns age 10 to 80
world_le_age_1950=world_le_age[world_le_age['Year']>=1950]
display(world_le_age_1950)
#generate line plot
world_le_age_1950.plot(x='Year',y=(['LifeExpectancy0','LifeExpectancy10','LifeExpectancy25','LifeExpectancy45','LifeExpectancy65','LifeExpectancy80']), title='World', ylabel='Life Expectancy')
```

## Generate scatter plot for world life expectancy vs life expectancy gap between males and females
```
#merge the two datasets together
life_expectancy_merged=pd.merge(life_expectancy, life_expectancy_female_male, on=['Entity','Code','Year'])
#select rows containing world data
world_gap=life_expectancy_merged.set_index('Code')
world_gap=world_gap.loc['OWID_WRL']
#display the selected rows
display(life_expectancy_merged,world_gap)
#generate scatter plot
plt.figure(figsize=(10,6))
sns.scatterplot(
    data=world_gap,
    x='LifeExpectancy',
    y='LifeExpectancyDiffFM'
)
plt.title(f'Life Expectancy vs Gender Gap', fontsize=12)
```

## Generate choropleth map for world life expectancy
```
#select rows excluding 'World'
countries_only_le=countries_le[countries_le['Entity']!='World']
#select year, this is done by making 'Year' as index first
countries_only_le=countries_only_le.set_index('Year')
#select year(s) of interest
countries_le_2021=countries_only_le.loc[2021]
fig=px.choropleth(countries_le_2021,locations='Entity',locationmode='country names',color='LifeExpectancy')
fig.show()
```

### Generate multi-line plot for life expectancy in Southeast Asia
```
#Making a line graph of life expectancies of South East Asian (SEA) Countries from 1950 to 2021
#Selecting SEA country codes
sea_country_codes=['IDN','TLS','BRN','MYS','SGP','THA','MMR','KHM','VNM','LAO']
#Selecting rows with SEA country codes
countries_sea_le=countries_le[countries_le['Code'].isin(sea_country_codes)]
#Set Entity and YEar as indices
sea_le_indexed=countries_sea_le.set_index(['Entity','Year'])
#select LifeExpectancy column
sea_le_nocode=sea_le_indexed.iloc[:,-1:]
#convert to wide format
sea_le_wide=sea_le_nocode.unstack()
display(countries_sea_le)
display(sea_le_wide)
#using level 0 of the index
sea_le_wide=sea_le_nocode.unstack(level=0)
#generate line plot for each country against year
fig=sea_le_wide.plot()
fig.legend(bbox_to_anchor=(1,1))
```

## Generate box plots for life expectancy in Southeast Asia
```
#generate box plots
plt.figure(figsize=(10,6))
sns.boxplot(data=countries_sea_le, x='Entity',y='LifeExpectancy')

plt.show()
display(countries_sea_le.drop(columns=['Year']).describe())
```

## Generate clustered bar chart for life expectancy by age group in Southeast Asia
```
#Selecting rows with SEA country codes
countries_sea_le_ages=life_expectancy_different_ages[life_expectancy_different_ages['Code'].isin(sea_country_codes)]
#set the index of the dataframe to Year
countries_sea_le_ages_indexed=countries_sea_le_ages.set_index('Year')
#select the year 1975
countries_sea_le_ages_1975=countries_sea_le_ages_indexed.loc[1975]
#display selected rows
display(countries_sea_le_ages,countries_sea_le_ages_1975)
#create a new dataframe that transforms the columns of the previous one into rows
group_countries_sea_age=countries_sea_le_ages_1975.set_index(['Entity','Code']).rename_axis(columns='Age').stack().reset_index(name='LifeExpectancy')
display(group_countries_sea_age)
#generate bar chart of the selected dataframe
plt.figure(figsize=(10,4))
sns.barplot(
    data=group_countries_sea_age,
    x='Entity',
    y='LifeExpectancy',
    hue='Age'
)
plt.legend(bbox_to_anchor=(1,1))
plt.show
```
