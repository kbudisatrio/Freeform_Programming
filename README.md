# Freeform_Programming
The aim of this project is to analyse the "Life Expectancy" Dataset and make graphs. The program was run on Colab
The Life Expectancy Dataset has 3 seperate datasets:
life_expectancy.csv
life_expectancy_different_ages.csv
life_expectancy_female_male.csv

Libraries/Packages
import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.pyplot as plt

The Datasets
#Load datasets into variables
life_expectancy=pd.read_csv('/content/drive/MyDrive/Freeform Programming/life_expectancy.csv')
life_expectancy_different_ages=pd.read_csv('/content/drive/MyDrive/Freeform Programming/life_expectancy_different_ages.csv')
life_expectancy_female_male=pd.read_csv('/content/drive/MyDrive/Freeform Programming/life_expectancy_female_male.csv')
'''
