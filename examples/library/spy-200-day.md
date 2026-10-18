# SPY above its 200-day moving average

A trend rule on one ETF: hold SPY while it closes above its 200-day moving average, else hold cash. It is not a library entry; it runs as a criteria backtest on a one-ticker list.

## The prompt

> With QuantGPT's run_backtest, backtest SPY alone: tickers ["SPY"], one filter px_vs_sma200 >= 0, top_n 1, monthly, against $SPXTR. Show the CAGR, Sharpe and worst drawdown beside the benchmark's, the decade table, how many months were in cash, and the net numbers at 10 bps per side. Then say plainly which decades it trailed buy and hold.

Tools: `run_backtest` (core).

## What the run is

At each month-end close, SPY is held for the next month when its close is at or above its 200-day average (`px_vs_sma200 >= 0`); otherwise the book holds cash. In this run the cash months read 0.0 in the return series. The check is monthly by default; `rebalance: "daily"` checks every session but runs from 2016 only. Criteria backtests start in 1998 unless you set `start_year`.

Two close relatives answer through other tools:

- `run_market_timing` with `sma10m` (category `equities`): the 10-month moving average rule on the S&P 500 index, from the library entry `faber-gtaa-2007`.
- `run_market_timing` with `goldencross`: the 50-day average against the 200-day average on the S&P 500 index (`golden-cross` in the library).

## What comes back

- `meta`: the criteria, including the line that a list chosen today knows how its names turned out, the cash rule, the start and end dates.
- `result.tickers`: each name's history range, first and last eligible period and the number of periods it qualified.
- `result.strat` and `result.bench`, `result.verdict` and `verdict_means`, `result.walkforward`, `result.decades`.
- `result.costs`: net figures at 5, 10, 25 and 50 bps per side, with turnover.
- `result.returns`: the monthly series, with the cash months visible.
- `notes`: how many periods held cash.

A good answer gives the number of cash months and every decade the rule trailed buy and hold, and says the rule was picked with hindsight on one ETF.
