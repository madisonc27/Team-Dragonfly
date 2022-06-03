# Team Dragonfly:  Title
Include a brief summary of the project and our goals.
Include links to slides and recorded presentation

## Team Members
Links to our Linkedins if we would like

## Table of Contents - could also remove because one is built in
Include links to each header below

# Trial 1: Categorize country of origin using professional coffee quality ratings
We decided to utilize a data set that was scraped from the [Coffee Quality Institute](https://www.coffeeinstitute.org/), which provides third-party coffee quality evaluation. Our goal was to use the professional rating values in each of 10 categories to see if we could predict the country of origin of the beans. We also decided to keep growing altitude and bean processing method as backup features for prediction if we were not able to accurately predict the country of origin. 

## Data Cleaning and Exploratory Analysis
We began by removing the columns that were not of interest to us and any entries that were missing review scores or information on the country of origin. We decided to set a cutoff for the minimum number of entries for a country to be included in the analysis. Initially we set the cutoff to 10, but also created a data set with a cutoff of 50 to hopefully improve the predicting power of the data. We also created a data set that grouped the countries into larger regions, again thinking it might allow for better predictions. More details can be found in the [Data Cleaning Folder](Trial%201/Data%20Cleaning).

After cleaning, we could explore the data more deeply. A pairplot exploring the relationships between the variables can be ***fix this part*** found [here](Trial%201/EDA/EDA.ipynb). From this image, it is clear that there is a large positive correlation between the features. However, there seemed to be some separation between some of the countries, particularly between Mexico and Colombia, so we were hopeful that our models could pick up on these differences.

## Model Creation and Conclusions
We used supervised learning to create five different preliminary models, including [K Nearest Neighbors](Trial%201/Models/K%20Nearest%20Neighbors.ipynb), [Decision Tree](Trial%201/Models/Decision%20Tree.ipynb), [Random Forest](Trial%201/Models/Random%20Forest.ipynb), [AdaBoost](Trial%201/Models/AdaBoost.ipynb), and [Support Vector Machines](Trial%201/Models/SVC.ipynb), where additional details on each model can be found in its respective notebook. We used accuracy as a base metric to compare the models. Unfortunately, we found that these models were not very accurate for predicting the country of origin, with most achieving accuracies in the 30-35% range.

Upon further reflection, the low accuracy found in our models was not altogether surprising. Our data had a few issues, chiefly a very high positive correlation between the predictors and very small if any differences in the mean and variance of the predictors between countries, as well as a lack of adequate sample size for many of the countries. Our models tended to place the samples into the categories with the highest number of samples, which can be seen in the example confusion matrix below resulting from the support vector machine model. Only the first 8 entries of the confusion matrix are shown, but this demonstrates the issue. Almost all of the samples were predicted to be Mexico, Colombia, and Guatemala, which have 189, 146, and 145 entries respectively. However, no samples were classified as Honduras, which has 41 samples.
![Confusion Matrix](Trial%201/Models/confusion_matrix_SVC.png)

Because it seemed we would not be able to accurately categorize by country of origin based on the issues mentioned above, we hoped to try categorizing by growth altitude or processing method instead. However, the features still did not appear to have a clear relationship to these variables when investigating exploratory plots.

Based on our results, we can conclude that the Coffee Quality Institute ratings do not differ significantly between different countries, growth altitudes, or processing methods. However, we still wanted to practice modeling and hoped to find some features that might affect the rating, so we set off in search of a new data set.

# Trial 2: Predict rating from Coffee Review using various features
The second data set that we used was scraped from [Coffee Review](https://www.coffeereview.com/), a review aggregate site. The data set consists of categorical variables, such as region, roast, organic, and decaf, and ratings, including an overall rating and specific categories such as aroma, body, and flavor. Our goal is to use the categorical variables to predict the overall rating.

## Data Cleaning and Exploratory Analysis
Again, we removed the categories that we were not interested in (more detail on cleaning can be found [here](Trial%202/Data%20Cleaning/data_cleaning.ipynb)) and began to explore the data. 

Based on the correlation matrix seen below, we decided to focus solely on the categorical variables. We discarded the specific category ratings because they had a similar issue to the previous data set, where the ratings are all highly correlated, and we didn't want to use a rating to predict another rating. 
![Correlation Matrix](Trial%202/EDA/correlation_matrix.png)

In the end, our features included:

* Region
* Roast
* Espresso
* Organic
* Fair Trade
* Decaffeinated
* Pod or Capsule
* Blend
* Estate
* Rating

Of these, region consisted of 6 different regions, roast included 6 different roasts, the remaining categories were binary for the presence or absence of that feature, and the rating was a score from 0-100.

Looking more closely at these features, we can first investigate the ratings, and their distribution is shown below. The ratings are slightly skewed, with a skew value of -1.80. *We may want to discuss kurtosis here as well if we have the value
![Ratings](Trial%202/EDA/rating_skew.png)

We can also look at how the categorical features tend to relate to the rating with the boxplots shown below. The roasts, regions, pod or capsule type, and decaffeination seem to have strong effects on the rating.
![Boxplots](Trial%202/EDA/categorical_boxplot.png)

A final thing to consider is the difference in counts between each class in the categorical variables. This may be important to consider when interpreting our models, as classes with higher counts may be more likely to have a stronger effect in the models. ***add in a plot for this***

## Model Creation and Results

### Multiple Linear Regression

### Lasso and Ridge Regression
Include rationale for using lasso and ridge - they both work well for smaller data sets

### Interaction Terms

### Conclusions

# Future Directions
