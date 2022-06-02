# Team Dragonfly:  Title
Include a brief summary of the project and our goals.
Include links to slides and recorded presentation

## Team Members
Links to our Linkedins if we would like

## Table of Contents
Include links to each header below

# Trial 1: Categorize country of origin using professional coffee quality ratings
We decided to utilize a data set that was scraped from the [Coffee Quality Institute](https://www.coffeeinstitute.org/), which provides third-party coffee quality evaluation. Our goal was to use the professional rating values in each of 10 categories to see if we could predict the country of origin of the beans. We also decided to keep growing altitude and bean processing method as backup features for prediction if we were not able to accurately predict the country of origin. 

## Data Cleaning and Exploratory Analysis
We began by removing the columns that were not of interest to us and any entries that were missing review scores or information on the country of origin. We decided to set a cutoff for the minimum number of entries for a country to be included in the analysis. Initially we set the cutoff to 10, but also created a data set with a cutoff of 50 to hopefully improve the predicting power of the data. We also created a data set that grouped the countries into larger regions, again thinking it might allow for better predictions.

After cleaning, we could explore the data more deeply. A pairplot exploring the relationships between the variables can be found [here](Trial 1/EDA/EDA.ipynb). From this image, it is evident that there is a large positive correlation between the features. However, there seemed to be some separation between some of the countries, particularly between Mexico and Colombia, so we were hopeful that our models could pick up on these differences.

## Model Creation and Conclusions
Mention each model and note that we were not able to obtain high accuracy. Note the problems with the data and that we decided to shift focus.

Unfortunately, we found that the models were not very accurate for predicting the country of origin. We briefly tested altitude and processing method, but ran into the same issues

# Trial 2: Predict rating from Coffee Review using various features
Brief description of data set and goals

## Data Cleaning and Exploratory Analysis
Describe how we treated the data and include important graphics

## Model Creation and Results

### Multiple Linear Regression

### Lasso and Ridge Regression
Include rationale for using lasso and ridge - they both work well for smaller data sets

### Interaction Terms

### Conclusions

# Future Directions
