# Forecasting_Net_Prophet

I'm a growth analyst at [MercadoLibre](http://investor.mercadolibre.com/investor-relations). With over 200 million users, MercadoLibre is the most popular e-commerce site in Latin America. I've been tasked with analyzing the company's financial and user data in clever ways to make the company grow. So, I want to find out if the ability to predict search traffic can translate into the ability to successfully trade the stock.

---

## Technologies

This project leverages Pandas, Prophet and hvplot on [Google Colab](https://colab.research.google.com/)


---

## Usage

*Open Google Colab and Upload **forecasting_net_prophet.ipynb**


*Install the required libraries:*

```python
!pip install pystan
!pip install fbprophet
!pip install hvplot
!pip install holoviews
```

*Import the following libraries and dependencies:*

``` python
import pandas as pd
import holoviews as hv
from fbprophet import Prophet
import hvplot.pandas
import datetime as dt
%matplotlib inline
```


---

## Find Unusual Patterns in Hourly Google Search Traffic

1. Upload the **google_hourly_search_trends.csv** file into Colab, then store in a Pandas DataFrame.
2. Review the data types of the DataFrame using the `info` function.
3. Holoviews extension to render hvPlots in Colab by running `hv.extension('bokeh')`.
4. Slice the DataFrame to just the month of May 2020 by `.loc["2020-05-01":"2020-05-31"]`.
5. Use hvPlot to visualize.

![<may20>](<Image/hvplot may 20.png>)

6. Calculate the sum of the total search traffic for May 2020 which is **38181**.
7. Calcluate the monhtly median search traffic across all months which is **35172.5**.
8. Compare the seach traffic for the month of May 2020 to the overall monthly median value gives the result of **1.09**.

*It looks like the Google search traffic increase by 1.09% during the month that MercadoLibre released its financial results.*

---

## Mine the Search Traffic Data for Seasonalit

1. Group the hourly search data to plot (use hvPlot) the average traffic by the day of week .

![<dayofweek>](<Image/dayofweek.png>)

2. Use hvPlot to visualize the hour of the day and day of week search traffic as a heatmap.

![<heatmap>](<Image/heatmap.png>)

3. Group the hourly search data to plot (use hvPlot) the average traffic by the week of the year.

![<weekofyear>](<Image/dayofweek.png>)

*The search traffic trend increases from weeks 40 to 51. There's sharp drop after week 51.*

---

## Relate the Search Traffic to Stock Price Patterns

1. Upload the "mercado_stock_price.csv" file into Colab, then store in a Pandas DataFrame

2. Use hvPlot to visualize the **closing price** of the **df_mercado_stock** DataFrame.

![<closing>](<Image/closing.png>)

3. Concatenate the **df_mercado_stock** DataFrame with the **df_mercado_trends** DataFrame.
4. Slice to just the first half of 2020 (2020-01 through 2020-06) by `.loc["2020-01-01":"2020-06-30"]`.
5. Use hvPlot to visualize the close and Search Trends data.

![<trend>](<Image/trend.png>)
![<searchtrend>](<Image/search trend.png>)

*Both time series indicate a common trend that's consistent with this narrative.*

6. Create a new column in the **mercado_stock_trends_df** DataFrame called **Lagged Search Trends** and shift the Search Trends information by one hour.
7. Create a new column in the **mercado_stock_trends_df** DataFrame called **Stock Volatility** and calculate the standard deviation of the closing stock price return data over a 4 period rolling window.
8. Use hvPlot to visualize the **stock volatility**.

![<vol>](<Image/stock vol.png>)

9. Create a new column in the **mercado_stock_trends_df** DataFrame called **Hourly Stock Return** and calculate **hourly return percentage** of the closing price.
10. Construct correlation table of **Stock Volatility**, **Lagged Search Trends**, and **Hourly Stock Return**.

![<table>](<Image/table.png>)

*It looks like the Lagged Search Trends will not predict the Stock Volatility as the correlation is (-0.14). And there's little to no evidence that the Trends can predict Stock Price Return as the correlation is (0.01), nearly 0.*

---

## Create a Time Series Model with Prophet

1. Using the **df_mercado_trends** DataFrame, reset the index so the date information is no longer the index.
2. Label the columns `ds` and `y` so that the syntax is recognized by Prophet.
3. Drop an NaN values from the **prophet_df** DataFrame.
4. Call the Prophet function, store as an object.
5. Fit the time-series model.
6. Create a future dataframe to hold predictions and make the prediction go out as far as 2000 hours (approx 80 days).
7. Make the predictions for the trend data using the **future_mercado_trends** DataFrame.
8. Plot the Prophet predictions for the Mercado trends data

![<prophet>](<Image/prop.png>)

*In the near-term forecast, it will stay on the same trend, the range will be big.*

9. Set the index in the **forecast_mercado_trends** DataFrame to the `ds `datetime column.
10. Use hvPlot to visualize the `yhat`, `yhat_lower`, and `yhat_upper` columns over the last 2000 hours.

![<yhat>](<Image/yhat.png>)

11. Reset the index in the **forecast_mercado_trends** DataFrame.
12. Use the  `plot_components` function to visualize the forecast results.

![<plot>](<Image/plot_com.png>)

*In 00:00 AM exhibits the greatest popularity.*
*Tuesday gets the most search traffic.*
*During October - November is the lowest point for search traffic in the calendar year.*

---

## Contributors

Feier Ou 

ffeierou1003@gmail.com 

---

## License

Feier Ou 
