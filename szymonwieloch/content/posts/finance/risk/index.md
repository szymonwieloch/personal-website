+++
date = '2026-05-18T20:41:19+02:00'
title = 'Mastering Market Risk: From Volatility to Cornish-Fisher VaR'
image = './featured.jpg'
categories = ["Finance"]
+++

In the world of investing, risk isn't just "the probability of losing money"—it is a multidimensional spectrum. To build resilient portfolios, quantitative analysts and risk managers rely on a robust toolkit of metrics to quantify uncertainty, tail risks, and asymmetric losses.

<!--more-->

{{< katex >}}

This guide breaks down five essential market risk metrics—**Volatility, Semi-variance, Drawdown, Value at Risk (VaR), and Conditional Value at Risk (CVaR)**—complete with Python implementations to add to your risk management arsenal.

# Volatility (Standard Deviation)

Volatility is the foundational metric of market risk. It measures the dispersion of asset returns around their mean.

While widely used (and the backbone of Modern Portfolio Theory), traditional volatility treats both massive gains and massive losses equally. It assumes a symmetrical distribution of returns, which rarely happens in real-world markets.

## The Math

The annualized volatility \(\sigma_{ann}\) is calculated from periodic returns \(r_t\) as:

$$\sigma = \sqrt{\frac{1}{N-1} \sum_{t=1}^{N} (r_t - \bar{r})^2}$$

$$\sigma_{ann} = \sigma \times \sqrt{P}$$

Where \(P\) is the number of trading periods in a year (e.g., 252 for daily data).

## Python Implementation

```python
import numpy as np
import pandas as pd

def calculate_volatility(returns, periods_per_year=252):
    """Calculates annualized volatility."""
    daily_vol = np.std(returns, ddof=1)
    return daily_vol * np.sqrt(periods_per_year)

# Example Usage
np.random.seed(42)
sample_returns = np.random.normal(0.0005, 0.01, 1000) # Simulated daily returns
print(f"Annualized Volatility: {calculate_volatility(sample_returns):.4f}")

```

# Semi-Variance (Downside Deviation)

Because investors only dislike *downside* volatility, Harry Markowitz introduced **Semi-variance**. This metric modifies standard deviation by only looking at returns that fall below a specific target or threshold (usually \(0\) or the risk-free rate).

## Python Implementation

```python
def calculate_semi_deviation(returns, target_return=0, periods_per_year=252):
    """Calculates annualized semi-deviation (downside risk)."""
    downside_returns = returns[returns < target_return]
    # Standard deviation calculation using only downside returns, but normalized by total observations
    semi_variance = np.sum((downside_returns - target_return) ** 2) / (len(returns) - 1)
    return np.sqrt(semi_variance) * np.sqrt(periods_per_year)

print(f"Annualized Downside Risk: {calculate_semi_deviation(sample_returns):.4f}")

```

# Drawdown (Peak-to-Trough Loss)

Drawdown measures the peak-to-trough decline of an investment's value during a specific period, expressed as a percentage. While volatility measures choppy waters day-to-day, drawdown tells you how much pain you actually have to endure from the absolute top to the absolute bottom.

**Maximum Drawdown (Max DD)** is the ultimate psychological stress test for fund managers.

## Python Implementation

```python
def calculate_drawdown_metrics(returns):
    """Calculates the drawdown series, Max Drawdown, and Average Drawdown."""
    # Convert returns to a wealth index starting at 1
    wealth_index = (1 + returns).cumprod()
    previous_peaks = wealth_index.cummax()
    drawdowns = (wealth_index - previous_peaks) / previous_peaks
    
    return {
        "drawdown_series": drawdowns,
        "max_drawdown": drawdowns.min(),
        "avg_drawdown": drawdowns.mean()
    }

# Example Usage with a simulated price path
dd_metrics = calculate_drawdown_metrics(sample_returns)
print(f"Maximum Drawdown: {dd_metrics['max_drawdown']:.2%}")

```

# Value at Risk (VaR)

Value at Risk answers a simple question: *"What is the maximum amount I could expect to lose over a given time horizon at a specific confidence level (e.g., 95% or 99%)?"*

VaR can be calculated using several methodologies, each with its own strengths and fatal flaws.

## Historical VaR

The simplest approach. It makes no assumptions about normality; it simply looks at past data, sorts the returns from worst to best, and finds the threshold cutting off the worst \(x\%\) of returns.

## Parametric (Variance-Covariance) VaR

This assumes returns follow a strict Normal (Gaussian) Distribution. It calculates VaR using just the mean (\(\mu\)) and standard deviation (\(\sigma\)) of the returns.

## Cornish-Fisher VaR

Real market returns exhibit **skewness** (asymmetry) and **kurtosis** (fat tails). Parametric VaR drastically underestimates risk during market crashes because it ignores these features. The Cornish-Fisher expansion adjusts the normal distribution Z-score to account for skewness and kurtosis.

The adjusted Z-score (\(Z_{cf}\)) is calculated as:

$$Z_{cf} = Z + \frac{S}{6}(Z^2 - 1) + \frac{K}{24}(Z^3 - 3Z) - \frac{S^2}{36}(2Z^3 - 5Z)$$

Where \(Z\) is the standard normal distribution quantile, \(S\) is skewness, and \(K\) is excess kurtosis.

## Python Implementation

```python
from scipy.stats import norm, skew, kurtosis

def calculate_var_historical(returns, alpha=0.05):
    """Historical Value at Risk."""
    return -np.percentile(returns, alpha * 100)

def calculate_var_parametric(returns, alpha=0.05):
    """Parametric (Gaussian) Value at Risk."""
    mu = np.mean(returns)
    sigma = np.std(returns, ddof=1)
    z_score = norm.ppf(1 - alpha)
    return -(mu - z_score * sigma)

def calculate_var_cornish_fisher(returns, alpha=0.05):
    """Cornish-Fisher Value at Risk (accounts for skewness and kurtosis)."""
    mu = np.mean(returns)
    sigma = np.std(returns, ddof=1)
    s = skew(returns)
    k = kurtosis(returns, fisher=True) # Excess kurtosis
    
    z = norm.ppf(1 - alpha)
    
    # Cornish-Fisher expansion for Z-score
    z_cf = (z + 
            (s / 6) * (z**2 - 1) + 
            (k / 24) * (z**3 - 3*z) - 
            (s**2 / 36) * (2*z**3 - 5*z))
    
    return -(mu - z_cf * sigma)

# Comparison
print(f"Historical VaR (95%):       {calculate_var_historical(sample_returns, 0.05):.4f}")
print(f"Parametric VaR (95%):       {calculate_var_parametric(sample_returns, 0.05):.4f}")
print(f"Cornish-Fisher VaR (95%):   {calculate_var_cornish_fisher(sample_returns, 0.05):.4f}")

```

# Conditional Value at Risk (CVaR) / Expected Shortfall

While VaR tells you the *boundary* of your worst-case scenarios, it remains completely blind to what happens *beyond* that boundary. If your 95% 1-day VaR is $10,000, you might lose $10,001 or $1,000,000 in that remaining 5% tail—VaR doesn't care.

**Conditional Value at Risk (CVaR)**, also known as Expected Shortfall, solves this. It measures the **average loss** of the portfolio given that the loss has already breached the VaR threshold. It is a coherent risk metric, making it highly favored by modern regulators and institutional allocators.

## Python Implementation

```python
def calculate_cvar_historical(returns, alpha=0.05):
    """Calculates historical Conditional Value at Risk (Expected Shortfall)."""
    # Find the historical VaR threshold
    var_threshold = -calculate_var_historical(returns, alpha)
    
    # Take the mean of all returns that are worse than (less than) the negative VaR threshold
    tail_losses = returns[returns <= var_threshold]
    return -tail_losses.mean()

print(f"Historical CVaR (95%): {calculate_cvar_historical(sample_returns, 0.05):.4f}")

```

# Summary Matrix

| Metric | Focus | Key Limitation | Best Used For |
| --- | --- | --- | --- |
| **Volatility** | Total dispersion | Treats upside gains as risk | Baseline baseline asset comparison |
| **Semi-Variance** | Pure downside risk | Disregards extreme tail probability | Asymmetric return profile analysis |
| **Drawdown** | Peak-to-trough pain | Slow moving, backward looking | Behavioral stress testing & CTA tracking |
| **VaR** | Threshold loss boundary | Blind to magnitude of catastrophic tail | Regulatory reporting & capital allocation |
| **CVaR** | Average tail loss severity | Requires significant data to map tails reliably | Absolute worst-case scenario protection |