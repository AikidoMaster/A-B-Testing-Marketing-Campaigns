
# A/B Testing Marketing Campaigns

## Project Overview

This project involves A/B testing to compare the effectiveness of two marketing campaigns: a Control Campaign and a Test Campaign. The goal is to identify which campaign strategy yields better results in terms of customer engagement and conversions. 

We will analyze metrics such as impressions, website clicks, content views, products added to the cart, and purchases. The project involves importing datasets, preparing the data, performing A/B testing, and visualizing the results.

## Table of Contents
1. [Getting Started](#getting-started)
2. [Data Import and Libraries](#data-import-and-libraries)
3. [Data Preparation](#data-preparation)
4. [A/B Testing Analysis](#ab-testing-analysis)
    - [Impressions vs. Amount Spent](#impressions-vs-amount-spent)
    - [Searches Received](#searches-received)
    - [Website Clicks](#website-clicks)
    - [Content Viewed](#content-viewed)
    - [Products Added to Cart](#products-added-to-cart)
    - [Amount Spent](#amount-spent)
    - [Purchases](#purchases)
5. [Conclusion](#conclusion)
6. [Dependencies](#dependencies)

## Getting Started

To run this project, clone the repository and ensure you have the necessary dependencies installed. You can install the required Python packages using the following command:

```bash
pip install -r requirements.txt
```

## Data Import and Libraries

We start by importing the necessary Python libraries and reading the datasets for both the Control and Test campaigns.

```python
import pandas as pd
import datetime
from datetime import date, timedelta
import plotly.graph_objects as go
import plotly.express as px
import plotly.io as pio
pio.templates.default = "plotly_white"

control_data = pd.read_csv("control_group.csv", sep=";")
test_data = pd.read_csv("test_group.csv", sep=";")
```

## Data Preparation

### Inspecting the Data

We first inspect both datasets to understand their structure and identify any data quality issues:

```python
print(control_data.head())
print(test_data.head())
```

### Renaming Columns

To standardize the datasets, we rename the columns for better readability:

```python
control_data.columns = ["Campaign Name", "Date", "Amount Spent", 
                        "Number of Impressions", "Reach", "Website Clicks", 
                        "Searches Received", "Content Viewed", "Added to Cart",
                        "Purchases"]

test_data.columns = ["Campaign Name", "Date", "Amount Spent", 
                     "Number of Impressions", "Reach", "Website Clicks", 
                     "Searches Received", "Content Viewed", "Added to Cart",
                     "Purchases"]
```

### Handling Missing Values

We identify missing values in the Control Campaign dataset and fill them with the mean value of the respective columns:

```python
control_data.fillna(control_data.mean(), inplace=True)
```

### Merging Datasets

We then merge both datasets for a unified view and sort them by date:

```python
ab_data = control_data.merge(test_data, how="outer").sort_values(["Date"]).reset_index(drop=True)
```

## A/B Testing Analysis

### Impressions vs. Amount Spent

We begin by analyzing the relationship between the number of impressions and the amount spent on both campaigns. The Ordinary Least Squares (OLS) regression is used to understand this relationship.

**Formula for OLS Regression**:

\[ \hat{y} = \beta_0 + \beta_1 x \]

Where:
- \( \hat{y} \) is the predicted value.
- \( \beta_0 \) is the intercept.
- \( \beta_1 \) is the coefficient for the independent variable \( x \) (in this case, the amount spent).

```python
figure = px.scatter(data_frame=ab_data, 
                    x="Number of Impressions",
                    y="Amount Spent", 
                    size="Amount Spent", 
                    color="Campaign Name", 
                    trendline="ols")
figure.show()
```

### Searches Received

We then compare the total number of searches received from both campaigns using a pie chart:

```python
label = ["Total Searches from Control Campaign", "Total Searches from Test Campaign"]
counts = [sum(control_data["Searches Received"]), sum(test_data["Searches Received"])]
colors = ['gold','lightgreen']
fig = go.Figure(data=[go.Pie(labels=label, values=counts)])
fig.update_layout(title_text='Control Vs Test: Searches')
fig.update_traces(hoverinfo='label+percent', textinfo='value', 
                  textfont_size=30, marker=dict(colors=colors, 
                                                line=dict(color='black', width=3)))
fig.show()
```

### Website Clicks

Similarly, we compare the total number of website clicks from both campaigns:

```python
label = ["Website Clicks from Control Campaign", "Website Clicks from Test Campaign"]
counts = [sum(control_data["Website Clicks"]), sum(test_data["Website Clicks"])]
colors = ['gold','lightgreen']
fig = go.Figure(data=[go.Pie(labels=label, values=counts)])
fig.update_layout(title_text='Control Vs Test: Website Clicks')
fig.update_traces(hoverinfo='label+percent', textinfo='value', 
                  textfont_size=30, marker=dict(colors=colors, 
                                                line=dict(color='black', width=3)))
fig.show()
```

### Content Viewed

Next, we analyze the content viewed on the website by users from both campaigns:

```python
label = ["Content Viewed from Control Campaign", "Content Viewed from Test Campaign"]
counts = [sum(control_data["Content Viewed"]), sum(test_data["Content Viewed"])]
colors = ['gold','lightgreen']
fig = go.Figure(data=[go.Pie(labels=label, values=counts)])
fig.update_layout(title_text='Control Vs Test: Content Viewed')
fig.update_traces(hoverinfo='label+percent', textinfo='value', 
                  textfont_size=30, marker=dict(colors=colors, 
                                                line=dict(color='black', width=3)))
fig.show()
```

### Products Added to Cart

We then compare the number of products added to the cart from both campaigns:

```python
label = ["Products Added to Cart from Control Campaign", "Products Added to Cart from Test Campaign"]
counts = [sum(control_data["Added to Cart"]), sum(test_data["Added to Cart"])]
colors = ['gold','lightgreen']
fig = go.Figure(data=[go.Pie(labels=label, values=counts)])
fig.update_layout(title_text='Control Vs Test: Added to Cart')
fig.update_traces(hoverinfo='label+percent', textinfo='value', 
                  textfont_size=30, marker=dict(colors=colors, 
                                                line=dict(color='black', width=3)))
fig.show()
```

### Amount Spent

We then compare the total amount spent on both campaigns:

```python
label = ["Amount Spent in Control Campaign", "Amount Spent in Test Campaign"]
counts = [sum(control_data["Amount Spent"]), sum(test_data["Amount Spent"])]
colors = ['gold','lightgreen']
fig = go.Figure(data=[go.Pie(labels=label, values=counts)])
fig.update_layout(title_text='Control Vs Test: Amount Spent')
fig.update_traces(hoverinfo='label+percent', textinfo='value', 
                  textfont_size=30, marker=dict(colors=colors, 
                                                line=dict(color='black', width=3)))
fig.show()
```

### Purchases

Finally, we analyze the number of purchases made from both campaigns:

```python
label = ["Purchases Made by Control Campaign", "Purchases Made by Test Campaign"]
counts = [sum(control_data["Purchases"]), sum(test_data["Purchases"])]
colors = ['gold','lightgreen']
fig = go.Figure(data=[go.Pie(labels=label, values=counts)])
fig.update_layout(title_text='Control Vs Test: Purchases')
fig.update_traces(hoverinfo='label+percent', textinfo='value', 
                  textfont_size=30, marker=dict(colors=colors, 
                                                line=dict(color='black', width=3)))
fig.show()
```

## Conclusion

Based on the A/B testing results, we conclude that:

- The **Control Campaign** resulted in more content views and products added to the cart, indicating higher engagement despite fewer website clicks.
- The **Test Campaign** had higher expenditures and led to more website clicks and searches, but did not translate as effectively into conversions as the Control Campaign.

Therefore, the **Control Campaign** is deemed more efficient, yielding better results with less expenditure.

## Dependencies

The project requires the following Python libraries:

- `pandas`
- `datetime`
- `plotly`

These can be installed via pip:

```bash
pip install pandas plotly
```
