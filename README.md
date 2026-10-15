# QuantGPT MCP

**The data and trading platform for AI: decades of US market data, backtests in seconds.**

QuantGPT is the data and trading platform for AI. Claude and ChatGPT connect over MCP to decades of US market data, and backtests run in seconds on QuantGPT Cloud. On QuantGPT Pro, your own strategy runs there too and trades in your own brokerage account. Start with a 3-day free trial of QuantGPT or QuantGPT Pro, cancel any time.

| | |
|---|---|
| Server | `https://terminal.quantgpt.co/mcp` |
| Transport | Streamable HTTP, JSON-RPC 2.0 over POST, stateless |
| Sign-in | OAuth 2.1 (dynamic client registration, PKCE S256), or a key in a header |
| Works with | Claude, ChatGPT, Gemini, Cursor, Claude Code and any MCP client |
| Plans | QuantGPT $49 a month, QuantGPT Pro $149 a month, each with a 3-day free trial |
| Website | https://quantgpt.co |
| Registry name | `co.quantgpt/mcp` |

Updated October 2026. This repository holds no server code. It is the public listing for the hosted server: how to connect each client, what every tool does, example prompts, plans and limits.

## What your AI can do with it

- **Backtest any idea in seconds.** Describe a strategy in plain words; your AI turns it into criteria (up to 8 filters, a rank, a holding count) and runs it walk-forward against a benchmark on decades of US market data, with a cost model and a decade-by-decade table. Up to 100 variants run in one call.
- **Grade any US stock.** A+ to F on six dimensions against the whole market, point in time, with the drivers, the peers and a chart pack. Up to 25 tickers in one call, or the leaderboard for a sector.
- **Query the data itself.** Prices, total returns, fundamentals joined by filing date, 55+ factors, FRED macro, insider filings and short interest, through ready-made calls or read-only SQL over the whole warehouse.
- **Start from the literature.** 175 documented strategies from papers, books and practitioners, 63 of them runnable in one call.
- **Work on a portfolio.** A frontier over your tickers, a review of your holdings, the trade list from your criteria to the book they imply today, paper books marked nightly, and alerts when a name enters or leaves a screen.
- **Go past US stocks.** Commodity futures, macro regimes, weather studies, crypto with funding rates, prediction markets and memecoins.
- **Chart and cite.** Branded charts with the numbers that drew them, and `search` and `fetch` so ChatGPT deep research can cite the library, the studies and the forum.
- **Run your own strategy on QuantGPT Pro.** QuantGPT Cloud hosts your strategy as your own code, on your schedule, and it trades in your own brokerage account under your keys. You set it up in the terminal; no MCP tool places an order.

## Contents

1. [Connect your AI](#connect-your-ai)
2. [What your AI can call](#what-your-ai-can-call)
3. [Strategies you can ask about](#strategies-you-can-ask-about)
4. [Plans and limits](#plans-and-limits)
5. [What the data is, and is not](#what-the-data-is-and-is-not)
6. [Disclaimer](#disclaimer)

## Connect your AI

Claude, ChatGPT and the Gemini app sign in to QuantGPT through OAuth: no key to paste. The connection lands on your account page as a key labelled with the client's name, and you can revoke it there. Claude Code, Gemini CLI, Cursor and any other client below use a key.

### Get a key

1. Sign in at https://terminal.quantgpt.co (new here? [start a 3-day free trial](https://terminal.quantgpt.co/join)).
2. Open your account page. Under Keys for your AI, type a label and press Issue key.
3. Copy the `qg_sk_...` value now. Only its hash is stored, so it is shown once.

### Claude (claude.ai and Claude Desktop)

1. Customize > Connectors, then + > Add custom connector (on Team or Enterprise an Owner may need to add it first).
2. Name it QuantGPT, paste `https://terminal.quantgpt.co/mcp`, leave the OAuth client fields empty, and click Add.
3. Press Connect. Claude opens QuantGPT's sign-in page: sign in and approve. The connection shows on your account page as `connector: Claude`.

Guide on quantgpt.co: https://quantgpt.co/learn/connect-claude.html

### Claude Code

```
claude mcp add --transport http quantgpt https://terminal.quantgpt.co/mcp --header "Authorization: Bearer qg_sk_YOUR_KEY"
```

Add `--scope user` to have it in every project. Then ask Claude Code to call `describe_capabilities`.

### ChatGPT

1. In ChatGPT, Settings > Security and login > turn on Developer mode. OpenAI offers it on Plus, Pro, Business, Enterprise and Education accounts, on the web.
2. Go to chatgpt.com/plugins and select the plus button. Name the app QuantGPT and, under Connection, enter `https://terminal.quantgpt.co/mcp`.
3. ChatGPT registers itself and sends you to QuantGPT's sign-in, now or on its first call. Sign in and press CONNECT; there is no key to paste. The connection shows on your account page as `connector:` plus the name ChatGPT registered. In a chat, choose Developer mode from the + menu and select QuantGPT.

On a Business, Enterprise or Education workspace, developer mode can depend on the workspace's policy, so an admin may have to allow it first. ChatGPT deep research works through a connector's `search` and `fetch` tools; QuantGPT ships both, so deep research can search the library, the studies and the forum and cite what it used.

Guide on quantgpt.co: https://quantgpt.co/learn/connect-chatgpt

### Gemini

Gemini CLI, with a key:

```
gemini mcp add -s user --transport http --header "Authorization: Bearer qg_sk_YOUR_KEY" quantgpt https://terminal.quantgpt.co/mcp
```

`-s user` saves it in `~/.gemini/settings.json` in your home folder. Without it, Gemini CLI saves to the current project's `.gemini/settings.json`, where a key can end up in a git repository.

Or add it to `~/.gemini/settings.json` (every project) or `.gemini/settings.json` (one project):

```json
{
  "mcpServers": {
    "quantgpt": {
      "httpUrl": "https://terminal.quantgpt.co/mcp",
      "headers": { "Authorization": "Bearer qg_sk_YOUR_KEY" }
    }
  }
}
```

Run `/mcp` inside Gemini CLI to see the server and its tools.

The Gemini app: at gemini.google.com open Settings > Connected Apps, and under Custom apps choose Add a custom app. Paste `https://terminal.quantgpt.co/mcp`, then sign in to QuantGPT. Google decides who can add custom apps (in September 2026: 18 or over, in the US, signed in with a personal Google Account, Keep Activity on, set up in the web app): https://support.google.com/gemini/answer/17209137

Guide on quantgpt.co: https://quantgpt.co/learn/connect-gemini

### Cursor

In `.cursor/mcp.json` (one project) or `~/.cursor/mcp.json` (every project):

```json
{
  "mcpServers": {
    "quantgpt": {
      "url": "https://terminal.quantgpt.co/mcp",
      "headers": { "Authorization": "Bearer ${env:QUANTGPT_KEY}" }
    }
  }
}
```

Set `QUANTGPT_KEY` to your `qg_sk_...` key in the environment Cursor starts from, so the key never sits in the file.

Guide on quantgpt.co: https://quantgpt.co/learn/connect-cursor

### Any other MCP client

```
URL:   https://terminal.quantgpt.co/mcp
Auth:  Authorization: Bearer qg_sk_YOUR_KEY   (X-API-Key: qg_sk_YOUR_KEY is also accepted)
Type:  Streamable HTTP, JSON-RPC 2.0, stateless
```

A request without a valid key gets `401` and a `WWW-Authenticate` header that points OAuth clients to `https://terminal.quantgpt.co/.well-known/oauth-protected-resource`.

No connector option on your plan? The setup prompt at https://terminal.quantgpt.co/pastepack.txt gives any chat AI the same context in one block you paste, without MCP. Signed-in members find the Claude and ChatGPT steps, this address and the prompt library at https://terminal.quantgpt.co/connect.

### Your first call

Ask your AI to call `describe_capabilities`: it returns the whole map (every factor, operator, benchmark, library category and methodology rule). On connect the server lists 14 core tools and 5 discovery tools; the other 57 sit in seven categories. Your AI calls `available_categories`, then `activate_category`, to list a category for your key, or runs any tool by name through `call_tool`. With a key-based client you can add `?tools=all` to the URL to list every tool at once.

## What your AI can call

76 tools, 68 of them switched on today; the 8 marked (coming soon) answer with a coming-soon note until they open. None of them places, modifies or cancels an order at any broker. A few write to your own QuantGPT account: saved studies, paper books, sealed books, alerts, event rules and the panels on your terminal.

### Listed on connect (14)

| Tool | What it does |
|---|---|
| `search` | Searches QuantGPT's own record (library strategies, studies, forum threads, your saved studies, the glossary) and returns citable results with ids and URLs. |
| `fetch` | Fetches one of those records in full by the id `search` returned. |
| `describe_capabilities` | The full machine-readable map: factors, operators, benchmarks, timing rules, library categories, methodology. Call it first. |
| `guide` | The onboarding curriculum (THE RAMP), taught one step at a time. |
| `get_prompt_library` | 31 ready prompts, each mapped to the tools that answer it. |
| `search_library` | Searches the 175 documented strategies by keyword; each result says whether it is runnable. |
| `grade_stock` | Grades one ticker A+ to F on six dimensions against the whole universe, point in time, with a chart pack. |
| `run_backtest` | Backtests any criteria (up to 8 filters, a rank, top N equal weight; monthly, weekly or daily formation) walk-forward against a benchmark, with a cost model and a decade table. |
| `run_sweep` | Up to 100 criteria variants in one call, returned as a ranked comparison table. |
| `challenge_finding` | The skeptic pass: eleven checks that try to break a finding, strongest objection first. |
| `fetch_job` | Collects a long-running job (a backtest, regression or skeptic pass) by its job id. |
| `pre_trade_brief` | (coming soon) A ticker's grade, whether it passes your criteria today, the criteria's finding with the skeptic pass, and what your past findings did next. Findings, not orders. |
| `query_warehouse` | Read-only SQL (DuckDB) over every research database: SELECT, WITH, DESCRIBE and SHOW only, 5,000-row cap, 45-second timeout. |
| `list_warehouse` | The map of every database and table `query_warehouse` can read. |

### Discovery (5)

| Tool | What it does |
|---|---|
| `available_categories` | The categories this key can switch on, with counts and what is active now. |
| `available_tools` | Names, descriptions and input schemas of the tools in a category. |
| `activate_category` | Adds categories to this key's tool list, remembered across sessions. |
| `deactivate_category` | Removes categories from this key's tool list. |
| `call_tool` | Runs any tool by name, listed or not. |

### Equities: category `equities` (21)

| Tool | What it does |
|---|---|
| `run_screen` | Today's tickers matching your criteria, in the same vocabulary as `run_backtest`: the output of your criteria, never a recommendation. |
| `top_graded` | The letter-grade leaderboard over the whole US universe, optionally by sector. |
| `grade_table` | Grades up to 25 tickers at once: the composite and the six dimension letters per name, best composite first, with a bad ticker listed in errors. |
| `run_library_strategy` | Runs a runnable library entry's documented criteria by id, with overrides; the result says what the run approximates. |
| `run_daily_backtest` | Single-ticker session legs on daily bars: overnight, intraday, buy and hold. |
| `short_interest` | FINRA short interest at each settlement date, days to cover and the SEC's fails to deliver for one US ticker. |
| `run_market_timing` | Classic index timing rules: 10-month moving average, golden cross, sell in May, Santa rally. |
| `run_breadth_study` | Decades of computed NYSE breadth: Hindenburg omens under both published specs, clusters, forward returns against a baseline. |
| `run_factor_regression` | Regresses a criteria set's monthly returns on the house factors: loadings, t-stats and alpha. |
| `list_factors` | The factor vocabulary for filters and ranking. |
| `mint_factor` | Writes a factor as an expression in qlib's Alpha158 grammar, then ranks or filters by it; look-ahead is refused. |
| `list_minted_factors` | The factors minted in this server session, with their expressions and proof stats. |
| `make_chart_pack` | A branded chart pack (finding card, growth, drawdowns, yearly bars) from a return series you computed, or one chart for a whole parameter sweep. |
| `findings_journal` | Your finished studies and what each one did next, on data that arrived after it. |
| `save_study` | Puts a configured study on your saved-studies shelf in the terminal. |
| `list_saved_studies` | Your saved-studies shelf. |
| `financial_model` | A company's model from point-in-time statements: TTM figures, a DCF in three scenarios, the growth the price implies and a sensitivity grid. Never a price target. |
| `filing_facts` | A company's latest quarters straight from its SEC filings, each figure with its filing link. |
| `halal_screen` | One stock against the five AAOIFI tests on the filings known at each date: an AAOIFI-based screen, not a ruling. |
| `disclosure_study` | The Filing-Date Ledger: what followed insider Form 4 filings, counted from the first open after the EDGAR filing date. Aggregates only. |
| `get_data` | Standard warehouse series without SQL: prices, a benchmark, the risk-free rate, fundamentals or factors as of a date, and macro series. |

### Portfolio: category `portfolio` (8)

| Tool | What it does |
|---|---|
| `run_portfolio` | A fixed-weight book walked day by day against a benchmark: CAGR, volatility, Sharpe, drawdown with its dates, every calendar year beside the benchmark's, beta and alpha. |
| `optimize_portfolio` | A long-only frontier over 2 to 12 tickers on monthly total returns: history, not a forecast. |
| `rebalance_plan` | (coming soon) The trade list from your criteria to the book they imply today, as a CSV for your broker or your own software. QuantGPT places nothing. |
| `paper_books` | Follows a criteria set on paper from today, marked nightly, rebalanced monthly. No broker. |
| `record_book` | Seals a stock list with a timestamped hash and follows it on paper against SPY's total return. |
| `strategy_alerts` | A nightly watch on a screen: you are told when a name enters or leaves. Nothing is placed. |
| `portfolio_review` | (coming soon) Your holdings read against the warehouse: grades, factors, concentration, correlation, the book's history. QuantGPT Pro. |
| `count_bets` | How many independent bets a book holds, from its correlation matrix, and the ETF its daily path resembles. QuantGPT and QuantGPT Pro. |

### Macro: category `macro` (4)

| Tool | What it does |
|---|---|
| `get_macro_series` | 42 FRED series (rates, curve spreads, CPI, unemployment, VIX and more). Latest vintage; revisions not modelled. |
| `run_macro_regime` | Holds an index or a commodity while a FRED series meets a condition, else T-bills. |
| `run_commodity_backtest` | Momentum across 26 continuous futures since about 2001. Roll costs not modelled. |
| `run_weather_study` | Degree-day or precipitation anomalies at a US weather station against a commodity's next-month return. A study, not a strategy. |

### Alternative markets: category `alt_markets` (13)

| Tool | What it does |
|---|---|
| `run_crypto_backtest` | Monthly cross-sectional crypto backtest over Binance USDT pairs against BTC buy and hold. |
| `run_funding_backtest` | Ranks Binance perpetuals by funding rate (865 perps, 2020 on) and holds spot, against BTC. |
| `run_pm_backtest` | (coming soon) Resolved Polymarket and Kalshi markets: buy YES N days before close, with ROI, Brier score and the calibration table. |
| `pm_screen` | (coming soon) Today's open Polymarket and Kalshi markets against the crowd's resolved record at similar prices. Never a forecast. |
| `pm_grade` | (coming soon) One open prediction market with the calibration tables behind its read. |
| `run_sports_backtest` | (coming soon) Flat-stake rules on Bet365 closing odds: 17 European soccer leagues, 128,477 matches, 2005/06 to 2025/26. |
| `run_meme_backtest` | Memecoin cross-sections with dead tokens counted as total losses. |
| `meme_survival` | Memecoin pools by launch month: how many still traded at 30, 90 and 180 days. |
| `meme_screen` | Today's living memecoins passing a filter, ranked by a factor. Never a recommendation. |
| `call_receipt` | What happened after a crypto call, from the post's link and the contract address. Grades the call, never the person. |
| `tape_now` | Where Hyperliquid's 24/7 stock perps have each US stock while NYSE is closed, with each name's record. Never a forecast. |
| `tape_study` | Whether the tape's pre-bell move says anything about the session that follows. Months deep, not decades. |
| `run_tape_backtest` | Follows or fades the 24/7 tape from 09:00 New York to the close, daily, gross. |

### Events: category `events` (3)

| Tool | What it does |
|---|---|
| `event_desk` | The world's events from public feeds against the market's record: evidence, analogues, ripples, chokepoints and your pins. |
| `futuretest` | (coming soon) Your book through every real analogue of a scenario, day by day for 63 sessions: the record of past episodes, not a forecast. |
| `event_rules` | Your own rules on events: notify you, open a paper position, or raise a signal for your own software. Never an order. |

### Charts: category `charts` (3)

| Tool | What it does |
|---|---|
| `render_chart` | Draws a branded chart from data you already have; the numbers come back with the picture. |
| `list_chart_types` | The chart catalog and the inputs each chart needs. |
| `event_study` | Give event dates and a ticker: forward returns at your horizons against every session's base rate, split by your groups and charted. |

### Your terminal: category `desk` (5)

| Tool | What it does |
|---|---|
| `ui_push` | Pushes a note, a saved study or a chart onto your open terminal. |
| `queue_desk` | Proposes a whole workspace of panels to your monitor; it loads only if you click. |
| `read_desk` | What is on your monitor now, with panel ids. |
| `update_panel` | Changes one panel on the desk you have open. |
| `list_panel_types` | The panel catalog for `queue_desk`. |

## Strategies you can ask about

A sample from the library of 175 documented strategies (63 of them runnable), drawn from papers, books and practitioners. They are listed so you can test them, not as promises. A library run is the closest the warehouse's factor vocabulary gets to the published rule, and every run says what it approximates. Run one and read its losing years before you trust it.

| Strategy | Source | Library id | Runnable |
|---|---|---|---|
| Cross-Sectional Momentum (Jegadeesh-Titman) | Jegadeesh and Titman (1993), Journal of Finance | `jegadeesh-titman-1993` | yes |
| Momentum Crashes | Daniel and Moskowitz (2016), Journal of Financial Economics | `daniel-moskowitz-2016` | documented only |
| Magic Formula | Joel Greenblatt (2005) | `greenblatt-magic-2005` | yes |
| Piotroski F-Score | Piotroski (2000), Journal of Accounting Research | `piotroski-2000` | yes |
| Gross Profitability | Novy-Marx (2013), Journal of Financial Economics | `novy-marx-2013` | yes |
| Quality Minus Junk (QMJ) | Asness, Frazzini and Pedersen (2019), Review of Accounting Studies | `qmj-2019` | yes |
| Betting Against Beta (BAB) | Frazzini and Pedersen (2014), Journal of Financial Economics | `bab-2014` | yes |
| Accruals Anomaly | Sloan (1996), The Accounting Review | `sloan-1996` | yes |
| Post-Earnings Announcement Drift (SUE) | Bernard and Thomas (1989, 1990) | `bernard-thomas-1989` | yes |
| 10-Month MA Timing (GTAA) | Meb Faber (2007) | `faber-gtaa-2007` | yes |
| Sell in May (Halloween Indicator) | Bouman and Jacobsen (2002), American Economic Review | `halloween-2002` | yes |
| CANSLIM | William O'Neil (1988) | `oneil-canslim-1988` | yes |
| Pre-FOMC Announcement Drift | Lucca and Moench (2015), Journal of Finance | `lucca-moench-2015` | documented only |
| Prediction Market Longshots | Favorite-longshot bias (Griffith 1949; Snowberg and Wolfers 2010), on resolved Polymarket markets | `pm-longshot-bias` | yes |

Ask: "Run the library strategy `piotroski-2000`, show me its losing years and its worst drawdown, and tell me what the run approximates from the paper." More prompts: [examples/prompts.md](examples/prompts.md) and [examples/first-session.md](examples/first-session.md). Browse the whole library at https://quantgpt.co/library/.

## Plans and limits

| | QuantGPT | QuantGPT Pro |
|---|---|---|
| Price | $49 a month | $149 a month |
| AI calls | 500 a day | 1,500 a day |
| Keys | 3 | 5 |

- QuantGPT and QuantGPT Pro start with a 3-day free trial: card up front, cancel any time. The trial has the plan's full daily limits.
- An AI call is one tool call. Connecting, listing tools and pings do not count. The count is per account, across all of its keys.
- A per-minute limit per account also applies. Past it the server answers `429`; try again a minute later.
- Some tools depend on the plan: `portfolio_review` opens on QuantGPT Pro, `count_bets` on QuantGPT and QuantGPT Pro, and `strategy_alerts` allows 3 alerts on QuantGPT and unlimited alerts on QuantGPT Pro.
- Teams: Business is custom (seats, limits built for your volume) and starts with a conversation at https://quantgpt.co/contact.

Plans: https://quantgpt.co/#plans. Start: https://terminal.quantgpt.co/join.

## What the data is, and is not

- Companies that later delisted are priced from 1998 and are in the market-wide backtest universe from then on; earlier years hold survivors only. A backtest on a list of tickers you choose, and a rule on a single index, are not affected. The server tells your AI to say so whenever it reports a market-wide result.
- Fundamentals are joined by filing date. About 9% of rows still carry the quarter-end as the filing date, so those quarters are seen early; a fix is in progress.
- Factor data begins in 1990. Macro series are the latest vintage; revisions are not modelled.
- Results are gross by default. Every `run_backtest` result also carries turnover and net figures at 5, 10, 25 and 50 bps per side.
- Every `run_backtest` result returns its worst drawdown, a decade-by-decade table and the full return series, so the losing years are in the result; its chart pack draws them year by year.
- The same criteria return the same numbers, to the last digit.
- Futuretests replay the record of past episodes. They do not forecast.
- Public data checks, no account needed: https://terminal.quantgpt.co/verify

## Disclaimer

QuantGPT is research and education software. Nothing it returns is investment advice or a recommendation to buy, sell or hold anything, and nothing is tailored to you. Every backtest, study, screen, futuretest and library entry is hypothetical and describes the past; past performance, real or simulated, does not indicate or guarantee future results.

No tool on this server places, modifies or cancels an order, and none connects to a broker. Where a tool produces a trade list or a signal, you or your own software act on it, at your own broker, under your own keys. On QuantGPT Pro, QuantGPT Cloud hosts your own strategy as your own code and sends the orders your code makes to your own brokerage account, under your keys; QuantGPT never decides, sizes or times a trade. That runs in the terminal, not through this server.

Full terms: https://quantgpt.co/terms. Risk and methodology disclosures: https://quantgpt.co/disclosures. Privacy: https://quantgpt.co/privacy. Security: https://quantgpt.co/security.

QuantGPT LLC. Questions: https://quantgpt.co/contact.
