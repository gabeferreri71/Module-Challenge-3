# Module-Challenge-3

In this challenge, we will be looking at two bitcoin exchanges and calculating related arbitrage opportunities.

# Crypto Arbitrage

In this section, we include our libraries and dependencies. We will be importing pandas and numpy, as well as our import path statement. 
%mathplotlib inline is included for developing plots of our data later on. 

# Collect the Data

We first need to create dataframes for both the bitstamp and coinbase .csv files. To do this we first create a variable, bitstamp_df or coinbase_df, and assign it to a placeholder variable, in this case being bitstamp_dataframe and coinbase_dataframe, respectively. That variable is then assigned to the pd.read_csv function, where the specific path for each .csv file is set, followed by index_col = 'Timestamp', parse_dates = True, and infer_datetime_format = True all within the pd.read_csv function. The variables within the read_csv function will format our data properly for the following steps.

To make sure this process worked, we then used the .head() function to preview the first five rows of data.

# Prepare the Data

To properly prepare the data, we first will drop all missing values (NaN). To see the missing values, we used the .isnull().sum() function with either the bitstamp_df or coinbase_df dataframes to sum how many values are NaN. Next, we use the .dropna() function to drop all of the missing values with each data frame, followed by repeating the .isnull().sum() function to then make sure that the are no NaN values (everything should read 0). 

With NaN values dropped, we now must remove the "$" from the 'Close' column of data, followed by changing the 'Close' values to floats. To remove the "$", we use str.replace function as well as the .loc function. Using bitstamp_df and/or coinbase_df, we "locate" the 'Close' column with bitstamp_df.loc[:, "Close"] which equates to locating all the rows but just the 'Close' column. This is then assigned to the same thing, bitstamp_df.loc[:, "Close"] = bitstamp_df.loc[:, "Close"].str.replace("$", ""), which uses the string replace function to change the "$" to nothing. A .head() function can be ran to validate the "$" is gone on both dataframes.

With the "$" gone, we now convert the 'Close' values to floats for further analysis using the .astype function. Similar to the last step, we have our dataframe using .loc for the 'Close' column assigned to itself with the .astype("float") added to change the data type, as can be seen as bitstamp_df.loc[:, "Close"] = bitstamp_df.loc[:, "Close"].astype("float"), once again followed by a .head() function of the specific dataframe.

The last step for both dataframes are to drop any duplicate values. Similar to the isnull().sum() function, we will first run .duplicated().sum() to find the number of duplicated values. We next take our dataframe, and assign it to itself using the .drop_duplicates() function; for example, bitstamp_df = bitstamp_df.drop_duplicates(). We then run the .duplicated().sum() function again to make sure there are 0 duplicates.

# Analyze the Data

To analyze the data, we just want the 'Close' prices with the associated Timestamp information. To get these, we create ariables bitstamp_sliced and coinbase_sliced, and assign them to their relevant dataframes using the .iloc function for the 'Close' column. For example, we would get bitstamp_sliced = bitstamp_df.iloc[:, [3]]. Similar to the .loc function for before, we want all the rows hence the ":", however, we want the column position with the .iloc function, which is "3" in this case.

Once we have our data slices, we can get summary statistics using the .describe() function for the original dataframes with the parameters (include='all') to see everything. 

We now move onto plotting the full length of time of close prices for the bitstamp_sliced and coinbase_sliced variables. For this, we used the .plot function with the parameters figsize, title, and color for each graph (bitstamp and coinbase will receive different titles and colors, but the same figize); for example, coinbase_sliced.plot(figsize = (10,5), title = "Coinbase Prices", color = "orange").

Once we plot both, we want to overlay them. To do with, we used:

bitstamp_sliced['Close'].plot(legend = True, figsize = (15,7), title = "Bitstamp V. Coinbase Prices", color = "blue", label = "Bitstamp Prices") 
coinbase_sliced['Close'].plot(legend = True, figsize = (15,7), color = "orange", label = "Coinbase Prices")

These overlay plots utilize the legend, figsize, title, color, and label variables for accurate formatting, proper labelage, and color differentials on the plot.

To look at specific months, we used a similar process. For a month early into the data period, we modified the above plot functions with the .loc functions to specifically locate the dates in January. This inclusion of the .loc function can be seen with:

bitstamp_sliced['Close'].loc['2018-01-01' : '2018-01-31'].plot(legend = True, figsize = (15,10), title = "January 2018 Prices: Bitstamp v. Coinbase", color = "blue", label = "Bitstamp Prices")
coinbase_sliced['Close'].loc['2018-01-01' : '2018-01-31'].plot(legend = True, figsize = (15,10), color = "orange", label = "Coinbase Prices") 

The exact same process is followed for the later month within the data set.

Using similar logic, we will look at three separate days within the data set from near the beginning, middle, and end. To do this, for example, we will similarly use the .loc function for an early date:

bitstamp_sliced['Close'].loc['2018-01-02'].plot(legend = True, figsize = (15,10), title = "January 02, 2018 Prices: Bitstamp v. Coinbase", color = "blue", label = "Bitstamp Prices")
coinbase_sliced['Close'].loc['2018-01-02'].plot(legend = True, figsize = (15,10), color = "orange", label = "Coinbase Prices") 

With each day, the arbitrage spread was then calculated by subtracting the lower bitstamp closing prices from the coinbase closing prices, with its summary statistics output through the following example:

arbitrage_spread_early = coinbase_sliced['Close'].loc['2018-01-02'] - bitstamp_sliced['Close'].loc['2018-01-02'] 
arbitrage_spread_early.describe() 

Last for each selected date, we created a boxplot of the arbitrage spread through the following code:

arbitrage_spread_early.plot(kind = 'box', title = 'January 02, 2018 Arbitrage Spread', figsize = (10,8)) 

With the variables kind set to 'box', title, and figsize.





