# 12-1 month momentum

Library id `jegadeesh-titman-1993`, named in the library "Inspired by Jegadeesh and Titman: Cross-Sectional Momentum". Source: Jegadeesh and Titman (1993), Journal of Finance.

## The prompt

> Run the QuantGPT library entry `jegadeesh-titman-1993` with run_library_strategy. Read me the decade table, including every decade it trailed the benchmark, the worst drawdown, and the net numbers at 10 and 25 bps per side. Then run the plain version with run_backtest: rank by ret_12_1, top 20, monthly, no filters, and tell me what changed between the two runs.

Tools: `run_library_strategy` (category `equities`), then `run_backtest` (core).

## What the run is

The 20 liquid US stocks with the highest 12-1 month return (the return over the past 12 months, skipping the latest month), leaving out names up more than 150%, equal weight, long only, re-formed monthly and held one month, against the S&P 500 total return. The paper ranked NYSE and AMEX stocks, bought the top decile and sold the bottom, and held 3 to 12 months in overlapping cohorts. The universe is too thin before mid-1995 to fill the book, so those months hold cash.

The second run in the prompt drops the 150% cap. `ret_12_1` is the factor id; `list_factors` (category `equities`) shows the rest of the vocabulary.

## What comes back

- From `run_library_strategy`: the same result as any criteria backtest (`meta`, `result.strat` and `result.bench`, `verdict`, `walkforward`, `decades`, `returns`, `costs`, `trades`, `charts`, `notes`) plus the `library` block with `as_published`, `ran_with`, `what_this_run_approximates`, `how_the_run_differs` and a note that a run named "Inspired by" only loosely follows its source.
- From `run_backtest`: the same result without the `library` block. A run that takes a while answers with a job id; your AI collects it with `fetch_job`.

A good answer says the library run is inspired by the paper, not the paper's own test, and never quotes it as the paper's result.
