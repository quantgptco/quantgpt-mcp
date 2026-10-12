# Your first session

Six prompts, in order, for the first time your AI is connected to QuantGPT. Each tool call counts as one AI call: the Free plan has 5 for the life of the account, so on it pick two or three of these. Paid plans have 500 (QuantGPT) or 1,500 (QuantGPT Pro) a day.

Updated September 2026.

**1. Learn what is there, and what is missing**

> Call describe_capabilities, then tell me in five lines what QuantGPT can and cannot do, including what its data leaves out.

A good answer says that market-wide backtests are not survivorship-free yet, that fundamentals are joined by filing date with a known share of rows dated early, and that factor data begins in 1990.

**2. Get ideas**

> Call get_prompt_library and pick three prompts for someone who has never run a backtest.

**3. Learn the method one step at a time**

> Use the guide tool and teach me the first step of THE RAMP.

**4. Run a documented strategy**

> Find a documented value strategy in the library, run it, and show me the numbers on the years it never saw. Then change one number, run it again, and state plainly what changed and what failed.

A good answer shows the losing years of both runs and says what the library run approximates from the original source.

**5. Try to break it**

> Run challenge_finding on the version I liked and lead with the strongest objection.

A slow skeptic pass answers with a job id; your AI collects it with `fetch_job` instead of running it again.

**6. Keep it**

> Save that study to my terminal with save_study.

`save_study` sits in the `equities` category: your AI switches it on with `activate_category` or runs it through `call_tool`. The study lands on your saved-studies shelf at https://terminal.quantgpt.co, ready to run again. The AI light in the terminal's navigation turns green after your AI's first call.

Everything above describes the past. None of it is a forecast, a recommendation or an order, and QuantGPT never places a trade.
