# Stock Portfolio Analysis

This project analyzes the performance of a stock portfolio by retrieving historical stock price data, calculating portfolio metrics, and visualizing the portfolio's performance over time.

## Overview

This project is solely to demostrate my Python skills. I have used Python to retrieve historical stock price data using the `yfinance` library. It then calculates various portfolio metrics, including cumulative returns, volatility, sharpe ratio, and maximum drawdown. Finally, it presents the results through console output and generates a line plot to visualize the portfolio's value over time.

## Getting Started

### Prerequisites

- Python 3.x
- `yfinance` library
- `pandas` library
- `matplotlib` library

### Note

Used Google Colab for coding. Thus, recommended Google Colab to run the code smoothly. In case of a problem, please make sure 'pip install yfinance' is installed in your terminal.
If not, Please add a separate line of coding chunck above and run the the above code first.

```bash
pip install yfinance
```
#### Install the required dependencies:
pip install yfinance pandas matplotlib

## Portfolio Metrics

The Stock Portfolio Analysis project calculates several metrics to evaluate the portfolio's performance:

- **Cumulative Returns:** The total returns of the portfolio over the specified date range.
- **Volatility:** The standard deviation of the portfolio's returns, representing its risk.
- **Sharpe Ratio:** The ratio of the portfolio's average returns to its volatility, indicating its risk-adjusted performance.
- **Maximum Drawdown:** The largest percentage drop in portfolio value from a peak to a subsequent trough.

## Results

When running the script, the calculated portfolio metrics will be displayed in the console, providing insights into the portfolio's performance. Additionally, a line plot will be generated, illustrating the value of the portfolio over time.

## Limitations and Future Improvements

Currently, the code assumes a fixed portfolio and date range. Future improvements could include making the portfolio and date range customizable through user input, allowing users to analyze different portfolios and time periods and connecting the data to API which requires premium packages to further acccess the data of which I do not have.

Furthermore, while the project utilizes a line plot to visualize the portfolio's performance, more sophisticated visualization techniques could be explored to enhance the representation. For example, candlestick charts or interactive plots could provide a more detailed view of the portfolio's fluctuations.

## The code
```
from datetime import datetime
import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt

def calculate_portfolio_metrics(stocks):
    # Define the date range
    start_date = datetime(2020, 1, 1)
    end_date = datetime(2021, 1, 1)

    # Retrieve the stock data
    stock_data = pd.DataFrame()
    for symbol in stocks:
        data = yf.download(symbol, start=start_date, end=end_date)
        stock_data[symbol] = data['Close']

    # Calculate daily returns
    daily_returns = stock_data.pct_change()

    # Calculate portfolio value over time
    portfolio_value = (daily_returns * (1 / len(stocks))).sum(axis=1).cumsum() + 1

    # Calculate portfolio metrics
    portfolio_returns = portfolio_value.pct_change()
    portfolio_cumulative_returns = portfolio_value[-1] - 1
    portfolio_volatility = portfolio_returns.std()
    portfolio_sharpe_ratio = portfolio_returns.mean() / portfolio_returns.std()
    portfolio_max_drawdown = (portfolio_value / portfolio_value.cummax() - 1).min()

    return portfolio_cumulative_returns, portfolio_volatility, portfolio_sharpe_ratio, portfolio_max_drawdown, daily_returns


# User input for stocks
stocks = input("Enter the stocks in your portfolio (comma-separated): ").split(",")

# Calculate portfolio metrics
portfolio_cumulative_returns, portfolio_volatility, portfolio_sharpe_ratio, portfolio_max_drawdown, daily_returns = calculate_portfolio_metrics(stocks)

# Print portfolio metrics
print("Portfolio Metrics:")
print("------------------")
print(f"Cumulative Returns: {portfolio_cumulative_returns:.2%}")
print(f"Volatility: {portfolio_volatility:.2%}")
print(f"Sharpe Ratio: {portfolio_sharpe_ratio:.2f}")
print(f"Max Drawdown: {portfolio_max_drawdown:.2%}")

# Calculate portfolio value for plotting
portfolio_value = (daily_returns * (1 / len(stocks))).sum(axis=1).cumsum() + 1

# Plot portfolio performance
plt.figure(figsize=(10, 6))
plt.plot(portfolio_value)
plt.title("Stock Portfolio Performance")
plt.xlabel("Date")
plt.ylabel("Portfolio Value")
plt.grid(True)
plt.show()
```
