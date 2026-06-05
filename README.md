# Stochastic Interest Rate Modelling and Prediction
**Implementation, Calibration, and Extension of the Cox-Ingersoll-Ross (CIR) Model**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1e51fcS5Ez4wcWhXvWQs99QZC2rxQhQb8?usp=sharing)

## 📌 Executive Summary
This repository contains a quantitative pipeline designed to reconstruct the US Treasury yield curve across 8 maturities (6M to 30Y) using only the 3-Month yield as a proxy for the instantaneous short rate ($r_t$). 

By establishing a strict out-of-sample **Information Barrier** to mathematically guarantee zero data leakage, the project evaluates the term structure using three distinct calibration paradigms:
1. **Base CIR Model:** Time-series Maximum Likelihood Estimation (MLE) under the physical measure ($\mathbb{P}$), transitioned to the risk-neutral measure ($\mathbb{Q}$) via a cross-sectional market price of risk.
2. **CIR++ Extension:** A static deterministic shift (Brigo-Mercurio) to test initial boundary fitting.
3. **Direct-Q Calibration:** A vectorized cross-sectional Non-Linear Least Squares (NLS) optimization via L-BFGS-B gradient descent.

## 📊 Key Results & Quantitative Insights
* **Predictive Accuracy:** The cross-sectionally calibrated Direct-Q model achieved an Out-of-Sample $R^2$ of **~0.89**, successfully reconstructing the entire yield curve from a single observable input and drastically outperforming the Naive Benchmark.
* **The Volatility Trade-Off:** The project mathematically proves that to fit a complex yield curve out-of-sample using a single-factor affine model, the non-linear optimizer actively crushes the stochastic volatility parameter ($\sigma \approx 0.0003$). This effectively strips the SDE of its randomness, forcing it to act as a deterministic ODE to maximize pure curve fit and bypass the Feller violation.
* **Extension Overfitting:** The notebook empirically demonstrates that naive, static-shift implementations of CIR++ catastrophically overfit the $T_0$ boundary curve, resulting in negative Out-of-Sample $R^2$ scores when macroeconomic regimes shift.

## 📂 Repository Structure
* `cir_project.ipynb`: The core quantitative notebook containing the data engineering pipeline, mathematical calibrations, out-of-sample predictions, and advanced parameter diagnostics.
* `train_data.csv` / `test_data.csv`: The raw daily bond yield data (cleaned via rolling Z-score first-difference filtering).
* `test_data_3M.csv`: The isolated proxy observables used to maintain the mathematical information barrier during out-of-sample forecasting.

## 🚀 How to Run the Pipeline
The simplest way to review and execute this project's quantitative pipeline is via Google Colab. 
1. Click the **Open in Colab** badge at the top of this document.
2. Upload the three CSV data files from this repository into the Colab environment.
3. Select **Runtime > Restart session and run all**.
