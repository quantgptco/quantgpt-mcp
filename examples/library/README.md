# Strategy Library walk-throughs

Runnable prompts for strategies in the QuantGPT Strategy Library (175 documented, 53 runnable in one call). Paste a prompt into Claude, ChatGPT or any client connected to `https://terminal.quantgpt.co/mcp`. Each file says which tool answers and which fields come back, so you can check that your AI read the whole result. No result numbers are printed here: run it and read your own.

| File | Strategy | Tool |
|---|---|---|
| [piotroski-f-score.md](piotroski-f-score.md) | Piotroski F-Score (`piotroski-2000`) | `run_library_strategy` |
| [momentum-12-1.md](momentum-12-1.md) | 12-1 month momentum (`jegadeesh-titman-1993`) | `run_library_strategy`, `run_backtest` |
| [magic-formula.md](magic-formula.md) | Magic Formula (`greenblatt-magic-2005`) | `run_library_strategy` |
| [spy-200-day.md](spy-200-day.md) | SPY above its 200-day moving average | `run_backtest` with `tickers` |

`run_library_strategy` sits in the `equities` category. If your client lists only the core tools, ask your AI to call `activate_category` for `equities`, or to run it through `call_tool`. Each tool call counts as one AI call (500 a day on QuantGPT, 1,500 on QuantGPT Pro).

Every result describes the past. None of it is a forecast, a recommendation or an order.

Updated October 2026.
