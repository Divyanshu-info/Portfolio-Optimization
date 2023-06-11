```python
import pandas as pd
import datetime
import yfinance as yf
import backtrader as bt
import numpy as np
import warnings

warnings.filterwarnings("ignore")

# Date range
start = '2000-01-01'
end = '2022-07-31'

# Tickers of assets
benchmark_index = "^BSESN"
assets = """AXISBANK.NS	BPCL.NS	BRITANNIA.NS	CIPLA.NS	DRREDDY.NS	EICHERMOT.NS	GAIL.NS	HDFCBANK.NS	HINDALCO.NS	HINDUNILVR.NS	INFY.NS	IOC.NS	ITC.NS	M&M.NS	MARICO.NS	ONGC.NS	RELIANCE.NS	SAIL.NS	SBIN.NS	SIEMENS.NS	SUNPHARMA.NS	TATACONSUM.NS	TATAMOTORS.NS	TATAPOWER.NS	TATASTEEL.NS	TITAN.NS VEDL.NS	WIPRO.NS ^BSESN""".split()
assets.sort()

# Downloading data
prices = yf.download(assets, start=start, end=end)
display(prices.head())
prices = prices.dropna()
```

    [*********************100%***********************]  29 of 29 completed



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">Adj Close</th>
      <th>...</th>
      <th colspan="10" halign="left">Volume</th>
    </tr>
    <tr>
      <th></th>
      <th>AXISBANK.NS</th>
      <th>BPCL.NS</th>
      <th>BRITANNIA.NS</th>
      <th>CIPLA.NS</th>
      <th>DRREDDY.NS</th>
      <th>EICHERMOT.NS</th>
      <th>GAIL.NS</th>
      <th>HDFCBANK.NS</th>
      <th>HINDALCO.NS</th>
      <th>HINDUNILVR.NS</th>
      <th>...</th>
      <th>SIEMENS.NS</th>
      <th>SUNPHARMA.NS</th>
      <th>TATACONSUM.NS</th>
      <th>TATAMOTORS.NS</th>
      <th>TATAPOWER.NS</th>
      <th>TATASTEEL.NS</th>
      <th>TITAN.NS</th>
      <th>VEDL.NS</th>
      <th>WIPRO.NS</th>
      <th>^BSESN</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-03</th>
      <td>3.699342</td>
      <td>13.014572</td>
      <td>54.370880</td>
      <td>100.327560</td>
      <td>324.122742</td>
      <td>2.891769</td>
      <td>4.213370</td>
      <td>14.292334</td>
      <td>57.240540</td>
      <td>135.385193</td>
      <td>...</td>
      <td>5868160</td>
      <td>390399</td>
      <td>1637610.0</td>
      <td>3528277</td>
      <td>994997.0</td>
      <td>35678775</td>
      <td>460000</td>
      <td>814840</td>
      <td>42639</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-04</th>
      <td>3.842174</td>
      <td>12.077395</td>
      <td>54.202076</td>
      <td>100.871414</td>
      <td>350.061249</td>
      <td>3.042721</td>
      <td>4.069246</td>
      <td>14.611811</td>
      <td>59.411533</td>
      <td>133.830429</td>
      <td>...</td>
      <td>5128620</td>
      <td>747102</td>
      <td>837330.0</td>
      <td>3544397</td>
      <td>1058302.0</td>
      <td>27698564</td>
      <td>526000</td>
      <td>894640</td>
      <td>117119</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-05</th>
      <td>3.742192</td>
      <td>11.733491</td>
      <td>52.819279</td>
      <td>98.792374</td>
      <td>336.973846</td>
      <td>3.288389</td>
      <td>3.876055</td>
      <td>14.035914</td>
      <td>64.162598</td>
      <td>129.069275</td>
      <td>...</td>
      <td>2141030</td>
      <td>788156</td>
      <td>854660.0</td>
      <td>5849540</td>
      <td>873414.0</td>
      <td>68399389</td>
      <td>412000</td>
      <td>732200</td>
      <td>3527919</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-06</th>
      <td>3.649352</td>
      <td>12.396851</td>
      <td>56.436096</td>
      <td>93.340042</td>
      <td>357.066956</td>
      <td>3.551815</td>
      <td>3.983383</td>
      <td>14.149412</td>
      <td>69.184242</td>
      <td>134.249878</td>
      <td>...</td>
      <td>2027820</td>
      <td>448235</td>
      <td>1328510.0</td>
      <td>10274966</td>
      <td>2088480.0</td>
      <td>45604218</td>
      <td>632000</td>
      <td>1032000</td>
      <td>1942399</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-07</th>
      <td>3.470811</td>
      <td>12.382183</td>
      <td>60.950829</td>
      <td>85.884430</td>
      <td>328.603485</td>
      <td>3.827081</td>
      <td>3.842323</td>
      <td>14.153619</td>
      <td>72.252655</td>
      <td>141.569046</td>
      <td>...</td>
      <td>924060</td>
      <td>532538</td>
      <td>3353820.0</td>
      <td>11477451</td>
      <td>514643.0</td>
      <td>64862245</td>
      <td>732000</td>
      <td>694440</td>
      <td>269599</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 174 columns</p>
</div>



```python
############################################################
# Showing data
############################################################

display(prices.head())
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">Adj Close</th>
      <th>...</th>
      <th colspan="10" halign="left">Volume</th>
    </tr>
    <tr>
      <th></th>
      <th>AXISBANK.NS</th>
      <th>BPCL.NS</th>
      <th>BRITANNIA.NS</th>
      <th>CIPLA.NS</th>
      <th>DRREDDY.NS</th>
      <th>EICHERMOT.NS</th>
      <th>GAIL.NS</th>
      <th>HDFCBANK.NS</th>
      <th>HINDALCO.NS</th>
      <th>HINDUNILVR.NS</th>
      <th>...</th>
      <th>SIEMENS.NS</th>
      <th>SUNPHARMA.NS</th>
      <th>TATACONSUM.NS</th>
      <th>TATAMOTORS.NS</th>
      <th>TATAPOWER.NS</th>
      <th>TATASTEEL.NS</th>
      <th>TITAN.NS</th>
      <th>VEDL.NS</th>
      <th>WIPRO.NS</th>
      <th>^BSESN</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-03</th>
      <td>3.699342</td>
      <td>13.014572</td>
      <td>54.370880</td>
      <td>100.327560</td>
      <td>324.122742</td>
      <td>2.891769</td>
      <td>4.213370</td>
      <td>14.292334</td>
      <td>57.240540</td>
      <td>135.385193</td>
      <td>...</td>
      <td>5868160</td>
      <td>390399</td>
      <td>1637610.0</td>
      <td>3528277</td>
      <td>994997.0</td>
      <td>35678775</td>
      <td>460000</td>
      <td>814840</td>
      <td>42639</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-04</th>
      <td>3.842174</td>
      <td>12.077395</td>
      <td>54.202076</td>
      <td>100.871414</td>
      <td>350.061249</td>
      <td>3.042721</td>
      <td>4.069246</td>
      <td>14.611811</td>
      <td>59.411533</td>
      <td>133.830429</td>
      <td>...</td>
      <td>5128620</td>
      <td>747102</td>
      <td>837330.0</td>
      <td>3544397</td>
      <td>1058302.0</td>
      <td>27698564</td>
      <td>526000</td>
      <td>894640</td>
      <td>117119</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-05</th>
      <td>3.742192</td>
      <td>11.733491</td>
      <td>52.819279</td>
      <td>98.792374</td>
      <td>336.973846</td>
      <td>3.288389</td>
      <td>3.876055</td>
      <td>14.035914</td>
      <td>64.162598</td>
      <td>129.069275</td>
      <td>...</td>
      <td>2141030</td>
      <td>788156</td>
      <td>854660.0</td>
      <td>5849540</td>
      <td>873414.0</td>
      <td>68399389</td>
      <td>412000</td>
      <td>732200</td>
      <td>3527919</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-06</th>
      <td>3.649352</td>
      <td>12.396851</td>
      <td>56.436096</td>
      <td>93.340042</td>
      <td>357.066956</td>
      <td>3.551815</td>
      <td>3.983383</td>
      <td>14.149412</td>
      <td>69.184242</td>
      <td>134.249878</td>
      <td>...</td>
      <td>2027820</td>
      <td>448235</td>
      <td>1328510.0</td>
      <td>10274966</td>
      <td>2088480.0</td>
      <td>45604218</td>
      <td>632000</td>
      <td>1032000</td>
      <td>1942399</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-07</th>
      <td>3.470811</td>
      <td>12.382183</td>
      <td>60.950829</td>
      <td>85.884430</td>
      <td>328.603485</td>
      <td>3.827081</td>
      <td>3.842323</td>
      <td>14.153619</td>
      <td>72.252655</td>
      <td>141.569046</td>
      <td>...</td>
      <td>924060</td>
      <td>532538</td>
      <td>3353820.0</td>
      <td>11477451</td>
      <td>514643.0</td>
      <td>64862245</td>
      <td>732000</td>
      <td>694440</td>
      <td>269599</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 174 columns</p>
</div>


## 2. Building the Backtest Function with Backtrader

### 2.1 Defining Backtest Function


```python
############################################################
# Defining the backtest function 
############################################################

def backtest(datas, strategy, start, end, plot=False, **kwargs):
    cerebro = bt.Cerebro()

    # Here we add transaction costs and other broker costs
    cerebro.broker.setcash(1000000.0)
    cerebro.broker.setcommission(commission=0.005) # Commission 0.5%
    cerebro.broker.set_slippage_perc(0.005, # Slippage 0.5%
                                     slip_open=True,
                                     slip_limit=True,
                                     slip_match=True,
                                     slip_out=False)
    for data in datas:
        cerebro.adddata(data)

    # Here we add the indicators that we are going to store
    cerebro.addanalyzer(bt.analyzers.SharpeRatio, riskfreerate=0.0)
    cerebro.addanalyzer(bt.analyzers.Returns)
    cerebro.addanalyzer(bt.analyzers.DrawDown)
    cerebro.addstrategy(strategy, **kwargs)
    cerebro.addobserver(bt.observers.Value)
    cerebro.addobserver(bt.observers.DrawDown)
    results = cerebro.run(stdstats=False)
    if plot:
        cerebro.plot(iplot=False, start=start, end=end)
    return (results[0].analyzers.drawdown.get_analysis()['max']['drawdown'],
            results[0].analyzers.returns.get_analysis()['rnorm100'],
            results[0].analyzers.sharperatio.get_analysis()['sharperatio'])
```

### 2.2 Building Data Feeds for Backtesting


```python
############################################################
# Create objects that contain the prices of assets
############################################################

# Creating Assets bt.feeds
assets_prices = []
for i in assets:
    if i != benchmark_index:
        prices_ = prices.drop(columns='Adj Close').loc[:, (slice(None), i)].dropna()
        prices_.columns = ['Close', 'High', 'Low', 'Open', 'Volume']
        assets_prices.append(bt.feeds.PandasData(dataname=prices_, plot=False))

# Creating Benchmark bt.feeds        
prices_ = prices.drop(
    columns='Adj Close').loc[:, (slice(None), benchmark_index)].dropna()
prices_.columns = ['Close', 'High', 'Low', 'Open', 'Volume']
benchmark = bt.feeds.PandasData(dataname=prices_, plot=False)

display(prices_.head())
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Close</th>
      <th>High</th>
      <th>Low</th>
      <th>Open</th>
      <th>Volume</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-03</th>
      <td>5375.109863</td>
      <td>5384.660156</td>
      <td>5209.540039</td>
      <td>5209.540039</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-04</th>
      <td>5491.009766</td>
      <td>5533.979980</td>
      <td>5376.430176</td>
      <td>5533.979980</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-05</th>
      <td>5357.000000</td>
      <td>5464.350098</td>
      <td>5184.479980</td>
      <td>5265.089844</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-06</th>
      <td>5421.529785</td>
      <td>5489.859863</td>
      <td>5391.330078</td>
      <td>5424.209961</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2000-01-07</th>
      <td>5414.479980</td>
      <td>5463.250000</td>
      <td>5330.580078</td>
      <td>5358.279785</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>


## 3. Building Strategies with Backtrader

### 3.1 Buy and Hold Sensex


```python
############################################################
# Building the Buy and Hold strategy
############################################################

class BuyAndHold(bt.Strategy):

    def __init__(self):
        self.counter = 0

    def next(self):
        if self.counter >= 1004:
            if self.getposition(self.data).size == 0:
                self.order_target_percent(self.data, target=0.99)
        self.counter += 1 
```

If you have an error related to 'warnings' modules when you try to plot, you must modify the 'locator.py' file from backtrader library following the instructions in this __[link](https://community.backtrader.com/topic/981/importerror-cannot-import-name-min_per_hour-when-trying-to-plot/8)__.


```python
############################################################
# Run the backtest for the selected period
############################################################
%matplotlib inline

import matplotlib.pyplot as plt
plt.rcParams["figure.figsize"] = (10, 6) # (w, h)
plt.plot() # We need to do this to avoid errors in inline plot

start = 1004
end = prices.shape[0] - 1

dd, cagr, sharpe = backtest([benchmark],
                            BuyAndHold,
                            start=start,
                            end=end,
                            plot=True)
```


    
![png](MD_backtest_files/MD_backtest_9_0.png)
    



```python
############################################################
# Show Buy and Hold Strategy Stats 
############################################################

print(f"Max Drawdown: {dd:.2f}%")
print(f"CAGR: {cagr:.2f}%")
print(f"Sharpe: {sharpe:.3f}")
```

    Max Drawdown: 60.88%
    CAGR: 10.54%
    Sharpe: 0.514


### 3.2 Rebalancing Quarterly using Riskfolio-Lib


```python
############################################################
# Calculate assets returns
############################################################

pd.options.display.float_format = '{:.4%}'.format

data = prices.loc[:, ('Adj Close', slice(None))]
data.columns = assets
data = data.drop(columns=[benchmark_index]).dropna()
returns = data.pct_change().dropna()
display(returns.head())
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AXISBANK.NS</th>
      <th>BPCL.NS</th>
      <th>BRITANNIA.NS</th>
      <th>CIPLA.NS</th>
      <th>DRREDDY.NS</th>
      <th>EICHERMOT.NS</th>
      <th>GAIL.NS</th>
      <th>HDFCBANK.NS</th>
      <th>HINDALCO.NS</th>
      <th>HINDUNILVR.NS</th>
      <th>...</th>
      <th>SBIN.NS</th>
      <th>SIEMENS.NS</th>
      <th>SUNPHARMA.NS</th>
      <th>TATACONSUM.NS</th>
      <th>TATAMOTORS.NS</th>
      <th>TATAPOWER.NS</th>
      <th>TATASTEEL.NS</th>
      <th>TITAN.NS</th>
      <th>VEDL.NS</th>
      <th>WIPRO.NS</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-04</th>
      <td>3.8610%</td>
      <td>-7.2010%</td>
      <td>-0.3105%</td>
      <td>0.5421%</td>
      <td>8.0027%</td>
      <td>5.2200%</td>
      <td>-3.4206%</td>
      <td>2.2353%</td>
      <td>3.7928%</td>
      <td>-1.1484%</td>
      <td>...</td>
      <td>6.3410%</td>
      <td>0.2936%</td>
      <td>-1.3406%</td>
      <td>-2.4092%</td>
      <td>-3.9446%</td>
      <td>0.1954%</td>
      <td>-1.0823%</td>
      <td>-5.3308%</td>
      <td>-1.4182%</td>
      <td>8.0005%</td>
    </tr>
    <tr>
      <th>2000-01-05</th>
      <td>-2.6022%</td>
      <td>-2.8475%</td>
      <td>-2.5512%</td>
      <td>-2.0611%</td>
      <td>-3.7386%</td>
      <td>8.0740%</td>
      <td>-4.7476%</td>
      <td>-3.9413%</td>
      <td>7.9969%</td>
      <td>-3.5576%</td>
      <td>...</td>
      <td>-4.1104%</td>
      <td>-4.4003%</td>
      <td>-3.5102%</td>
      <td>-4.1659%</td>
      <td>2.4255%</td>
      <td>1.9506%</td>
      <td>3.8130%</td>
      <td>-6.1058%</td>
      <td>-0.6103%</td>
      <td>1.6298%</td>
    </tr>
    <tr>
      <th>2000-01-06</th>
      <td>-2.4809%</td>
      <td>5.6536%</td>
      <td>6.8475%</td>
      <td>-5.5190%</td>
      <td>5.9628%</td>
      <td>8.0108%</td>
      <td>2.7690%</td>
      <td>0.8086%</td>
      <td>7.8264%</td>
      <td>4.0138%</td>
      <td>...</td>
      <td>5.0513%</td>
      <td>1.4354%</td>
      <td>-0.0973%</td>
      <td>3.3431%</td>
      <td>4.1501%</td>
      <td>3.1250%</td>
      <td>7.4736%</td>
      <td>8.0202%</td>
      <td>4.6492%</td>
      <td>-1.9347%</td>
    </tr>
    <tr>
      <th>2000-01-07</th>
      <td>-4.8924%</td>
      <td>-0.1183%</td>
      <td>7.9997%</td>
      <td>-7.9876%</td>
      <td>-7.9715%</td>
      <td>7.7500%</td>
      <td>-3.5412%</td>
      <td>0.0297%</td>
      <td>4.4351%</td>
      <td>5.4519%</td>
      <td>...</td>
      <td>4.7126%</td>
      <td>-7.9622%</td>
      <td>-7.9991%</td>
      <td>7.9729%</td>
      <td>8.0144%</td>
      <td>-0.1855%</td>
      <td>2.1991%</td>
      <td>-2.1070%</td>
      <td>-2.3471%</td>
      <td>-7.9990%</td>
    </tr>
    <tr>
      <th>2000-01-10</th>
      <td>3.0864%</td>
      <td>1.3426%</td>
      <td>7.5074%</td>
      <td>-3.3386%</td>
      <td>-1.6480%</td>
      <td>1.3148%</td>
      <td>-0.5587%</td>
      <td>-1.4553%</td>
      <td>-6.5802%</td>
      <td>0.7584%</td>
      <td>...</td>
      <td>-1.3904%</td>
      <td>-1.8246%</td>
      <td>-4.5933%</td>
      <td>0.8827%</td>
      <td>7.8991%</td>
      <td>7.0013%</td>
      <td>5.9901%</td>
      <td>-0.9908%</td>
      <td>2.1459%</td>
      <td>0.1353%</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>



```python
############################################################
# Selecting Dates for Rebalancing
############################################################

# Selecting last day of month of available data
index = returns.groupby([returns.index.year, returns.index.month]).tail(1).index
index_2 = returns.index

# Quarterly Dates
index = [x for x in index if float(x.month) % 3.0 == 0 ] 

# Dates where the strategy will be backtested
index_ = [index_2.get_loc(x) for x in index if index_2.get_loc(x) > 1000]
```


```python
############################################################
# Building Constraints
############################################################

asset_classes = {'Assets': """AXISBANK.NS	BPCL.NS	BRITANNIA.NS	CIPLA.NS	DRREDDY.NS	EICHERMOT.NS	GAIL.NS	HDFCBANK.NS	HINDALCO.NS	HINDUNILVR.NS	INFY.NS	IOC.NS	ITC.NS	M&M.NS	MARICO.NS	ONGC.NS	RELIANCE.NS	SAIL.NS	SBIN.NS	SIEMENS.NS	SUNPHARMA.NS	TATACONSUM.NS	TATAMOTORS.NS	TATAPOWER.NS	TATASTEEL.NS	TITAN.NS VEDL.NS	WIPRO.NS""".split(),
                 'Industry': ['Consumer Discretionary','Consumer Discretionary',
                              'Consumer Discretionary', 'Consumer Staples',
                              'Consumer Staples','Energy','Financials',
                              'Financials','Financials','Financials',
                              'Health Care','Health Care','Industrials','Industrials',
                              'Industrials','Health care','Industrials',
                              'Information Technology','Information Technology',
                              'Materials','Telecommunications Services','Utilities',
                              'Utilities', 'Telecommunications Services', 'Financials', 'Financials', 'Financials', 'Financials']}

asset_classes = pd.DataFrame(asset_classes)
asset_classes = asset_classes.sort_values(by=['Assets'])

constraints = {'Disabled': [True, True, True],
               'Type': ['All Assets', 'All Classes', 'All Classes'],
               'Set': ['', 'Industry', 'Industry'],
               'Position': ['', '', ''],
               'Sign': ['<=', '<=', '>='],
               'Weight': [0.10, 0.20, 0.03],
               'Type Relative': ['', '', ''],
               'Relative Set': ['', '', ''],
               'Relative': ['', '', ''],
               'Factor': ['', '', '']}

constraints = pd.DataFrame(constraints)

display(constraints)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Disabled</th>
      <th>Type</th>
      <th>Set</th>
      <th>Position</th>
      <th>Sign</th>
      <th>Weight</th>
      <th>Type Relative</th>
      <th>Relative Set</th>
      <th>Relative</th>
      <th>Factor</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>True</td>
      <td>All Assets</td>
      <td></td>
      <td></td>
      <td>&lt;=</td>
      <td>10.0000%</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>True</td>
      <td>All Classes</td>
      <td>Industry</td>
      <td></td>
      <td>&lt;=</td>
      <td>20.0000%</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>True</td>
      <td>All Classes</td>
      <td>Industry</td>
      <td></td>
      <td>&gt;=</td>
      <td>3.0000%</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



```python
############################################################
# Building constraint matrixes for Riskfolio Lib
############################################################

import riskfolio as rp

A, B = rp.assets_constraints(constraints, asset_classes)
```


```python
%%time
############################################################
# Building a loop that estimate optimal portfolios on
# rebalancing dates
############################################################

models = {}

# rms = ['MV', 'MAD', 'MSV', 'FLPM', 'SLPM',
#        'CVaR', 'WR', 'MDD', 'ADD', 'CDaR']

rms = ['MV']

for j in rms:
    
    weights = pd.DataFrame([])

    for i in index_:
        Y = returns.iloc[i-1000:i,:] # taking last 4 years (250 trading days per year)

        # Building the portfolio object
        port = rp.Portfolio(returns=Y)
        
        # Add portfolio constraints
        #port.ainequality = A
        #port.binequality = B
        
        # Calculating optimum portfolio

        # Select method and estimate input parameters:

        method_mu='hist' # Method to estimate expected returns based on historical data.
        method_cov='hist' # Method to estimate covariance matrix based on historical data.

        port.assets_stats(method_mu=method_mu, method_cov=method_cov, d=0.94)
        port.mu = pd.DataFrame(
            np.sqrt(np.diag(port.cov)).reshape(-1, 28), columns=assets[:-1])
        # Estimate optimal portfolio:
        
        #port.solvers = ['MOSEK']
        #port.alpha = 0.05
        model='Classic' # Could be Classic (historical), BL (Black Litterman) or FM (Factor Model)
        rm = j # Risk measure used, this time will be variance
        obj = 'Sharpe' # Objective function, could be MinRisk, MaxRet, Utility or Sharpe
        hist = True # Use historical scenarios for risk measures that depend on scenarios
        rf = 0 # Risk free rate
        l = 0 # Risk aversion factor, only useful when obj is 'Utility'

        w = port.optimization(model=model, rm=rm, obj=obj, rf=rf, l=l, hist=hist)

        if w is None:
            w = weights.tail(1).T
        weights = pd.concat([weights, w.T], axis = 0)
    
    models[j] = weights.copy()
    models[j].index = index_
```

    CPU times: user 8.53 s, sys: 27 s, total: 35.5 s
    Wall time: 19.5 s



```python
############################################################
# Building the Asset Allocation Class
############################################################

class AssetAllocation(bt.Strategy):

    def __init__(self):

        j = 0
        for i in assets:
            setattr(self, i, self.datas[j])
            j += 1
        
        self.counter = 0
        
    def next(self):
        if self.counter in weights.index.tolist():
            for i in assets:
                w = weights.loc[self.counter, i]
                self.order_target_percent(getattr(self, i), target=w)
        self.counter += 1
```


```python
############################################################
# Backtesting Mean Variance Strategy
############################################################

assets = returns.columns.tolist()
weights = models['MV']

dd, cagr, sharpe = backtest(assets_prices,
                            AssetAllocation,
                            start=start,
                            end=end,
                            plot=True)
```


    
![png](MD_backtest_files/MD_backtest_18_0.png)
    



```python
############################################################
# Show Mean Variance Strategy Stats 
############################################################

print(f"Max Drawdown: {dd:.2f}%")
print(f"CAGR: {cagr:.2f}%")
print(f"Sharpe: {sharpe:.3f}")
```

    Max Drawdown: 31.31%
    CAGR: 17.85%
    Sharpe: 0.911

