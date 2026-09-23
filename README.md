# dino-polska-capm-analysis

## Project Overview
This notebook applies the Capital Asset Pricing Model (CAMP) to predict the stock performance of 'Dino' (a Polish retail chain) using the WIG20 index as a market benchmark. The analysis covers the period from 2017 to 2025, with weekly data points.

## Data
Two primary datasets are used, loaded from CSV files:
- `dnp_w.csv`: Contains weekly opening and closing prices for Dino stock.
- `wig20_w.csv`: Contains weekly opening and closing prices for the WIG20 stock market index.

Both datasets are merged based on the 'Data' column and filtered to include relevant price information. A risk-free rate of 4.4% annually (converted to weekly) is assumed for calculations.

## Methodology

### 1. Data Preparation
- **Weekly Returns Calculation**: Weekly returns (`K_dnp` for Dino, `K_wig20` for WIG20) are calculated based on opening and closing prices.
- **Data Splitting**: The dataset is split into:
  - **Training Data (`df1`)**: 2017-2024, used for estimating CAMP parameters.
  - **Prediction Data (`df_pred`)**: 2025, used for evaluating the model's predictive power.

### 2. CAMP Model Parameter Estimation
The CAMP model is defined as: 
$$K_{\text{dino}}-r = \alpha + \beta (K_{MP}-r), $$
where:
- $K_{\text{Dino}}$: Return of Dino stock
- $K_{MP}$: Return of the market portfolio (WIG20 index)
- $r$: Risk-free rate
- $\alpha$: Alpha coefficient (intercept)
- $\beta$: Beta coefficient (sensitivity to market movements)

The parameters $\alpha$ and $\beta$ are estimated by minimizing the squared difference between the observed excess returns ($y = K_{\text{Dino}}-r$) and the model's prediction ($\alpha + \beta x$, where $x = K_{MP}-r$). The formulas used for estimation are derived from statistical principles:
$$\beta = \frac{\bar{x}\bar{y} - \overline{xy}}{\bar{x}\bar{x} - \overline{xx}}, \quad \alpha = \bar{y} - \beta \bar{x}.$$

### 3. Model Prediction and Evaluation
- **Prediction**: The estimated $\alpha$ and $\beta$ values are used to predict Dino's excess returns for 2025 (`y_pred`).
- **Comparison**: `y_pred` is compared against the actual excess returns for 2025 (`y_true`).
- **Visualization**: A plot comparing predicted vs. true returns for 2025 is generated.
- **R^2 Score**: The R-squared ($R^2$) metric is calculated to quantify the proportion of variance in Dino's returns that can be explained by the WIG20 index. A higher $R^2$ indicates a better fit.

### 4. Rolling Window Analysis
To investigate how model parameters and reliability change over time, a rolling window analysis is performed:
- The model is re-estimated using a 1-year (52-week) rolling window.
- Plots are generated to visualize the evolution of $\beta$ and $R^2$ over time, showing how Dino's market sensitivity and the model's explanatory power fluctuate.

## Key Findings
- **Initial Model Performance**: For the full 2017-2024 training period, the model yields a $\beta$ value of approximately 0.676, indicating that Dino stock behaves less sensitively to market movements than the overall market (i.e., it's considered 'safer'). The R-squared score of 0.1859 suggests that while the model captures some variance, there's significant unexplained variation.
- **Time-Dependent Reliability**: The rolling window analysis reveals that the model performs poorly in the early years (before 2020), likely due to Dino's initial period on the stock exchange. After 2020, the model's reliability (as indicated by $R^2$) improves. The $\beta$ coefficient, after an initial decline, tends to converge towards values that suggest Dino's risk profile becomes more aligned with the broader market over time.

## Usage
To run this analysis, simply execute the cells in the notebook sequentially. Ensure that the `dnp_w.csv` and `wig20_w.csv` files are accessible at the specified path (`/content/drive/MyDrive/portfel/`).