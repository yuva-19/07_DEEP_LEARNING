# Tesla Stock Price Forecasting Using LSTM

A portfolio-focused time-series forecasting project that uses a **PyTorch LSTM** to predict the **next trading day's Tesla (TSLA) closing price**.

> **Disclaimer:** This is an educational and portfolio project, not financial advice or a trading strategy. Stock prices are highly uncertain and affected by many external factors.

---

## 1. Project Objective

The objective is to forecast the next trading day's TSLA closing price using the previous **60 trading days** of historical closing prices.

The project follows a complete machine-learning workflow:

```text
Data
  ↓
EDA
  →
Chronological Split
  →
Leakage-Free Scaling
  →
Sliding Windows
  →
Naive Baseline
  →
LSTM
  →
Controlled Experiments
  →
Validation Model Selection
  →
Unseen Test Evaluation
  →
Error Analysis
  →
One-Step Inference
  →
Model Saving
```

The final selected model predicts the **next-day price change (Delta)** rather than directly predicting the absolute closing price.

---

## 2. Why Use an LSTM?

Stock prices are sequential observations, where the order of observations matters.

The LSTM receives a sequence of the previous 60 closing prices:

```text
60 trading days × 1 feature
          ↓
          LSTM
          ↓
   Last hidden representation
          →
      Linear Layer
          →
 Predicted next-day Delta (Δ̂)
```

The predicted Delta is then converted back into a predicted closing price.

```text
Predicted Next Close (ŷ)
    =
Current Close
    +
Predicted Delta (Δ̂)
```

---

## 3. Dataset

- **Asset:** Tesla, Inc. (`TSLA`)
- **Frequency:** Daily
- **Primary feature:** `Close`
- **Target:** Next trading day's closing price
- **Source:** Yahoo Finance through `yfinance`
- **Download:** Historical daily data using `period="max"` and `interval="1d"`

The notebook also performs basic data-quality checks including:

- chronological ordering
- duplicate dates
- missing values
- positive prices
- OHLC consistency

---

## 4. Target Formulation

The original target is the next trading day's closing price:

```text
Target_Close = Close shifted by -1 trading day
```

For the final Delta experiment, the learning target becomes:

```text
Δₜ = Close₍ₜ₊₁₎ - Closeₜ
```

The model therefore learns the expected one-day price movement.

After prediction:

```text
Predicted Next Close (ŷ)
    =
Current Close
    +
Predicted Delta (Δ̂)
```

This allows the final predictions to be evaluated in actual dollar prices.

---

## 5. Train / Validation / Test Split

Because this is a time-series problem, the data is split chronologically.

```text
80%  -> Training
10%  -> Validation
10%  -> Test
```

No random split is used.

### Why?

Randomly mixing observations could allow information from a later time period to influence training for an earlier period.

The test set remains untouched until the final evaluation.

---

## 6. Scaling and Data Leakage Prevention

A `StandardScaler` is fitted **only on the training data**.

```text
Training data
    →
Fit scaler
    →
Transform training data
    →
Use the same scaler for validation and test
```

The Delta target also has its own scaler, which is fitted only on the training Delta values.

This prevents future information from leaking into preprocessing.

---

## 7. Sliding Windows

The final sequence length is:

```text
60 trading days
```

The LSTM input shape is:

```text
(samples, 60, 1)
```

where:

- `samples` = number of training examples
- `60` = historical time steps
- `1` = Close feature

Validation and test windows use only historical observations that were available before their prediction dates.

---

## 8. Naive Baseline

Before evaluating the LSTM, the project uses a simple persistence baseline.

```text
Predicted Next Close (ŷ) = Today's Close (Pₜ)
```

This baseline is important because a sophisticated neural network should demonstrate that it provides value over a simple forecasting strategy.

---

## 9. LSTM Architecture

The final architecture is:

```text
Input: 60 × 1
      ↓
2-Layer LSTM
      ↓
Hidden Size = 128
      →
Dropout = 0.2
      →
Last Timestep
      →
Linear Layer: 128 -> 1
      →
Predicted Delta (Δ̂)
```

### Configuration

| Parameter | Value |
|---|---:|
| Sequence length | 60 |
| Input features | 1 |
| Hidden size | 128 |
| LSTM layers | 2 |
| Dropout | 0.2 |
| Optimizer | Adam |
| Learning rate | 0.001 |
| Batch size | 64 |
| Primary loss | MSE |
| Device | CUDA when available |
| Mixed precision | AMP when CUDA is available |

---

## 10. GPU Acceleration

The project is implemented using **PyTorch** with CUDA support.

When a compatible NVIDIA GPU is available:

- the model is moved to GPU
- training batches are moved to GPU
- pinned-memory DataLoaders are used
- mixed-precision training is enabled through AMP

The dataset itself is not unnecessarily copied entirely to GPU memory.

---

## 11. Controlled Experiments

The project uses controlled experiments rather than changing many variables at the same time.

Earlier experiments established the main architecture:

| Experiment | Change | Validation Conclusion |
|---|---|---|
| Sequence length | 60 -> 30 | 30 days was worse |
| Features | Close -> OHLCV | OHLCV was worse |
| Hidden size | 64 -> 128 | 128 improved validation |
| LSTM depth | 2 -> 3 layers | 3 layers was worse |
| Dropout | 0.2 -> 0.4 | 0.4 was worse |
| Learning rate | 0.001 -> 0.0005 | 0.0005 was worse |

The final compact experiment compares:

1. **Close + MSE**
2. **Close + Huber / SmoothL1**
3. **Delta + MSE**

The Delta experiment tests whether predicting one-day price movement is easier than directly predicting the absolute stock price.

---

## 12. Validation Model Selection

The three final configurations produced:

| Model | Validation MAE | Validation RMSE |
|---|---:|---:|
| **Delta + MSE (128)** | **$6.6577** | **$9.5708** |
| Close + MSE (128) | $10.3676 | $14.4266 |
| Close + Huber (128) | $11.1814 | $15.7160 |

### Validation Winner

**Delta + MSE (128)** was selected because it achieved the lowest validation MAE and RMSE.

The final model therefore predicts:

```text
Next-Day Delta
```

and reconstructs:

```text
Next-Day Close = Current Close + Predicted Delta (Δ̂)
```

---

## 13. Final Test Evaluation

After model selection, the test set is used for the first final evaluation.

The test set is not used to choose:

- architecture
- sequence length
- target representation
- loss function
- hyperparameters

The final test comparison is:

```text
Naive Baseline
      vs
Final Delta + MSE LSTM
```

The notebook reports:

- MAE
- MSE
- RMSE

The final test numbers shown by the completed notebook should be treated as the authoritative final results.

---

## 14. Error Analysis

Model performance is not evaluated only through a single metric.

The project examines:

- actual vs predicted prices
- prediction error over time
- error distribution
- largest absolute errors
- LSTM vs naive baseline

The purpose is to understand **when and why the model fails**.

Large errors can occur during abrupt price movements and changing market regimes, where historical price patterns may not adequately represent sudden changes.

---

## 15. One-Step Forecasting

After evaluation, the saved final model can be used for inference.

The latest 60 available closing prices are passed through the same preprocessing pipeline:

```text
Latest 60 Close Prices
        ↓
Training Close Scaler
        →
Final Delta LSTM
        →
Predicted Delta (Δ̂)
        →
Delta Scaler
        →
Current Close + Predicted Delta (Δ̂)
        →
Forecast Next-Day Close
```

The notebook reports:

```text
Latest available Close
Forecast next Close
Forecast change
```

---

## 16. Model Saving

The final model is saved as:

```text
models/tesla_lstm_final.pt
```

The checkpoint stores:

- model weights
- model name
- target representation
- sequence length
- hidden size
- number of LSTM layers
- dropout
- input feature
- Close scaler parameters
- Delta scaler parameters
- random seed

This allows the trained model and preprocessing information to be reproduced later.

---

## 17. Current Project Structure

The project currently contains only the files and folder shown below:

```text
Tesla-LSTM/
├── 1 Tesla LSTM.ipynb
├── Tesla_LSTM_Final_Presentation.ipynb
├── README.md
└── models/
    └── tesla_lstm_final.pt
```

### File roles

**`1 Tesla LSTM.ipynb`**

The original/full development notebook containing the broader experimentation and development history.

**`Tesla_LSTM_Final_Presentation.ipynb`**

The compact notebook prepared for the final presentation.

**`README.md`**

Project documentation, methodology, results, limitations, and interview context.

**`models/tesla_lstm_final.pt`**

Saved checkpoint of the selected Delta + MSE LSTM model.

> No `src/`, `results/`, `figures/`, `metrics/`, or `requirements.txt` folders/files are currently part of the project structure.

---

## 18. Presentation Flow

A concise presentation can follow this sequence:

### 1. Problem

Forecast the next trading day's TSLA closing price using the previous 60 trading days.

### 2. Data

Explain:

- TSLA daily data
- data cleaning
- price trend
- volatility

### 3. Time-Series Methodology

Explain:

- chronological splitting
- training-only scaling
- sliding windows
- leakage prevention

### 4. Baseline

Explain:

```text
ŷₜ₊₁ = Pₜ
```

### 5. LSTM

Show:

```text
60 days x 1 feature
        →
2-Layer LSTM
        →
128 Hidden Units
        →
Linear Layer
        →
Predicted Delta (Δ̂)
```

### 6. Controlled Experiments

Show the three final configurations:

```text
Close + MSE
Close + Huber
Delta + MSE
```

Then show why **Delta + MSE** was selected.

### 7. Final Test

Compare:

```text
Naive Baseline
        vs
Final LSTM
```

using MAE, MSE, and RMSE.

### 8. Error Analysis and Conclusion

Discuss where the model performs well, where it fails, and why a complex model does not automatically outperform a simple baseline.

---

## 19. Interview Questions

### Why did you use LSTM?

The problem contains sequential dependencies, and LSTM provides gated memory mechanisms designed to model dependencies across time steps.

### Why not randomly split the data?

Because temporal order is part of the problem. Random splitting can introduce future information into the training process.

### Why fit the scaler only on training data?

To prevent future distribution information from leaking into preprocessing.

### Why use a 60-day window?

A 60-day lookback provides a reasonable historical context and performed better than the tested 30-day configuration.

### Why is the naive baseline important?

Because a complex model must demonstrate value over a simple forecasting strategy.

### Why test a Delta target?

Absolute stock prices are non-stationary. Predicting the one-day price change is an alternative formulation that tests whether the learning problem becomes easier.

### Why use MSE?

MSE strongly penalizes larger errors and is a standard regression training objective.

### Why might the LSTM lose to the naive baseline?

Short-term stock prices are noisy and affected by changing market regimes. Today's price can already be a very strong predictor of tomorrow's price, making the forecasting problem difficult for a more complex model.

### What is the biggest limitation?

The model mainly uses historical price information and does not incorporate external variables such as:

- news
- market-wide factors
- macroeconomic conditions
- sentiment
- company fundamentals

---

## 20. Resume Description

### Technical Version

**Tesla Stock Price Forecasting Using LSTM - PyTorch**

- Developed a GPU-accelerated PyTorch LSTM forecasting pipeline for one-step-ahead TSLA price prediction using 60-day sliding windows and leakage-free chronological train/validation/test splits.
- Implemented training-only scaling, CUDA/AMP training, persistence baseline comparison, controlled experiments, and error analysis using MAE, MSE, and RMSE.
- Compared absolute-price and Delta-target formulations and selected a Delta + MSE LSTM based on validation performance before evaluating on an unseen chronological test set.

### Short Version

- Built a PyTorch LSTM time-series forecasting system for TSLA using chronological splitting, leakage-free scaling, 60-day sequences, GPU training, and baseline comparison.
- Performed controlled architecture and target experiments and evaluated the selected model on an untouched chronological test set.

---

## 21. Limitations and Future Improvements

Possible future improvements include:

- predict returns instead of absolute price
- add NASDAQ / S&P 500 market features
- add volatility indicators
- add carefully engineered technical indicators
- incorporate news and sentiment
- compare against GRU
- compare against Transformer-based time-series models
- use walk-forward validation
- investigate multi-step forecasting
- investigate probabilistic forecasting
- perform strict backtesting with transaction costs

These are future experiments, not claims that the current model can reliably generate trading profits.

---

## 22. Final Conclusion

This project demonstrates a complete time-series machine-learning workflow rather than simply training an LSTM.

The main engineering practices demonstrated are:

- chronological data splitting
- leakage prevention
- training-only scaling
- sliding-window sequence generation
- naive baseline comparison
- GPU-accelerated PyTorch training
- mixed precision
- controlled experimentation
- validation-based model selection
- untouched final test evaluation
- error analysis
- one-step inference
- model checkpointing

The final validation experiment selected **Delta + MSE (128)** as the best configuration.

Most importantly, the project treats the baseline and experimental results honestly rather than assuming that a more complex neural network must always perform better.

---

## Disclaimer

This project is for **educational and portfolio purposes only**.

It does not constitute financial advice, an investment recommendation, or a reliable trading strategy.

Historical stock-price patterns do not guarantee future performance.
