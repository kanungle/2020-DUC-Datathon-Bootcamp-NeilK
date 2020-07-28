# Exercise 2 - Regression Analysis

TIBCO Spotifre has many tools to help you with Machine Learning and Statistical Analysis right out of the box :package:! In this quick example, you'll walk through some basic Well Production Data for the Bakken Formation, and determine the top predictors for best production without writing any code.

![Overview](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/images/Ex2%20-%20Results.png)

__:arrow_right: [Download Example DXP](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/Exercise%202%20-%20Regression%20Analysis/Exercise%202%20-%20Regression%20Analysis.dxp)__

#### Step-by-step
  1. Download and open the DXP provided above
  2. Go to Tools > Regression Modeling, and select:
  
    - Response column: Production at 12 Months (BOE)
	- Predictor columns: all the other columns except Latitude and Longitude
  3. Then click OK and review the output.
  4. Enlarge the variable importance plot to show which columns are predictive of Production at 12 Months (BOE).

    - What are the most important variables? 
	- How do the residuals compare to fitted values? 
	- Can you use Cook's Distance to identify outliers?


__:arrow_left: [Return to Main](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/README.md)__