# Historical Pattern Stock Scanner

A small Python project for scanning historical stock data and flagging chart setups that resemble a classic "lifetime high acts as support" pattern.

This repo is intentionally simple: it fetches price history, identifies the stock's historical peak, checks whether price repeatedly holds near that level, and then looks for a follow-through move higher. It is built as a research and screening tool rather than a trading system.

## What it does

The script:

- downloads stock data from Yahoo Finance
- finds a symbol's lifetime high
- checks whether the price stays near that high within a tolerance band
- counts how many trading sessions the stock remains near that level
- looks for a later price appreciation from the support area
- returns matching symbols for further review

This is useful for quickly identifying patterns that may deserve deeper technical or fundamental research.

## Why this project exists

The goal is to experiment with a behavioral market pattern:

- a stock reaches or nears a historical high
- price fails to break down materially
- the area around the previous high starts acting like support
- later, the stock shows renewed strength and moves higher

That setup is not a prediction engine, but it creates a structured way to screen for historical price behavior that often gets attention in technical analysis.

## Project structure

```text
historical-pattern-Stock-Scanner/
├── AutomatedStockScanner.py
├── README.md
└── .gitignore
```

The main logic lives in `AutomatedStockScanner.py`.

## Requirements

- Python 3.8+
- pandas
- yfinance

Install dependencies:

```bash
pip install pandas yfinance
```

## Quick start

Run the script directly:

```bash
python AutomatedStockScanner.py
```

The script currently includes example usage for a short list of stocks:

```python
stocks_to_scan = ['AAPL', 'MSFT', 'GOOGL']
results = stock_scanner(stocks_to_scan)
print(results)
```

## Core functions

### `get_stock_data(stock_symbol, years=10)`
Fetches historical OHLC data for a symbol over the requested time window.

### `find_lifetime_high(stock_data)`
Returns the highest `High` value seen in the dataset.

### `check_support(stock_data, lifetime_high, threshold=0.02, days=5)`
Checks if price repeatedly stays within a defined percentage of the lifetime high for enough consecutive trading sessions.

### `check_price_appreciation(stock_data, support_date, appreciation_threshold=0.10)`
Looks for a later move upward from the support area, measuring whether price gains a set percentage.

### `stock_scanner(stocks, years=10, threshold=0.02, days=5, appreciation_threshold=0.10)`
Loops through a list of symbols and returns candidates that meet the pattern conditions.

### `analyze_results(results)`
Provides a basic summary of matches and opportunity counts.

## Example output

```python
[('AAPL', Timestamp('2023-06-15 00:00:00'))]
```

This output is a simple list of stock symbols and the date at which the pattern was detected.

## Tuning the scanner

You can adjust the sensitivity of the pattern by changing the parameters:

```python
stock_scanner(
    stocks=['AAPL', 'MSFT', 'NVDA'],
    years=10,
    threshold=0.02,
    days=5,
    appreciation_threshold=0.10
)
```

Parameter meanings:

- `years`: how much history to download
- `threshold`: maximum allowed distance from the lifetime high in percentage terms
- `days`: how many consecutive sessions must remain near the level
- `appreciation_threshold`: required upside move after the support test

## Limitations

This project is designed for learning and screening, not for live trading decisions.

- it depends on Yahoo Finance data availability
- it uses a fixed and simple logic model
- it does not include risk management, position sizing, or trade execution
- it does not account for earnings, news, macro conditions, or sector context
- it cannot confirm whether a pattern will continue to work in the future

## Disclaimer

This project is for educational and research purposes only. It should not be considered financial advice or a signal to buy or sell securities.

## License

This project is licensed under the MIT License.

## Future ideas

Possible improvements include:

- chart plotting for detected patterns
- CSV export of scanner results
- scanning a watchlist from a file
- more robust pattern scoring
- backtesting against historical price action
- support for additional market data sources

