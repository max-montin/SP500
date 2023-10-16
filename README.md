# S&P 500 analysis from 2014 to 2017
In this project, I analyzed data that was gathered from the S&P 500 between 2014 and 2017.
The original data contained daily open/low/high values and trading volumes for each stock on the S&P 500. All other columns have been added by me.

Below are screenshots of the dashboards I made, and under them you can find a step-by-step description of how I completed this project.

![](https://github.com/max-montin/SP500/blob/main/sp500_1.png)

![](https://github.com/max-montin/SP500/blob/main/sp500_2.png)

To analyze the trading volumes for different days of the week, I needed to convert the dates to written formats for improved readability.
First, I converted the full date values to day numbers:

```DayNo = WEEKDAY(SP500_StockPrices_2014_2017[date],1)```

After converting the days to numbers, I made a simple table to find written names for each number and used ```LOOKUPVALUE``` as the function:

```Weekday = LOOKUPVALUE(Weekdays[Weekday], Weekdays[DayNo], SP500_StockPrices_2014_2017[DayNo])```

Next, to measure volatilities I calculated the differences between a stock's lowest point and highest point for each day:

```Volatility = SP500_StockPrices_2014_2017[high]-SP500_StockPrices_2014_2017[low]```

After calculating the daily volatilities, I needed to find the highest volatility for each stock during the period:

```
HighestVolatility = 
VAR z = SP500_StockPrices_2014_2017[symbol]
RETURN
MAXX(FILTER(SP500_StockPrices_2014_2017, SP500_StockPrices_2014_2017[symbol]=z), SP500_StockPrices_2014_2017[Volatility])
```

Next, I needed to compare the highest volatilities to each day in order to find which dates were the most volatile:

```MaxDate = IF(SP500_StockPrices_2014_2017[Volatility]=SP500_StockPrices_2014_2017[HighestVolatility], SP500_StockPrices_2014_2017[date].[Date], BLANK())```

Last, to find the best performing stocks from the given period, I created functions to find the lowest and highest prices for each stock:

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
