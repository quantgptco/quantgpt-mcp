# Example prompts

Prompts to paste into Claude, ChatGPT, Gemini, Cursor or any client connected to QuantGPT MCP. Replace words in CAPS with your own tickers or lists. Each one names the tools it leads to and what a good answer contains, so you can tell when your AI skipped a step.

These are research questions. The answers describe the past: none of them is a forecast, a recommendation or an order, and a good answer shows the losing years, the sample size and the caveats.

Updated October 2026.

## Test a documented strategy

**1. A library strategy, losing years first**

> Search the QuantGPT library for value strategies. Run `piotroski-2000` with run_library_strategy from 1995, then show me its losing years, its worst drawdown and what the run approximates from the paper.

Tools: `search_library`, `run_library_strategy` (category `equities`). A good answer quotes the result's own approximation note next to the numbers and lists every year the strategy trailed its benchmark.

**2. A decade at a time**

> Run the library entry `jegadeesh-titman-1993` and read me the decade table, including every decade it trailed the benchmark. Then quote the net numbers at 10 and 25 bps per side.

Tools: `run_library_strategy` (category `equities`). A good answer says which cost level it quoted.

**3. What the library documents but cannot run yet**

> Look up `daniel-moskowitz-2016` (Momentum Crashes) in the library. What does it document, and why is it not runnable on QuantGPT yet?

Tools: `search_library`, `fetch`. A good answer separates the paper's claim from anything QuantGPT computed.

## Check a finding before you trust it

**4. Costs and the skeptic pass**

> Backtest the top 20 US stocks by 12-1 momentum with a Piotroski F-Score of 7 or more, monthly, from 1995. Quote the net numbers at 10 and 25 bps per side, then run challenge_finding and lead with the strongest objection.

Tools: `run_backtest`, `challenge_finding`, `fetch_job`. A good answer says that a market-wide backtest is not survivorship-free yet, and gives HOLDS, FRAGILE or DOES NOT HOLD with the objection first.

**5. Is the best variant real or luck?**

> Sweep top_n from 10 to 50 in steps of 10 on that backtest, then draw the sweep chart and tell me whether the best variant can be told apart from sweep luck.

Tools: `run_sweep` for the quick ranked table; for the chart, `run_backtest` on each variant with charts off, then `make_chart_pack` with `sweep` (category `equities`). A good answer reports the deflated Sharpe and says so plainly when it is under 0.95.

**6. Edge or exposure?**

> Run run_factor_regression on the same criteria. How much of the return is market, size, value, momentum and quality exposure, and is the alpha's t-stat above 2?

Tools: `run_factor_regression` (category `equities`). A good answer calls an alpha with a t-stat under 2 noise.

**7. A list you picked today**

> Backtest TICKER1, TICKER2 and TICKER3 held equal weight since 2010, rebalanced monthly, against the S&P 500. Tell me which names were not there for the whole window.

Tools: `run_backtest` with `tickers`. A good answer adds that a list chosen today knows how its names turned out.

## Read one company

**8. The report card**

> Grade TICKER with grade_stock and name its two weakest dimensions.

Tools: `grade_stock`.

For a list, ask for grade_table: "Grade TICKER1, TICKER2 and TICKER3 in one table" (category `equities`, up to 25 names).

**9. Straight from the filings**

> Pull TICKER's latest quarters with filing_facts and cite the filing link for every figure you quote.

Tools: `filing_facts` (category `equities`).

**10. What the price implies**

> Build a financial_model for TICKER and lead with the growth the current price implies. No price target.

Tools: `financial_model` (category `equities`). A good answer states its assumptions (discount rate, terminal growth) as inputs you can change.

**11. The AAOIFI screen**

> Run halal_screen on TICKER and pass on its disclosures.

Tools: `halal_screen` (category `equities`). A good answer calls it an AAOIFI-based screen, never a ruling.

## Markets, macro and events

**12. A classic timing rule**

> Test the 10-month moving average rule on the S&P 500 with run_market_timing and show every year it lagged buy and hold.

Tools: `run_market_timing` (category `equities`).

**13. A yield-curve regime**

> With run_macro_regime, hold the S&P 500 only while T10Y3M is above 0, else T-bills. Show the drawdowns and repeat the latest-vintage caveat.

Tools: `get_macro_series`, `run_macro_regime` (category `macro`).

**14. The event desk**

> On the event desk, list the last 30 days of events in the markets lens and show the S&P 500 since each one.

Tools: `event_desk` (category `events`).

**15. A futuretest**

> Futuretest this book against the oil_shock scenario: TICKER1 40%, TICKER2 30%, TICKER3 30%. Show every analogue's path, the worst one first, and say how many analogues there are.

Tools: `futuretest` (category `events`, coming soon: until it opens it answers with a coming-soon note). A good answer says it is the record of past episodes, not a forecast.

## Other markets

**16. Prediction market calibration**

> Backtest buying YES on resolved prediction markets 7 days before close and show the calibration table: at which prices was the crowd furthest from what happened, and how many markets sit behind each row?

Tools: `run_pm_backtest` (category `alt_markets`, coming soon: until it opens it answers with a coming-soon note; the library lists `pm-longshot-bias`, on resolved Polymarket markets, as runnable through `run_library_strategy`). A good answer gives the count behind each price bucket and says that no fees or spread are modelled.

**17. The memecoin graveyard**

> Show me meme_survival: of the pools launched each month, how many still traded at 180 days?

Tools: `meme_survival` (category `alt_markets`). A good answer leads with the survival numbers.

If your client lists only the core tools, ask your AI to call `activate_category` for the category named above, or to run the tool through `call_tool`.
