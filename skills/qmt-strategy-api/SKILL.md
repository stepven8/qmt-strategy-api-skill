---
name: qmt-strategy-api
description: QMT/迅投极速策略交易系统 Python API development guide and reference. Use when Codex needs to design, write, modify, review, debug, or port QMT quantitative trading strategies, including handlebar K-line strategies, quote subscription callbacks, scheduled tasks, QMT order placement, account/position/order/deal queries, market data fetching, financial data, backtest-vs-live behavior, or API-compatible strategy templates.
---

# QMT Strategy API

## Core Rule

Develop QMT strategies against the bundled API reference, not generic Python trading assumptions. Before using unfamiliar QMT functions, parameters, enums, account types, or data structures, search `references/qmt_api_full.md` and follow its documented signatures and parameter-linkage notes.

## Required Reference

Use `references/qmt_api_full.md` as the authoritative local API reference. Prefer targeted searches instead of loading the whole file.

Useful searches:

```bash
rg -n "passorder|orderType|prType|quickTrade|参数联动" references/qmt_api_full.md
rg -n "get_market_data_ex|get_full_tick|subscribe_quote|subscribe_whole_quote" references/qmt_api_full.md
rg -n "get_trade_detail_data|Account|Order|Deal|Position" references/qmt_api_full.md
rg -n "run_time|schedule_run|handlebar|is_last_bar|is_new_bar" references/qmt_api_full.md
rg -n "数据字典|opType|orderType|prType|VIP|回测与实盘差异" references/qmt_api_full.md
```

## Strategy Workflow

1. Identify the QMT run style:
   - Use `handlebar(ContextInfo)` for K-line driven backtest/live logic.
   - Use `subscribe_quote` or `subscribe_whole_quote` callbacks for tick or push-driven live logic.
   - Use `run_time` or `schedule_run` for fixed-time or interval tasks.
2. Start QMT scripts with `#coding:gbk` and keep syntax compatible with QMT's embedded Python 3.6 unless the user's target environment says otherwise.
3. Keep QMT system entrypoints exactly named when required: `init(ContextInfo)`, `after_init(ContextInfo)`, `handlebar(ContextInfo)`, and `stop(ContextInfo)`.
4. In live `handlebar` code, usually guard with `ContextInfo.is_last_bar()` before trading so historical bars do not trigger unintended orders.
5. Store strategy state on `ContextInfo` or module-level globals as documented; avoid assumptions about multiprocessing or thread persistence beyond QMT behavior.
6. Before placing orders, verify `opType`, `orderType`, `prType`, `price`, `volume`, `quickTrade`, `accountID`, and `strAccountType` against the reference.
7. Separate data retrieval, signal generation, risk control, and execution so QMT-specific API calls remain easy to audit.

## Common Patterns

Minimal K-line skeleton:

```python
#coding:gbk

def init(ContextInfo):
    ContextInfo.stock = ContextInfo.stockcode + '.' + ContextInfo.market

def handlebar(ContextInfo):
    if not ContextInfo.is_last_bar():
        return

    # Fetch data, compute signal, then execute according to QMT API rules.
```

Quote subscription skeleton:

```python
#coding:gbk

def on_quote(data):
    for stock_code in data:
        tick = data[stock_code]
        # Handle pushed quote data.

def init(ContextInfo):
    ContextInfo.sub_id = ContextInfo.subscribe_quote(
        "000001.SZ",
        "1d",
        callback=on_quote,
    )
```

Scheduled task skeleton:

```python
#coding:gbk

def init(ContextInfo):
    ContextInfo.run_time("scheduled_check", "1nSecond", "2019-10-14 13:20:00")

def scheduled_check(ContextInfo):
    # Periodic strategy logic.
    pass
```

## API Safety Checklist

Check these before finalizing QMT strategy code:

- Encoding header is present: `#coding:gbk`.
- Strategy lifecycle function names and callback signatures match QMT conventions.
- Symbol format follows the target API's expected convention, for example `000001.SZ` or `stockcode + '.' + market`.
- Market data functions use documented `period`, `dividend_type`, `count`, `start_time`, `end_time`, and return-shape assumptions.
- Order functions use documented parameter combinations; do not invent enums.
- Account queries use documented `strAccountType` values such as `STOCK`, `CREDIT`, or `FUTURE`.
- Backtest-only functions are not used as if they work in live trading.
- VIP-gated Level2 or advanced data is called only when the user has that permission, or clearly marked as requiring VIP.

## Reference Map

`references/qmt_api_full.md` contains:

- Sections 2-5: lifecycle, variable conventions, `ContextInfo`, system functions.
- Sections 6-7: data download and market data APIs.
- Section 8: trading APIs, order placement, cancellation, account/order/deal/position query, baskets, algo orders, push callbacks.
- Section 9: financial data APIs and fields.
- Sections 10-11: reference/plot functions and backtest-only trading functions.
- Sections 12-13: backtest/live differences, dictionaries, enums, parameter linkage, VIP permissions, original source links.
