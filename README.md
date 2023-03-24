# Problem Identification

It would be useful to predict how long a power outage may take. In order to accomplish this, we will build a regression model that can predict the outage duration using data collected at the time of outage. In this dataset, we will use the response variable `OUTAGE.DURATION`. In order to evaluate the effectiveness of this model, we will be using R-squared, since it gives a good idea of how the data fits the model in an easy and concise manner.

---

# Baseline Model

The Baseline Model for this project uses a linear regression model and two variables, those being `CUSTOMERS.AFFECTED` and `CAUSE.CATEGORY`. `CUSTOMERS.AFFECTED` is a quantitative feature, while `CAUSE.CATEGORY` is a nominal categorical feature consiting of the following categories: severe weather, intentional attack, system operability disruption, public appeal, equipment failure, fuel supply emergency, and islanding. For this reason, `CAUSE.CATEGORY` was One-Hot encoded using sklearn's preprocessing library to divide the categories into multiple variables.

The R-squared value for this model was approximately `0.0745` on the training set and `0.051` on the testing set. This allows us to conclude that the model is highly inaccurate, as we are close to a value of `0`, meaning that the model does not relate at all to the response variable.

---

# Final Model

For preprocessing the final model, three new features were added, from the columns `TOTAL.PRICE` and `OUTAGE.START`. First, both `CUSTOMERS.AFFECTED` and `TOTAL.PRICE` were standardized, as they had outliers that could potentially effect the data. In regressors such as the K-nearest-neighbors regressor, this could potentially skew the model and give inaccurate results. For `OUTAGE.START`, both the hour and month were used in order to fit the model. It is likely that during some hours and  months, response times and outage fixes will happen faster, which the model can use to give more accurate results.

After running grid search against a decision tree and knn regressor, the most accurate model was found to be a decision tree. The hyperparameters that were tested were the max depth, the minimum samples required to split a node, and the error calculation. Using grid search, the model was chosen with the following parameters:

- Max Depth: 5
- Min Samples: 20
- Error: squared error

The R-squared value for this model was approximately `0.335` on the training set and `0.218` on the testing set, which is a major improvement from the baseline model. As it is closer to the theoretical maximum of `1`, this means that this model suits the response variable much more than the baseline model.

---

# Fairness Analysis

It would be interesting to tell if the model was more accurate for certain states. In order to do this, we ran a permutation test against California and Texas to see if the model was more accurate on one or the other.

The following permutation test was conducted:

- **Null Hypothesis**: The prediciton remains consistent for both Texas and California, and differences are up to chance
- **Alternative Hypothesis**: There is a bias for the value between Texas and California
- **Test Statistic**: Difference in R-squared for Texas and California
- **Test Statistic**: 0.05

The reported p-value for this test was `0.278`. This suggests that the model is unbiased between Texas and California, and neither state has a significant error difference.
