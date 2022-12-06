# US Stocks Behaviour During Financial Crisis 2008


<br>

[2008's Financial Crisis](https://en.wikipedia.org/wiki/Financial_crisis_of_2007%E2%80%932008) Analysis.

We can see real life events ocurring in the Data Frame and Graphics.

This is a EDA - Exploratory Data Analysis (not include a ML - Machine Learning Model)

<br>

---

<br>

## _The Data:_

<br>

Scraped data from web:

- Stock Info: stock prices (High, Low, Open, Close)	
- Volume
- Date: datetime
- Bank Ticker: BAC, C, GS, JPM, MS, WFC

<br>

## _Goal / Target:_

Explore to undertand facts and events that ocurrs during the crisis and their impact on stock prices.

<br>

## _Tools:_

- Python
- Numpy, Pandas
- Matplotlib, Seaborn
- Plotly, Cufflinks
- Yahoo Finance

<br>

---

<br>

## **Step 1: Imports and Data Reading**

<br>

We have to install it beforehand:

```python
pip install pandas_datareader # to read data direct from source
pip install cufflinks # to see data with interactive graphic
pip install yfinance # to get data direct from web
```

<br>

Importing libraries:
```python
from pandas_datareader import data as pdr
from pandas_datareader import wb

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly as px
import cufflinks as cf

import datetime

import yfinance as yf
```

<br>

Period of analysis:

```python
start = '2006-1-1'
end = '2016-1-1'
```

<br>

Getting data direct from Yahoo Finance Data Base: 

```python
#Tickers from NYSE:

BAC = pdr.get_data_yahoo('BAC', start, end)

C = pdr.get_data_yahoo('C', start, end)

GS = pdr.get_data_yahoo('GS', start, end)

JPM = pdr.get_data_yahoo('JPM', start, end)

MS = pdr.get_data_yahoo('MS', start, end)

WFC = pdr.get_data_yahoo('WFC', start, end)
```

<br>

A sample looks like that:

```python
BAC.head()
```

![img ticker dataframe](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/ticker%20data%20frame.png?raw=true#gh-light-mode)

<br>

Creating data frame with all the stocks together:

```python
df = pdr.get_data_yahoo(['BAC','C','GS','JPM','MS','WFC'], start, end)

tickers = ['BAC', 'C', 'GS', 'JPM', 'MS', 'WFC']

bank_stocks = pd.concat([BAC, C, GS, JPM, MS, WFC],axis=1,keys=tickers)

bank_stocks.head()

```
![img all tickers](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/all%20tickers.png?raw=true)

<br>

Info:

```python
bank_stocks.info()

DatetimeIndex: 2517 entries, 2006-01-03 to 2015-12-31
Data columns (total 36 columns):
 #   Column            Non-Null Count  Dtype  
---  ------            --------------  -----  
 0   (BAC, High)       2517 non-null   float64
 1   (BAC, Low)        2517 non-null   float64
 2   (BAC, Open)       2517 non-null   float64
 3   (BAC, Close)      2517 non-null   float64
 4   (BAC, Volume)     2517 non-null   float64
 5   (BAC, Adj Close)  2517 non-null   float64
 6   (C, High)         2517 non-null   float64
 7   (C, Low)          2517 non-null   float64
 8   (C, Open)         2517 non-null   float64
 9   (C, Close)        2517 non-null   float64
 10  (C, Volume)       2517 non-null   float64
 11  (C, Adj Close)    2517 non-null   float64
 12  (GS, High)        2517 non-null   float64
 13  (GS, Low)         2517 non-null   float64
 14  (GS, Open)        2517 non-null   float64
 15  (GS, Close)       2517 non-null   float64
 16  (GS, Volume)      2517 non-null   float64
 17  (GS, Adj Close)   2517 non-null   float64
 18  (JPM, High)       2517 non-null   float64
 19  (JPM, Low)        2517 non-null   float64
...
 34  (WFC, Volume)     2517 non-null   float64
 35  (WFC, Adj Close)  2517 non-null   float64
dtypes: float64(36)
```
<br>

Setting columns level name:

```python
bank_stocks.columns.names = ['Bank Ticker','Stock Info']
bank_stocks.head()
```

![img columns levels](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/column%20levels.png?raw=true)

<br>

---

<br>

## **Step 2: EDA - Exploratory Data Analysis**

<br>

Historical maximuns at closing:

```python
bank_stocks.xs(key='Close',axis=1,level='Stock Info').max() #  using cross sections (.xs)

Bank Ticker
BAC     54.900002
C      564.099976
GS     247.919998
JPM     70.080002
MS      89.300003
WFC     58.520000
```

<br>

% of appreciation / depreciation: 

```python
returns = pd.DataFrame()

for tick in tickers:
    returns[tick+' Return'] = bank_stocks[tick]['Close'].pct_change() # .pct_change() METHOD = percentage change in the time-series

returns.head()
```

![img % of depreciation](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/appreciation%20depreciation.png?raw=true)

<br>

In graph:

```python
plt.figure(figsize=(16,6))

plt.subplot(321)
sns.lineplot(returns, x='Date', y='BAC Return', alpha=0.70)

plt.subplot(322)
sns.lineplot(returns, x='Date', y='C Return', alpha=0.70)

plt.subplot(323)
sns.lineplot(returns, x='Date', y='GS Return', alpha=0.70)

plt.subplot(324)
sns.lineplot(returns, x='Date', y='JPM Return', alpha=0.70)

plt.subplot(325)
sns.lineplot(returns, x='Date', y='MS Return', alpha=0.70)

plt.subplot(326)
sns.lineplot(returns, x='Date', y='WFC Return', alpha=0.30)

plt.tight_layout()
plt.show()
```

![img graph of depreciation](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/graph%20depreciation.png?raw=true)

<br>

% of depreciation of MS and CITI hit a highest point than the others.

CITI's recovery looks faster than the others. Probably, this ocurrs because CITI was early split in two (restructuring of the company when the crisis started).

<br>

Comparing tickers:

```python
sns.pairplot(returns[1:])
```

![img comparing tickers](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/comparing%20tickers.png?raw=true)

<br>

Rock bottom day:

```python
returns.idxmin()

BAC Return   2009-01-20
C Return     2009-02-27
GS Return    2009-01-20
JPM Return   2009-01-20
MS Return    2008-10-09
WFC Return   2009-01-20
```

Observatiosn about the minimums:

- 2009-01-20: the market was concerned about the changing of the USA presidency this day.

- 2008-10-09: MS tumbled 25.9%

- 2009-02-27: CITI deal inspires no confidence.

<br> 

Standard Deviation:

```python
returns.std().sort_values(ascending=False)

C Return      0.038672
MS Return     0.037819
BAC Return    0.036647
WFC Return    0.030238
JPM Return    0.027667
GS Return     0.025390
```

There was a bigger std for MS and CITI too (+ risk/volatility).

<br>

Highest risk/volatility in 2015 (after the crisis, last year of this DB):

```python
returns['2015-01-01':'2015-12-31'].std().sort_values(ascending=False)

MS Return     0.016249
BAC Return    0.016163
C Return      0.015289
GS Return     0.014046
JPM Return    0.014017
WFC Return    0.012591
```

<br>

Closing prices for each bank/ticker for the entire period:

```python
for tick in tickers:
    bank_stocks[tick]['Close'].plot(label=tick,figsize=(12,8))
```

![img closing prices](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/closing%20pricespng.png?raw=true)

<br>

Look at the same graph with an interactive plotly:

```python
cf.go_offline() # needed to run on jupiter

--NotebookApp.iopub_data_rate_limit=10000000 # increasing the memory used by jupiter

bank_stocks.xs(key='Close',axis=1,level='Stock Info').iplot()
```

![img plotly](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/plotly.png?raw=true)

<br> 

GS recovery was due, in part, because of the purchaseof $5 billion in shares by Warren Buffet and the bailout from US goverment.

<br>

Looking close to GS. Moving averages vs. stock prices:


```python
GS['Close']['2008-01-01':'2008-12-31'].rolling(window=30).mean().plot(label='GS 30D Mov Avg',figsize=(12,8))

GS['Close']['2008-01-01':'2008-12-31'].plot(label='GS Close',figsize=(14,6))
plt.legend()
```

![img moving avg](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/moving%20averages.png?raw=true)

<br>

Simple mov. avg with plotly:

```python
gs_sma = GS['Close']['2015-01-01':'2015-03-31'].ta_plot(study='sma',periods=[14,28,60])
```

![img mov. avg. plotly](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/mov%20avg%20plotly.png?raw=true)

<br>

Correlation between stock prices:

```python
sns.heatmap(bank_stocks.xs(key='Close', axis=1, level='Stock Info').corr(), annot=True, cmap='mako_r')
```

![img heatmap](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/heatmap.png?raw=true)

<br>

To see more clear:

```python
sns.clustermap(bank_stocks.xs(key='Close', axis=1, level='Stock Info').corr(), annot=True, cmap='mako_r')
```

![img clustermap](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/clustermap.png?raw=true)

<br>

Tipical interactive candle plot:

```python
gs_candle = GS[['Open','High','Low','Close']]['2015-01-01':'2015-03-31']
gs_candle.iplot(kind='candle')
```

![img candle plot](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/candle%20plot.png?raw=true)

<br>

Technical Analysis plot:

```python
gs_sma = GS['Close']['2015-01-01':'2015-03-31'].ta_plot(study='boll')
```

![img technical analysis](https://github.com/TSLSouth/US-Stocks-Behaviour-During-Financial-Crisis-2008/blob/main/img/technical%20analysis.png?raw=true)
