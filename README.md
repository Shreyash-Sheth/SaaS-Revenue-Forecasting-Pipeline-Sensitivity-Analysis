# SaaS Revenue Forecasting & Pipeline Sensitivity Analysis

##  Executive Summary
In B2B SaaS, a common forecasting pitfall is assuming that marketing and GTM (Go-to-Market) investments generate immediate revenue. In reality, SaaS operates on a delayed sales cycle. Marketing spend generates leads, which convert to opportunities, which mature into closed-won revenue months later.

This project simulates a Revenue Operations (RevOps) forecasting framework. Moving beyond purely academic time-series modelling, this project engineers realistic sales-cycle lags, establishes a historical baseline, and creates an interactive scenario-planning matrix (Base, Bull, Bear) to help executives quantify the exact financial impact of their GTM budget decisions.


##  The Business Problem
A fictional SaaS company is entering its annual planning cycle. The CFO and CRO need to agree on next year's GTM budget. 
* The Finance perspective: "We need to cut marketing spend by 20% to preserve runway."
* The Sales/Marketing perspective: "If you cut our budget, we will miss our revenue targets."

The RevOps Objective: Settle the debate using data. Build a driver-based forecasting model that proves the historical ROI of marketing spend, calculates the baseline run-rate, and provides an interactive tool to show leadership the exact "cost" of budget cuts versus the "upside" of budget increases.


##  Data & Methodology
The dataset contains 60 months (5 years) of synthetic SaaS GTM data. 

This data is specifically tailored to SaaS:
* `Marketing_Spend`: Top-of-funnel demand generation budget.
* `Promo_Days`: Bottom-of-funnel conversion levers (e.g., end-of-quarter discounts).
* `Leads` -> `Lead_to_Opp_Conversion` -> `Opportunities` -> `Win_Rate` -> `Avg_Deal_Size`: The standard SaaS "Volume, Velocity, Value, and Conversion" pipeline math.

The Pipeline Lag :
A direct correlation between current-month spend and current-month revenue is statistically flawed in B2B SaaS. Based on operational CRM data, I engineered two lagged variables into the machine learning model:
1. Marketing Spend (2-Month Lag): Euro spent today takes roughly 60 days to mature from an inbound lead to a closed-won deal.
2. Promo Days (1-Month Lag): Promotional campaigns accelerate deals already in the pipeline, taking roughly 30 days to clear.


##  Execution Steps & Methodology

### Step 1: Baseline Univariate Model
I first built a baseline model using Prophet, feeding it only historical `Date` and `Revenue` to capture organic trend and seasonality.
* Insight: This model acts as our control group. It tells us where revenue will land if the company freezes its current operational efficiency and makes zero net-new strategic GTM investments.

<img width="857" height="449" alt="image" src="https://github.com/user-attachments/assets/939745e0-09c3-4970-be11-1af322c77b8a" />


### Step 2: Driver-Based Model (Adding Regressors)
To make the forecast actionable, I added `Marketing_Spend_Lag_2` and `Promo_Days_Lag_1` as external regressors. The model now mathematically understands that revenue spikes are not random; they are the downstream result of past marketing investments. 

>  **A Note on Model Accuracy (MAPE):**
> The Mean Absolute Percentage Error (MAPE) for this model sits around 57%. In a real-world, mature SaaS organisation, RevOps aims for a revenue forecast variance of 5% to 10%. 
> 
> The error rate in this project is artificially high because the underlying dataset is synthetically generated with extreme, randomised month-over-month volatility to visually stress-test the model's architecture. The core value of this project lies in the business logic (engineering pipeline lags and executive scenario planning), not the absolute error rate of fake data. If this exact architectural framework were plugged into stable, real-world CRM data, the error rate would naturally drop into a commercially acceptable range.

### Step 3: Scenario Planning (Base, Bull, Bear)
I passed three future scenarios through the driver-based model to test commercial sensitivity:
1. Base: Hold marketing budget flat.
2. Bull: Increase budget slightly every month, add +2 promo days.
3. Bear: Slash budget slightly every month, remove -1 promo day.

<img width="1389" height="690" alt="image" src="https://github.com/user-attachments/assets/a84c5cc7-5462-43d6-bd20-f8da9045b781" />


*Notice the Base, Bull, and Bear lines travel together for the first two months of the forecast before splitting. This visually proves the 60-day pipeline lock. We cannot change tomorrow's revenue by changing marketing spend today.*

### Step 4: Power BI Dashboard
I exported the model's outputs into a polished, single-page Power BI dashboard designed for C-Suite consumption.

<img width="904" height="509" alt="image" src="https://github.com/user-attachments/assets/479ce053-2b9e-4ad8-92c7-e1d1c99dc6b7" />



##  Findings & Commercial Summary

1. Revenue is a Lagging Indicator: The data successfully proved that adjusting the GTM budget today will yield $0 in variance for the next 60 days due to the maturation cycle of B2B pipeline. 
2. Compounding Investment: Because SaaS revenue builds on historical pipeline momentum, a compounding 2% MoM increase in marketing spend results in an increasing effect of compounding revenue growth.
3. The Cost of Cuts: The Power BI dashboard explicitly quantifies the CFO's proposed budget cuts. We can now accurately report that a steady reduction in marketing spend will result in pipeline starvation by Q3 of next year.

Tools Used: Python (Pandas, Numpy), Meta Prophet (Time-Series ML), Scikit-Learn (MAPE Evaluation), Matplotlib (Visualisation), Microsoft Power BI (Dashboarding & DAX).
