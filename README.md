# Team Dragonfly:  Coffee Ratings
Coffee is consumed daily by 30-40% of the world's population, and produced in over 70 countries worldwide. Though coffee drinkers all have their preferences, we wanted to see if we could find relationships between how a coffee rates in taste tests and its features, such as country or region of origin, roast, or type of preparation method.

***Include link to slides and recorded presentation***

## Team Members
* [Cassidy Madison](https://www.linkedin.com/in/cassidymadison/) has a master's in Biology from Harvard University. Surprisingly, she doesn't particularly enjoy coffee.
* [Ching-Lung Hsu](https://www.linkedin.com/in/ching-lung-hsu-b5611b139/) is pursuing a PhD studying Bayesian Nonparametrics at Duke University. He likes light roast coffee.
* [Ethan Semrad](https://math.fsu.edu/People/grads.php?id=1823) has a master's in Mathematics from University of South Dakota, and is currently pursuing a PhD in Biomathematics at Florida State University. He takes his coffee black.

## Table of Contents
* [Trial 1: Categorize country of origin using professional coffee quality ratings](https://github.com/madisonc27/Team-Dragonfly#trial-1-categorize-country-of-origin-using-professional-coffee-quality-ratings))
  * [Data Cleaning and Exploratory Analysis](https://github.com/madisonc27/Team-Dragonfly#data-cleaning-and-exploratory-analysis)
  * [Model Creation and Conclusions](https://github.com/madisonc27/Team-Dragonfly#model-creation-and-conclusions)
* [Trial 2: Predict rating from Coffee Review using various features](https://github.com/madisonc27/Team-Dragonfly#trial-2-predict-rating-from-coffee-review-using-various-features)
  * [Data Cleaning and Exploratory Analysis](https://github.com/madisonc27/Team-Dragonfly#data-cleaning-and-exploratory-analysis-1)
  * [Model Creation and Results](https://github.com/madisonc27/Team-Dragonfly#model-creation-and-results)
    * [Multiple Linear Regression](https://github.com/madisonc27/Team-Dragonfly#multiple-linear-regression)
    * [Lasso and Ridge Regression](https://github.com/madisonc27/Team-Dragonfly#lasso-and-ridge-regression)
    * [Interaction Terms](https://github.com/madisonc27/Team-Dragonfly#interaction-terms)
    * [Conclusions](https://github.com/madisonc27/Team-Dragonfly#conclusions)
* [Key Takeaways](https://github.com/madisonc27/Team-Dragonfly#key-takeaways)
* [Future Directions](https://github.com/madisonc27/Team-Dragonfly#future-directions)

# Trial 1: Categorize country of origin using professional coffee quality ratings
We decided to utilize a data set that was scraped from the [Coffee Quality Institute](https://www.coffeeinstitute.org/), which provides third-party coffee quality evaluation. Our goal was to use the professional rating values in each of 10 categories to see if we could predict the country of origin of the beans. We also decided to keep growing altitude and bean processing method as backup features for prediction if we were not able to accurately predict the country of origin. 

## Data Cleaning and Exploratory Analysis
We began by removing the columns that were not of interest to us and any entries that were missing review scores or information on the country of origin. We decided to set a cutoff for the minimum number of entries for a country to be included in the analysis. Initially we set the cutoff to 10, but also created a data set with a cutoff of 50 to hopefully improve the predicting power of the data. We also created a data set that grouped the countries into larger regions, again thinking it might allow for better predictions. More details can be found in the [Data Cleaning Folder](Trial%201/Data%20Cleaning).

After cleaning, we could explore the data more deeply. A pairplot exploring the relationships between the variables can be seen below. From this image, it is clear that there is a large positive correlation between the features. However, there seemed to be some separation between some of the countries, particularly between Mexico and Colombia, so we were hopeful that our models could pick up on these differences.

![Pairplot](Trial%201/EDA/small_pairplot.png)

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

Now we can look more closely at these features, the details of which can be found in the [EDA Folder](Trial%202/EDA/EDA/ipynb). First we investigate the ratings, and their distribution is shown below. The ratings are slightly skewed, with a skew value of -1.80. 

![Ratings](Trial%202/EDA/rating_skew.png)

We can also look at how the categorical features tend to relate to the rating with the boxplots shown below. The roasts, regions, pod or capsule type, and decaffeination seem to have strong effects on the rating.

![Boxplots](Trial%202/EDA/categorical_boxplot.png)

A final thing to consider is the difference in counts between each class in the categorical variables. For example, a histogram showing the counts of the region and roast categories below shows that there is not an equal amount of samples in each category. This may be important to consider when interpreting our models, as classes with higher counts may be more likely to have a stronger effect in the models.

![Predictor Counts](Trial%202/EDA/region_roast_counts.png)

## Model Creation and Results
Next we began to create our models. Because each of our features is categorical and binary, we decided to use multiple linear regression to establish an initial simple model. We are not able to use a polynomial model because our predictors consist entirely of 0s and 1s. Importantly, we also created a baseline model that simply took the mean rating from the training set and predicted this mean regardless of the features.

### Multiple Linear Regression
Details on the multiple linear regression model can be found [here](Trial%202/Models/Multiple%20Linear%20Regression.ipynb). In cross validation, this model performed better than the baseline when using both mean squared error (MSE) and mean absolute error (MAE). Results for all models can be found in the [Conclusions](https://github.com/madisonc27/Team-Dragonfly/edit/main/README.md#conclusions) section.

### Lasso and Ridge Regression
After performing multiple linear regression, we decided to extend the model with both lasso and ridge regression. These models allow us to impose a penalty on the coefficients, which helps to elucidate which features are most important for the model. In theory, smaller coefficients chosen through optimization of the hyperparameter alpha should also help us avoid overfitting to the training data. Details for the lasso and ridge models can be found [here](Trial%202/Models/Lasso%20Ridge%20v1). 

Lasso regression in particular can be very helpful for feature selection, since it tends to push the coefficients of less important features to zero as the hyperparameter alpha is increased. An example trial using lasso can be seen below (click the table to see a larger version if needed). We can see that as alpha increases, some coefficients such as those for organic, fair trade, and decaf quickly become zero, while others, such as medium-light roast and the africa/arabia region tend to stay around and stay larger in magnitude. We can conclude that the categories with coefficients that are larger and remain included in higher alphas are more important for determining the rating. Interestingly, it seems that organic and fair trade coffees are not rated more highly than non-organic and non-fair trade.

![Alpha table](Trial%202/Models/lasso_alphas.png)

### Interaction Terms
We decided to include some interaction terms in our model to see if the predictions could be improved. Coefficients with a stronger main effect are more likely to have interactions. From our previous regression coefficients, we determined that roast was particularly important, as were espresso and pod/capsule. We decided to try including interaction terms between espresso and each roast as well as between pod/capsule and each roast. More details on the interaction terms can be found [here](Trial%202/Models/Interaction%20Terms.ipynb) and the results can be seen in the [Conclusions](https://github.com/madisonc27/Team-Dragonfly/edit/main/README.md#conclusions).

It is important to note that many other interaction terms could have been chosen. For example, some of the regions, such as Africa/Arabia seemed to be quite important. However, adding in interaction terms greatly increases the number of features in the model. We saw only modest gains in model performance when adding the 12 roast with espresso and pod/capsule interaction terms, corresponding to a drop in the MSE of approximately 0.13 and in the MAE of approximately 0.05. Therefore, we decided not to continue adding more interaction terms.

### Conclusions
A table summarizing the mean squared error (MSE), mean absolute error (MAE), and root mean squared error (RMSE) obtained from testing each cross-validated model on the test set can be found below. We can see that all models performed similarly, and did better than the baseline which simply assumed the mean from the training set. As expected, RMSE is larger than MAE for all models, which means that there is some variation in the magnitude of the errors and some very large errors likely occurred, which RMSE penalizes more due to the square. 

![Results](Trial%202/Models/testing_results.png)

# Key Takeaways
Our data may be of interest to coffee importers or sellers, who may wish to know which features are associated with higher consumer ratings. In our results, the features that had the largest positive coefficients were medium-light roast and to a lesser extent light roast. On the other hand, darker roasts had negative coefficients and tended to be rated lower by consumers. Coffees from the Africa/Arabia region also tended to rate higher, while Caribbean coffees rated slightly lower.

Perhaps equally important may be the attributes that did not seem to be associated with any change in rating. These features included organic, fair trade, decaffeination, and blends. Organic and fair trade may be surprising, because these are often thought of as "higher end" features and likely are more expensive. However, organic had a very slight negative coefficient, and fair trade had a very slight positive coefficient.

# Future Directions

