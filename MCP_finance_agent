import yfinance as yf
import logging
from typing import Optional, Dict, Any, List

# Use the import path that worked for 'mcp install'
from mcp.server.fastmcp.server import FastMCP

# Set up basic logging (optional but helpful for debugging)
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# --- Define the FastMCP server ---
mcp = FastMCP(
    title="Enhanced Finance Server 📈",
    description="Provides comprehensive stock market data and KPIs using yfinance."
)


# --- Helper function to safely get nested values ---
def safe_get(data: Dict[str, Any], key: str, default: Any = "N/A") -> Any:
    """Safely get a value from a dictionary, returning default if key doesn't exist or value is None."""
    value = data.get(key)
    return value if value is not None else default


def format_number(value: Any, is_currency: bool = False, is_percentage: bool = False) -> str:
    """Format numbers for display."""
    if value == "N/A" or value is None:
        return "N/A"

    try:
        if is_percentage:
            return f"{float(value) * 100:.2f}%" if isinstance(value, (int, float)) else str(value)
        elif is_currency:
            return f"${float(value):,.2f}" if isinstance(value, (int, float)) else str(value)
        else:
            return f"{float(value):,.2f}" if isinstance(value, (int, float)) else str(value)
    except (ValueError, TypeError):
        return str(value)


# --- Original tool ---
@mcp.tool()
def get_market_cap(ticker: str) -> str:
    """
    Retrieves the current market capitalization for a given stock ticker.

    Args:
        ticker: The stock ticker symbol (e.g., 'AAPL', 'GOOGL', 'MSFT').

    Returns:
        A string indicating the market capitalization or an error message
        if the ticker is not found or data is unavailable.
    """
    logger.info(f"Attempting to fetch market cap for ticker: {ticker}")
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info
        market_cap = info.get("marketCap")

        if market_cap:
            logger.info(f"Successfully found market cap for {ticker}: {market_cap}")
            return f"The market cap for {ticker.upper()} is: ${market_cap:,.0f}"
        else:
            if info and info.get('regularMarketPrice') is not None:
                logger.warning(f"Ticker {ticker} seems valid but lacks market cap data.")
                return f"Could not find market cap data for {ticker.upper()}, although it might be a valid ticker (e.g., an index or ETF)."
            else:
                logger.warning(f"Could not find any info for ticker: {ticker}. Likely invalid.")
                return f"Could not find market cap data for {ticker.upper()}. Please ensure it's a valid stock ticker."

    except Exception as e:
        logger.error(f"An unexpected error occurred for {ticker}: {e}")
        return f"An error occurred while trying to fetch data for {ticker.upper()}: {e}"


# --- Comprehensive KPI Tools ---

@mcp.tool()
def get_company_overview(ticker: str) -> str:
    """
    Get basic company information and overview.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Company overview including name, sector, industry, and description
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        overview = f"""
Company Overview for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Company Name: {safe_get(info, 'longName')}
Sector: {safe_get(info, 'sector')}
Industry: {safe_get(info, 'industry')}
Country: {safe_get(info, 'country')}
Website: {safe_get(info, 'website')}
Employees: {format_number(safe_get(info, 'fullTimeEmployees'))}

Business Summary:
{safe_get(info, 'longBusinessSummary', 'No description available')[:500]}...
        """
        return overview.strip()

    except Exception as e:
        return f"Error fetching company overview for {ticker.upper()}: {e}"


@mcp.tool()
def get_valuation_metrics(ticker: str) -> str:
    """
    Get comprehensive valuation metrics and ratios.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Valuation metrics including P/E, P/B, EV/EBITDA, etc.
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        metrics = f"""
Valuation Metrics for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Market Cap: {format_number(safe_get(info, 'marketCap'), is_currency=True)}
Enterprise Value: {format_number(safe_get(info, 'enterpriseValue'), is_currency=True)}

Price Ratios:
  • P/E Ratio (TTM): {format_number(safe_get(info, 'trailingPE'))}
  • Forward P/E: {format_number(safe_get(info, 'forwardPE'))}
  • P/B Ratio: {format_number(safe_get(info, 'priceToBook'))}
  • P/S Ratio (TTM): {format_number(safe_get(info, 'priceToSalesTrailing12Months'))}
  • PEG Ratio: {format_number(safe_get(info, 'pegRatio'))}

Enterprise Ratios:
  • EV/Revenue: {format_number(safe_get(info, 'enterpriseToRevenue'))}
  • EV/EBITDA: {format_number(safe_get(info, 'enterpriseToEbitda'))}

Book Value per Share: {format_number(safe_get(info, 'bookValue'), is_currency=True)}
        """
        return metrics.strip()

    except Exception as e:
        return f"Error fetching valuation metrics for {ticker.upper()}: {e}"


@mcp.tool()
def get_financial_health(ticker: str) -> str:
    """
    Get financial health indicators and balance sheet metrics.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Financial health metrics including debt ratios, liquidity ratios, etc.
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        health = f"""
Financial Health for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Balance Sheet:
  • Total Cash: {format_number(safe_get(info, 'totalCash'), is_currency=True)}
  • Total Debt: {format_number(safe_get(info, 'totalDebt'), is_currency=True)}
  • Net Cash: {format_number(safe_get(info, 'totalCash', 0) - safe_get(info, 'totalDebt', 0), is_currency=True)}

Liquidity Ratios:
  • Current Ratio: {format_number(safe_get(info, 'currentRatio'))}
  • Quick Ratio: {format_number(safe_get(info, 'quickRatio'))}

Debt Ratios:
  • Debt-to-Equity: {format_number(safe_get(info, 'debtToEquity'))}
  • Total Cash per Share: {format_number(safe_get(info, 'totalCashPerShare'), is_currency=True)}

Other Metrics:
  • Working Capital: {format_number(safe_get(info, 'workingCapital'), is_currency=True)}
  • Free Cash Flow: {format_number(safe_get(info, 'freeCashflow'), is_currency=True)}
        """
        return health.strip()

    except Exception as e:
        return f"Error fetching financial health for {ticker.upper()}: {e}"


@mcp.tool()
def get_profitability_metrics(ticker: str) -> str:
    """
    Get profitability and efficiency metrics.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Profitability metrics including margins, ROE, ROA, etc.
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        profitability = f"""
Profitability Metrics for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Revenue & Earnings:
  • Total Revenue (TTM): {format_number(safe_get(info, 'totalRevenue'), is_currency=True)}
  • Net Income (TTM): {format_number(safe_get(info, 'netIncomeToCommon'), is_currency=True)}
  • EBITDA: {format_number(safe_get(info, 'ebitda'), is_currency=True)}

Margins:
  • Profit Margin: {format_number(safe_get(info, 'profitMargins'), is_percentage=True)}
  • Operating Margin: {format_number(safe_get(info, 'operatingMargins'), is_percentage=True)}
  • Gross Margin: {format_number(safe_get(info, 'grossMargins'), is_percentage=True)}
  • EBITDA Margin: {format_number(safe_get(info, 'ebitdaMargins'), is_percentage=True)}

Returns:
  • Return on Equity (ROE): {format_number(safe_get(info, 'returnOnEquity'), is_percentage=True)}
  • Return on Assets (ROA): {format_number(safe_get(info, 'returnOnAssets'), is_percentage=True)}

Per Share Metrics:
  • EPS (TTM): {format_number(safe_get(info, 'trailingEps'), is_currency=True)}
  • Forward EPS: {format_number(safe_get(info, 'forwardEps'), is_currency=True)}
  • Revenue per Share: {format_number(safe_get(info, 'revenuePerShare'), is_currency=True)}
        """
        return profitability.strip()

    except Exception as e:
        return f"Error fetching profitability metrics for {ticker.upper()}: {e}"


@mcp.tool()
def get_growth_metrics(ticker: str) -> str:
    """
    Get growth-related metrics and estimates.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Growth metrics including revenue growth, earnings growth, etc.
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        growth = f"""
Growth Metrics for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Historical Growth:
  • Revenue Growth (TTM): {format_number(safe_get(info, 'revenueGrowth'), is_percentage=True)}
  • Earnings Growth: {format_number(safe_get(info, 'earningsGrowth'), is_percentage=True)}
  • Quarterly Revenue Growth: {format_number(safe_get(info, 'revenueQuarterlyGrowth'), is_percentage=True)}
  • Quarterly Earnings Growth: {format_number(safe_get(info, 'earningsQuarterlyGrowth'), is_percentage=True)}

Analyst Estimates:
  • Next Year EPS Growth: {format_number(safe_get(info, 'earningsGrowth'), is_percentage=True)}
  • Next 5 Years Growth: {format_number(safe_get(info, 'earningsGrowth'), is_percentage=True)}

Book Value Growth:
  • Book Value: {format_number(safe_get(info, 'bookValue'), is_currency=True)}
  • Tangible Book Value: {format_number(safe_get(info, 'tangibleBookValue'), is_currency=True)}
        """
        return growth.strip()

    except Exception as e:
        return f"Error fetching growth metrics for {ticker.upper()}: {e}"


@mcp.tool()
def get_dividend_metrics(ticker: str) -> str:
    """
    Get dividend and shareholder return metrics.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Dividend metrics including yield, payout ratio, dividend history
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        dividend_rate = safe_get(info, 'dividendRate', 0)

        dividend_info = f"""
Dividend & Shareholder Returns for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Dividend Information:
  • Annual Dividend Rate: {format_number(dividend_rate, is_currency=True)}
  • Dividend Yield: {format_number(safe_get(info, 'dividendYield'), is_percentage=True)}
  • Payout Ratio: {format_number(safe_get(info, 'payoutRatio'), is_percentage=True)}
  • Ex-Dividend Date: {safe_get(info, 'exDividendDate')}
  • Last Dividend Date: {safe_get(info, 'lastDividendDate')}

Share Information:
  • Shares Outstanding: {format_number(safe_get(info, 'sharesOutstanding'))}
  • Float: {format_number(safe_get(info, 'floatShares'))}
  • Shares Short: {format_number(safe_get(info, 'sharesShort'))}
  • Short Ratio: {format_number(safe_get(info, 'shortRatio'))}
  • Short % of Float: {format_number(safe_get(info, 'shortPercentOfFloat'), is_percentage=True)}

Share Buybacks:
  • Shares Short Prior Month: {format_number(safe_get(info, 'sharesShortPriorMonth'))}
        """
        return dividend_info.strip()

    except Exception as e:
        return f"Error fetching dividend metrics for {ticker.upper()}: {e}"


@mcp.tool()
def get_trading_metrics(ticker: str) -> str:
    """
    Get trading and market performance metrics.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Trading metrics including price ranges, volume, volatility
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        trading = f"""
Trading & Market Metrics for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Current Price Information:
  • Current Price: {format_number(safe_get(info, 'regularMarketPrice'), is_currency=True)}
  • Previous Close: {format_number(safe_get(info, 'regularMarketPreviousClose'), is_currency=True)}
  • Open: {format_number(safe_get(info, 'regularMarketOpen'), is_currency=True)}
  • Day High: {format_number(safe_get(info, 'regularMarketDayHigh'), is_currency=True)}
  • Day Low: {format_number(safe_get(info, 'regularMarketDayLow'), is_currency=True)}

Price Ranges:
  • 52-Week High: {format_number(safe_get(info, 'fiftyTwoWeekHigh'), is_currency=True)}
  • 52-Week Low: {format_number(safe_get(info, 'fiftyTwoWeekLow'), is_currency=True)}
  • 50-Day Average: {format_number(safe_get(info, 'fiftyDayAverage'), is_currency=True)}
  • 200-Day Average: {format_number(safe_get(info, 'twoHundredDayAverage'), is_currency=True)}

Volume & Liquidity:
  • Volume: {format_number(safe_get(info, 'regularMarketVolume'))}
  • Average Volume (10d): {format_number(safe_get(info, 'averageVolume10days'))}
  • Average Volume (3m): {format_number(safe_get(info, 'averageVolume'))}

Risk Metrics:
  • Beta: {format_number(safe_get(info, 'beta'))}
  • 52-Week Change: {format_number(safe_get(info, '52WeekChange'), is_percentage=True)}
        """
        return trading.strip()

    except Exception as e:
        return f"Error fetching trading metrics for {ticker.upper()}: {e}"


@mcp.tool()
def get_analyst_data(ticker: str) -> str:
    """
    Get analyst recommendations and target prices.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Analyst data including recommendations, target prices, and estimates
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        analyst_data = f"""
Analyst Data for {ticker.upper()}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Price Targets:
  • Target High Price: {format_number(safe_get(info, 'targetHighPrice'), is_currency=True)}
  • Target Low Price: {format_number(safe_get(info, 'targetLowPrice'), is_currency=True)}
  • Target Mean Price: {format_number(safe_get(info, 'targetMeanPrice'), is_currency=True)}
  • Target Median Price: {format_number(safe_get(info, 'targetMedianPrice'), is_currency=True)}

Recommendations:
  • Recommendation Mean: {format_number(safe_get(info, 'recommendationMean'))}
  • Recommendation Key: {safe_get(info, 'recommendationKey')}
  • Number of Analyst Opinions: {format_number(safe_get(info, 'numberOfAnalystOpinions'))}

Estimates:
  • Current Quarter Estimate: {format_number(safe_get(info, 'earningsQuarterlyGrowth'), is_percentage=True)}
  • Next Quarter Estimate: {format_number(safe_get(info, 'earningsGrowth'), is_percentage=True)}
        """
        return analyst_data.strip()

    except Exception as e:
        return f"Error fetching analyst data for {ticker.upper()}: {e}"


@mcp.tool()
def get_complete_stock_analysis(ticker: str) -> str:
    """
    Get a comprehensive analysis combining all major KPIs and metrics.

    Args:
        ticker: Stock ticker symbol

    Returns:
        Complete stock analysis with all key metrics
    """
    try:
        stock = yf.Ticker(ticker.upper())
        info = stock.info

        if not info:
            return f"No data found for ticker {ticker.upper()}"

        # Get current price for context
        current_price = safe_get(info, 'regularMarketPrice', 0)
        market_cap = safe_get(info, 'marketCap', 0)

        analysis = f"""
🏢 COMPLETE STOCK ANALYSIS: {ticker.upper()}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 SNAPSHOT
Company: {safe_get(info, 'longName')}
Sector: {safe_get(info, 'sector')} | Industry: {safe_get(info, 'industry')}
Current Price: {format_number(current_price, is_currency=True)}
Market Cap: {format_number(market_cap, is_currency=True)}

💰 VALUATION METRICS
P/E Ratio: {format_number(safe_get(info, 'trailingPE'))} | Forward P/E: {format_number(safe_get(info, 'forwardPE'))}
P/B Ratio: {format_number(safe_get(info, 'priceToBook'))} | P/S Ratio: {format_number(safe_get(info, 'priceToSalesTrailing12Months'))}
EV/EBITDA: {format_number(safe_get(info, 'enterpriseToEbitda'))} | PEG Ratio: {format_number(safe_get(info, 'pegRatio'))}

📈 PROFITABILITY & EFFICIENCY
Revenue (TTM): {format_number(safe_get(info, 'totalRevenue'), is_currency=True)}
Net Income: {format_number(safe_get(info, 'netIncomeToCommon'), is_currency=True)}
Profit Margin: {format_number(safe_get(info, 'profitMargins'), is_percentage=True)}
ROE: {format_number(safe_get(info, 'returnOnEquity'), is_percentage=True)} | ROA: {format_number(safe_get(info, 'returnOnAssets'), is_percentage=True)}

🚀 GROWTH METRICS
Revenue Growth: {format_number(safe_get(info, 'revenueGrowth'), is_percentage=True)}
Earnings Growth: {format_number(safe_get(info, 'earningsGrowth'), is_percentage=True)}
EPS (TTM): {format_number(safe_get(info, 'trailingEps'), is_currency=True)}

💎 FINANCIAL HEALTH
Current Ratio: {format_number(safe_get(info, 'currentRatio'))}
Debt-to-Equity: {format_number(safe_get(info, 'debtToEquity'))}
Free Cash Flow: {format_number(safe_get(info, 'freeCashflow'), is_currency=True)}
Total Cash: {format_number(safe_get(info, 'totalCash'), is_currency=True)}

💸 SHAREHOLDER RETURNS
Dividend Yield: {format_number(safe_get(info, 'dividendYield'), is_percentage=True)}
Dividend Rate: {format_number(safe_get(info, 'dividendRate'), is_currency=True)}
Payout Ratio: {format_number(safe_get(info, 'payoutRatio'), is_percentage=True)}

📊 TRADING METRICS
52W High: {format_number(safe_get(info, 'fiftyTwoWeekHigh'), is_currency=True)} | 52W Low: {format_number(safe_get(info, 'fiftyTwoWeekLow'), is_currency=True)}
Beta: {format_number(safe_get(info, 'beta'))}
Average Volume: {format_number(safe_get(info, 'averageVolume'))}

🎯 ANALYST CONSENSUS
Target Price: {format_number(safe_get(info, 'targetMeanPrice'), is_currency=True)}
Recommendation: {safe_get(info, 'recommendationKey')}
Number of Analysts: {format_number(safe_get(info, 'numberOfAnalystOpinions'))}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        """
        return analysis.strip()

    except Exception as e:
        return f"Error fetching complete analysis for {ticker.upper()}: {e}"


@mcp.tool()
def list_available_kpis() -> str:
    """
    List all available KPIs and metrics that can be retrieved for stocks.

    Returns:
        Comprehensive list of all available KPIs organized by category
    """
    kpi_list = """
📊 AVAILABLE STOCK KPIs & METRICS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏢 COMPANY OVERVIEW
• Company Name, Sector, Industry
• Country, Website, Employee Count
• Business Summary

💰 VALUATION METRICS
• Market Capitalization
• Enterprise Value
• P/E Ratio (Trailing & Forward)
• Price-to-Book Ratio
• Price-to-Sales Ratio
• PEG Ratio
• EV/Revenue, EV/EBITDA

📈 PROFITABILITY METRICS
• Total Revenue (TTM)
• Net Income
• EBITDA
• Profit Margins (Gross, Operating, Net, EBITDA)
• Return on Equity (ROE)
• Return on Assets (ROA)
• Earnings per Share (EPS)
• Revenue per Share

🚀 GROWTH METRICS
• Revenue Growth
• Earnings Growth
• Quarterly Growth Rates
• Book Value Growth

💎 FINANCIAL HEALTH
• Current Ratio
• Quick Ratio
• Debt-to-Equity Ratio
• Total Cash & Debt
• Working Capital
• Free Cash Flow

💸 DIVIDEND & SHAREHOLDER RETURNS
• Dividend Yield
• Dividend Rate
• Payout Ratio
• Ex-Dividend Date
• Shares Outstanding
• Share Buyback Information

📊 TRADING METRICS
• Current Price, Open, High, Low
• 52-Week High/Low
• Moving Averages (50-day, 200-day)
• Volume Metrics
• Beta (Volatility)

🎯 ANALYST DATA  
• Price Targets (High, Low, Mean, Median)
• Analyst Recommendations
• Number of Analyst Opinions
• Earnings Estimates

🔧 AVAILABLE FUNCTIONS:
• get_market_cap(ticker) - Original market cap function
• get_company_overview(ticker) - Company basic information
• get_valuation_metrics(ticker) - All valuation ratios
• get_financial_health(ticker) - Balance sheet & liquidity metrics
• get_profitability_metrics(ticker) - Profitability & efficiency ratios  
• get_growth_metrics(ticker) - Growth rates & estimates
• get_dividend_metrics(ticker) - Dividend & shareholder return data
• get_trading_metrics(ticker) - Price & volume data
• get_analyst_data(ticker) - Analyst recommendations & targets
• get_complete_stock_analysis(ticker) - Comprehensive analysis with all KPIs
• list_available_kpis() - This function listing all available metrics

Each function provides detailed, formatted output for the specified category of metrics.
    """
    return kpi_list.strip()


# --- Block to run the server directly (for testing) ---
if __name__ == "__main__":
    print("Starting Enhanced Finance Server...")
    print("Available functions:")
    print("- get_market_cap")
    print("- get_company_overview")
    print("- get_valuation_metrics")
    print("- get_financial_health")
    print("- get_profitability_metrics")
    print("- get_growth_metrics")
    print("- get_dividend_metrics")
    print("- get_trading_metrics")
    print("- get_analyst_data")
    print("- get_complete_stock_analysis")
    print("- list_available_kpis")
    print("\nUse 'mcp install MCP_finance_agent.py' to register it.")
    mcp.run()  # This will start a local server and print its address
