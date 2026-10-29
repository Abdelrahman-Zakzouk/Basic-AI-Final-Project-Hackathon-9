*Final Project, Part 1*

# Used Car Price Prediction with Linear Regression

## Objective

Predict the selling price of a used car from its specifications and history, using a linear regression model.

## Data Cleaning

- **Duplicates:** duplicate rows (14.79%) were removed.
- **Text to numbers:** units were removed from mileage, engine and max power. Mileage in km/kg was converted to kmpl (x1.4, an approximation). Torque was converted to Nm and split into torque value and max rpm.
- **Invalid values:** a km driven value of 1 was set to missing.
- **Outliers:** 22 rows were removed (km driven above 1,000,000, max power of 0 or 400+, mileage of 0 or less, max rpm of 20,000+).
- **Missing values:** filled after the train/test split, using training data only (mode for seats, median for km driven, engine and max rpm, mean for mileage, max power and torque).

## Feature Engineering

- Brand was taken from the car name, and age was calculated as 2026 minus year.
- Skewed columns and the selling price were log-transformed (log1p). Predictions are converted back to real prices at the end.
- Owner was encoded in order (Test Drive Car 0, First Owner 1, up to Fourth & Above 4), following the price trend. Brand, fuel, seller type and transmission were one-hot encoded.

## Model

The data was split 80% training and 20% testing (random\_state 42). Features were standardized with a scaler fitted on the training set, and a linear regression model was trained on the log price.

## Evaluation

| Metric | Value |
| --- | --- |
| R-squared | 0.87 |
| Mean Absolute Error | about 94,900 |
| Root Mean Squared Error | about 174,400 |

The model explains about 87% of the variation in car prices, and its predictions are typically about 95,000 away from the real price.

## Conclusion

A linear regression model predicts used car prices well after cleaning, log-transforming and encoding the data. Price falls as the car gets older and as the number of owners rises, while Test Drive cars are by far the most expensive.

**Limitations:** rare brands have very few cars, Test Drive cars are only a handful of rows, and a linear model cannot capture every non-linear effect. Grouping rare brands or trying a non-linear model could improve the results.


*Final Project, Part 2*

# Airline Passenger Satisfaction Prediction with Logistic Regression

## Objective

Classify each passenger as **satisfied (1)** or **neutral or dissatisfied (0)** from their flight details and survey ratings, using logistic regression.

## Data

The dataset has 8,440 passengers and 18 columns: gender, customer type, age, type of travel, class, flight distance, nine service ratings (wifi, online booking, online boarding, seat comfort, entertainment, onboard service, leg room, baggage handling, cleanliness), departure and arrival delay, and satisfaction. There are no duplicate rows, but 81.61% of rows have at least one missing value, so dropping rows was not an option.

## Data Cleaning

- **Invalid ratings:** values outside 1 to 5 (such as 0 and 10) were set to missing (94 values).
- **Outliers:** impossible values were set to missing, and the rest of each row was kept: age below 5 or 100+ (53), flight distance of 10,000+ (55), departure delay of 500+ (75) and arrival delay of 500+ (73).
- **Missing values:** filled after the train/test split, using training data only (mode for categorical columns and ratings, median for numeric columns).
- **Skewness:** flight distance and the delay columns were log-transformed (log1p) where skew was above 1.

## Feature Engineering

Binary columns were mapped to 0 and 1 (gender, customer type, type of travel, and the satisfaction target). Class was encoded in order: Eco 0, Eco Plus 1, Business 2. All features were then standardized with a scaler fitted on the training set.

## Key Findings

The strongest links to satisfaction were class, in-flight entertainment and online boarding. Personal travel was negatively linked. Age and gender had almost no link.

## Model

The data was split 80% training and 20% testing (random\_state 42), and a logistic regression model was trained on the scaled training data.

## Evaluation

| Metric | Value |
| --- | --- |
| Accuracy | 0.842 |
| Precision | 0.841 |
| Recall | 0.878 |
| F1 score | 0.859 |

The accuracy is inside the 0.8 to 0.9 target. The model finds about 88% of satisfied passengers, and about 84% of the passengers it labels satisfied really are.

## Conclusion

Logistic regression predicts passenger satisfaction well. Handling outliers by blanking and filling the bad values, instead of deleting rows, kept all 8,440 passengers in the data.

**Limitations:** the outlier thresholds are judgment calls, the split was not stratified, and the model used default settings. Tuning it or trying another classifier could improve the results.
