![GitHub last commit](https://img.shields.io/github/last-commit/Ramkat12/dsc-phase-1-project?color=Blue&label=Phase1_project&logo=Project&logoColor=Blue)

# Phase 1 Project<bt>

# INTRODUCTION <br><hr>
<font color=green>

Hello .In this notebook , we will be working on various movie datasets so that we can help microsoft to be able to create their new studio.<br>
 When starting out something like a new business it is always good to be able to atleast have some knowledge about what you are  venturing into.Microsoft wants to create a new film studio as film studio business are on the rise, our goal in the lab is to help  them have some knowledge about the film business.For example using data analysis we can be able to tell the company the genre that has the most ratings so that they can produce more of thet genre's movies<br><br></font>

### Business Problem
<font color=green>
Now that we know microsoft needs to have some information about the film industry, what kind of information do you think they need,pause for a minute and think ,if you were to start a film studio what is it you would like to know first before venturing in the business?<br>
I would give an example like ,finding out the most watched genre of movies.Great,lets go ahead and find out what kind of information microsoft would like to know.</font>

* i)How much money should they set aside as production budget for the production of a movie*<br>
* ii)To know the amount of profit or loss that they could gain from the business both domestically and worldwide*<br>
* iii)To know the genre that sells the most*<br>
* iv)To know which type of movie rating they should produce more* <br>
* v)To know if the amount of money used for production budget could affect the amount of domestic gross or world wide gross gained form the sale of the movie.* <br>
* vi)To know if the popularity of the movie affects the  average voting*<br>

### Our approach to the business problem
* *i)Find the mean of the production budget column.**<br>
<font color=green>
Each movie has a production budget ,if we get the mean of the production budget ,this could be like an estimated value that microsoft could use when setting out a budget for one of their movies.<br></font>
* ii) Find the mean of the domestic gross column and worldwide gross column and subtracting the mean of production budget*<br>
<font color=green>
When we find the mean of the domestic gross and the worldwide gross this could be agian used as estimate values of the amount that could be gained from the sale of one of microsoft's movie. We could see if it would bring a profit or loss in one of the areas of sale by subtracting it from the production budget assuming that its the amount of money used to produce the movie.We could then recommend Microsoft to sell the movie either world wide or domestically according to our result.<br></font>

* iii)Group the genre by the average rating per genre then sort the values in a descending format select the first five*<br>
<font color=green>
Below is a graph of the top five genre's against their average rating<br></font>

```python
## Importing our necesary libraries
import seaborn as sns
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

```

``` python
### Loading our dataset
movie_info=pd.read_csv(r"C:\Users\Hp\Downloads\rt.movie_info.tsv.gz",sep='\t',encoding='latin1')
movie_info
```
``` python
## Calculating the count of each Genre and plotting it in our graph
movie_info.rating.value_counts().plot(kind='bar')
```

* iv)We shall caluculate the total number of each ratings in our dataset,the number with the most rating 

```python
### Reading our 
ratings_df=pd.read_csv(r"C:\Users\Hp\Downloads\imdb.title.ratings.csv.gz")
ratings_df
```
```python
# lets check for null values
ratings_df.isnull().sum()
```
```python
#lets check for outliers
ratings_df.describe()
```
``` python
#Our dataset doesn,t have null values but has some outliers
## Removing outliers that lie out side 1.5*IQR +|- q1 and q3 from the average rating column which has outliers
q1=ratings_df['averagerating'].quantile(0.25)
q3=ratings_df['averagerating'].quantile(0.75)
IQR=q3-q1
ratings_df = ratings_df[~((ratings_df['averagerating']<(q1-1.5*IQR)) | (ratings_df['averagerating']>(q3+1.5*IQR)))]

## Removing outliers that lie out side 1.5*IQR +|- q1 and q3 from the numvotes column which has some outliers
q1=ratings_df['numvotes'].quantile(0.25)
q3=ratings_df['numvotes'].quantile(0.75)
IQR=q3-q1
ratings_df = ratings_df[~((ratings_df['numvotes']<(q1-1.5*IQR)) | (ratings_df['numvotes']>(q3+1.5*IQR)))]
```

``` python
Reading the title basics
title_basics=pd.read_csv(r"C:\Users\Hp\Downloads\imdb.title.basics.csv.gz")
title_basics
```
```python
# to find if our data has missing values ,there are two methods either using the dataframe.info() 
# the other method is by counting the sum of null values dataframe.isnull().sum() this is what we shall use
title_basics.isnull().sum()
```
```python
## lets check for outliers inour 
title_basics.describe()

```
``` python
#cleaning the missing values
# I will replace the runtimeminutes with zero
title_basics['runtime_minutes']=title_basics['runtime_minutes'].fillna(value=0)
### i am going to replace the values in the original title with the values in the primary title since they are the same
title_basics['runtime_minutes']=title_basics['runtime_minutes'].fillna(value=0)
### I am going to replace the values in the missing values in the genre with unknown since the genre is unknown
title_basics['genres']=title_basics['genres'].fillna(value='unknown')
```

```python
# Removing outliers in our dataset
### To find the outliers we find the lower quantile and upper quantile then Calculate the interquatile Range to find where the middle
### fifty is Then find outliers by setting ourlimits to be 1.5*IQR of the upper and lower quartile
### Then we remove the outliers from our dataset
## runtime_minutes column
q1=title_basics['runtime_minutes'].quantile(0.25)
q3=title_basics['runtime_minutes'].quantile(0.75)
IQR=q3-q1
title_basics= title_basics[~((title_basics['runtime_minutes']<(q1-1.5*IQR)) | (title_basics['runtime_minutes']>(q3+1.5*IQR)))]

## start_year column
q1=title_basics['start_year'].quantile(0.25)
q3=title_basics['start_year'].quantile(0.75)
IQR=q3-q1
title_basics = title_basics[~((title_basics['start_year']<(q1-1.5*IQR)) | (title_basics['start_year']>(q3+1.5*IQR)))]

```
``` python
# Combining the DataFrames title_basics and  ratings_df
combined_genre_rating=title_basics.join(ratings_df,how='inner',lsuffix=True)
combined_genre_rating
```
``` python 

## We also group each genre by its mean rating 
## to get the most rated genre's we sort our values using the Mean of the ratings of each genre from those with the highest mean
## Those with the highest mean of rating means that the have most ratings

grouped_genre=combined_genre_rating.groupby(['genres'])['averagerating'].agg([('Mean', 'mean')])
grouped_genre=grouped_genre.sort_values(by='Mean',ascending=False)
grouped_genre.head()
grouped_genre=grouped_genre.rename(columns={'Mean':'Average_rating_per_genre'})
grouped_genre.head(20)
```
``` python
## in order for us to work with the genres we msut reset the index so that the genres can be a column
grouped=grouped_genre.reset_index()
grouped.columns
```
``` python 
### We can plot a scatter plot to show a plot of the top 5 genres against their average rating
import matplotlib.pyplot as plt
import seaborn as sns
data2=grouped.head(5)
sns.scatterplot(data=data2,x='genres',y='Average_rating_per_genre',s=100)
sns.set(rc={'figure.figsize':(15,8)})
sns.set(font_scale=2)
```

v)We find the correlation between the column production_budget,domestic_gross and worldwide_gross
``` python
## reading the 'tn.movie_budgets.csv' and storing it in the variable movie_budgets
movie_budgets=pd.read_csv(r"C:\Users\Hp\Downloads\tn.movie_budgets.csv.gz")
movie_budgets
```
```python
## Finding if our datset has null values
movie_budgets.isnull().sum()
```
```python
### This is a way for checking out outliers but our data doesn't have outliers since only the id column has numerical values
movie_budgets.describe(include='all')
```
```python
## removing the dollar sign in the production_budget,domestic_gross and worldwide_gross columns
movie_budgets['production_budget']=movie_budgets['production_budget'].map(lambda x:x[1:])
movie_budgets['domestic_gross']=movie_budgets['domestic_gross'].map(lambda x:x[1:])
movie_budgets['worldwide_gross']=movie_budgets['worldwide_gross'].map(lambda x:x[1:])
```
```python
### removing the commas and changing the datatype of the columns product_budget,domestic_budget and worldwide_gross
import string
### a) production_budget column
movie_budgets['production_budget']=movie_budgets['production_budget'].map(lambda x:x.strip(string.punctuation))
movie_budgets['production_budget']=movie_budgets['production_budget'].str.replace(',','').astype(float)
### b) domestic_gross column
movie_budgets['domestic_gross']=movie_budgets['domestic_gross'].map(lambda x:x.strip(string.punctuation))
movie_budgets['domestic_gross']=movie_budgets['domestic_gross'].str.replace(',','').astype(float)
### c) worldwide_gross column
movie_budgets['worldwide_gross']=movie_budgets['worldwide_gross'].map(lambda x:x.strip(string.punctuation))
movie_budgets['worldwide_gross']=movie_budgets['worldwide_gross'].str.replace(',','').astype(float)

### 1)so what basically is happenning is we are first removing the quatation marks and other punctuations within each cell value
### using the x.strip(string.punctuation), x.strip removes  punctations in our valus
### 2)What happens next is we are removing the comma separetor that separetes the digits in our value and replacing it with an 
### an empty factor using the str.replace
```
```python
## Removing outliers that lie out side 1.5*IQR +|- q1 and q3

q1=movie_budgets['domestic_gross'].quantile(0.25)
q3=movie_budgets['domestic_gross'].quantile(0.75)
IQR=q3-q1
movie_budgets = movie_budgets[~((movie_budgets['domestic_gross']<(q1-1.5*IQR)) | (movie_budgets['domestic_gross']>(q3+1.5*IQR)))]

## Removing outliers that lie out side 1.5*IQR +|- q1 and q3
q1=movie_budgets['worldwide_gross'].quantile(0.25)
q3=movie_budgets['worldwide_gross'].quantile(0.75)
IQR=q3-q1
movie_budgets = movie_budgets[~((movie_budgets['worldwide_gross']<(q1-1.5*IQR)) | (movie_budgets['worldwide_gross']>(q3+1.5*IQR)))]

```
```python
### lets start by finding the getting the columns we want ie production_budget,domestic_gross and worldwide_gross
df_correlation=movie_budgets.iloc[:,3:]
df_correlation
```
```python
## Calculating the correlation
df_correlation.corr()
```
```python
### Plotting a SCatter matrix to show the correlation between production_budget,Domestic_gross and worldwide_gross
pd.plotting.scatter_matrix(df_correlation,figsize=(15,10),alpha=0.5);
```
<font color= green >We can see there is apositive correlation between domestic gross and worldwidegross.The correlation between worldwide gross and domesticgross to production_budget is a neutral correlation </font>

# Conclusion 
From The analysis we have dine , we can conclude that:<br>
<font color=green>
a)  The popularity of a movie does not affect it average votes<br>
b) If more money is used in the production of movies, the amount of money in the world wide gross increases more and slightly for the domestic gross.<br>
c)The rating **R** has most number of ratings<br>
d)The Genre Animation,MyseryThriller has most average ratings<br></font>


# Recomendations
I would recommend that<br>
<font color=green>
a)Microsoft should focus on the following genres of movies since they have most averagerating:<br>
    <font color = purple> 
    -Animation,MyseryThriller<br>
    -Adventure,Romance,Sport<br>
    -Adventure,Music,Romance<br>
    -Family,Fantasy,Horror <br>
    -Comedy,Sci-Fi,Western
    </font><br>
 b)Microsoft should produce more of the **R** rating movies<br>
 c)Microsoft should use more money in production since it could amount to more worldwide profits<br>
 d)Microsoft  should sell more of their movies worldwide since it colud amout to a profit of upto **12080445.950397253**<br>
