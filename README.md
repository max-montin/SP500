# S&P 500 analysis from 2014 to 2017
In this project, I analyzed data that was gathered from the S&P 500 between 2014 and 2017.
The original data included daily open/low/high prices, and trading volumes for S&P 500 stocks. All other columns have been added by me.

Below are screenshots of the dashboards I made, and under them you can find a step-by-step description of how I completed this project.

<img src="https://github.com/max-montin/SP500/blob/main/sp500_1.png" width="700"><img src="https://github.com/max-montin/SP500/blob/main/sp500_2.png" width="700">

[Link to Power BI file](https://github.com/max-montin/SP500/blob/main/sp500.pbix)

To analyze trading volumes for each day of the week, I converted dates to written formats for clarity.
First, I converted the full date values to day numbers:

```DayNo = WEEKDAY(SP500_StockPrices_2014_2017[date],1)```

 I created a new table to match day numbers with their written names and used the ```LOOKUPVALUE``` function:

```Weekday = LOOKUPVALUE(Weekdays[Weekday], Weekdays[DayNo], SP500_StockPrices_2014_2017[DayNo])```

Next, I measured volatilities by calculating the daily differences between a stock's lowest and highest points.:

```Volatility = SP500_StockPrices_2014_2017[high]-SP500_StockPrices_2014_2017[low]```

After calculating the daily volatilities, I needed to find the highest volatility for each stock during the period:

```
HighestVolatility = 
VAR z = SP500_StockPrices_2014_2017[symbol]
RETURN
MAXX(FILTER(SP500_StockPrices_2014_2017, SP500_StockPrices_2014_2017[symbol]=z), SP500_StockPrices_2014_2017[Volatility])
```

I compared the highest volatilities to identify the most volatile dates for each stock:

```MaxDate = IF(SP500_StockPrices_2014_2017[Volatility]=SP500_StockPrices_2014_2017[HighestVolatility], SP500_StockPrices_2014_2017[date].[Date], BLANK())```

Finally, I developed functions to determine the lowest and highest prices for each stock, allowing me to identify the best performers:

```
LowestLow = 
VAR x = SP500_StockPrices_2014_2017[symbol]
RETURN
MINX(FILTER(SP500_StockPrices_2014_2017, SP500_StockPrices_2014_2017[symbol]=x), SP500_StockPrices_2014_2017[low])
```
```
HighestHigh = 
VAR y = SP500_StockPrices_2014_2017[symbol]
RETURN
MAXX(FILTER(SP500_StockPrices_2014_2017, SP500_StockPrices_2014_2017[symbol]=y), SP500_StockPrices_2014_2017[high])
```

After finding the individual values, I calculated the increase in percentages to find the best-performing stocks:

```ProfitPercentage = (SP500_StockPrices_2014_2017[HighestHigh]-SP500_StockPrices_2014_2017[LowestLow])/SP500_StockPrices_2014_2017[LowestLow]```

The data used in this project was downloaded from [Maven Analytics](https://www.mavenanalytics.io/data-playground).

[CSV file](https://github.com/max-montin/SP500/blob/main/S%26P%20500%20Stock%20Prices%202014-2017.csv)
