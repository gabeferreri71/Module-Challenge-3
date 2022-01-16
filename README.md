# Module-Challenge-3

In this challenge, we will be looking at two bitcoin exchanges and calculating related arbitrage opportunities.

# Crypto Arbitrage

In this section, we include our libraries and dependencies. We will be importing pandas and numpy, as well as our import path statement. 
%mathplotlib inline is included for developing plots of our data later on. 

# Collect the Data

1-3) We first need to create dataframes for both the bitstamp and coinbase .csv files. To do this we first create a variable, bitstamp_df or coinbase_df, and assign it to a placeholder variable, in this case being bitstamp_dataframe and coinbase_dataframe, respectively. That variable is then assigned to the pd.read_csv function, where the specific path for each .csv file is set, followed by index_col = 'Timestamp', parse_dates = True, and infer_datetime_format = True all within the pd.read_csv function. The variables within the read_csv function will format our data properly for the following steps.

To make sure this process worked, we then used the .head() function to preview the first five rows of data.

# Prepare the Data

1) To properly prepare the data, we first will drop all missing values (NaN). To see the missing values, we used the .isnull().sum() function with either the bitstamp_df or coinbase_df dataframes to sum how many values are NaN. Next, we use the .dropna() function to drop all of the missing values with each data frame, followed by repeating the .isnull().sum() function to then make sure that the are no NaN values (everything should read 0). 

2) With NaN values dropped, we now must remove the "$" from the 'Close' column of data, followed by changing the 'Close' values to floats. To remove the "$", we use str.replace function as well as the .loc function. Using bitstamp_df and/or coinbase_df, we "locate" the 'Close' column with bitstamp_df.loc[:, "Close"] which equates to locating all the rows but just the 'Close' column. This is then assigned to the same thing, bitstamp_df.loc[:, "Close"] = bitstamp_df.loc[:, "Close"].str.replace("$", ""), which uses the string replace function to change the "$" to nothing. A .head() function can be ran to validate the "$" is gone on both dataframes.

3) With the "$" gone, we now convert the 'Close' values to floats for further analysis using the .astype function. Similar to the last step, we have our dataframe using .loc for the 'Close' column assigned to itself with the .astype("float") added to change the data type, as can be seen as bitstamp_df.loc[:, "Close"] = bitstamp_df.loc[:, "Close"].astype("float"), once again followed by a .head() function of the specific dataframe.

4) The last step for both dataframes are to drop any duplicate values. Similar to the isnull().sum() function, we will first run .duplicated().sum() to find the number of duplicated values. We next take our dataframe, and assign it to itself using the .drop_duplicates() function; for example, bitstamp_df = bitstamp_df.drop_duplicates(). We then run the .duplicated().sum() function again to make sure there are 0 duplicates.

5) steps 1-4 again

# Analyze the Data

1) To analyze the data, we just want the 'Close' prices with the associated Timestamp information. To get these, we create ariables bitstamp_sliced and coinbase_sliced, and assign them to their relevant dataframes using the .iloc function for the 'Close' column. For example, we would get bitstamp_sliced = bitstamp_df.iloc[:, [3]]. Similar to the .loc function for before, we want all the rows hence the ":", however, we want the column position with the .iloc function, which is "3" in this case.


2) Once we have our data slices, we can get summary statistics using the .describe() function for the original dataframes with the parameters (include='all') to see everything. 

We now move onto plotting the full length of time of close prices for the bitstamp_sliced and coinbase_sliced variables. For this, we used the .plot function with the parameters figsize, title, and color for each graph (bitstamp and coinbase will receive different titles and colors, but the same figize); for example, coinbase_sliced.plot(figsize = (10,5), title = "Coinbase Prices", color = "orange").


Once we plot both, we want to overlay them. To do with, we used:

bitstamp_sliced['Close'].plot(legend = True, figsize = (15,7), title = "Bitstamp V. Coinbase Prices", color = "blue", label = "Bitstamp Prices") 
coinbase_sliced['Close'].plot(legend = True, figsize = (15,7), color = "orange", label = "Coinbase Prices")

These overlay plots utilize the legend, figsize, title, color, and label variables for accurate formatting, proper labelage, and color differentials on the plot.

To look at specific months, we used a similar process. For a month early into the data period, we modified the above plot functions with the .loc functions to specifically locate the dates in January. This inclusion of the .loc function can be seen with:

bitstamp_sliced['Close'].loc['2018-01-01' : '2018-01-31'].plot(legend = True, figsize = (15,10), title = "January 2018 Prices: Bitstamp v. Coinbase", color = "blue", label = "Bitstamp Prices")
coinbase_sliced['Close'].loc['2018-01-01' : '2018-01-31'].plot(legend = True, figsize = (15,10), color = "orange", label = "Coinbase Prices") 

The exact same process is followed for the later month within the data set.


3) Using similar logic, we will look at three separate days within the data set from near the beginning, middle, and end. To do this, for example, we will similarly use the .loc function for an early date:

bitstamp_sliced['Close'].loc['2018-01-02'].plot(legend = True, figsize = (15,10), title = "January 02, 2018 Prices: Bitstamp v. Coinbase", color = "blue", label = "Bitstamp Prices")
coinbase_sliced['Close'].loc['2018-01-02'].plot(legend = True, figsize = (15,10), color = "orange", label = "Coinbase Prices") 

With each day, the arbitrage spread was then calculated by subtracting the lower bitstamp closing prices from the coinbase closing prices, with its summary statistics output through the following example:

arbitrage_spread_early = coinbase_sliced['Close'].loc['2018-01-02'] - bitstamp_sliced['Close'].loc['2018-01-02'] 
arbitrage_spread_early.describe() 

Last for each selected date, we created a boxplot of the arbitrage spread through the following code:

arbitrage_spread_early.plot(kind = 'box', title = 'January 02, 2018 Arbitrage Spread', figsize = (10,8)) 

With the variables kind set to 'box', title, and figsize.

We will follow the same process for two more dates, 02-22 and 03-29.


4) I. We first will measure the arbitrage spread between the three dates selected, by subtracting the lower-priced exchange from the higher-priced exchange; in this case we will use the coinbase_sliced - bitstamp_sliced through:

early_arbit = coinbase_sliced['Close'].loc['2018-01-16'] - bitstamp_sliced['Close'].loc['2018-01-16'] 

middle_arbit = coinbase_sliced['Close'].loc['2018-02-22'] - bitstamp_sliced['Close'].loc['2018-02-22'] 

late_arbit = coinbase_sliced['Close'].loc['2018-03-29'] - bitstamp_sliced['Close'].loc['2018-03-29'] 

*** Note our next few calculations/plots will show us arbitrage trends. This data period represents a major price correction for Bitcoin, so arbitrage spread profit opportunities decrease as more arbitrageurs enter the market (as per Module 3.4.12) ***

*********************************************************************************************************************************************************************************
II. Next, the spread returns are calculated for each date, with the format:

early_return = early_arbit[early_arbit > 0] / bitstamp_sliced['Close'].loc['2018-01-16']

Where the middle and late day would use there respective dataframe (middle_arbit, late_arbit) and date (2018-02-22, 2018-03-29). 

*********************************************************************************************************************************************************************************
III. We now want the number of positive returns based on a 1% minimum threshold by creating profitable_trades variables for each date, as modeled with:

profitable_trades_early = early_return[early_return > 0.01]
profitable_trades_early.head(10)

The .head(10) function will show us some of the values that have met the threshold. Note, when profitable_trades_middle and profitable_trades_late are created and have the .head() function applied, only ONE arbitrage spread from the 2018-02-22 and 2018-03-29 dates met the minimum threshold. This also begins to show the trend of lower spread profitability over time as indicated in the module. 

*********************************************************************************************************************************************************************************
IV. After creating our profitable_trades variables for the early, middle, and late date, we generate summary statistics to further visualize the data trends. Using the early example, this can be done with:

profitable_trades_early.describe(include='all') 

Which shows the count, mean, std, min, 25%, 50%, 75%, and max of profitable_trades_early. Note, when ran for the profitable_trade_middle, the count is 0.0 with all NaN values, indicating that no trades met the 1% spread threshold on that day. For profitable_trades_late, there is only one profitable trade, so the count is 1, std = NaN, and all other stats are equal. 

*********************************************************************************************************************************************************************************
V. To calculate profits for each period, we want to multiply the profitable_trades for each period by the bitstamp_sliced dataframe for that specific day and set it to a profit_ variable. We then will create a new variable, profit_per_trade_, which is assigned to the specific profit_ variable using the function .dropna() to get rid of any NaN values. Using our early date, the code is expressed as:

profit_early = profitable_trades_early * bitstamp_sliced['Close'].loc['2018-01-16'] 
profit_per_trade_early = profit_early.dropna()
profit_per_trade_early

Note, when determining and creating profit_per_trade_middle and profit_per_trade_late, everything will be dropped for profit_per_trade_middle as that day had zero trades meeting the 1% min. threshold; for profit_per_trade_late, only one value will be present as there was only one profitable trade meeting the threshold. 

*********************************************************************************************************************************************************************************
VI. Similar to before, we are going the use the .describe() function on profit_per_trade_early, profit_per_trade_middle, and profit_per_trade_late. The count for each should be the same as each asscociated profitable_trades_ variable, but the other statistics now reflect the profit. Now, using profit_per_trade_early, which has by far the most eligible trades and profitability of the three dates, we will plot the profit per trade over the course of the day, using:

profit_per_trade_early.plot(figsize = (10, 5), title = "January 16, 2018 Profit Per Trade", color = "blue", ylabel = "Profit($)") 

Note that some additional parameters like ylabel were included to make the plots more presentable anf formatted.

*********************************************************************************************************************************************************************************
VII. Using the sum function, we calculate the total profits from the early date, which is reflected with:

profit_sum_early = profit_per_trade_early.sum()
profit_sum_early

This results in us viewing the total sum of the profits of the early date, equating to 14147.169999999998.

*********************************************************************************************************************************************************************************
VIII. In this step we calculate the cumulative sums from each date using the .cumsum() function. Note that for our selected days, we already know that the middle date will have a sum of 0 and the late date will have a sum of 89.9 based on cumulative_profit_middle and cumulative_profit_late, repectively. Using the early date as an example, we get:

cumulative_profit_early = profit_per_trade_early.cumsum()
cumulative_profit_early

This reflects the profitbale trade count of 73 from before, but now with other statistics priced in dollars.

Lastly, we plot the the cumulative_profit_early determined earlier in VIII. using the following code:

cumulative_profit_early.plot(figsize=(10, 7), title="Cumulative Bitcoin Profits", ylabel = "Profits($)")

Note: for written answer to trend analysis, please refer to the last cell in the crypto_arbitrage notebook.
