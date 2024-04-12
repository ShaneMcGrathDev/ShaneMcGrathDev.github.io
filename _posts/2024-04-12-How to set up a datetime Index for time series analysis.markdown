---
layout: post
title: "How to Set Up a DateTime Index for Time Series Analysis With Pandas"
date: 2024-04-12
categories: Pandas, time series analysis
---

![Title pic]({{ "/assets/time_series_title_pic.jpg" | relative_url }}) Photo by Jon Tyson on Unsplash

So, you're looking to analyze data dealing with time and you'd like to use Python. Maybe you're new to this style of analysis and not sure where to start. 

A great entrypoint is to set up your dataset with a datetime index, which sets the stage for powerful time series analysis capabilities. 

I'll walk you through how to do this using one library (Pandas) and a little baby data set which you can recreate that I made, shown here:

![Data set time series]({{ "/assets/time_series_post_1.jpg" | relative_url }}) A view of my data set

## How to set up the datetime index

If you're brand new to Python analysis, all good, these two ideas are helpful:

- **DataFrame**-think of this as your Excel or CSV tabular data but within Python. Like a spreadsheet it has columns, rows, etc. You can write all kinds of code and act on the dataframe, or use multiple dataframes. Very powerful stuff. 

- **Index**-This is similar to an index in Excel. It's the overall sequence of your dataFrame which can be used to manipulate the dataframe in a ton of different ways. For our purpose-we set up a special, time based, index, as opposed to a simple numerical one. 

You just need an excel file of data to get started, added to your code editor of choice.

Start by importing Pandas. This is the only library we'll use today.

**Here's the code:**

```
Import pandas as pd

```
Easy enough. If you don't have it yet you'll need to install it. It's an easy process but we won't cover it today.

There's two ways that I can show you to set up your special datetime index. The easy way I'll show you first accomplishes 3 things in one line of code:

- Reads your Excel dataset into Pandas, turning it into the Python usable data structure we touched on called a dataframe
- Make the 0 index column (Your first Excel column) the main index of your dataFrame
- Take your date column data, which starts as a less useful string data type, and convert it to a highly useful datetime data structure

**Here's the code:**

```
time_series = pd.read_excel('Dataset_Dummy Grocery.xlsx', index_col=0, parse_dates=True)
```

If you'd like, have a look at your newly created dataFrame using print():

```
print(time_series)

```
This will show you the whole data set. You can also use head() and tail() to look at the top or bottom of a dataset as well, useful for bigger datasets, not so relevant with our mini data table in this example. 


## A things to consider 

- In my code example, column 0 happens to be the date index, but you can specify any column as the index by it's number (remember, your first column = 0 and not 1, this is called 0 based indexing)
- You can read a CSV file in instead, just change the method to read_csv 
- Don't forget the file extension in the file name argument, otherwise the read in won't work


So if you stop here you're good to go and have created your time based index. But I want to cover two additional topics:

- A bit more about the datetime index for extra context
- An alternate way to set up the index (2 lines of code instead of 1)

## What is the datetime index? 

When you run the code we covered, your date column within your dataFrame becomes a new data type called **datetime64[ns]**

This date type is useful to Python itself because it converts strings into a more efficient 64 bit integer which reduces memory burden and improves the speed of computation. The [ns] means the integer is measured down to the nanosecond level. Pretty cool stuff, and applicable in some industries such as finance and engineering. Who knows, maybe this is relevant to you! 

This data type is useful to us as data analysts because it unlocks a massive amount of functionality including charting, slicing and dicing the data, resampling the data, and analyzing the data at different time scales. 

## Alternate set up

As promised, here is how we set up the data frame using the two step method.

Once we read in the dataFrame, we first target the date column and convert it to dateTime structure. 

**Here's the code:**

```
time_series['Date'] = pd.to_datetime(time_series['Date'])

```
This step only converts the column to the datetime64[ns] structure, but doesn't make it the index. We have to do that separately using more code. 

Side note-This method is helfpul to see because it shows you how to act on columns within your dataFrame, useful in many other types of analysis. I use this heavily in my work. 


Next, we set the date column as the index

**Here's the code:**

```
time_series = time_series.set_index('Date')
```
Now we are set up with a datetime index in our dataframe once again, albeit in a slightly less efficient code. If you like this method, probably no harm in using it.


## One final thought

So now you've seen two ways to create your datetime index and next can begin to venture into different analysis with your super useful datetime index. I'll walk you through additional analysis methods in future posts.


I have one cool thing that is a little technical to leave you with as an optional technique when setting up your datetime index.

When you use the easy method we walked through, you can optinally create a function, and pass it into your original datetime set up code. A function is just a reusable chunk of code you can call whenever you need it. 

This function allows you to specify the format of the datetime index. This becomes increasingly important with large datasets because it speeds up computation. My example shows one type of format, but you can research many of the alternatives to suit your needs. 

**Note** You can only use this technique if your date column string data (before conversion) is all identical in format. If not, you'll need to preprocess the data set to enable this. 

**Here's the code:**

```
import pandas as pd

#Your custom function to format your datetime gets defined here:
def custom_date_parser(date_string):
    return pd.to_datetime(date_string, format='%H:%M:%S')

#The original code with the date_parser parameter added
time_series = pd.read_excel('Dataset_Dummy Grocery.xlsx', index_col=0, parse_dates=True, date_parser=custom_date_parser)
```
