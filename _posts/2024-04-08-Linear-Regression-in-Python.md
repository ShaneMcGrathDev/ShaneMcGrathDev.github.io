---
layout: post
title: "Build Your First Prediction Model in Python using Linear Regression"
date: 2024-04-08
categories: Python, Linear Regression, Pandas, Forecasting
---


<!-- ![Railroads]({{ "/assets/LinearPost.jpg" | relative_url }})
_Photo by [Photo by Ryan Stone on Unsplash]_ -->

![Blog Post Pic3]({{ site.baseurl }}/assets/LinearPost.jpg)


This tutorial will walk you through how to make use of predictive analytics using Python and Excel data in a beginner-friendly manner.

You will also see how flexible and versatile Python can be which will help you add new skills to your data skill stack.

## The business case for prediction

When it comes to building data models for prediction, it helps to position these efforts in the context of business need and value.

Professionals who work with Excel Data often need to forecast the future in functional areas such as marketing, finance, or supply chain.

An intelligent business will always value informed predictions about the future because it can dramatically improve planning activities resulting in lower costs, increased sales, optimized headcount, stronger strategy, and better execution.

Every percentage of accuracy a predictive model can achieve can result in incremental cost savings, efficient resource allocation, avoided waste, and help improve your company’s operational performance overall.

Further I would add that businesses that do not execute with a predictive clarity of the future miss out on developing a key strategic capability which can both sustain and drive success.

Python is one of the most business friendly programming languages, and serves predictive planning activities very well. With Python you can create basic to advanced prediction models to suit your needs.

Combining business knowledge with a flexible predictive analytics capability is extremely powerful.

## Lesson goals and benefits
Now that we have some business context to set the stage for your predictive analytics efforts, we will walk through a method to set up a basic linear regression model in Python so you can start on the path of building your own models using Python.

## A few things we will cover:

- Using a collection of useful data and prediction libraries
- Visualize data in Python (in this case, our linear regression)
- How to set up a basic linear regression model
- How to apply the regression model to a simple data set to produce future values
- How to write the output to an Excel file (your prediction)

## Prerequisites
- You should have Python installed on your machine with a code editor of your choice. I personally use VS code and Anaconda Navigator.
- Two Excel files: A training data set (with an x column and a y column set of values), and a separate prediction set-a single set of x values you’d like to predict the dependent y variable for (battery inventory demand in my case)
- Some basic knowledge of Python syntax is helpful but not completely necessary since we will walk through it step by step.
- If you have existing experience with NumPy and Pandas libraries, that is helpful too but again not required.

## What is Linear Regression?

- Linear regression is a fairly basic, beginner-friendly statistical prediction model
- The goal with linear regression is to optimize the fit between two variables, which can be calculated mathematically and graphed visually
- The independent variable x, drives a response dependent variable y
- With an observed set of x/y variable pairs, data can be trained to produce a regression model which can then in turn be used to predict future values
- Linear regression is not a perfect fit for every type of scenario and there are a multitude of other model variations, but it is still a useful model to learn and apply in the right scenarios.

I have used this model in my own work. The Excel data I use is monthly battery powered screwdriver placements and corresponding battery sales demand.

The model example we’ll walk through was used in inventory forecasting to model the relationship between the battery powered screw driver(x) and how many batteries will be sold(y). Historical data was fed into the Python script to train the regression model, and then the model was applied to predict future values.

The basic assumption in my data set is as follows: As demand rises for drivers, more batteries are consumed. So I wanted to predict future battery needs as a quantity with my model using forecasted x values (the number of driver placements for the future in the business).

We’ll also gauge how correlated these drivers and batteries are as part of the walkthrough.

# Step 1: Import your libraries
The first step is to start a new Python file and import the needed libraries. I added some comments for a little extra context (denoted with #):

```
 # Fundamental data handling libraries
import numpy as np 
import pandas as pd

# Visualization to chart the variable's linear relationship
import matplotlib.pyplot as plt 

#Regression and statistical model libraries to build the prediction model
from scipy import stats
from sklearn.linear_model import LinearRegression
```

# Step 2: Read your Excel files into Python for use
To do this use the code below, and change the file paths to match yours. You can see a sampling of my training data below for context if you need it.

The df and df2 are variables containing your Excel data sets, which are converted into a Python Pandas compatible data structure, called a DataFrame with the aid of the Python Pandas library.

DataFrames are how the Python Library Pandas stores your excel data for use and manipulation. Once your data frame is established, you can do a massive variety of data analysis and manipulation using this data structure.

Your data analysis does not need to end with the regression model we are building, you can keep exploring more applications.

```
#Step2: Read the Excel files into Python as DataFrames
df = pd.read_excel('TrainingDataV2.xlsx')
df2 = pd.read_excel('PredictedForecast.xlsx')
```

A view of the simple training data set: existing x and y value pairs to establish the regression model

Prediction file with only the x values we are looking to predict the y for
Step 3: Store DataFrame columns as arrays to be used as regression variables
Now that you’ve established your DataFrame, you’ll store the columns from the files as arrays, another data structure which is a sequence of values of a similar data type (in our case integers), which will be used as your x and y variable inputs for your regression model.

The x and y variables represent our existing data results, corresponding x and y pairs, which will be used to train our new prediction model.

the new_x variable is the single column of x values, where we are looking to predict corresponding y values for.

The bracketed names denote specific columns in the DataFrame which you can see match the column header names in the Excel table examples in step 2.

Essentially we are plucking out columns from the DataFrame, originally from your Excel file, and using them as an array. The .values is what converts the DataFrame column to the different array data structure, one dimensional (1D) arrays.

Keep the 1D array in the back of your mind because we will convert it to a different data structure to build our model.

#Step 3: Convert the desired columns within the two dataframes into arrays to begin the regression model
x = df['Driver Balance'].values
y = df['Battery Demand'].values
new_x = df2['QTY'].values
Step 4: Calculate the regression and plot the variables and visualize them
Add this code next:

#Step 4: Regression calculations charting 
#Plot the regression of x and y
slope, intercept, r, p, std_err = stats.linregress(x, y)

#Print the r value variable 
print('The R value for the regression to understand correlation is:')
print(r)

#Define function 
def myfunc(x):
  return slope * x + intercept

#Store the linear equation as a variable for plotting purposes 
mymodel = list(map(myfunc, x))
This code creates new variables which store the key measurements of the linear regression by using the stats module from the linear regression library.

The first line accepts our x and y variables as inputs to calculate the slope, intercept, r, p, and standard error of the regression. Essentially this is the mathematical representation of our training data.

The next lines print out the r value when you run the code which displays in the terminal when the script executes. The r value, the correlation coefficient, helps quantify to what extent x affects y. A 1 would be a perfect positive correlation, a -1 would be a perfect negative correlation.

For reference my training set has the following r value which is considered to be fairly strong correlation (0.8 or greater as a relative rule of thumb):


The next code lines define a function which represents the linear regression which is then stored in the mymodel variable to prepare for plotting on the scatter plot.

Next, let’s see the values visually on a scatter plot to see how the coordinates of the x and y variables are dispersed. This is accomplished using the matplotlib library:

plt.scatter(x, y)
plt.plot(x, mymodel)
plt.show()
This code produces a nice pop up visual chart of our linear regression where we can see our x and y values positioned on a scatter plot and see the line which best fits the value pairs.


Step 5: Reshape the 1D arrays to 2D arrays
Remember the 1D arrays we extracted from the DataFrame?

In order to use our variables in the prediction library sklearn, it requires us to adjust our data structure. We use this code to adjust our 1D arrays to 2D arrays (known as reshaping): Think of a 2D array as a a row and column structured set of values (of a similar type), versus a single line. This reshaping is commonly needed in machine learning model development as well.

#Step 5 Convert the variable values from 1D to 2D arrays so sklearn accepts it:
x = np.array(x).reshape(-1, 1)
y = np.array(y).reshape(-1, 1)
new_x = np.array(new_x).reshape(-1, 1)
Step 6: Train the model
Next add this code to pass the x and y variables into the regression model and store as a variable.

#Step 6: Train your model on linear regression 
model = LinearRegression()
model.fit(x, y)
Step 7: Apply the trained model to predict new values
Next, add this code block:

#Step 7: Use the model to predict new y values
y_pred = model.predict(new_x)
What we are doing here is generating our prediction values by feeding in our new_x values into our model. We now have a stored list of output values which we will write to excel for access and use.

Step 8: Write the prediction values to Excel
Now that we’ve passed our Excel values into Python, converted them to regression variables, visually charted our values, generated our prediction model, and predicted additional values, we can write our results into an Excel file.

Add this code here (adjust your file names as needed):

# Step 8: Use your predicted ND Array output and turn it into an adjacent column in the dataframe where your X values you wanted predictions for is housed
df2['Prediction'] = y_pred

# create excel writer object, write your dataFrame with X and predicted y values to it, and save the file-output should be ready
writer = pd.ExcelWriter('PredictedForecast.xlsx')
# write dataframe to excel
df2.to_excel(writer)
# save the excel
writer.save()
print('DataFrame is written successfully to Excel File.')
This code stores our new prediction set into a DataFrame column named “Prediction” in our existing DataFrame df2. This gives us an x and y value set in Excel format similar to the following:


The predicted y values with corresponding x variable
Then, we write the the updated dataframe into the file path and save the file. From this point the file should be ready for use.

Wrap up
Whew! We covered a lot of ground! My hope is this has given a good foundational taste for what is possible with data analysis, manipulation, and predictive modelling uses in Python.

And never lose sight of the value of prediction in business!