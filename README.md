# Driving Style Predictors

# Problem Statement

The aim of this project is to build a classification model that can accurately classify a drivers driving style as "Even Paced" or "Aggressive". Having an effective model is important since it will be tracking the driving behavior in real time to predict if the driver is agressive or not. With the use of this predictive model, a billing or savings strategy can be deployed to auto insurance companies in order to determaine car insurance rates for there drivers based on the driving styles.

### For this project I will be using the OSEMiN framework, wich stands for Obtain, Scrub, Explore, Model and Interpret. 

* Obtain: The dataset consists of car data belonging to 24,000 instanceses and was downloaded from the Kaggle Datasets Library.

* Scrub: I will search for null values and outliers to replace them with more suitable values or delete them form the data set. I will also loo at multicollinearity issues amongst the features and find creative ways to deal with them.

* Explore: Throughout the whole process, it is important to lookout for other important information that can be gathered from the data. I will carry out Exploratory Data Analysis and answer relevant questions about the current state of business.

* Model: I will use single and ensemble binary classification models. The aim of the project is to select the best performing model and fine-tune its hyperparameters to achieve correct identification of revenue generating sessions.

* Interpret: Interpret the results of the models to make recommendations that generate value.

### Numerical Features

* altitude change, calculated over 10 seconds;

* current speed value; average speed in the last 60 seconds;

* speed variance in the last 60 seconds;

* speed variation for every second of detection;

* longitudinal acceleration, measured by the smartphone accelerometer and pre-processed with a low-pass filter;

* engine load, expressed as a percentage;

* engine coolant temperatures in celsius degree;

* Manifold Air Pressure (MAP), a parameter the internal combustion engine uses to compute the optimal air/fuel ratio;

* Revolutions Per Minute (RPM) of the engine;

* Mass Air Flow (MAF) Rate measured in g/s, used by the engine to set fuel delivery and spark timing;

* Intake Air Temperature (IAT) at the engine entrance;

* vertical acceleration, measured by the smartphone accelerometer and pre-processed with a low-pass filter;

* average fuel consumption, calculated as needed liters per 100 km.

# Results

## K Nearest Neighbors

 <img src= "https://raw.githubusercontent.com/phillipojo24/dsc-mod-3-project-v2-1-onl01-dtsc-pt-041320/master/Screen%20Shot%202020-10-16%20at%202.30.09%20AM.png">

Our Initial Model has an accuracy of 89% however this is not a good criterion to judge whether the model is predicting correctly for our business case because the data set is imbalanced. We have to rely on our recall value which is a metric that quantifies the number of correct positive predictions made out of all positive predictions that could have been made and Receiving Operator Characteristic Curve to give a better understanding of how our model is performing with this dataset. For this Base Model, the recall value is 0.26 which is not a good as we would want. However, for this Base model, the ROC Curve is 0.80, which is okay but not as good as we want. Let's try using a different model to see if that score can be improved. Since we are using K Nearest Neighbors, our 'n_neighbors' parameter can help improve our model performance but choosing the optional neighors. We will fill this "best value" by running a grid search. 


After Runing Grid search for best parameter, Our revised model has a slight improved recall value of 0.34 which is a little better but not as good as we would want. The ROC Curve drop slightly to 0.77, which is still pretty good as well. While using the best model parameters we still werent able to imporve our model as much.

## Random Forest Classifier

<img src = 'https://raw.githubusercontent.com/phillipojo24/dsc-mod-3-project-v2-1-onl01-dtsc-pt-041320/master/Screen%20Shot%202020-10-16%20at%202.30.43%20AM.png'>

The Base Random Forest over-fitted the data, causing the training accuracy to be 100%, but the test accuracy wasn't too bad at 93.28%. the recall value improved to 0.52 which not as good as we would want. The ROC Curve improved to 0.98 which the higher the value, the better, with a 1.0 giving perfect predictions, which is still pretty good as well. Looks like this imbalnced data set is going to be a problem with these models so I will use SMOTENC to balance the data set. 

SMOTENC is an oversampling method that helps with imbalanced data sets. The continuous features of the new synthetic minority class sample are created using the same approach of SMOTE as described earlier. The nominal feature is given the value occuring in the majority of the k-nearest neighbors. 

Model is no longer overfitted but we have experrienced a drop in ROC curve 0.81 and recall values of 0.74 . Random Forest Classifer was an improvement but we will try one more model to see if we can get better accuracy.

## XGBoost Classifier

<img src= 'https://raw.githubusercontent.com/phillipojo24/dsc-mod-3-project-v2-1-onl01-dtsc-pt-041320/master/Screen%20Shot%202020-10-16%20at%202.31.01%20AM.png'>

Grid Search was able to find the optimal parameter for the XGBoost model. The training and test accuracy improved slightly, but the ROC Curve remained pretty much the same.

## Feature Importance

<img src='https://raw.githubusercontent.com/phillipojo24/dsc-mod-3-project-v2-1-onl01-dtsc-pt-041320/master/Screen%20Shot%202020-10-17%20at%201.40.32%20PM.png'> 

This plot illustrates the top 20 important features in our model in decending order. The "VehicleSpeedInstantaneous" which stands for Current Speed Value is the feature that was used the most by the XGBoost tree. This graph does not show how each feature values positivily or negativily affect the model. 



Using the shap.summary_plot will show the true relationship of each of the predictors with the target variable. Feature importance variable are ranked in descending order with the original values color coded whether that variable is high (in red) or low (in blue) for visual observations.

## SHAP Feature Importance

<img src='https://raw.githubusercontent.com/phillipojo24/dsc-mod-3-project-v2-1-onl01-dtsc-pt-041320/master/Screen%20Shot%202020-10-17%20at%201.40.59%20PM.png'>

Things that help predict Aggressive Driving

    * Road Condition (categorical)
    * Engine Load - Force produce by the engine
    * Average Fuel Consumption - Fuel consumed over a certain amount of distance
    * Vertical and Longitudinal Acceleration 
    * Vehicle Speed Variation/ Variance - Speed fluctuation over a period of time
    * Engine RPM - Reveloutions per min

## SHAP Dependence Plots

<img src='https://raw.githubusercontent.com/phillipojo24/dsc-mod-3-project-v2-1-onl01-dtsc-pt-041320/master/Screen%20Shot%202020-10-17%20at%201.41.12%20PM.png'>

When the Average Fuel Consumption is low the predicted probability is higher while when the fuel consumption is high the predicted probability is lower.

<img src='https://raw.githubusercontent.com/phillipojo24/dsc-mod-3-project-v2-1-onl01-dtsc-pt-041320/master/Screen%20Shot%202020-10-17%20at%201.41.31%20PM.png'>

Speeds over 60 Mph negatively impacts the model because it becomes hard to tell if the driver is aggressive or not. That would be the thresshold for the model predictions strength but after 60 mph this feature becomes irrelevant.

# Recomendations

To Summarize, the purpose of building this model was to be able to predict wheter a driver was driving agressive or not. Its intent was to give insurance companies another risk factor to implement in giving out accurate insurance premeiums. Today, most companies just use driving history whether it be lenght of driving history or any previous traffic violation. This gives companies a real time predictor and give the drivers a better scenario to always be able to be a better driver.

> * Engine Load - Pay attention to higher percentages of engine load that will be predicted as aggressive driving and driver premiums should be higher.  

> * Average Fuel Consumption - Distance traveled and amount of fuel consumed. The higher fuel consumption will not be predicted as aggressive driving so driver premiums should be lower.


> * Engine RPM- Higher RPMs will be predicted as aggressive driving so driver premiums should be higher.

# Future Work

*  The improve the models predicting capability i would like to add Age and Gender predictors and she if the will help the models performance. For Example, do younger men tend to drive more aggressively?

* More car attributies should be recorded like Averge Steering Wheel Jerk and Aggressive braking could also be added as a predictor and help make the model better.

* Gather new data from all type of cars. These mini smart cars dont have much aggressive ability. It would be intresting to see diffrent sedans, coupes and trucks involved in the next round of data.


```python

```
