# Zenatix_Solutions
This is the solution to the assessment given by [Zenatix Solutions](https://www.zenatix.com/) in the technical round for the selection of internship.

## **Problem Statement:**<br>
The data contains power for multiple ACs at some hotel in Gurgaon.
  - Identify patterns/trends in the data?
  - Which AC was used the most/least?
  - Relate this power data with the outside temperature of Gurgaon. (Feel free to use temperature data from any website online. How will you fetch that data in your analysis?
  - Using the power data, predict/forecast the power consumption?

## **My Approach:**<br>
**Click here to view my first notebook**:- [Click here](https://github.com/Gaurav1401/Zenatix_Solutions/blob/main/Part%20-%201.ipynb) <br>
In the initial phase, I just tried to analyse the data to have a basic idea about this, my initial observations include:
 - The values of alternate minutes are missing for each AC i.e. we have only 50% data only.
 - The Power Consumption ranges from 0 - 11.2 in the month of August and September.
 - The Overall power consumption in the two months was the highest for `AC 18` with a sum of greater than 250,000.
 - `AC 1` was used the least amount of time and `AC 17` was used the mode amount of time. 

### **Missing Value Treatment**
Initially, I was thinking to use "Last Observation Carried Forward"(LOCF)/Backward Filling to fill the missing values, but that would disturb the patterns in the data as it is not practically possible that AC power consumption changes suddenly after a constant value for a particular amount of time.<br>
So, I tried another approach to fill the missing values considering that the power consumpiton follows `Gaussian Phenomenan`.<br>
```
AC power consumption depends on the Outside Temparature and temperature is a natural phenomenan and since most of the 
naturally ocurring phenomenan follows Gaussian Distribution, I used Gaussian Process Regression to predict the missing 
values considering given power consumption as y_train and the corresponding alternate sequence of 0-87839 as X_train 
and similarly the sequence corresponding to missing values became my X_test.
```
[Click here](https://scikit-learn.org/stable/auto_examples/gaussian_process/plot_gpr_noisy_targets.html#sphx-glr-auto-examples-gaussian-process-plot-gpr-noisy-targets-py) to read the basic implementation of **Gaussian Process Regression**.

But in order to apply Gaussian Process regression **Feature Scaling** was necessary so I **Standardised** the whole dataset before making predictions for missing value treatment.

After getting the predictions, the missing values were filled and **inverse_transformation** was done to get the original values back.

### Forecasting 
For future value prediction, I splitted my data into train and test with test set having the last 120 observations i.e. the data of last 2 hours.

 - **Vector Autoregression (VAR)**<br>
 ```In this method the assumption is made that the future values of a time series data depends on its own past values and also on the past values of other time series data.```
 Just like there are multiple AC's in the **dinning hall** or a **lounge** and all the AC's can't consume maximum power at the same time i.e. their power consumption is dependent on one another.
 So, with ```lag = 5```, I used this model for prediction and the evaluation metric used was **Mean Absolute Percentage Error(MAPE)**. The results are available in the notebook :)
 
 - **Facebook Prophet**<br>
 **Click here to view its implementation**:- [Click here](https://github.com/Gaurav1401/Zenatix_Solutions/blob/main/Part%20-%202.ipynb)<br>
 This approach was used with the consideration that power consumption of all AC's are independent of each other just like they are Room AC, AC temperature in one room won't decide the temperature in another room.<br>
 The results I got using this approach were not as good as **VAR**.<br>
 I used the concept of **Multiprocessing** to predict the value of each column parallely in order to reduce time.
 
### Conclusion:
 - Looking at the results of the two methods that I used and keeping their assumptions in mind, it can be said that this data is of those AC's which are located in a big room or a lounge in a **Gurugram hotel**.

### Challenges Faced:
 - It was very difficult to come up with an approch to fill the missing values which also preserves the patterns in the data.
 - It took me a lot of time to implement Gaussian Regression on the mssing values and then combining y_train and y_test alternately to get the final column with correct sequence of values.

### Future Work:
Due to lack of time, I could only apply two methods on the data but the evaluation score can be improved by:
 - Using a Deep Learning approach like LSTMs. (As the data is sufficiently large)
 - Using Auto ARIMA.(In case the values are independent of each other).
 
Both the approaches specified above take lot of time.
 
 

