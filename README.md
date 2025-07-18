# Crypto-Price-Forecasting-Analysis-2025
Analysis and forecasting attempt of 2025 Ethereum prices, highlighting the critical impact of data sparsity on model performance.

# Cryptocurrency Market Sentiment & Price Forecasting (2025)

## Table of Contents
- [1. Overview](#1-overview)
- [2. Data Source](#2-data-source)
- [3. Methodology](#3-methodology)
    - [3.1 Data Cleaning & Preprocessing](#31-data-cleaning--preprocessing)
    - [3.2 Exploratory Data Analysis (EDA)](#32-exploratory-data-analysis-eda)
    - [3.3 Feature Engineering](#33-feature-engineering)
    - [3.4 Train-Test Split](#34-train-test-split)
    - [3.5 Model Selection & Training](#35-model-selection--training)
- [4. Model Evaluation & Key Learnings](#4-model-evaluation--key-learnings)
- [5. The Big Takeaway for an Analyst](#5-the-big-takeaway-for-an-analyst)
- [6. Tools Used](#6-tools-used)
- [7. Future Work](#7-future-work)
- [8. Contact Information](#8-contact-information)

---

## 1. Overview

This project delves into **Cryptocurrency Market Sentiment & Price Forecasting** using recent 2025 data, focusing specifically on **Ethereum**. The primary goal was to explore the feasibility of building a predictive model for daily cryptocurrency prices, leveraging various market indicators and sentiment scores. This project also served as a crucial learning experience regarding the fundamental challenges of forecasting highly volatile time series with limited data.

## 2. Data Source

The dataset used for this analysis is **'Cryptocurrency Market Sentiment & Price Data 2025'** from Kaggle. It contains daily (or sub-daily, which was resampled) price, volume, market cap, social/news sentiment, fear/greed index, volatility, and technical indicator data for various cryptocurrencies. For focused analysis, the project concentrated on **Ethereum**, which had the most data points in the dataset.

* **Link to Dataset:** [Cryptocurrency Market Sentiment & Price Data 2025](https://www.kaggle.com/datasets/pratyushpuri/crypto-market-sentiment-and-price-dataset-2025)

---

## 3. Methodology

My analytical approach involved several key steps tailored for time series data:

### 3.1 Data Cleaning & Preprocessing

* **Cryptocurrency Selection:** Filtered the dataset to focus exclusively on **Ethereum** data.
* **Timestamp Conversion & Indexing:** Converted the `timestamp` column to datetime objects and set it as the DataFrame index, crucial for time series operations. Data was then sorted chronologically.
* **Daily Resampling:** Resampled the data to a consistent **daily frequency**, calculating the mean for each day's metrics. This step confirmed **no missing days** in the processed Ethereum time series.
* **Feature Removal:** The original `cryptocurrency` column was dropped after filtering, as it was no longer needed.

### 3.2 Exploratory Data Analysis (EDA)

EDA was performed to understand Ethereum's price dynamics and the behavior of associated metrics over the observed 31-day period:

* **Price Trend:** The `current_price_usd` showed **short-term volatility** with noticeable fluctuations but no strong, sustained long-term trend over the brief duration.
    ![Ethereum Price Trend](plots/[YOUR_ETH_PRICE_PLOT_FILENAME].png)
* **Sentiment Trends:** Both `social_sentiment_score` and `news_sentiment_score` exhibited **dynamic fluctuations**, often crossing the neutral line.
    ![Ethereum Sentiment Trends](plots/[YOUR_SENTIMENT_TRENDS_PLOT_FILENAME].png)
* **Volume & Market Cap:** `trading_volume_24h` showed high variability, while `market_cap_usd` closely mirrored price movements as expected.
    ![Ethereum Trading Volume](plots/[YOUR_VOLUME_PLOT_FILENAME].png)
    ![Ethereum Market Cap](plots/[YOUR_MARKET_CAP_PLOT_FILENAME].png)
* **Correlations:** A **correlation matrix** revealed strong positive relationships between `current_price_usd` and `market_cap_usd`, `social_sentiment_score` (0.64), and `fear_greed_index` (0.65). Moderate negative correlations were observed with `volatility_index` (-0.49) and `rsi_technical_indicator` (-0.43).
    ![Correlation Matrix](plots/[YOUR_CORRELATION_MATRIX_PLOT_FILENAME].png)
* **Sentiment vs. Price:** While correlated, the scatter plot of `social_sentiment_score` vs. `current_price_usd` showed a **wide scatter**, indicating sentiment is a contributing factor but not a sole determinant.
    ![Social Sentiment vs. Price Scatter](plots/[YOUR_SENTIMENT_VS_PRICE_PLOT_FILENAME].png)

### 3.3 Feature Engineering

* To adapt the data for time series forecasting, **lagged features** (e.g., price from previous days, sentiment from previous days) and **rolling window statistics** (e.g., 3-day rolling mean/std of price and volume) were created.
* A **simplified set of lags and rolling windows** was used (lags: 1, 2; windows: 3 days) to maximize the number of usable data points after `dropna()` operation, which removes rows with initial NaN values. This resulted in **29 effective data points** for modeling.

### 3.4 Train-Test Split

* A **time-based split** was implemented, with the earliest **23 samples** used for training and the latest **6 samples** reserved for testing, preventing data leakage from future into past.

### 3.5 Model Selection & Training

* **LightGBM Regressor** was chosen for its efficiency and ability to handle tabular data with complex relationships. The model was trained on the prepared features.

---

## 4. Model Evaluation & Key Learnings

The performance of the LightGBM Regressor on the test set revealed critical insights:

* **Mean Absolute Error (MAE):** **$[YOUR\_MAE\_VALUE]$** (e.g., $27.01)
* **Root Mean Squared Error (RMSE):** **$[YOUR\_RMSE\_VALUE]$** (e.g., $31.40)
* **R-squared (R2):** **$[YOUR\_R2\_VALUE]$** (e.g., -0.8588)

**Why the Model Performed Poorly:**

* The **negative R-squared** is a definitive indicator that the model performed worse than simply predicting the average price. This means it failed to capture any meaningful predictive patterns.
* During training, LightGBM issued repeated warnings, stating **"There are no meaningful features which satisfy the provided configuration"** and **"Stopped training because there are no more leaves that meet the split requirements."** This confirms that, despite the correct setup, the model could not build effective decision trees.
* Consequently, the feature importance analysis showed **all features having zero importance**, as the model was unable to leverage any of them effectively.
    ![Feature Importance (Zero)](plots/[YOUR_ZERO_IMPORTANCE_PLOT_FILENAME].png)

This outcome is primarily due to the **severe data sparsity**. With only 23 training samples, the model lacked the sufficient historical context to learn robust relationships for highly volatile cryptocurrency prices. Complex models like LightGBM require a much larger volume of data (often years of daily observations) to identify stable trends and patterns.

---

## 5. The Big Takeaway for an Analyst

This project, while not yielding a high-performing predictive model, served as a **powerful, real-world lesson on the fundamental importance of data quantity** in machine learning. It clearly demonstrated that:

* **Data Sufficiency is Paramount:** Even with sophisticated models and correct methodology, insufficient historical data for a volatile phenomenon like crypto prices can prevent any reliable predictive learning.
* **Diagnostic Skills are Crucial:** Interpreting model warnings, understanding metrics like a negative R-squared, and diagnosing the root cause (data sparsity) are vital skills for any data professional.
* **Realistic Expectations:** It's important to recognize when data limitations dictate that a problem cannot be solved reliably with the available information.

This experience highlights my ability not just to execute a data science pipeline, but also to critically analyze results, diagnose issues, and articulate data limitations – qualities highly valued in any analytical role.

## 6. Tools Used

* `Python`
* `Pandas` (for data manipulation and time series handling)
* `NumPy` (for numerical operations)
* `Scikit-learn` (for model splitting and evaluation)
* `LightGBM` (for gradient boosting regression model)
* `Matplotlib` & `Seaborn` (for data visualization)

## 7. Future Work

* **Acquire More Data:** The most critical step for future improvement would be to obtain a **significantly larger dataset** (e.g., several years of daily data) to allow the model to learn long-term trends and robust patterns.
* **Deep Learning Models:** With more data, explore advanced time series models like LSTMs (Long Short-Term Memory networks) or Transformers, which are well-suited for sequence prediction.
* **Event-Driven Analysis:** Incorporate major news events or regulatory changes as external features to see their impact on price.

## 8. Contact Information

* **Naveen Lakshman Kumar Basina**
* **LinkedIn:** [https://www.linkedin.com/in/naveen-lakshman/](https://www.linkedin.com/in/naveen-lakshman/)
* **Email:** [naveenklaxman22@gmail.com](mailto:naveenklaxman22@gmail.com)
