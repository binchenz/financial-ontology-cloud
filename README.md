# Financial Ontology Cloud Skill

A Claude Code skill for querying real-time Chinese A-share (A股) financial data — stock prices, valuation metrics, financial statements, technical indicators, and personal watchlist management.

## What You Can Do

- **Stock lookup** — search by name (茅台, 平安银行) or code (000001.SZ)
- **Valuation analysis** — PE, PB, market cap, free float ratio
- **Financial health** — ROE, profit margins, DuPont analysis, debt ratios
- **Technical analysis** — MA, MACD, RSI, KDJ signals and trend scores
- **Three financial statements** — income statement, balance sheet, cash flow
- **Stock screening** — filter by any financial criteria
- **Watchlist (自选股)** — add, view, and remove stocks from your personal list

## Data Coverage

- 5,493 A-share stocks
- Daily quotes and valuation metrics (updated regularly)
- Quarterly/annual financial statements for all listed companies
- 1,000+ sector/concept classifications

## Installation

```bash
# In your Claude Code project
mkdir -p .claude/skills/financial-ontology-cloud
curl -o .claude/skills/financial-ontology-cloud/SKILL.md \
  https://raw.githubusercontent.com/binchenz/financial-ontology-cloud/main/SKILL.md
```

Or clone the full repo (includes examples and setup guide):

```bash
cd .claude/skills
git clone https://github.com/binchenz/financial-ontology-cloud.git
```

## Setup

This skill requires an API token. Contact the maintainer to get one, then replace the token in `SKILL.md`:

```
omaha_hNDLNwGCBtK7JMfZcjxdRl96YKW1IX5CazquZV_-AXM  ← replace with your token
```

See [setup.md](setup.md) for detailed configuration instructions.

## Usage

Once installed, just talk to Claude naturally:

> 帮我查一下茅台最近的估值，PE和PB是多少？

> 找出银行股里ROE最高的5只

> 把比亚迪加到我的自选股，备注新能源龙头

> 分析一下平安银行的财务健康状况

The skill triggers automatically when you ask about Chinese stock data.

## Example Queries

```bash
# Latest valuation for a stock
curl -X POST http://69.5.23.70/api/public/v1/query \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"object_type": "ValuationMetric", "filters": {"ts_code": "600519.SH"}, "limit": 1, "format": true, "order_by": "trade_date", "order": "desc"}'

# Screen stocks by financial criteria
curl -X POST http://69.5.23.70/api/public/v1/query \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"object_type": "FinancialIndicator", "select": ["ts_code", "end_date", "roe", "financial_health_score"], "limit": 10, "format": true, "order_by": "financial_health_score", "order": "desc"}'

# Add to watchlist
curl -X POST http://69.5.23.70/api/public/v1/watchlist \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"ts_code": "000001.SZ", "note": "等待回调"}'
```

See [examples.md](examples.md) for more query patterns and [use-cases.md](use-cases.md) for investment scenarios.

## API Reference

Full API documentation is in [SKILL.md](SKILL.md).

**Available objects:** `Stock`, `DailyQuote`, `ValuationMetric`, `FinancialIndicator`, `IncomeStatement`, `BalanceSheet`, `CashFlow`, `TechnicalIndicator`, `Sector`, `SectorMember`

**Endpoints:**
- `POST /api/public/v1/query` — query any object with filters, sorting, pagination
- `POST /api/public/v1/aggregate` — statistical aggregations (avg, max, min, sum, count)
- `GET/POST/DELETE /api/public/v1/watchlist` — manage personal watchlist

## Contact

To request an API token, open an issue on this repository.
