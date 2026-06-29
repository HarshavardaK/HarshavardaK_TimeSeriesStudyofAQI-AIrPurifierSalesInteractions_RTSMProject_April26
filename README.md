# HarshavardaK_TimeSeriesStudyofAQI-AIrPurifierSalesInteractions_RTSMProject_April26

1
Modeling Daily AQI Trends in Delhi Using Revenue Data
from Air Purifier Sales by Blue Star and Regional
Consumer Behavior Data
Regression and Time Series Models (MA60280)
Spring 25-26
Course Project | Prof. Buddhananda Banerjee
Group Members:
1. Harshavarda Kumarasamy(23NA3FP47)
2. Ritvik Puvvada (23NA3FP32)
3. Vasa Harish (23EC3FP55)
4. Vangala Akshay Reddy (23CE3FP11)
2
Contents
1 Introduction . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3
2 Modeling the Delhi AQI Time Series . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3
2.1 Data Preprocessing and AQI Derivation........................................................................ 3
2.2 Stationarity Check............................................................................................................4
2.3 ADF Test...........................................................................................................................5
2.4 ARMA Model Fitting on Raw AQI . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6
2.5 STL Decomposition of AQI Series..................................................................................8
2.6 Forecast Reconstruction and Performance . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 9
3 Modeling the Revenue Time Series.................................................................... 10
3.1 Data Cleaning and Structure........................................................................................ 10
3.2 Stationarity Assessment................................................................................................. 11
3.3 STL Decomposition of Revenue Series.........................................................................11
3.4 Forecasting Revenue with Residual Model.................................................................. 12
4 Final Synthesis: Strategic Insight from Forecasting.....................................13
4.1 Key Insights and Contributions..................................................................................... 13
4.2 Broader Implications.......................................................................................................14
5 Conclusion.................................................................................................................. 16
3
1 Introduction
This report examines how variations in the Air Quality Index (AQI), particularly driven by PM2.5
concentrations, influence consumer behavior in the air purifier market. Air pollution has become a
critical issue, especially in densely populated urban centers like Delhi, where air quality levels have
frequently reached hazardous conditions. Increasing awareness about the health impacts of fine
particulate matter, such as PM2.5-which can severely affect the respiratory system-has made people
more cautious about their exposure to polluted air.
As a result of these growing concerns, the demand for air purifiers tends to surge during periods of
poor or hazardous AQI. This highlights a clear connection between environmental conditions and
consumer purchasing patterns. The report aims to bridge environmental data analysis with business
decision-making by applying advanced time series modeling techniques.
The study is built around two primary objectives.
First, to develop accurate forecasting models for AQI values based on historical PM2.5 data. Such
forecasts are valuable not only for public health preparedness but also for businesses that are
sensitive to environmental fluctuations.
Second, to analyze how changes in air quality impact air purifier sales. Understanding this
relationship can help companies optimize inventory management, refine marketing strategies, and
improve demand forecasting.
The findings demonstrate that time series forecasting has practical applications beyond
environmental monitoring. By linking AQI trends with business outcomes, the report showcases
how data-driven insights can enable companies to respond effectively to dynamic market
conditions. Ultimately, it underlines the importance of incorporating environmental analytics into
business strategies to better meet consumer needs while supporting public health and well-being..
2 Modeling the Delhi AQI Time Series
2.1 Data Preprocessing and AQI Derivation
The AQI dataset originates from historical pollution data, including PM2.5 concentrations
recorded daily over a span of more than 10 years. However, for modeling purposes,
the analysis focuses on the subset from January 1, 2018, to March 31, 2023.
AQI Explanation
Primary Contributor Estimated Contribution
Vehicular Emissions 41%
Industrial Activities 18%
Construction and Road Dust 21.5%
Stubble Burning (Seasonal) 30–38%
Table 1: Pollution Sources and Their Estimated Contribution to Air Quality Degradation
4
Year Avg. PM2.5 (μg/m3) Avg. AQI Severe AQI Days Key Events
2018 209 338 24 GRAP implemented
2019 205 345 19 Supreme Court bans
petcoke
2020 94 185 6 COVID-19 lockdowns
2021 209 462 15 Post-Diwali AQI peak
2022 100 204 15 CAQM directives
2023 100.9 367 13 Low winter winds
Table 2: Annual summary of average PM2.5 concentration, average AQI, number of
severe AQI days, and key air quality events in Delhi from 2018 to 2023.
The AQI is computed from PM2.5 using the EPA’s breakpoint method, which transforms
raw PM2.5 concentrations into AQI scores on a scale from 0 to 500. This transformation
is non-linear and piecewise, based on pre-defined concentration intervals and their
corresponding AQI values. After calculating the AQI, the dataset undergoes a forward-fill
interpolation to handle missing values. Only the daily AQI values are retained, and the
index is converted into a daily frequency time series. By the end of this process, the AQI
time series is clean, continuous, and ready for analysis.
2.2 Stationarity Check
Before fitting any model, assessing the stationarity of the AQI time series is crucial, as
ARMA models require the data to have constant statistical properties over time. The
Augmented Dickey-Fuller (ADF) test is applied, which evaluates the null hypothesis that
the time series has a unit root, implying non-stationarity.
2.3 ADF Test
Augmented Dickey-Fuller (ADF) Test
The Augmented Dickey-Fuller (ADF) test is a statistical test used to determine whether
a given time series is stationary or has a unit root, implying non-stationarity. The ADF
test is an augmented version of the Dickey-Fuller test which includes lagged differences
of the time series to account for higher-order correlation.
5
p
The general form of the ADF regression equation is given by:
where:
i=1
• yt is the time series at time t,
• Δ denotes the first difference operator,
• α is a constant,
• βt is the coefficient on a time trend,
• γ is the coefficient used to test for stationarity,
• p is the number of lagged difference terms,
• εt is white noise.
Hypotheses:
• Null Hypothesis (H0): The time series has a unit root (i.e., it is non-stationary).
• Alternative Hypothesis (H1): The time series does not have a unit root (i.e., it
is stationary).
For performing this test, we use a confidence level of 0.95. This means that if the p-value
obtained from the test is less than 0.05, we reject the null hypothesis in favor of the
alternative, concluding that the series is stationary. The same method will be used in
the future to assess the stationarity of time series.
The result shows a test statistic of approximately -4.35 and a p-value around 0.0004.
Since the p-value is far below the threshold of 0.05, the null hypothesis is rejected, and
the series is considered stationary. This means the AQI values can be modeled without
differencing.
Metric Value
ADF Statistic -4.347
p-value 0.00037
Critical Value (1%) -3.434
Critical Value (5%) -2.863
Critical Value (10%) -2.568
Table 3: Results of the Augmented Dickey-Fuller test for stationarity on the daily AQI
time series.
6
2.4 ARMA Model Fitting on Raw AQI
The ARMA (AutoRegressive Moving Average) model is a widely used forecasting method
in time series analysis that combines two components: the autoregressive (AR) part and
the moving average (MA) part. It is typically used to model stationary time series data.
Model Parameters:
• μ: Constant term (mean of the time series).
• ϕi: AR coefficients for lagged values of the time series.
• θj: MA coefficients for lagged error terms.
• εt: White noise (error term) at time t.
ARMA Model Components:
• AR (AutoRegressive) part: The current value of the time series is explained by
a linear combination of its past values. The parameter p denotes the number of lag
observations included in the model.
• MA (Moving Average) part: The current value is also influenced by the past
forecast errors, represented by q lags of the forecast errors.
Autocorrelation and partial autocorrelation plots are used to visually inspect the data’s
memory structure.
However, to select the optimal ARMA(p, q) configuration more systematically, a grid
search over all combinations of p and q in the range [0, 5] is performed. Each model
is evaluated based on the Akaike Information Criterion (AIC), a measure that balances
model fit and complexity.
7
The best model identified by AIC is ARMA(3, 5), with an AIC score of approximately
17,701. This model is fitted on the training period from 2018 to 2022, and then used
to forecast AQI for the entire year of 2023. The model also provides 0.95 confidence
intervals for each predicted value, allowing us to assess the uncertainty of the forecast.
The root mean square error (RMSE) of the ARMA(3, 5) forecast against the actual AQI
values of 2023 is calculated as 40.9. Although predictions generally follow the trend of
actual values, error and residual plots reveal that the model struggles to fully account for
seasonal effects and long-term trends.
2.5 STL Decomposition of AQI Series
To improve forecasting performance, the AQI series is decomposed into three components
using STL (Seasonal-Trend decomposition using Loess): trend (Tt), seasonal (St), and
8
residual (Rt). It uses locally weighted regression (LOESS) to estimate the trend and
seasonal components of the series. The STL (Seasonal-Trend decomposition using Loess)
method is an additive decomposition technique that splits a time series yt into three
distinct components:
• Tt: Trend component
• St: Seasonal component
• Rt: Remainder (residual or irregular) component
Main Decomposition Formula:
yt = Tt + St + Rt (3)
Detrending Formula:
Deseasonalizing Formula:
Residual Calculation:
yt − Tt = St + Rt (4)
yt − St = Tt + Rt (5)
Rt = yt − Tt − St (6)
In Seasonal-Trend decomposition using Loess (STL), the Loess smoothing technique
plays a central role as a non-parametric method used to smooth the components
of a time series. Each component is estimated using iterative Loess smoothing, which
fits local weighted regressions to subsets of the data. The Loess smoother used in STL
assigns weights wij to observations based on their distance from the target point xi, using
a kernel function K and a bandwidth parameter h, as in:
This allows STL to flexibly adapt to local variations in the trend and seasonality
without assuming a fixed parametric model.
This residual series is then subjected to another ADF test, which confirms that it
is stationary (test statistic -10.71, p-value < 0.0001). The residuals are then modeled
separately using an AR(2) model, as determined by further AIC-guided selection.
9
Figure 1: Time series decomposition
Figure 2: STL residuals (Pre-2023)
2.6 Forecast Reconstruction and Performance
The final forecast is reconstructed by summing the forecasts of the individual components:
Xˆ
t = Tˆt + Sˆt +Rˆt
10
Where:
• Tˆt : Trend component forecast, typically extrapolated from the last known trend
values.
• Sˆt : Seasonal component forecast, often obtained by repeating the last full seasonal
cycle.
• Rˆ t : Residual component forecast, obtained from the ARMA model fitted to the
residuals.
Predicted residuals from ARMA(3,3) Evaluation:
• New RMSE (2023): 24.19
• Improvement: 41% reduction from initial ARMA-only forecast
This shows that STL + ARMA on residuals captures seasonality and irregular fluctuations
better than direct ARMA modeling.
3 Modeling the Revenue Time Series
3.1 Data Cleaning and Structure
The revenue dataset contains daily total revenue figures from January 1, 2018, to March
31, 2023. Initial preprocessing involves replacing missing entries with zero values , then
interpolating based on time and applying a centered rolling median with a window size
of 5 to smooth out noise. This ensures continuity and reduces the effect of irregular
spikes or drops in the data. Once cleaned, the time series is set to a daily frequency, with
forward-filling used to handle any residual gaps. At this point, the revenue data is ready
for decomposition and modeling.
11
3.2 Stationarity Assessment
The revenue series undergoes the Augmented Dickey-Fuller test, returning a test statistic
of -4.37 and a p-value of 0.0003. As with the AQI data, this indicates the series is
stationary and can be modeled using ARMA without differencing.
3.3 STL Decomposition of Revenue Series
As done with AQI, the revenue series is decomposed using STL into its trend, seasonal,
and residual components. This decomposition reveals cyclical patterns in revenue-
possibly due to monthly, quarterly, or holiday-related effects-and highlights the
presence of long-term growth or decline in economic activity.
The residual component, isolated after removing trend and seasonality, is subjected
to modeling using ARMA.
Figure 3: Revenue residuals
First plot the ACF and PACF during the training phase.
12
Figure 4: ACF and PACF of residuals during the training phase
Based on the plots, the ACF appears to be exponentially decaying and the PACF
cuts off after lag 1, suggesting AR(1) as a possible initial guess. However, to determine
the optimal model more systematically, a grid search was conducted over p and q values
in the range [0, 5], selecting the model with the lowest AIC. The optimal residual model
is found to be ARMA(4, 5), with an AIC of approximately 38,955.
13
3.4 Forecasting Revenue with Residual Model
The residual ARMA(4, 5) model is trained on data up to 2022. Forecasts for 2023 are
generated, and then combined with trend and seasonal components to reconstruct the
complete revenue forecast for 2023.
The final forecast is reconstructed as:
Xˆ
t = Tˆt + Sˆt +Rˆt
Where:
• Tˆt : Trend component forecast, extrapolated from the last known trend values.
• Sˆt : Seasonal component forecast, repeating the last full seasonal cycle.
• Rˆ t : Residual component forecast, from the fitted ARMA(4, 5) model.
14
Confidence intervals at 0.95 are computed for the residual forecast and propagated
into the final revenue forecasting.
These bounds allow us to construct a reliable envelope of uncertainty for each day’s
forecast. A comparison of predicted revenue and actual revenue in 2023 shows improved
alignment, with most observed values falling within the confidence bounds.
Figure 5: Revenue Forecast for 2023 (STL + ARMA)
4 Final Synthesis: Strategic Insight from Forecasting
This section consolidates our key findings by integrating statistical modeling outcomes
with consumer behavior insights, providing a holistic understanding of how
Delhi’s deteriorating air quality translates into commercial opportunities and strategic
decisions for air purifier companies like Blue Star.
4.1 Key Insights and Contributions
• High Correlation: There is a high positive correlation between AQI levels and
Blue Star’s revenue, notably during the months of November to January. This
indicates that AQI acts as a leading indicator for sales, which can inform marketing
and inventory strategies.
15
• Seasonal Patterns: Revenue peaks are distinct during winter months when AQI
exceeds 300, enabling predictable demand cycles and supply chain optimization.
• Consumer Behavior: There is a sharp increase in purchases during pollution
spikes, with up to a 70% sales surge, highlighting health-driven consumption and
the market’s responsiveness to environmental changes.
• COVID-19 Impact: The year 2020 showed anomalies due to lockdowns, resulting
in the lowest AQI and revenue. This validates the model’s sensitivity to external
shocks.
• Post-2020 Growth: There has been a sharp rebound in AQI and sales from
2021 to 2023, with record values in 2023, indicating rising health awareness and
increasing market penetration.
• Modeling Performance: The use of STL + ARMA reduced AQI forecast RMSE
by 41%, and revenue forecasts effectively captured seasonality, demonstrating the
power of advanced time series techniques in real-world forecasting.
4.2 Broader Implications
• Environmental Analytics Meets Market Strategy: Our approach showcases
how classical statistical models (ADF, ARMA) combined with STL decomposition
can deliver both environmental insight and commercial foresight.
• Business Sensitivity to Climate Indicators: Firms in pollution-sensitive sectors
can benefit from predictive analytics by treating AQI as a macroeconomic
driver.
• Policy and Corporate Synergy: Government regulations like GRAP and CAQM
influence consumer behavior and therefore must be integrated into business forecasts.
16
We will now demonstrate how similar both curves are using a graphic.
Aspect Observation Implication
Correlation High positive correlation between
AQI levels and Blue
Star’s revenue (notably in
Nov–Jan)
AQI acts as a leading indicator
for sales-can inform marketing
and inventory strategies
Seasonality Distinct revenue peaks during
winter months when AQI exceeds
300
Predictable demand cycles enable
supply chain optimization
Consumer Behavior
Sharp increase in purchases
during pollution spikes (up to
70% sales surge)
Health-driven consumption
underlines the market’s responsiveness
to environmental
changes
COVID-19 Impact 2020 showed anomalies due to
lockdowns, with lowest AQI
and revenue
Validates the model’s sensitivity
to external shocks
Post-2020 Growth Sharp rebound in AQI and
sales in 2021–2023, reaching
record values in 2023
Indicates rising health awareness
and increasing market
penetration
Modeling Performance
STL + ARMA reduced AQI
forecast RMSE by 41%, revenue
forecast captured seasonality
effectively
Demonstrates power of advanced
time series techniques
in real-world forecasting
Table 4: Key Statistical and Behavioral Insights
17
Forecasting Output Business Application
Accurate AQI Prediction Public health alerts, policy intervention timing
Revenue Forecasting Inventory planning, workforce allocation, supply chain
readiness
Seasonal Decomposition Tailored promotional campaigns aligned with highdemand
months
Residual Modeling Accuracy
Risk-adjusted business planning using confidence intervals
Long-Term Trend Analysis Product development and pricing strategies in anticipation
of future pollution patterns
Table 5: Practical Business Applications of Our Forecasting Approach
5 Conclusion
This study demonstrates how classical time series models, when complemented with STL
decomposition, can be used effectively for modeling environmental and economic indicators.
The AQI data benefits substantially from decomposition and residual modelling,
showing a dramatic reduction in forecasting error. Revenue data, while more volatile and
complex, also sees improvement with the STL + ARMA approach, although not to the
same extent. Both models are validated with strong diagnostics, including ADF tests,
RMSE metrics, and confidence interval analysis. Forecasts from both series are not only
point predictions but are also wrapped in probabilistic bounds, allowing for transparent
uncertainty communication.
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from statsmodels.tsa.stattools import adfuller
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
from statsmodels.tsa.seasonal import seasonal_decompose, STL, MSTL
from statsmodels.tsa.arima.model import ARIMA
from statsmodels.tsa.holtwinters import ExponentialSmoothing
from sklearn.metrics import mean_squared_error
import warnings
warnings.filterwarnings("ignore")
import itertools
# Load your data
df_loaded = pd.read_csv("/content/delhi_aqi.csv")
# Ensure the date column is properly formatted as datetime
df_loaded['From Date'] = pd.to_datetime(df_loaded['From Date'])
df_loaded.set_index('From Date', inplace=True)
# Quick check of the data
print(f"Data shape: {df_loaded.shape}")
print(df_loaded.head())
# Ensure index is datetime type
if not isinstance(df_loaded.index, pd.DatetimeIndex):
print(f"Warning: Index is type '{type(df_loaded.index)}', attempting conversion.")
try:
df_loaded.index = pd.to_datetime(df_loaded.index, errors='coerce')
df_loaded.dropna(subset=[df_loaded.index.name], inplace=True) # Drop rows where index conversion failed
print("Index successfully converted to DatetimeIndex.")
except Exception as idx_err:
print(f"Error: Could not convert index to DatetimeIndex: {idx_err}")
exit()
Data shape: (4838, 12) PM2_5 PM10 NO2 SO2 CO_ug Ozone Temp \From Date 2010-01-01 0.0 0.0 32.591083 5.999417 0.0 29.307250 7.822417 2010-01-02 0.0 0.0 44.230333 6.785167 0.0 16.458833 5.964417 2010-01-03 0.0 0.0 32.089833 5.332417 0.0 17.234667 5.968833 2010-01-04 0.0 0.0 14.383588 3.837176 0.0 18.384656 6.878244 2010-01-05 0.0 0.0 36.045903 3.765139 0.0 22.128750 8.070278 RH WS BP WD RF From Date 2010-01-01 42.778417 0.188167 392.411500 44.862667 0.0 2010-01-02 53.555833 0.259833 393.107667 21.817833 0.0 2010-01-03 45.518500 0.396250 310.380583 27.094250 0.0 2010-01-04 52.399847 0.930458 426.012824 47.768779 0.0 2010-01-05 54.378472 1.009375 471.394236 36.517361 0.0
def
calculate_aqi_pm25
(
pm25
)
:
# AQI calculation function for PM2.5
breakpoints =
[
0
,
12
,
35.4
,
55.4
,
150.4
,
250.4
,
350.4
,
500.4
]
index_values =
[
0
,
50
,
100
,
150
,
200
,
300
,
400
,
500
]
try
:
pm25 =
float
(
pm25
)
except
(
ValueError
,
TypeError
):
return
np.nan
if
pd.isna
(
pm25
):
return
np.nan
if
pm25 <= breakpoints
[
0
]:
return
index_values
[
0
]
for
i
in
range
(
len
(
breakpoints
)
-
1
):
if
i ==
0
:
if
breakpoints
[
i
]
<= pm25 <= breakpoints
[
i +
1
]:
bp_low_idx
,
bp_high_idx = i
,
i +
1
break
else
:
if
breakpoints
[
i
]
< pm25 <= breakpoints
[
i +
1
]:
bp_low_idx
,
bp_high_idx = i
,
i +
1
break
else
:
if
pm25 > breakpoints
[
-1
]:
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 1/16
return
index_values
[
-1
]
# Cap at 500
else
:
return
np.nan
aqi_low
,
aqi_high = index_values
[
bp_low_idx
],
index_values
[
bp_high_idx
]
breakpoint_low
,
breakpoint_high = breakpoints
[
bp_low_idx
],
breakpoints
[
bp_high_idx
]
if
breakpoint_high == breakpoint_low
:
return
aqi_low
aqi =
((
aqi_high - aqi_low
)
/
(
breakpoint_high - breakpoint_low
))
*
(
pm25 - breakpoint_low
)
+ aqi_low
# Ensure AQI doesn't exceed max due to float issue
s if extrapolating
return
min
(
aqi
,
index_values
[
-1
])
if
pm25 > breakpoints
[
-1
]
else
aqi
# Process the data for time series analysis
df_processed = df_loaded
[[
'PM2_5'
]]
.copy
()
df_processed
[
'AQI (PM2.5)'
]
= df_processed
[
'PM2_5'
]
.apply
(
calculate_aqi_pm25
)
# Handle missing values
df_processed.fillna
(
method=
'ffill'
,
inplace=
True
)
df_processed.dropna
(
inplace=
True
)
df_processed = df_processed.sort_index
()
# Filter to more recent data (2018 onwards)
df_daily_final = df_processed.loc
[
'2018-01-01'
:]
.copy
()
print
(
f
"Daily data points (2018+):
{
len
(
df_daily_final
)}
"
)
if
len
(
df_daily_final
)
==
0
:
raise
ValueError
(
"No daily data found starting from 2018-01-01."
)
print
(
df_daily_final.head
())
# Set a proper frequency for the time series
if
df_daily_final.index.freq
is
None
:
df_daily_final = df_daily_final.asfreq
(
'D'
)
# Fill any gaps created by setting frequency
df_daily_final.fillna
(
method=
'ffill'
,
inplace=
True
)
print
(
f
"Time series frequency:
{
df_daily_final.index.freq
}
"
)
Daily data points (2018+): 1916 PM2_5 AQI (PM2.5)From Date 2018-01-01 263.389213 312.9892132018-01-02 255.326528 304.9265282018-01-03 197.158032 246.7580322018-01-04 226.977569 276.5775692018-01-05 221.421412 271.021412Time series frequency: <Day>
# Check stationarity using Augmented Dickey-Fuller
test
from
statsmodels.tsa.stattools
import
adfuller
def
test_stationarity
(
timeseries
,
title
=
''
)
:
# Calculate rolling statistics
rolling_mean = timeseries.rolling
(
window=
12
)
.mean
()
rolling_std = timeseries.rolling
(
window=
12
)
.std
()
# Plot rolling statistics
plt.figure
(
figsize=
(
14
,
6
))
plt.plot
(
timeseries
,
label=
'Original'
)
plt.plot
(
rolling_mean
,
label=
'Rolling Mean'
)
plt.plot
(
rolling_std
,
label=
'Rolling Std'
)
plt.legend
(
loc=
'best'
)
plt.title
(
f
'Rolling Mean & Standard Deviation -
{
title
}
'
)
plt.tight_layout
()
plt.show
()
# Perform Dickey-Fuller test
print
(
'Results of Dickey-Fuller Test:'
)
dftest = adfuller
(
timeseries.dropna
(),
autolag=
'AIC'
)
dfoutput = pd.Series
(
dftest
[
0
:
4
],
index=
[
'Test Statistic'
,
'p-value'
,
'#Lags Used'
,
'Number of Observations Used'
])
for
key
,
value
in
dftest
[
4
]
.items
():
dfoutput
[
'Critical Value (%s)'
%key
]
= value
print
(
dfoutput
)
if
dftest
[
1
]
<=
0.05
:
print
(
"Conclusion: The series is stationary"
)
else
:
print
(
"Conclusion: The series is not stationary"
)
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 2/16
Results of Dickey-Fuller Test:
Test Statistic -4.347028
p-value 0.000368
#Lags Used 11.000000
Number of Observations Used 1904.000000
Critical Value (1%) -3.433789
Critical Value (5%) -2.863059
Critical Value (10%) -2.567579
dtype: float64
Conclusion: The series is stationary
# Test stationarity on the AQI series
test_stationarity
(
df_daily_final
[
'AQI (PM2.5)'
],
title=
'AQI (PM2.5)'
)
# Select the AQI column for decomposition
ts = df_daily_final
[
'AQI (PM2.5)'
]
# CRITICAL FIX: Set the correct period parameter
# For daily data with annual seasonality, use peri
od=365
# This is the most important fix for your flat res
idual issue
period =
365
# Specify annual seasonality for daily data
len
(
ts
)
1916
Directly Applying ARMA on serieskeyboard_arrow_down
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 3/16
# Plot ACF and PACF to determine AR and MA orders
from
statsmodels.graphics.tsaplots
import
plot_acf
,
plot_pacf
import
matplotlib.pyplot
as
plt
# Function to plot ACF and PACF
def
plot_acf_pacf
(
series
,
lags
=
40
,
figsize
=
(
12
,
8
)
)
:
fig
,
axes = plt.subplots
(
2
,
1
,
figsize=figsize
)
# Plot ACF
plot_acf
(
series
,
ax=axes
[
0
],
lags=lags
)
axes
[
0
]
.set_title
(
'Autocorrelation Function (ACF)'
)
# Plot PACF
plot_pacf
(
series
,
ax=axes
[
1
],
lags=lags
)
axes
[
1
]
.set_title
(
'Partial Autocorrelation Function (PACF)'
)
plt.tight_layout
()
plt.show
()
# Plot ACF and PACF for the AQI series
plot_acf_pacf
(
ts.loc
[:
'2022-12-31'
],
lags=
50
)
# Grid search for optimal ARMA parameters
import
itertools
from
statsmodels.tsa.arima.model
import
ARIMA
import
pandas
as
pd
import
numpy
as
np
def
grid_search_arma
(
series
,
p_range
,
q_range
)
:
best_aic =
float
(
'inf'
)
best_bic =
float
(
'inf'
)
best_params =
None
best_model =
None
results =
[]
for
p
,
q
in
itertools.product
(
p_range
,
q_range
):
try
:
model = ARIMA
(
series
,
order=
(
p
,
0
,
q
))
# d=0 for ARMA model
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 4/16
Best ARMA Parameters (based on AIC): ARMA(3, 5)
Best AIC: 17701.103165647943
model_fit = model.fit
()
results.append
({
'p'
:
p
,
'q'
:
q
,
'aic'
:
model_fit.aic
,
'bic'
:
model_fit.bic
})
if
model_fit.aic < best_aic
:
best_aic = model_fit.aic
best_params =
(
p
,
q
)
best_model = model_fit
except
:
continue
results_df = pd.DataFrame
(
results
)
print
(
f
"Best ARMA Parameters (based on AIC): ARMA(
{
best_params
[
0
]}
,
{
best_params
[
1
]}
)"
)
print
(
f
"Best AIC:
{
best_aic
}
"
)
return
best_model
,
results_df
# Define ranges for p and q
p_range =
range
(
0
,
6
)
# AR order
q_range =
range
(
0
,
6
)
# MA order
# Perform grid search
best_model
,
results_df = grid_search_arma
(
ts.loc
[:
'2022-12-31'
],
p_range
,
q_range
)
# Visualize the AIC results
plt.figure
(
figsize=
(
10
,
6
))
pivot_table = results_df.pivot
(
index=
'p'
,
columns=
'q'
,
values=
'aic'
)
sns.heatmap
(
pivot_table
,
annot=
True
,
fmt=
'.1f'
,
cmap=
'viridis'
)
plt.title
(
'AIC Values for Different ARMA(p,q) Models'
)
plt.xlabel
(
'q (MA Order)'
)
plt.ylabel
(
'p (AR Order)'
)
plt.tight_layout
()
plt.show
()
train = df_daily_final.loc
[:
'2022-12-31'
][
'AQI (PM2.5)'
]
test = df_daily_final.loc
[
'2023-01-01'
:][
'AQI (PM2.5)'
]
model = ARIMA
(
train
,
order=
(
3
,
0
,
5
))
fitted_model = model.fit
()
forecast_result = fitted_model.get_forecast
(
steps=
len
(
test
))
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 5/16
forecast = forecast_result.predicted_mean
conf_int = forecast_result.conf_int
(
alpha=
0.05
)
from
sklearn.metrics
import
mean_squared_error
import
numpy
as
np
# Ensure forecast index aligns with test
forecast.index = test.index
# Compute RMSE manually by taking the square root
of MSE
rmse = np.sqrt
(
mean_squared_error
(
test
,
forecast
))
print
(
f
"RMSE on 2023 test data:
{
rmse
:.2f
}
"
)
RMSE on 2023 test data: 40.90
plt.figure
(
figsize=
(
14
,
6
))
plt.plot
(
train
,
label=
'Training Data'
)
plt.plot
(
test
,
label=
'Actual AQI 2023'
,
color=
'green'
)
plt.plot
(
forecast
,
label=
'Forecasted AQI 2023'
,
color=
'red'
)
plt.fill_between
(
conf_int.index
,
conf_int.iloc
[:,
0
],
conf_int.iloc
[:,
1
],
color=
'pink'
,
alpha=
0.3
,
label=
'95% Confidence Interval'
)
plt.title
(
'ARMA(3,5) Forecast of AQI (PM2.5) for 2023'
)
plt.xlabel
(
'Date'
)
plt.ylabel
(
'AQI (PM2.5)'
)
plt.legend
()
plt.tight_layout
()
plt.show
()
Forecast Lower CI (95%) Upper CI (95%) Actual
2023-01-01 185.952020 125.825144 246.078896 189.758405
2023-01-02 204.148409 125.255254 283.041564 264.970937
2023-01-03 205.497102 121.020310 289.973894 276.021000
2023-01-04 203.494166 115.543906 291.444426 219.746562
2023-01-05 203.779797 113.262511 294.297083 246.976677
Next steps:
forecast_df = pd.DataFrame
({
'Forecast'
:
forecast
,
'Lower CI (95%)'
:
conf_int.iloc
[:,
0
],
'Upper CI (95%)'
:
conf_int.iloc
[:,
1
],
'Actual'
:
test
})
forecast_df.head
()
Generate code with
forecast_df
View recommended plotstoggle_off
New interactive sheet
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 6/16
Decomposing the series and then applying ARMAkeyboard_arrow_down
import
pandas
as
pd
import
numpy
as
np
from
statsmodels.tsa.seasonal
import
STL
stl = STL
(
df_daily_final
[
'AQI (PM2.5)'
],
period=
365
,
robust=
True
)
result = stl.fit
()
# Extract components
trend = result.trend
seasonal = result.seasonal
residual = result.resid
# STL Decomposition (Seasonal-Trend decomposition
using LOESS)
# This is often more robust than the traditional s
easonal_decompose
stl = STL
(
ts
,
period=period
,
# Same annual period
seasonal=
13
,
# Controls smoothness of seasonal component
trend=
None
,
# Automatically determine trend window
robust=
True
# Use robust estimation (less sensitive to outlier
s)
)
result = stl.fit
()
# Plot the STL decomposition
fig
,
axes = plt.subplots
(
4
,
1
,
figsize=
(
14
,
12
),
sharex=
True
)
# Original data
axes
[
0
]
.plot
(
ts
,
label=
'Original'
)
axes
[
0
]
.legend
(
loc=
'upper left'
)
axes
[
0
]
.set_title
(
'Original Time Series'
)
# Trend component
axes
[
1
]
.plot
(
result.trend
,
label=
'Trend'
,
color=
'red'
)
axes
[
1
]
.legend
(
loc=
'upper left'
)
axes
[
1
]
.set_title
(
'Trend Component (STL)'
)
# Seasonal component
axes
[
2
]
.plot
(
result.seasonal
,
label=
'Seasonality'
,
color=
'green'
)
axes
[
2
]
.legend
(
loc=
'upper left'
)
axes
[
2
]
.set_title
(
'Seasonal Component (STL)'
)
# Residual component
axes
[
3
]
.plot
(
result.resid
,
label=
'Residuals'
,
color=
'purple'
)
axes
[
3
]
.axhline
(
y=
0
,
color=
'black'
,
linestyle=
'-'
,
alpha=
0.3
)
axes
[
3
]
.legend
(
loc=
'upper left'
)
axes
[
3
]
.set_title
(
'Residual Component (STL)'
)
plt.tight_layout
()
plt.show
()
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 7/16
from
statsmodels.tsa.stattools
import
adfuller
import
matplotlib.pyplot
as
plt
# STL residuals (from earlier STL decomposition)
residual_train = residual
[:
'2022-12-31'
]
# Plot residuals
plt.figure
(
figsize=
(
12
,
4
))
plt.plot
(
residual_train
,
label=
'STL Residuals'
)
plt.title
(
'STL Residuals (Pre-2023)'
)
plt.legend
()
plt.tight_layout
()
plt.show
()
# ADF test
adf_result = adfuller
(
residual_train.dropna
())
print
(
f
"ADF Test Statistic:
{
adf_result
[
0
]
:.4f
}
"
)
print
(
f
"p-value:
{
adf_result
[
1
]
:.4f
}
"
)
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 8/16
ADF Test Statistic: -10.6398
p-value: 0.0000
Residuals are stationary.
if
adf_result
[
1
]
<=
0.05
:
print
(
" Residuals are stationary."
)
stationary_residual = residual_train
else
:
print
(
" Residuals are NOT stationary. Differencing appli
ed."
)
stationary_residual = residual_train.diff
()
.dropna
()
from
statsmodels.graphics.tsaplots
import
plot_acf
,
plot_pacf
plt.figure
(
figsize=
(
12
,
8
))
plt.subplot
(
2
,
1
,
1
)
plot_acf
(
stationary_residual
,
ax=plt.gca
(),
lags=
40
)
plt.title
(
"ACF of Residuals"
)
plt.subplot
(
2
,
1
,
2
)
plot_pacf
(
stationary_residual
,
ax=plt.gca
(),
lags=
40
)
plt.title
(
"PACF of Residuals"
)
plt.tight_layout
()
plt.show
()
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 9/16
from
statsmodels.tsa.arima.model
import
ARIMA
from
sklearn.metrics
import
mean_squared_error
import
numpy
as
np
import
matplotlib.pyplot
as
plt
def
grid_search_arma
(
series
,
p_range
,
q_range
,
d
=
0
)
:
"""Performs grid search for ARIMA(p, d, q) and ret
urns best model based on AIC."""
best_aic =
float
(
'inf'
)
best_params =
None
best_model_fit =
None
results =
[]
# Ensure the series has no NaNs which can cause is
sues
series = series.dropna
()
if
series.empty
:
print
(
"Warning: Series is empty after dropping NaNs. Can
not perform grid search."
)
return
None
,
None
,
pd.DataFrame
(
results
)
print
(
f
"\nStarting grid search for ARIMA(p,
{
d
}
, q) orders..."
)
print
(
f
"p range:
{
list
(
p_range
)}
, q range:
{
list
(
q_range
)}
"
)
for
p
,
q
in
itertools.product
(
p_range
,
q_range
):
# Skip ARMA(0,0) model as it's usually trivial (wh
ite noise)
if
p ==
0
and
q ==
0
:
continue
order =
(
p
,
d
,
q
)
try
:
model = ARIMA
(
series
,
order=order
)
model_fit = model.fit
()
current_aic = model_fit.aic
results.append
({
'p'
:
p
,
'd'
:
d
,
'q'
:
q
,
'aic'
:
current_aic
})
if
current_aic < best_aic
:
best_aic = current_aic
best_params =
{
'p'
:
p
,
'd'
:
d
,
'q'
:
q
}
best_model_fit = model_fit
# Store the fitted model object
except
Exception
as
e
:
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 10/16
# print(f"Could not fit ARIMA{order}: {e}") # Opti
onal: uncomment for detailed errors
results.append
({
'p'
:
p
,
'd'
:
d
,
'q'
:
q
,
'aic'
:
np.nan
})
# Log failure
continue
results_df = pd.DataFrame
(
results
)
if
best_params
:
print
(
"-"
*
30
)
print
(
f
"Grid Search Complete."
)
print
(
f
"Best Parameters (based on AIC): ARIMA(
{
best_params
[
'p'
]}
,
{
best_params
[
'd'
]}
,
{
best_params
[
'q'
]}
)"
)
print
(
f
"Best AIC:
{
best_aic
:.2f
}
"
)
print
(
"-"
*
30
)
else
:
print
(
"-"
*
30
)
print
(
"Grid Search Warning: Could not find a suitable mo
del."
)
print
(
"-"
*
30
)
return
best_model_fit
,
best_params
,
results_df
residual_train = residual
[:
'2022-12-31'
]
.dropna
()
# Ensure no leading/trailing NaNs
residual_test = residual
[
'2023-01-01'
:]
n_forecast =
len
(
residual_test
)
# Define parameter ranges for the grid search
# Adjust ranges as needed, smaller ranges run fast
er
p_param_range =
range
(
0
,
4
)
# Example range for AR order (p)
q_param_range =
range
(
0
,
4
)
# Example range for MA order (q)
# Perform grid search on the training residuals to
find the best ARMA(p,q) order (d=0)
best_residual_model_fitted
,
best_arma_params
,
_ = grid_search_arma
(
residual_train
,
p_range=p_param_range
,
q_range=q_param_range
,
d=
0
# d=0 because we want ARMA on potentially stationa
ry residuals
)
# Check if the grid search was successful
if
not
best_residual_model_fitted
or
not
best_arma_params
:
print
(
"Error: Grid search failed to find the best model.
Exiting."
)
# Optionally, fall back to a default order or rais
e an error
# best_arma_params = {'p': 2, 'd': 0, 'q': 0} # Ex
ample fallback
# model = ARIMA(residual_train, order=(best_arma_p
arams['p'], best_arma_params['d'], best_arma_param
s['q']))
# best_residual_model_fitted = model.fit()
# if not best_residual_model_fitted:
raise
ValueError
(
"Could not fit a fallback model after grid search
failed."
)
# Use the best model found by grid search to forec
ast residuals
print
(
f
"\nForecasting residuals using best model: ARIMA
{
tuple
(
best_arma_params.values
())}
..."
)
forecast_obj = best_residual_model_fitted.get_fore
cast
(
steps=n_forecast
)
resid_forecast = forecast_obj.predicted_mean
# Optional: Get confidence intervals if needed lat
er
# resid_conf_int = forecast_obj.conf_int(alpha=0.0
5)
# Reconstruct full forecast
# Make sure trend/seasonal components cover the fo
recast period
trend_forecast = trend.reindex
(
residual_test.index
)
# Use reindex for safety
seasonal_forecast = seasonal.reindex
(
residual_test.index
)
# Use reindex for safety
# Ensure residual forecast index aligns with test
period before adding
resid_forecast.index = residual_test.index
final_forecast = trend_forecast + seasonal_forecas
t + resid_forecast
# Align final forecast index with actual data inde
x for evaluation
actual = ts
[
'2023-01-01'
:]
final_forecast = final_forecast.reindex
(
actual.index
)
# Align index
# Evaluate (handle potential NaNs from reindexing/
missing data)
valid_idx = actual.notna
()
& final_forecast.notna
()
actual_eval = actual
[
valid_idx
]
final_forecast_eval = final_forecast
[
valid_idx
]
if
len
(
actual_eval
)
>
0
:
rmse = np.sqrt
(
mean_squared_error
(
actual_eval
,
final_forecast_eval
))
print
(
f
"\nRMSE with STL + Best ARMA
{
tuple
(
best_arma_params.values
())}
residual forecast:
{
rmse
:.2f
}
"
)
rmse_str =
f
"
{
rmse
:.2f
}
"
else
:
print
(
"\nWarning: No overlapping valid data for RMSE cal
culation."
)
rmse_str =
"N/A"
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 11/16
Starting grid search for ARIMA(p, 0, q) orders...
p range: [0, 1, 2, 3], q range: [0, 1, 2, 3]
------------------------------
Grid Search Complete.
Best Parameters (based on AIC): ARIMA(3, 0, 3)
Best AIC: 17704.24
------------------------------
Forecasting residuals using best model: ARIMA(3, 0, 3)...
RMSE with STL + Best ARMA(3, 0, 3) residual forecast: 24.19
Generating plot...
# Plot
print
(
"Generating plot..."
)
plt.figure
(
figsize=
(
14
,
7
))
sns.set_theme
(
style=
"whitegrid"
)
# Optional: apply seaborn style
plt.plot
(
ts
[:
'2022-12-31'
],
label=
"Training Data (Actual)"
,
color=
'grey'
,
alpha=
0.7
)
plt.plot
(
actual
,
label=
"Test Data (Actual)"
,
color=
'blue'
,
linewidth=
1.5
)
plt.plot
(
final_forecast
,
label=
f
"Forecast (STL + ARMA
{
tuple
(
best_arma_params.values
())}
)"
,
color=
'red'
,
linestyle=
'--'
)
# Optional: Add confidence intervals to plot if ca
lculated
# plt.fill_between(final_forecast.index, final_low
er_ci, final_upper_ci, color='red', alpha=0.2, lab
el='95% CI')
plt.title
(
f
"Forecast using STL + Best Residual ARMA Model (RM
SE:
{
rmse_str
}
)"
,
fontsize=
14
)
plt.xlabel
(
"Date"
,
fontsize=
12
)
plt.ylabel
(
ts.name
if
ts.name
else
"Value"
,
fontsize=
12
)
# Use series name if available
plt.legend
()
plt.tight_layout
()
plt.show
()
sns.reset_defaults
()
# Optional: reset seaborn style
Revenue of AIR Purifierskeyboard_arrow_down
import
pandas
as
pd
import
numpy
as
np
import
matplotlib.pyplot
as
plt
from
statsmodels.tsa.seasonal
import
STL
from
statsmodels.graphics.tsaplots
import
plot_acf
,
plot_pacf
# 1. Load and preprocess revenue data
df = pd.read_csv
(
'/content/delhi_revenue_data_2018_2023.csv'
)
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 12/16
df
[
'Date'
]
= pd.to_datetime
(
df
[
'Date'
])
df.set_index
(
'Date'
,
inplace=
True
)
df = df.sort_index
()
df = df.loc
[
'2018-01-01'
:]
df = df.asfreq
(
'D'
)
df
[
'Revenue'
]
= df
[
'Revenue'
]
.replace
(
0
,
np.nan
)
.interpolate
(
method=
'time'
)
.fillna
(
method=
'ffill'
)
# 2. STL decomposition (annual seasonality)
ts = df
[
'Revenue'
]
stl = STL
(
ts
,
period=
365
,
robust=
True
)
result = stl.fit
()
trend = result.trend
seasonal = result.seasonal
residual = result.resid
# 3. Use only residuals up to end of 2022 for ARMA
model selection
residual_train = residual
[:
'2022-12-31'
]
# 4. Plot residuals
plt.figure
(
figsize=
(
12
,
4
))
plt.plot
(
residual_train
,
label=
'STL Residuals'
)
plt.title
(
'STL Residuals (Pre-2023)'
)
plt.legend
()
plt.tight_layout
()
plt.show
()
# 5. Plot ACF and PACF for the residuals
plt.figure
(
figsize=
(
12
,
8
))
plt.subplot
(
2
,
1
,
1
)
plot_acf
(
residual_train.dropna
(),
ax=plt.gca
(),
lags=
40
)
plt.title
(
"ACF of Residuals"
)
plt.subplot
(
2
,
1
,
2
)
plot_pacf
(
residual_train.dropna
(),
ax=plt.gca
(),
lags=
40
)
plt.title
(
"PACF of Residuals"
)
plt.tight_layout
()
plt.show
()
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 13/16
import
itertools
import
pandas
as
pd
import
numpy
as
np
import
seaborn
as
sns
import
matplotlib.pyplot
as
plt
from
statsmodels.tsa.arima.model
import
ARIMA
# 3. Split residuals into train/test
residual_train = residual.loc
[:
'2022-12-31'
]
residual_test = residual.loc
[
'2023-01-01'
:]
# 4. Grid search for ARMA(p, q) on residuals
def
grid_search_arma
(
series
,
p_range
,
q_range
)
:
best_aic =
float
(
'inf'
)
best_params =
None
best_model =
None
results =
[]
for
p
,
q
in
itertools.product
(
p_range
,
q_range
):
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 14/16
Best ARMA Parameters (AIC): ARMA(4, 5)
Best AIC: 38955.14258873749
try
:
model = ARIMA
(
series
,
order=
(
p
,
0
,
q
))
model_fit = model.fit
()
results.append
({
'p'
:
p
,
'q'
:
q
,
'aic'
:
model_fit.aic
})
if
model_fit.aic < best_aic
:
best_aic = model_fit.aic
best_params =
(
p
,
q
)
best_model = model_fit
except
:
continue
results_df = pd.DataFrame
(
results
)
print
(
f
"Best ARMA Parameters (AIC): ARMA
{
best_params
}
"
)
print
(
f
"Best AIC:
{
best_aic
}
"
)
return
best_model
,
results_df
p_range =
range
(
0
,
6
)
q_range =
range
(
0
,
6
)
best_model
,
results_df = grid_search_arma
(
residual_train.dropna
(),
p_range
,
q_range
)
# 5. Visualize AIC results
plt.figure
(
figsize=
(
10
,
6
))
pivot_table = results_df.pivot
(
index=
'p'
,
columns=
'q'
,
values=
'aic'
)
sns.heatmap
(
pivot_table
,
annot=
True
,
fmt=
'.1f'
,
cmap=
'viridis'
)
plt.title
(
'AIC Values for Different ARMA(p,q) Models (Residu
als)'
)
plt.xlabel
(
'q (MA Order)'
)
plt.ylabel
(
'p (AR Order)'
)
plt.tight_layout
()
plt.show
()
# 6. Forecast residuals for 2023 using best ARMA m
odel
n_forecast =
len
(
residual_test
)
forecast_res = best_model.get_forecast
(
steps=n_forecast
)
resid_pred = forecast_res.predicted_mean
resid_ci = forecast_res.conf_int
(
alpha=
0.05
)
# Align indices
resid_pred.index = residual_test.index
resid_ci.index = residual_test.index
# 7. Plot residual forecast vs actual residuals
plt.figure
(
figsize=
(
14
,
5
))
plt.plot
(
residual_test
,
label=
'Actual Residuals (2023)'
,
color=
'blue'
)
plt.plot
(
resid_pred
,
label=
'Forecasted Residuals (2023)'
,
color=
'red'
)
plt.fill_between
(
resid_ci.index
,
resid_ci.iloc
[:,
0
],
resid_ci.iloc
[:,
1
],
color=
'gray'
,
alpha=
0.3
plt.title
(
'Residuals Forecast vs Actual (2023)'
)
plt.xlabel
(
'Date'
)
plt.ylabel
(
'Residual'
)
plt.legend
()
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 15/16
plt.tight_layout
()
plt.show
()
# 8. Reconstruct full forecast
trend_forecast = trend
[
'2023-01-01'
:]
seasonal_forecast = seasonal
[
'2023-01-01'
:]
final_forecast = trend_forecast + seasonal_forecas
t + resid_pred
lower = trend_forecast + seasonal_forecast + resid
_ci.iloc
[:,
0
]
upper = trend_forecast + seasonal_forecast + resid
_ci.iloc
[:,
1
]
# 9. Plot the final forecast with confidence inter
vals
actual = df
[
'Revenue'
][
'2023-01-01'
:]
plt.figure
(
figsize=
(
14
,
6
))
plt.plot
(
df
[
'Revenue'
],
label=
'Historical Revenue (2018-2022)'
,
color=
'blue'
)
plt.plot
(
actual
,
label=
'Actual Revenue (2023)'
,
color=
'green'
,
linewidth=
2
)
plt.plot
(
final_forecast
,
label=
'Forecasted Revenue (STL + ARMA)'
,
color=
'red'
)
plt.fill_between
(
final_forecast.index
,
lower
,
upper
,
color=
'pink'
,
alpha=
0.3
)
plt.title
(
'Revenue Forecast for 2023 (STL + ARMA)'
)
plt.xlabel
(
'Date'
)
plt.ylabel
(
'Revenue'
)
plt.legend
()
plt.tight_layout
()
plt.show
()
20/04/2025, 09:35 Final_RTSM.ipynb - Colab
https://colab.research.google.com/drive/1kcBZ3IeFoh_pgqx0nhdxz30I_CanKCSZ?usp=sharing#scrollTo=y2La7Y4S379i&printMode=true 16/16
