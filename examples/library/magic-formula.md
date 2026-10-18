# The Magic Formula

Library id `greenblatt-magic-2005`, named in the library "Inspired by Greenblatt: Magic Formula". Source: Joel Greenblatt, "The Little Book That Beats the Market" (2005).

## The prompt

> Run the QuantGPT library entry `greenblatt-magic-2005` with run_library_strategy. Put its CAGR, Sharpe and worst drawdown beside the benchmark's, show the decades it trailed, and quote the net numbers at 25 bps per side. Then tell me in two lines how this run differs from Greenblatt's own formula, using the result's how_the_run_differs field.

Tools: `run_library_strategy` (category `equities`).

## What the run is

Liquid US stocks with a trailing twelve-month return on equity of at least 15%, the top 30 by earnings yield (net income over market cap), equal weight, long only, re-formed monthly, against the S&P 500 total return; cash until late 1996. Greenblatt summed the ranks of EBIT over enterprise value and return on capital, left out financials and utilities, and held each stock for a year. Return on equity and net income differ from his inputs, so both legs differ.

## What comes back

- `meta`: the criteria, the universe, the cash rule and the first period that held a book.
- `result.strat` and `result.bench`: CAGR, volatility, Sharpe, worst drawdown, share of winning months, growth of 1 and the probabilistic Sharpe ratio.
- `result.verdict` and `verdict_means`, `result.walkforward` (before and after the split year) and `result.decades`.
- `result.costs`: net figures at 5, 10, 25 and 50 bps per side, with turnover.
- `result.returns`: the full monthly series, so the losing years can be counted.
- `library`: `as_published`, `ran_with`, `what_this_run_approximates` and `how_the_run_differs`.

A good answer leads with what the run approximates, then the numbers, losing decades included.
