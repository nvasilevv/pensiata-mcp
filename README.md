# Pensiata MCP Server

A [Model Context Protocol](https://modelcontextprotocol.io/) server providing analytics for Bulgarian pension funds — covering all three pillar II schemes (UPF, PPF, DPF).

**Hosted at:** `https://mcp.pensiata.me/mcp` (streamable HTTP transport)

## Connect

Add to your MCP client configuration:

```json
{
  "mcpServers": {
    "pensiata": {
      "type": "streamable-http",
      "url": "https://mcp.pensiata.me/mcp"
    }
  }
}
```

No API key required.

## Tools

### `list_managers`
List pension fund management companies with AUM and supported schemes.

| Parameter | Type | Description |
|-----------|------|-------------|
| `scheme_code` | `upf` \| `ppf` \| `dpf` (optional) | Filter by scheme |

### `list_funds`
List individual funds with metadata.

| Parameter | Type | Description |
|-----------|------|-------------|
| `scheme_code` | `upf` \| `ppf` \| `dpf` (optional) | Filter by scheme |
| `active_only` | bool (default: `true`) | Only active funds |

### `get_nav_series`
Retrieve NAV (Net Asset Value) time series for one or more funds.

| Parameter | Type | Description |
|-----------|------|-------------|
| `fund_id` | string (optional) | e.g. `allianz:upf` |
| `manager_slug` | string (optional) | All funds for a manager |
| `scheme_code` | `upf` \| `ppf` \| `dpf` (optional) | All funds in a scheme |
| `frequency` | `daily` \| `weekly` \| `monthly` | Default: `daily` |
| `date_from` | string (optional) | Start date (YYYY-MM-DD) |
| `date_to` | string (optional) | End date (YYYY-MM-DD) |

### `compute_metric`
Compute a performance metric for one or more funds over a given period.

| Parameter | Type | Description |
|-----------|------|-------------|
| `metric` | string | Metric name (see below) |
| `period` | string or object | e.g. `"5y"`, `"10y"`, `"ytd"`, or `{"date_from": "...", "date_to": "..."}` |
| `fund_id` | string (optional) | Specific fund |
| `manager_slug` | string (optional) | All funds for a manager |
| `scheme_code` | `upf` \| `ppf` \| `dpf` (optional) | All funds in a scheme |
| `benchmark_slug` | string (optional) | Benchmark or fund ID for relative metrics |
| `frequency` | `daily` \| `weekly` \| `monthly` | Default: `daily` |
| `risk_free_rate` | number (optional) | Override risk-free rate |
| `risk_free_slug` | string (optional) | Benchmark to use as risk-free rate |

### `rank`
Rank funds within a scheme by any metric.

| Parameter | Type | Description |
|-----------|------|-------------|
| `scheme_code` | `upf` \| `ppf` \| `dpf` | Required |
| `metric` | string | Metric to rank by |
| `period` | string or object | Time period |
| `order` | `desc` \| `asc` | Default: `desc` |
| `include_metrics` | string[] (optional) | Additional metrics in output |
| `benchmark_slug` | string (optional) | For relative metrics |
| `limit` | int | Default: `50` |
| `offset` | int | Default: `0` |

### `list_benchmarks`
List available benchmark indices (S&P 500, Euro Stoxx 600, Gold, BG Inflation, etc.).

### `get_benchmark_series`
Retrieve benchmark time series.

| Parameter | Type | Description |
|-----------|------|-------------|
| `benchmark_slug` | string | e.g. `sp500`, `bg_inflation`, `gold` |
| `frequency` | `daily` \| `weekly` \| `monthly` | Default: `daily` |
| `date_from` | string (optional) | Start date |
| `date_to` | string (optional) | End date |

### `list_metrics`
List all available metric names.

## Available Metrics

| Metric | Description |
|--------|-------------|
| `return_total` | Cumulative total return |
| `cagr` | Compound annual growth rate |
| `real_cagr` | Inflation-adjusted CAGR |
| `ytd_return` | Year-to-date return |
| `volatility_ann` | Annualized volatility |
| `downside_volatility_ann` | Annualized downside volatility |
| `max_drawdown` | Maximum drawdown |
| `sharpe` | Sharpe ratio |
| `sortino` | Sortino ratio |
| `calmar` | Calmar ratio |
| `information_ratio` | Information ratio (vs benchmark) |
| `tracking_error_ann` | Annualized tracking error |
| `excess_return` | Excess return vs benchmark |
| `correlation` | Correlation with benchmark or another fund |
| `ulcer_index` | Ulcer index |
| `consistency_score` | Return consistency score |

## Available Benchmarks

| Slug | Description |
|------|-------------|
| `sp500` | S&P 500 (USD) |
| `eurostoxx600` | STOXX Europe 600 (USD) |
| `gold` | Gold (USD) |
| `bg_inflation` | Bulgarian CPI Inflation |
| `us10y_gov_bond` | US 10Y Government Bond (USD) |
| `de10y_gov_bond` | German 10Y Government Bond (EUR) |

## Coverage

- **Schemes:** UPF (Universal Pension Fund), PPF (Professional Pension Fund), DPF (Voluntary Pension Fund)
- **Managers:** 10 active + 2 historical pension management companies
- **Data:** Daily NAV data, some funds dating back to 2005
- **Currency:** BGN

## Website

[pensiata.me](https://pensiata.me)

## License

MIT
