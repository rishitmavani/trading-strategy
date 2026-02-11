# Adaptive Trading Strategy with Market Regime Detection

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A sophisticated algorithmic trading strategy that adapts to different market conditions using machine learning-based regime detection and multi-timeframe analysis.

## 🎯 Features

### Market Regime Detection
- **Multiple Detection Methods:**
  - Hidden Markov Model (HMM) with Gaussian Mixture
  - Volatility-based classification
  - Trend strength analysis (ADX + Moving Averages)
  - Combined voting system for robust classification

- **Three Market Regimes:**
  - 🟢 **Trending Up**: Strong upward momentum
  - 🔵 **Ranging**: Sideways/consolidation markets
  - 🔴 **Trending Down**: Strong downward momentum

### Adaptive Signal Generation
- **Regime-Specific Strategies:**
  - **Trending Up**: Momentum-based entries, pullback opportunities
  - **Ranging**: Mean reversion, support/resistance trading
  - **Trending Down**: Short opportunities, breakdown patterns

- **Technical Indicators:**
  - Moving Averages (SMA, EMA)
  - MACD (Moving Average Convergence Divergence)
  - RSI (Relative Strength Index)
  - Bollinger Bands
  - ADX (Average Directional Index)
  - Stochastic Oscillator
  - ATR (Average True Range)

### Multi-Timeframe Analysis
- Analyze multiple timeframes simultaneously
- Higher timeframe confirmation for signal quality
- Adjustable confidence scoring

### Risk Management
- Position sizing based on account risk
- Dynamic stop-loss and take-profit levels
- Volume and volatility filters
- Confidence-based signal filtering

### Comprehensive Backtesting
- Historical performance analysis
- Key metrics:
  - Total Return
  - Win Rate
  - Profit Factor
  - Sharpe Ratio
  - Sortino Ratio
  - Maximum Drawdown
- Visual equity curves and trade distribution
- Detailed trade-by-trade analysis

## 📁 Project Structure

```
trading-strategy/
│
├── src/
│   ├── data_collection.py      # Data fetching and preprocessing
│   ├── regime_detection.py     # Market regime classification
│   ├── signal_generation.py    # Trading signal generation
│   ├── backtesting.py          # Strategy backtesting engine
│   └── utils.py                # Helper functions
│
├── config/
│   └── config.yaml             # Configuration parameters
│
├── tests/
│   ├── test_regime_detection.py
│   └── test_signal_generation.py
│
├── results/                    # Backtest results (auto-generated)
│
├── main.py                     # Main execution script
├── requirements.txt            # Python dependencies
└── README.md                   # This file
```

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/rishitmavani/trading-strategy.git
cd trading-strategy
```

2. **Create virtual environment (recommended):**
```bash
python -m venv venv

# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

4. **Install TA-Lib (required for technical indicators):**

**On Windows:**
- Download the appropriate wheel file from [here](https://www.lfd.uci.edu/~gohlke/pythonlibs/#ta-lib)
- Install: `pip install TA_Lib‑0.4.XX‑cpXX‑cpXX‑win_amd64.whl`

**On macOS:**
```bash
brew install ta-lib
pip install TA-Lib
```

**On Linux:**
```bash
sudo apt-get install ta-lib
pip install TA-Lib
```

### Configuration

Edit `config/config.yaml` to customize:

```yaml
data:
  symbols:
    - BTC-USD
    - ETH-USD
  timeframes:
    - 1h
    - 4h
  start_date: '2023-01-01'

regime:
  n_regimes: 3
  method: 'combined'  # Options: hmm, volatility, trend, combined

signals:
  min_confidence: 0.6
  use_multi_timeframe: true

risk:
  initial_capital: 10000
  risk_per_trade: 0.02  # 2% risk per trade

backtest:
  commission: 0.001  # 0.1%
  slippage: 0.0005   # 0.05%
```

### Running the Strategy

**Basic execution:**
```bash
python main.py
```

The script will:
1. Load configuration
2. Fetch historical data
3. Detect market regimes
4. Generate trading signals
5. Run backtest
6. Display results and generate plots

## 📊 Output

### Console Output
```
======================================================================
        ADAPTIVE TRADING STRATEGY WITH REGIME DETECTION
======================================================================

[1/6] Loading configuration...
[2/6] Initializing components...

======================================================================
Processing: BTC-USD
======================================================================
[3/6] Fetching data for BTC-USD...
  Loaded 8760 data points
  Date range: 2023-01-01 to 2024-12-31

[4/6] Detecting market regimes...

  Regime Distribution:

  Trending Up:
    Occurrence: 35.2% (3083 periods)
    Avg Return: 0.15%
    Volatility: 2.31%
    Sharpe: 1.82

  Ranging:
    Occurrence: 42.8% (3749 periods)
    Avg Return: 0.02%
    Volatility: 1.45%
    Sharpe: 0.34

  Trending Down:
    Occurrence: 22.0% (1928 periods)
    Avg Return: -0.18%
    Volatility: 2.67%
    Sharpe: -1.45

[5/6] Generating trading signals...
  Generated 8760 signal periods:
    Buy:  145
    Sell: 132
    Hold: 8483
    Average Confidence: 68.45%

[6/6] Running backtest...

  Performance Metrics:
    Total Return (%): 47.23
    Total Trades: 87
    Winning Trades: 52
    Losing Trades: 35
    Win Rate (%): 59.77
    Average Win ($): 324.56
    Average Loss ($): 187.43
    Profit Factor: 1.89
    Max Drawdown (%): -12.34
    Sharpe Ratio: 1.67
    Sortino Ratio: 2.34
```

### Generated Files

- **Equity Curve Plot**: `results/BTC_USD_20240211_103045.png`
- **Results JSON**: `results/results_20240211_103045.json`
- **Log File**: `trading_strategy.log`

## 🧪 Testing

Run unit tests:
```bash
python -m pytest tests/
```

Run specific test:
```bash
python -m pytest tests/test_regime_detection.py -v
```

## 📈 Strategy Logic

### Regime Detection

1. **Feature Calculation**:
   - Returns and volatility
   - Trend strength (MA crossovers)
   - ADX for trend intensity
   - Volume ratios

2. **Classification**:
   - Gaussian Mixture Model clustering
   - Volatility percentiles
   - Trend direction indicators
   - Majority voting across methods

### Signal Generation

**Trending Up Strategy**:
- Entry: Price above SMA20, SMA20 > SMA50, RSI < 70, MACD bullish
- Exit: Price below SMA20 or RSI > 80
- Stop Loss: Entry - (2 × ATR)
- Take Profit: Entry + (3 × ATR)

**Ranging Strategy**:
- Entry (Long): Price near lower Bollinger Band, RSI < 30
- Entry (Short): Price near upper Bollinger Band, RSI > 70
- Exit: Mean reversion to middle Bollinger Band
- Stop Loss: Entry ± (1.5 × ATR)

**Trending Down Strategy**:
- Entry: Price below SMA20, SMA20 < SMA50, RSI > 30, MACD bearish
- Exit: Price above SMA20 or RSI < 20
- Stop Loss: Entry + (2 × ATR)
- Take Profit: Entry - (3 × ATR)

### Multi-Timeframe Confirmation

- Primary timeframe (e.g., 1h) generates signals
- Higher timeframe (e.g., 4h) confirms direction
- Signals require alignment across timeframes
- Confidence boosted by 20% when aligned

## 🎓 Key Concepts

### Market Regimes

Markets don't behave uniformly. Different conditions require different strategies:

- **Trending Markets**: Momentum strategies work best
- **Ranging Markets**: Mean reversion is more effective
- **Volatile Markets**: Risk management becomes critical

### Adaptive Approach

By detecting the current regime, the strategy:
1. Applies appropriate entry/exit logic
2. Adjusts position sizing
3. Modifies risk parameters
4. Improves overall performance

## 🔧 Customization

### Add New Indicators

Edit `src/data_collection.py`:
```python
def add_technical_indicators(self, df: pd.DataFrame) -> pd.DataFrame:
    # Add your custom indicator
    df['my_indicator'] = talib.YOUR_INDICATOR(df['close'])
    return df
```

### Create Custom Strategy

Edit `src/signal_generation.py`:
```python
def _my_custom_strategy(self, df: pd.DataFrame, idx) -> Optional[Dict]:
    # Your strategy logic
    if condition:
        return {
            'signal': SignalType.BUY.value,
            'confidence': 0.8,
            'stop_loss': calculated_sl,
            'take_profit': calculated_tp
        }
    return None
```

## 📊 Performance Metrics Explained

- **Total Return**: Overall percentage gain/loss
- **Win Rate**: Percentage of profitable trades
- **Profit Factor**: Gross profit / Gross loss
- **Sharpe Ratio**: Risk-adjusted return (>1 is good, >2 is excellent)
- **Sortino Ratio**: Like Sharpe but only considers downside volatility
- **Max Drawdown**: Largest peak-to-trough decline

## ⚠️ Disclaimer

**This software is for educational purposes only. Trading cryptocurrencies and other financial instruments involves substantial risk of loss. Past performance is not indicative of future results. Use at your own risk.**

- Not financial advice
- No guarantee of profits
- Test thoroughly before live trading
- Start with paper trading
- Never risk more than you can afford to lose

## 🛣️ Roadmap

- [ ] Real-time trading integration
- [ ] More exchange connectors (Binance, Coinbase, etc.)
- [ ] Machine learning-based signal optimization
- [ ] Portfolio management across multiple assets
- [ ] Web dashboard for monitoring
- [ ] Telegram/Discord notifications
- [ ] Cloud deployment support

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📧 Contact

Rishit Mavani - [@RishitMavani](https://twitter.com/RishitMavani)

Project Link: [https://github.com/rishitmavani/trading-strategy](https://github.com/rishitmavani/trading-strategy)

## 🙏 Acknowledgments

- [TA-Lib](https://ta-lib.org/) for technical analysis functions
- [yfinance](https://github.com/ranaroussi/yfinance) for market data
- [scikit-learn](https://scikit-learn.org/) for machine learning algorithms

---

**Happy Trading! 📈**