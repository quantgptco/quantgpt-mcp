# Piotroski F-Score

Library id `piotroski-2000`. Source: Piotroski (2000), Journal of Accounting Research.

## The prompt

> Search the QuantGPT library for the Piotroski F-Score, then run `piotroski-2000` with run_library_strategy. Show me the CAGR, Sharpe and worst drawdown beside the benchmark's, the decade table, the first month it actually held stocks, and the net numbers at 10 and 25 bps per side. Then quote what the run approximates from the paper and how it differs.

Tools: `search_library`, then `run_library_strategy` (category `equities`).

## What the run is

Liquid US stocks with an F-Score of at least 7 (8 of the 9 signals; the current-ratio signal is not in the data) and price-to-book at most 1.5, the 30 lowest price-to-book, equal weight, long only, re-formed monthly, against the S&P 500 total return (`$SPXTR`). Piotroski formed annually and held one year. Fundamentals are thin before about 1998, so the early months hold cash, and the result names its first invested month.

## What comes back

- `meta`: the criteria in plain lines (the universe, the cash rule, the first period that held a book, the share of delisted companies the universe could include), the start and end dates, the number of months and the walk-forward split year.
- `result.strat` and `result.bench`: CAGR, volatility, Sharpe, worst drawdown (`maxdd`), share of winning months, years, growth of 1 (`final`) and the probabilistic Sharpe ratio (`psr`).
- `result.verdict` and `verdict_means`: one of three findings (DATA VALIDATED, TRADE-OFF, NO EDGE), each a description of what the data did over the window, not a prediction.
- `result.walkforward`: the same statistics before and after the split year, for the strategy and the benchmark.
- `result.decades`: the annualized return per decade beside the benchmark's.
- `result.returns`: the full monthly return series for the strategy and the benchmark.
- `result.costs`: average turnover and, at 5, 10, 25 and 50 bps per side, the net CAGR, Sharpe, drawdown and the yearly drag.
- `result.trades`, `result.benchmark`, `result.charts`: the trade count, the benchmark basis, and public chart pack links (finding card, growth, drawdowns, calendar years, costs).
- `notes`: how many periods held cash and how delisted holdings were handled.
- `library`: `as_published` (the paper's rule), `ran_with` (the exact criteria), `what_this_run_approximates` and `how_the_run_differs`.

A good answer quotes `how_the_run_differs` next to the numbers, names the cash months, and lists every decade the run trailed the benchmark.
