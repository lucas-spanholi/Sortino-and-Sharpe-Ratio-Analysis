# Sortino-and-Sharpe-Ratio-Analisys
Bitcoin and S&amp;P500 Performance Analysis with Sharpe and Sortino Ratios  Analyze and compare the performance of Bitcoin (BTC) and S&amp;P500 using Sharpe and Sortino Ratios. This project includes detailed visualizations, Pearson correlation analysis, and modular code for easy extension to other assets.

# Bitcoin and S&P500 Performance Analysis with Sharpe and Sortino Ratios

This project analyzes the performance of Bitcoin (BTC) and S&P500 by calculating and comparing the Sharpe and Sortino Ratios. The analysis includes plotting the data, calculating Pearson correlation, and visually comparing the risk-adjusted performance of these financial instruments.

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
  - [Calculating Sharpe and Sortino Ratios](#calculating-sharpe-and-sortino-ratios)
  - [Plotting Performance](#plotting-performance)
  - [Calculating Pearson Correlation](#calculating-pearson-correlation)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)

## Introduction

The Sharpe Ratio and Sortino Ratio are crucial metrics for assessing the risk-adjusted returns of an investment. This project leverages these metrics to analyze the performance of Bitcoin (BTC) and S&P500. By using these ratios, we can better understand the risk and return profile of these assets.

## Features

- **Sharpe and Sortino Ratios Calculation**: Compute rolling Sharpe and Sortino Ratios for BTC and S&P500.
- **Plotting**: Generate detailed plots to visualize Sharpe and Sortino Ratios along with the asset performance.
- **Pearson Correlation**: Calculate and display the Pearson correlation between Sharpe and Sortino Ratios for the given assets.
- **Modular and Extensible**: Easily extendable to include more assets or other risk metrics.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/bitcoin-sp500-analysis.git
   cd bitcoin-sp500-analysis
2. Create a virtual environment:
   python -m venv env
   source env/bin/activate  # On Windows, use `env\Scripts\activate`
3. Install the required packages:
   pip install -r requirements.txt
## Usage
# Calculating Sharpe and Sortino Ratios
# The notebook includes a function get_sharpe_sortino that calculates the rolling Sharpe and Sortino Ratios for the given data.

def get_sharpe_sortino(data, rf):
  for i in range(len(data.columns)):
    data[data.columns[i]+'_sharpe'] = qs.stats.rolling_sharpe(data[data.columns[i]], rf=rf)
    data[data.columns[i]+'_sortino'] = qs.stats.rolling_sortino(data[data.columns[i]], rf=rf)
  return data

## Plotting Performance
# The notebook provides a function plot_sharpe_sortino that generates separate plots for each asset, displaying their Sharpe and Sortino Ratios alongside their log-transformed performance.

def plot_sharpe_sortino(data, *tickers):
    for ticker in tickers:
        fig = make_subplots(specs=[[{"secondary_y": True}]])

        # Columns to be used
        sharpe_col = f"{ticker}_sharpe"
        sortino_col = f"{ticker}_sortino"
        ticker_col = ticker

        # Checking if the columns exist in the DataFrame
        if sharpe_col in data.columns:
            fig.add_trace(go.Scatter(
                x=data.index,
                y=data[sharpe_col],
                name=sharpe_col,
                line=dict(color='blue', width=2)),
                secondary_y=False
            )
        else:
            print(f"Column {sharpe_col} not found in the data")

        if sortino_col in data.columns:
            fig.add_trace(go.Scatter(
                x=data.index,
                y=data[sortino_col],
                name=sortino_col,
                line=dict(color='green', width=2)),
                secondary_y=False
            )
        else:
            print(f"Column {sortino_col} not found in the data")

        if ticker_col in data.columns:
            fig.add_trace(go.Scatter(
                x=data.index,
                y=np.log(data[ticker_col]),
                name=ticker_col,
                line=dict(color='black', width=2)),
                secondary_y=True
            )
        else:
            print(f"Column {ticker_col} not found in the data")

        # Updating plot layout
        fig.update_layout(
            title=f'Sharpe and Sortino Ratios with {ticker} Performance',
            xaxis_title='Date',
            yaxis_title='Ratios',
            yaxis2_title='Log Scale (Ticker Value)'
        )

        # Displaying the plot
        fig.show()

# Example usage:
# Ensure that `data` is a DataFrame containing the columns 'BTC_sharpe', 'BTC_sortino', and 'BTC'
plot_sharpe_sortino(data, 'BTC', 'S&P500')

## Calculating Pearson Correlation
# The notebook includes a function pearson_correlation that calculates the Pearson correlation coefficient between the Sharpe and Sortino Ratios for the specified tickers.

def pearson_correlation(data, *tickers):
    for ticker in tickers:
        corr, p = stats.pearsonr(data[f'{ticker}_sharpe'], data[f'{ticker}_sortino'])
        print(f'Pearson Correlation - {ticker}, r=%.3f' % corr, 'p=%.3f' % p)

# Example usage:
pearson_correlation(data, 'BTC', 'S&P500')

## Examples
# Scatter Plot of BTC Sharpe vs. Sortino Ratios

fig_corr = go.Figure()

fig_corr.add_trace({'type': 'scatter',
                    'x': data['BTC_sharpe'],
                    'y': data['BTC_sortino'],
                    'mode': 'markers',
                    'line': {'color': 'blue'}})

fig_corr.update_layout(template='simple_white', paper_bgcolor="#f7f8fa",
                       margin=dict(l=70, r=20, t=20, b=70),
                       xaxis_title='<b>Sharpe', yaxis_title='<b>Sortino',
                       width=500, height=500)

fig_corr.show()

## Contributing
We welcome contributions! Please follow these steps:

Fork the repository.
Create a new branch (git checkout -b feature-branch).
Make your changes.
Commit your changes (git commit -m 'Add some feature').
Push to the branch (git push origin feature-branch).
Open a pull request.

