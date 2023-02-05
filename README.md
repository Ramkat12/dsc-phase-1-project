![GitHub last commit](https://img.shields.io/github/last-commit/Ramkat12/dsc-phase-1-project?color=Blue&label=Phase1_project&logo=Project&logoColor=Blue)

# Phase 1 Project<bt>

# INTRODUCTION <br><hr>


Hello .In this notebook , we will be working on various movie datasets so that we can help microsoft to be able to create their new studio.<br>
 When starting out something like a new business it is always good to be able to atleast have some knowledge about what you are  venturing into.Microsoft wants to create a new film studio as film studio business are on the rise, our goal in the lab is to help  them have some knowledge about the film business.For example using data analysis we can be able to tell the company the genre that has the most ratings so that they can produce more of thet genre's movies<br><br>
 NOTE: If you happen to go through the codes and there somethings you don't understand,don't sweat it ,there will be  a detailed explanation in the notebook

### Business Problem

Now that we know microsoft needs to have some information about the film industry, what kind of information do you think they need,pause for a minute and think ,if you were to start a film studio what is it you would like to know first before venturing in the business?<br>
I would give an example like ,finding out the most watched genre of movies.Great,lets go ahead and find out what kind of information microsoft would like to know.

* How much money should they set aside as production budget for the production of a movie<br>
* To know the amount of profit or loss that they could gain from the business both domestically and worldwide<br>
* To know the genre that sells the most<br>
* To know which type of movie rating they should produce more <br>
* To know if the amount of money used for production budget could affect the amount of domestic gross or world wide gross gained form the sale of the movie. <br>
* To know if the popularity of the movie affects the  average voting<br>

### Our approach to the business problem
*  Find the mean of the production budget column.<br>
<font color=green>
Each movie has a production budget ,if we get the mean of the production budget ,this could be like an estimated value that microsoft could use when setting out a budget for one of their movies.<br></font>
* Find the mean of the domestic gross column and worldwide gross column and subtracting the mean of production budget<br>

When we find the mean of the domestic gross and the worldwide gross this could be agian used as estimate values of the amount that could be gained from the sale of one of microsoft's movie. We could see if it would bring a profit or loss in one of the areas of sale by subtracting it from the production budget assuming that its the amount of money used to produce the movie.We could then recommend Microsoft to sell the movie either world wide or domestically according to our result.<br>

* Group the genre by the average rating per genre then sort the values in a descending format select the first five<br>



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
 
|   id|	synopsis                                    |	rating	|genre|	director	       |writer       |theater_date|	dvd_date	|currency	|box_office|	runtime	studio|
|-----|---------------------------------------------|-------|------|-----------------|-------------|------------|----------|---------|----------|--------------|
|1|This gritty, fast-paced, and innovative police...|R	Action and Adventure|Classics|Drama|William Friedkin|Ernest Tidyman|Oct 9, 1971|Sep 25, 2001|NaN|NaN|104 minutes|NaN|
|3|New York City, not-too-distant-future: Eric Pa...|R	Drama|Science Fiction and Fantasy|David Cronenberg|David Cronenberg|Don DeLillo|Aug 17, 2012|Jan 1, 2013|$	600,000|108 minutes|Entertainment One|
|5|Illeana Douglas delivers a superb performance ...|R	Drama|Musical and Performing Arts|Allison Anders|Allison Anders|Sep 13, 1996|Apr 18, 2000|NaN|NaN|116 minutes|NaN|
|6|Michael Douglas runs afoul of a treacherous su...|R	Drama|Mystery and Suspense|Barry Levinson|Paul Attanasio|Michael Crichton|Dec 9, 1994|Aug 27, 1997|NaN|NaN|128 minutes|NaN|
|7|NaN|NR|Drama|Romance|Rodney Bennett|Giles Cooper|NaN|NaN|NaN|NaN|200 minutes|NaN|
 
 
 
``` python
## Calculating the count of each Genre and plotting it in our graph
movie_info.rating.value_counts().plot(kind='bar')
```
 Below is a graph of the top five genre's against their average rating<br></font>
![movierating](https://user-images.githubusercontent.com/109750154/216829895-958538c3-a069-44a8-84af-1605f82f87c9.png)


* iv)We shall caluculate the total number of each ratings in our dataset,the number with the most rating 

```python
### Reading our 
ratings_df=pd.read_csv(r"C:\Users\Hp\Downloads\imdb.title.ratings.csv.gz")
ratings_df.head()
```
 
 |tconst|averagerating|numvotes|
 |------|-------------|--------|
 |tt10356526|8.3|31|
 |tt10384606|8.9|559|
 |tt1042974|6.4|20|
 |tt1043726|4.2|50352|
 |tt1060240|6.5|21|
```python
# lets check for null values
ratings_df.isnull().sum()
```
  tconst           0<br>
averagerating    0<br>
numvotes         0<br>
dtype: int64<br>
 
```python
#lets check for outliers
ratings_df.describe()
```
|  |tconst|averagerating|numvotes|
|---|------|-------------|--------|
|count|73856	|3856.000000|7.385600e+04
|unique|73856|NaN|NaN|
|top|tt10356526|NaN|NaN|
|freq|1|NaN|NaN|
|mean|NaN|6.332729	|3.523662e+03|
|std|NaN|1.474978|3.029402e+04|
|min|NaN|1.000000|5.000000e+00|
|25%|NaN|5.500000|1.400000e+01|
|50%|NaN|6.500000|4.900000e+01|
|75%|NaN|7.400000|2.820000e+02|
|max|NaN|10.000000|1.841066e+06|


``` python
Reading the title basics
title_basics=pd.read_csv(r"C:\Users\Hp\Downloads\imdb.title.basics.csv.gz")
title_basics.head()
```
 
 
|  |tconst|primary_title|original_title|start_year|runtime_minutes|genres|
|---|------|-------------|--------------|---------|----------------|------|
|0|tt0063540|Sunghursh|Sunghursh|2013|175.0|Action,Crime,Drama|
|1|tt0066787|One Day Before the Rainy Season|Ashad Ka Ek Din|2019|114.0|Biography,Drama|
|2|tt0069049|The Other Side of the Wind|The Other Side of the Wind|2018|122.0|Drama|
|3|tt0069204|Sabse Bada Sukh|Sabse Bada Sukh|2018|NaN|Comedy,Drama|
|4|tt0100275|The Wandering Soap Opera|La Telenovela Errante|2017|80.0|Comedy,Drama,Fantasy|
```python
# to find if our data has missing values ,there are two methods either using the dataframe.info() 
# the other method is by counting the sum of null values dataframe.isnull().sum() this is what we shall use
title_basics.isnull().sum()
```
 tconst             0<br>
primary_title      0<br>
original_title     0<br>
start_year         0<br>
runtime_minutes    0<br>
genres             0<br>
dtype: int64<br>
```python
## lets check for outliers inour 
title_basics.describe()
```
 
| |	tconst|primary_title|original_title|start_year|runtime_minutes|genres|
|--|-------|-------------|-------------|-----------|---------------|------|
|count|146144|146144|146123|146144.000000|114405.000000|140736|
|unique|146144|136071|137773|NaN|NaN|1085|
|top|tt0063540|Home|Broken|NaN|NaN|Documentary|
|freq|1|24|19|NaN|NaN|32185|
|mean|NaN|NaN|NaN|2014.621798|86.187247|NaN|
|std|NaN|NaN|NaN|2.733583|166.360590|NaN|
|min|NaN|NaN|NaN|2010.000000|1.000000|NaN|
|25%|NaN|NaN|NaN|2012.000000|70.000000|NaN|
|50%|NaN|NaN|NaN|2015.00000|87.000000|NaN|
|75%|NaN|NaN|NaN|2017.000000|99.000000|NaN|
|max|NaN|NaN|NaN|2115.000000|51420.000000|NaN|
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
combined_genre_rating.head()
```
 
|  |tconst|primary_title|original_title|start_year|runtime_minutes|genres|averagerating|numvotes|
|--|------|-------------|--------------|----------|---------------|------|-------------|---------|
|0|tt0063540|Sunghursh|Sunghursh|2013|175.0|Action,Crime,Drama|7.0|77|
|1|tt0066787|One Day Before the Rainy Season|Ashad Ka Ek Din|2019|114.0|Biography,Drama|7.2|43|
|2|tt0069204|Sabse Bada Sukh|Sabse Bada Sukh|2018|0.0|Comedy,Drama|6.1|13|
|3|tt0100275|The Wandering Soap Opera|La Telenovela Errante|2017|80.0|Comedy,Drama,Fantasy|6.5|119|
|4|tt0112502|Bigfoot|Bigfoot|2017|0.0|Horror,Thriller|4.1|32|
``` python 
## A subset is a subdataset from our main dataset
## This subset has the various columns we need
subset=combined_genre_rating.iloc[:,5:]
subset.head()
 ```
 
| |genres|averagerating|numvotes|
|--|-----|-------------|--------|
|0|Action,Crime,Drama|7.0|77|
|1|Biography,Drama|7.2|43|
|2|Comedy,Drama|6.1|13|
|3|Comedy,Drama,Fantasy|6.5|119|
|4|Horror,Thriller|4.1|32|
 
 ```python
## now that we have our combined tables in the combined_genre_rating, we realize that we have columns that are not needed when
## finding the most rated genres hence we group the genre and the average rating which is the data that we want
## We also group each genre by its mean rating 
## to get the most rated genre's we sort our values using the Mean of the ratings of each genre from those with the highest mean
## Those with the highest mean of rating means that the have most ratings
### We also want the genre that has also ahigh number of votes
## Because a genre  might have high rating from only 5 votes

grouped_genre=subset.groupby(['genres'])['averagerating','numvotes'].agg([('Mean', 'mean'),('Sum','sum')])
```
 ```python
 
 ## we are splitting the multi-indexed columns to single columns to make it easy for us to code
## Don't sweat about the code just know that instead of having one single column that is average rating which has subcolumns mean and sum
## We will now have two columns averagerating_Mean,averagerating_Sum
cols0 = grouped_genre.columns.get_level_values(0)
cols1 = grouped_genre.columns.get_level_values(1)
grouped_genre.columns = [col0 + '_' + col1 if col1 != '' else col0 for col0, col1 in list(zip(cols0, cols1))]
# The list comprehension above is more complicated then what we need but creates a nicer formatting and
# demonstrates using a conditional within a list comprehension.
# This simpler version works but has some tail underscores where col1 is blank:
# grouped.columns = [col0 + '_' + col1 for col0, col1 in list(zip(cols0, cols1))]
grouped_genre.columns
 Now this are the columns we have
```
Index(['averagerating_Mean', 'averagerating_Sum', 'numvotes_Mean',
       'numvotes_Sum'],
      dtype='object')

``` python
## in order for us to work with the genres we msut reset the index so that the genres can be a column
grouped=grouped_genre.reset_index()
grouped.columns
```
 ```python
 ### Remember when I said that we want to find genres that not only have high rating but also high number of votes
### We now set a limit for the number of votes where any genre whose sum is below this limit is not
### consudered as genre with high rating
## Again you may wonder why I am using median ,this is because if I use the mean the number of votes that are high will skew 
## My accuracy hence I think using median is better
lower_limit=grouped_genre.numvotes_Sum.median()
 ```
 ```python
 ### Here we are now slicing our dataset to the values that are above our limit
grouped_genre=grouped_genre.loc[grouped_genre['numvotes_Sum']>lower_limit]
## We are now sorting our data according to the average rating and renaming the column so that you can easily understand what we are doing
grouped=grouped_genre.sort_values('averagerating_Mean',ascending=False)
grouped.rename(columns={'averagerating_Mean':'Average_rating_per_genre'},inplace=True)
grouped.reset_index()
## Now our well aranged data set looks like this
 
 ```
| |genres|Average_rating_per_genre|averagerating_Sum|numvotes_Mean|numvotes_Sum|
|--|-----|-------------------------|-----------------|-------------|------------|
|0|Animation,Crime,Mystery|8.200000|8.2|526.000000|526|
|1|Documentary,Family,Sport|8.062500|129.0|55.000000|880|
|2|Action,Documentary,Sport|7.866667|141.6|71.166667|1281|
|3|Adventure,Animation,Horror|7.850000|15.7|254.500000|509|
|4|Animation,Drama,History|7.800000|7.8|584.000000|584|
``` python 
### We can plot a scatter plot to show a plot of the top 5 genres against their average rating
import matplotlib.pyplot as plt
import seaborn as sns
data2=grouped.head(5)
sns.scatterplot(data=data2,x='genres',y='Average_rating_per_genre',s=100)
sns.set(rc={'figure.figsize':(15,8)})
sns.set(font_scale=2)
```
 ![newgenre](https://user-images.githubusercontent.com/109750154/216836104-18c715a0-6bf9-4fe5-bc65-0353ee5470d4.png)

v)We find the correlation between the column production_budget,domestic_gross and worldwide_gross
``` python
## reading the 'tn.movie_budgets.csv' and storing it in the variable movie_budgets
movie_budgets=pd.read_csv(r"C:\Users\Hp\Downloads\tn.movie_budgets.csv.gz")
movie_budgets.head()
```
|   |id|release_date|movie|production_budget|domestic_gross|worldwide_gross|
|----|---|-----------|-----|-----------------|--------------|--------------|
|0|1|Dec 18, 2009|Avatar|$425,000,000|$760,507,625|$2,776,345,279|
|1|2|May 20, 2011|Pirates of the Caribbean: On Stranger Tides|$410,600,000|$241,063,875|$1,045,663,875|
|2|3|Jun 7, 2019|Dark Phoenix|$350,000,000|$42,762,350|$149,762,350|
|3|4|May 1, 2015|Avengers: Age of Ultron|$330,600,000|$459,005,868|$1,403,013,963|
|4|5|Dec 15, 2017|Star Wars Ep. VIII: The Last Jedi|$317,000,000|$620,181,382|$1,316,721,747|
```python
## Finding if our datset has null values
movie_budgets.isnull().sum()
```
 id                   0<br>
release_date         0<br>
movie                0<br>
production_budget    0<br>
domestic_gross       0<br>
worldwide_gross      0<br>
dtype: int64<br>
```python
### This is a way for checking out outliers but our data doesn't have outliers since only the id column has numerical values
movie_budgets.describe(include='all')
```
| |id|release_date|movie|production_budget|domestic_gross|worldwide_gross|
|--|---|----------|-----|-----------------|---------------|--------------|
|count|5782.000000|5782|5782|5782|5782|5782|
|unique|NaN|2418|5698|509|5164|5356|
|top|NaN|Dec 31, 2014|Halloween|$20,000,000|$0|$0|
|freq|NaN|4|3|231|548|367|
|mean|50.372363|NaN|NaN|NaN|NaN|NaN|
|std|28.821076|NaN|NaN|NaN|NaN|NaN|
|min|1.000000|NaN|NaN|NaN|NaN|NaN|
|25%|25.000000|NaN|NaN|NaN|NaN|NaN|
|50%|50.000000|NaN|NaN|NaN|NaN|NaN|
|75%|75.000000|NaN|NaN|NaN|NaN|NaN|
|max|100.000000|NaN|NaN|NaN|NaN|NaN|
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
 movie_budgets.head()
```
| |id|release_date|movie|production_budget|domestic_gross|worldwide_gross|
|---|----|---------|------|----------------|-------------|----------------|
|0|1|Dec 18, 2009|Avatar|425000000.0|760507625.0|2.776345e+09|
|1|2|May 20, 2011|Pirates of the Caribbean: On Stranger Tides|410600000.0|241063875.0|1.045664e+09|
|2|3|Jun 7, 2019|Dark Phoenix|350000000.0|42762350.0|1.497624e+08|
|3|4|May 1, 2015|Avengers: Age of Ultron|330600000.0|459005868.0|1.403014e+09|
|4|5|Dec 15, 2017|Star Wars Ep. VIII: The Last Jedi|317000000.0|620181382.0|1.316722e+09|
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
df_correlation=df_correlation.corr()
 df_correlation.head() 
```
| |production_budget|domestic_gross|worldwide_gross|
|--|----------------|---------------|---------------|
|434|92000000.0|22406362.0|23911362.0|
|435|92000000.0|10017322.0|18517322.0|
|466|90000000.0|62362560.0|143049560.0|
|468|90000000.0|60874615.0|106000000.0|
|470|90000000.0|58220776.0|87420776.0|
```python
### Plotting a SCatter matrix to show the correlation between production_budget,Domestic_gross and worldwide_gross
pd.plotting.scatter_matrix(df_correlation,figsize=(15,10),alpha=0.5);
```
 ![correlations](https://user-images.githubusercontent.com/109750154/216837312-f0224621-cd1d-4326-a786-423e52739039.png)
We can see there is apositive correlation between domestic gross and worldwidegross.The correlation between worldwide gross and domesticgross to production_budget is a neutral correlation 

# Conclusion 
From The analysis we have dine , we can conclude that:<br>

a)  The popularity of a movie does not affect it average votes<br>
b) If more money is used in the production of movies, the amount of money in the world wide gross increases more and slightly for the domestic gross.<br>
c)The rating **R** has most number of ratings<br>
d)The Genre Animation,MyseryThriller has most average ratings<br></font>


# Recomendations
I would recommend that<br>

a)Microsoft should focus on the following genres of movies since they have most averagerating:<br>
    <font color = purple> 
    -Animation,MyseryThriller<br>
    -Adventure,Romance,Sport<br>
    -Adventure,Music,Romance<br>
    -Family,Fantasy,Horror <br>
    -Comedy,Sci-Fi,Western
   <br>
 b)Microsoft should produce more of the **R** rating movies<br>
 c)Microsoft should use more money in production since it could amount to more worldwide profits<br>
 d)Microsoft  should sell more of their movies worldwide since it colud amout to a profit of upto **12080445.950397253**<br>
