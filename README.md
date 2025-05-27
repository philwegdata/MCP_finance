# Enhanced Finance MCP Server 📈

A Model Context Protocol (MCP) server that provides detailed stock market data and financial KPIs from Yahoo Finance. This server offers financial analysis capabilities through the amazing yfinance library!


## Features

### 🏢 Company Information
- Company overview, sector, industry details
- Business summaries and key company metrics
- Employee count and geographic information

### 💰 Valuation Metrics
- Market capitalization and enterprise value
- Price ratios (P/E, P/B, P/S, PEG)
- Enterprise ratios (EV/Revenue, EV/EBITDA)

### 📈 Profitability Analysis
- Revenue and earnings metrics
- Profit margins (gross, operating, net, EBITDA)
- Return metrics (ROE, ROA)
- Per-share calculations (EPS, revenue per share)

### 🚀 Growth Metrics
- Historical growth rates (revenue, earnings)
- Quarterly growth analysis
- Future growth estimates

### 💎 Financial Health
- Liquidity ratios (current, quick)
- Debt analysis (debt-to-equity, total debt)
- Cash flow metrics
- Working capital analysis

### 💸 Dividend & Returns
- Dividend yield and payout ratios
- Share buyback information
- Shareholder return metrics

### 📊 Trading Data
- Current price and trading ranges
- Volume analysis
- Technical indicators (Beta, moving averages)
- 52-week highs and lows

### 🎯 Analyst Data
- Price targets and recommendations
- Analyst consensus data
- Earnings estimates

## Installation

### Prerequisites
- Python 3.8+
- MCP-compatible client (Claude Desktop, etc.)

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/enhanced-finance-mcp-server.git
   cd enhanced-finance-mcp-server
   ```

2. **Install dependencies:**
   ```bash
   pip install yfinance mcp
   ```

3. **Install the MCP server:**
   ```bash
   mcp install MCP_finance_agent.py
   ```

### Configuration for Claude Desktop

Add the following to your Claude Desktop MCP configuration file:

**Windows:** `%APPDATA%/Claude/claude_desktop_config.json`
**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "enhanced-finance": {
      "command": "python",
      "args": ["path/to/your/MCP_finance_agent.py"]
    }
  }
}
```

## Available Functions

| Function | Description |
|----------|-------------|
| `get_market_cap(ticker)` | Get market capitalization for a stock |
| `get_company_overview(ticker)` | Company basic information and business summary |
| `get_valuation_metrics(ticker)` | All valuation ratios and multiples |
| `get_financial_health(ticker)` | Balance sheet and liquidity metrics |
| `get_profitability_metrics(ticker)` | Profitability and efficiency ratios |
| `get_growth_metrics(ticker)` | Growth rates and estimates |
| `get_dividend_metrics(ticker)` | Dividend and shareholder return data |
| `get_trading_metrics(ticker)` | Price and volume data |
| `get_analyst_data(ticker)` | Analyst recommendations and targets |
| `get_complete_stock_analysis(ticker)` | Comprehensive analysis with all KPIs |
| `list_available_kpis()` | List all available metrics and functions |

## Usage Examples

### Basic Market Cap Query
```
What's the market cap of Apple?
```
*Uses: `get_market_cap("AAPL")`*

### Company Analysis
```
Give me an overview of Microsoft's business
```
*Uses: `get_company_overview("MSFT")`*

### Valuation Analysis
```
What are Tesla's valuation metrics?
```
*Uses: `get_valuation_metrics("TSLA")`*

### Comprehensive Analysis
```
Provide a complete financial analysis of Google
```
*Uses: `get_complete_stock_analysis("GOOGL")`*

### Compare Multiple Stocks
```
Compare the profitability metrics of Apple, Microsoft, and Google
```
*Uses: Multiple `get_profitability_metrics()` calls*

## Supported Stock Symbols

The server works with any valid stock ticker symbol available on Yahoo Finance, including:
- **US Stocks:** AAPL, MSFT, GOOGL, TSLA, AMZN, etc.
- **International Stocks:** Use appropriate suffixes (e.g., SAP.DE for German stocks)
- **ETFs:** SPY, QQQ, VTI, etc.
- **Indices:** ^GSPC (S&P 500), ^IXIC (NASDAQ), etc.

## Output Format

All functions return well-formatted text with:
- Clear section headers and visual separators
- Properly formatted numbers (currency, percentages)
- Organized categorical data
- Error handling for missing data

Example output:
```
Valuation Metrics for AAPL:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Market Cap: $2,876,542,000,000.00
Enterprise Value: $2,865,234,000,000.00

Price Ratios:
  • P/E Ratio (TTM): 28.45
  • Forward P/E: 24.12
  • P/B Ratio: 45.23
  ...
```

## Error Handling

The server includes robust error handling for:
- Invalid ticker symbols
- Missing data points
- API connectivity issues
- Malformed requests

## Dependencies

- **yfinance**: Yahoo Finance API access
- **mcp**: Model Context Protocol framework
- **logging**: Built-in Python logging

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This tool is for informational purposes only and should not be considered as financial advice. Always consult with qualified financial professionals before making investment decisions.

## Support

For issues, questions, or feature requests, please:
1. Check the [Issues](https://github.com/yourusername/enhanced-finance-mcp-server/issues) page
2. Create a new issue if your problem isn't already listed
3. Provide detailed information about your setup and the issue

## Changelog

### v1.0.0
- Initial release with comprehensive financial metrics
- Support for 11 different analysis categories
- Robust error handling and data formatting
- Complete MCP integration

---

**Built with ❤️ for the MCP ecosystem**
