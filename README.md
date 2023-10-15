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


The data used in this project was downloaded from [Maven Analytics](https://www.mavenanalytics.io/data-playground).
