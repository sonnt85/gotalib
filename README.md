# gotalib

Technical analysis library for Go — streaming and batch implementations of common financial indicators.

## Installation

```bash
go get github.com/sonnt85/gotalib
```

## Features

- Streaming (per-tick) API: create an indicator object, call `Update(value)` each tick
- Batch array API: pass a full slice, receive a result slice
- Moving averages: SMA, EMA, WMA, DEMA, TEMA, TRIMA, KAMA
- Momentum indicators: RSI, CMO, ROC, TRIX
- Trend indicators: ADX, ADXR, Aroon, AroonOsc, DX, MACD, MACDExt
- Volatility indicators: ATR, NATR, TrueRange, BollingerBands, StdDev
- Volume indicators: AD, ADOSC, OBV, MFI
- Oscillators: CCI, BOP, PPO, APO, StochFast, StochSlow, StochRSI, WillR, UltOsc
- Utility: rolling Max, Min, Sum, Variance, Diff, circular buffer (CBuf)

## Usage

```go
import "github.com/sonnt85/gotalib"

// Streaming: update one value at a time
rsi := gotalib.NewRsi(14)
for _, close := range closes {
    value := rsi.Update(close)
    fmt.Printf("RSI: %.2f\n", value)
}

// Batch: compute over full slice
rsiValues := gotalib.RsiArr(closes, 14)

// Bollinger Bands (batch)
upper, middle, lower := gotalib.BBandsArr(gotalib.SMA, closes, 20, 2.0, 2.0)

// MACD (batch)
macdLine, signalLine, histogram := gotalib.MacdArr(closes, 12, 26, 9)

// Moving average with type selection
ma := gotalib.NewMa(gotalib.EMA, 20)
value := ma.Update(price)
```

## API

### Types
- `MaType` — moving average type constant: `SMA`, `EMA`, `WMA`, `DEMA`, `TEMA`, `TRIMA`, `KAMA`
- `CBuf` — circular buffer for rolling window calculations

### Moving Averages
- `NewSma/SmaArr(n)` — Simple Moving Average
- `NewEma/EmaArr(n, k)` — Exponential Moving Average
- `NewWma/WmaArr(n)` — Weighted Moving Average
- `NewDema/DemaArr(n)` — Double EMA
- `NewTema/TemaArr(n)` — Triple EMA
- `NewTrima/TrimaArr(n)` — Triangular Moving Average
- `NewKama/KamaArr(n)` — Kaufman Adaptive Moving Average
- `NewMa/MaArr(type, n)` — Generic moving average dispatcher

### Momentum & Trend
- `NewRsi/RsiArr(n)` — Relative Strength Index
- `NewMacd/MacdArr(fast, slow, signal)` — MACD line, signal, histogram
- `NewAdx/AdxArr(n)` — Average Directional Index
- `NewAroon/AroonArr(n)` — Aroon Up/Down
- `NewAtr/AtrArr(n)` — Average True Range
- `NewBBands/BBandsArr(type, n, upStd, dnStd)` — Bollinger Bands (upper, middle, lower)
- `NewCci/CciArr(n)` — Commodity Channel Index
- `NewStochFast/StochFastArr(...)` — Fast Stochastic %K and %D
- `NewStochSlow/StochSlowArr(...)` — Slow Stochastic
- `NewStochRsi/StochRsiArr(...)` — Stochastic RSI

### Volume
- `NewAd/AdArr(h, l, c, v)` — Accumulation/Distribution Line
- `NewObv/ObvArr(c, v)` — On-Balance Volume
- `NewMfi/MfiArr(h, l, c, v, n)` — Money Flow Index

## License

MIT License - see [LICENSE](LICENSE) for details.
