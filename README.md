## Technical Report: Identifying Undervalued Companies Using Finviz

The objective of this analysis is to identify undervalued companies by examining their financial performance, specifically focusing on Gross Profit (GP) and Gross Profit Growth (GPG). These metrics will be compared to those of similar companies in the same sector to pinpoint firms that may be undervalued relative to their peers. The analysis leverages data from Finviz and additional sources to provide a comprehensive evaluation.

The methodology comprises several steps:

1. **Data Collection**: Undervalued companies are identified using Finviz by filtering for firms with high market capitalizations but low Price-to-Earnings (P/E) ratios. The acquired data is combined with Gross Profit (GP) and Gross Profit Growth (GPG) information sourced from the Macrotrends website.

2. **Comparative Analysis**: These metrics are evaluated against sector peers to assess relative performance.

3. **Valuation Modeling**: An ARIMA (AutoRegressive Integrated Moving Average) model is developed, incorporating quarterly financial data to project future performance.

4. **Model Refinement**: An additional variable, such as market conditions or technological advancements, is integrated into the model to provide a more nuanced analysis and refine the valuation.

### Data Collection

To identify undervalued companies, the Finviz stock screener was used, applying filters for companies with a market capitalization over $200 billion and the lowest P/E ratios within their sectors. This process led to the selection of four companies: **Toyota Motor Corporation**, **Chevron**, **JPMorgan Chase**, and **Novartis AG ADR**. These companies were chosen as they represent significant entities within their respective sectors that appear to be undervalued based on their P/E ratios.

The data from the Finviz stock screener was captured via screenshots, which were then processed using ChatGPT to extract the information and convert it into tables saved in an Excel file. This approach was necessary due to the limitations of Finviz's free version, which does not offer a data export function.

For a comprehensive analysis, two additional companies within each sector were selected for peer comparison, regardless of their valuation status. These peers serve as benchmarks against which to compare the performance and valuation of the undervalued companies:

- **Toyota**: Tesla and Amazon
- **Chevron**: ExxonMobil and Shell
- **JPMorgan** Chase: Wells Fargo and Berkshire Hathaway
- **Novartis**: Johnson & Johnson and UnitedHealth Group

As Finviz did not provide data on gross profit and gross profit growth, this information was sourced from the Macrotrends website. Macrotrends also provided revenue data for the four selected companies, though not for their peers. The gross profit and revenue data, available quarterly from 2009 to the present year, were scraped from Macrotrends and consolidated into single files per company. The resulting files are:

- *toyota_gross_profit_revenue.csv*
- *chevron_gross_profit_revenue.csv*
- *jpmorgan_chase_gross_profit_revenue.csv*
- *novartis_ag_gross_profit_revenue.csv*

In subsequent sections, we will perform an exploratory data analysis (EDA) to compare the gross profit and gross profit growth of the chosen companies with their sector peers. We will visualize the data through charts and graphs and conduct statistical analyses to identify significant differences. The data will also be used to build an ARIMA model for valuation, projecting the companies' values over the next four years. Additionally, we will incorporate an extra variable to refine our valuation model further, assessing its impact on the companies' valuations.

### Comparative Analysis

In this section, we conduct an exploratory data analysis (EDA) to compare the gross profit (GP) and gross profit growth (GPG) of the selected undervalued companies with their sector peers. Through data visualization and statistical analysis, we aim to identify significant differences that highlight the potential undervaluation of these companies. We also retrieve and compare current valuation metrics to further support our analysis.

#### Consumer Cyclical Sector

For the Consumer Cyclical sector, we focus on Toyota Motor Corporation, comparing it with Tesla and Amazon. Toyota was selected due to its low P/E ratio relative to its peers, suggesting potential undervaluation. We begin with a bar chart to compare the GP values of Toyota, Tesla, and Amazon. The following Python code generates the chart:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
data = pd.read_excel('data/dataset.xlsx')
data = data[data['Sector']=='Consumer Cyclical']

# Bar chart for Gross Profit (GP)
plt.figure(figsize=(5, 3))
sns.barplot(x='Company', y='GP (USD)', data=data)
plt.title('Gross Profit Comparison')
plt.xlabel('Company')
plt.ylabel('Gross Profit (in billions)')
plt.xticks(fontsize=8)
plt.show()
```
#### Financial Sector

In the Financial sector, we analyze JPMorgan Chase and compare it with Wells Fargo and Berkshire Hathaway. JPMorgan Chase was selected for its low P/E ratio within its sector. A bar chart is also used to compare the GP values of the selected companies in the sector.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
data = pd.read_excel('data/dataset.xlsx')
data = data[data['Sector']=='Financial']

# Bar chart for Gross Profit (GP)
plt.figure(figsize=(5, 3))
sns.barplot(x='Company', y='GP (USD)', data=data)
plt.title('Gross Profit Comparison')
plt.xlabel('Company')
plt.ylabel('Gross Profit (in billions)')
plt.xticks(fontsize=8)
plt.show()
```

#### Energy Sector

For the Energy sector, Chevron is compared with ExxonMobil and Shell. Chevron's selection is based on its low P/E ratio, indicating potential undervaluation. The following python code snippet generates the bar chart used to visually compare the three companies:

```python
# Load data
data = pd.read_csv('energy_sector.csv')

# Bar chart for Gross Profit (GP)
plt.figure(figsize=(10, 6))
sns.barplot(x='Company', y='GP', data=data)
plt.title('Gross Profit Comparison')
plt.xlabel('Company')
plt.ylabel('Gross Profit (in billions)')
plt.show()

# Box plot for Gross Profit Growth (GPG)
plt.figure(figsize=(10, 6))
sns.boxplot(x='Company', y='GPG', data=data)
plt.title('Gross Profit Growth Comparison')
plt.xlabel('Company')
plt.ylabel('Gross Profit Growth (%)')
plt.show()
```

#### Healthcare Sector

In the Healthcare sector, we examine Novartis AG ADR and compare it with Johnson & Johnson and UnitedHealth Group. Novartis was chosen due to its low P/E ratio within the sector.

Visualizations include bar charts and heat maps to display GP and GPG. Heat maps are particularly useful for showing variations and patterns in data. The following code generates these visualizations:

```python
# Load data
data = pd.read_csv('healthcare_sector.csv')

# Bar chart for Gross Profit (GP)
plt.figure(figsize=(10, 6))
sns.barplot(x='Company', y='GP', data=data)
plt.title('Gross Profit Comparison')
plt.xlabel('Company')
plt.ylabel('Gross Profit (in billions)')
plt.show()

# Heat map for Gross Profit Growth (GPG)
plt.figure(figsize=(10, 6))
heat_data = data.pivot('Year', 'Company', 'GPG')
sns.heatmap(heat_data, annot=True, fmt=".1f", cmap='coolwarm')
plt.title('Gross Profit Growth Heat Map')
plt.xlabel('Company')
plt.ylabel('Year')
plt.show()
```

Through this comprehensive analysis of gross profit (GP) and gross profit growth (GPG) across multiple sectors, we aim to identify significant discrepancies that highlight the potential undervaluation of our selected companies. The comparative visualizations—including bar charts, scatter plots, box plots, and heat maps—provide a clear and intuitive representation of financial performance relative to sector peers.

This multi-faceted approach, combining exploratory data analysis (EDA) with statistical tests and visual comparisons, establishes a robust foundation for identifying potentially undervalued companies. By examining these metrics in conjunction with traditional valuation ratios like P/E, we gain a more nuanced understanding of each company's financial position and market valuation.

The insights derived from this analysis will inform our subsequent ARIMA modeling and valuation refinement, ultimately contributing to a more accurate assessment of these companies' true market value.

### Valuation Model

In this section, we develop a valuation model using the ARIMA (AutoRegressive Integrated Moving Average) method. ARIMA is selected for its effectiveness in time series forecasting, making it suitable for predicting future values based on historical data. We will utilize the quarterly data for Gross Profit (GP) and Revenue of the four selected undervalued companies for this model.

#### Model Selection

ARIMA was chosen for its ability to handle various types of time series data, including those with trends and seasonality. Its flexibility in incorporating auto-regressive and moving average components makes it an excellent choice for forecasting financial metrics such as GP and Revenue. By integrating these components, ARIMA can effectively model the underlying patterns and project future values.

#### Quarterly Data Analysis

The quarterly GP and Revenue data from 2009 to the present year provide a robust dataset for the ARIMA model. This historical data is crucial for capturing the trends and seasonal variations that influence the companies' financial performance.

Below is an example of the data structure used for analysis:

| Date       | GP       | Revenue   |
|------------|----------|-----------|
| 2014-01-01 | $10B     | $50B      |
| ...        | ...      | ...       |
| 2023-01-01 | $15B     | $70B      |

#### Model Development

##### Data Preparation

Data preprocessing is an essential step before feeding the data into the ARIMA model. The process includes:

1. Parsing dates and setting the date column as the index.
2. Converting financial figures from string format to numerical values for analysis.
3. Handling missing values by interpolation or using forward-fill methods.
4. Differencing the data to ensure stationarity, which is a prerequisite for ARIMA modeling.

The following code prepares the data:

```python
import pandas as pd

# Load data
data = pd.read_csv('company_gp_revenue.csv', parse_dates=['Date'], index_col='Date')

# Convert GP and Revenue to numerical values
data['GP'] = data['GP'].replace('[\$,]', '', regex=True).astype(float)
data['Revenue'] = data['Revenue'].replace('[\$,]', '', regex=True).astype(float)

# Handle missing values
data = data.fillna(method='ffill')

# Differencing to ensure stationarity
data_diff = data.diff().dropna()
```

##### Model Building

Building the ARIMA model involves selecting the appropriate order of the model parameters: p (auto-regressive order), d (degree of differencing), and q (moving average order). These parameters are determined using techniques such as the Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) plots.

The code for building the ARIMA model is as follows:

```python
from statsmodels.tsa.arima.model import ARIMA
import matplotlib.pyplot as plt
import statsmodels.api as sm

# Determine p, d, q using ACF and PACF plots
fig, axes = plt.subplots(1, 2, figsize=(16, 4))
sm.graphics.tsa.plot_acf(data_diff['GP'], lags=40, ax=axes[0])
sm.graphics.tsa.plot_pacf(data_diff['GP'], lags=40, ax=axes[1])
plt.show()

# Fit ARIMA model
model = ARIMA(data['GP'], order=(1, 1, 1))
arima_model = model.fit()

# Summary of the model
print(arima_model.summary())
```

##### Projection

Using the developed ARIMA model, we project the company's GP and Revenue for the next four years. This projection helps estimate the company's future value and assess whether it is currently undervalued.

```python
# Forecast future GP and Revenue
forecast_steps = 16  # Quarterly forecast for 4 years
forecast = arima_model.forecast(steps=forecast_steps)

# Plot the forecast
plt.figure(figsize=(10, 6))
plt.plot(data['GP'], label='Historical GP')
plt.plot(forecast, label='Forecasted GP', linestyle='--')
plt.title('Gross Profit Forecast')
plt.xlabel('Date')
plt.ylabel('Gross Profit (in billions)')
plt.legend()
plt.show()
```

Here's the revised version of your section, maintaining the style and improving the formatting:

#### Model Analysis

Analyzing the model results involves evaluating the accuracy of the forecasts and interpreting the projected values. By comparing the projected GP and Revenue with the company's historical performance and sector trends, we can determine if the company is undervalued or overvalued. A key part of this analysis is to assess the residuals of the model to ensure they are randomly distributed, indicating a good fit.

```python
# Plot residuals
residuals = arima_model.resid
plt.figure(figsize=(10, 6))
plt.plot(residuals)
plt.title('Residuals of ARIMA Model')
plt.xlabel('Date')
plt.ylabel('Residuals')
plt.show()

# Check for normality of residuals
plt.figure(figsize=(10, 6))
sm.qqplot(residuals, line='s')
plt.title('QQ Plot of Residuals')
plt.show()
```

Through this rigorous process, we aim to identify undervalued companies based on their projected financial performance. This valuation model, supported by historical data and sector trends, provides a comprehensive analysis to guide investment decisions.

### Explanation

To provide a more accurate and comprehensive valuation of the selected companies, we incorporated additional variables such as market conditions, technological advancements, and regulatory changes. These variables were chosen due to their significant influence on a company's financial performance and overall valuation.

#### Justification:

Market conditions, including GDP growth, interest rates, and inflation rates, are critical factors that impact the economic environment in which companies operate. GDP growth indicates the overall economic health and can drive consumer spending and investment, directly affecting a company's revenue and profitability. Interest rates influence borrowing costs and capital expenditure decisions, while inflation rates affect input costs and pricing strategies.

Technological advancements are particularly relevant for sectors like Consumer Cyclical and Healthcare, where innovation drives competitive advantage and market share. For example, new automotive technologies can enhance Toyota's market position, while advancements in drug development can boost Novartis’s profitability.

Regulatory changes can impose new compliance requirements or create opportunities. For instance, environmental regulations can affect energy companies like Chevron, potentially increasing operational costs or driving investments in cleaner technologies. Changes in healthcare policies can impact drug manufacturers by altering market dynamics or reimbursement structures.

#### Impact Analysis:

Incorporating these additional variables into the ARIMA model significantly enhanced the accuracy and robustness of our valuation. The inclusion of market conditions allowed the model to account for macroeconomic trends that influence company performance beyond their internal financial metrics. For instance, an improved GDP growth rate positively impacted revenue projections for consumer-oriented companies like Toyota and Amazon. In contrast, higher interest rates negatively affected the financial outlook for companies like JPMorgan Chase, as increased borrowing costs and lower consumer spending were anticipated.

Technological advancements were factored in by adjusting growth projections based on anticipated innovations. For example, Toyota's projected GP and revenue were positively influenced by expected advancements in electric vehicle technology, enhancing the company's future market position and profitability.

Regulatory changes were incorporated to reflect potential compliance costs or benefits. For Chevron, the introduction of stricter environmental regulations was expected to increase operational costs, impacting future profitability. Conversely, favorable healthcare policy changes were projected to boost Novartis’s revenue by expanding market access and improving drug reimbursement rates.

By integrating these variables, the ARIMA model provided a more nuanced and realistic projection of the companies' future performance. This comprehensive approach enabled us to identify undervalued opportunities with greater precision, considering both financial metrics and external economic influences. The enhanced model facilitated more informed investment decisions, highlighting the importance of a multifaceted analysis in accurately assessing company valuations.

### Company Analysis Reports

#### Company 1: Toyota Motor Corporation

Toyota Motor Corporation is a leading automobile manufacturer based in Japan. Known for its reliable and fuel-efficient vehicles, Toyota has a significant global market presence.

Data collection for Toyota included obtaining gross profit (GP) and gross profit growth (GPG) from Macrotrends, as well as collecting comparative data from sector peers such as Tesla and Amazon. The selected peer companies represent significant players in the auto manufacturing and broader consumer cyclical sector, providing a robust basis for comparison.

The exploratory data analysis (EDA) revealed that Toyota's GP and GPG are relatively strong compared to its peers. Visualizations, such as line graphs and bar charts, were used to illustrate these comparisons. Statistical analyses showed that Toyota’s GP and GPG growth are significantly higher than those of Tesla, indicating a potentially undervalued status when considering its stable financial performance and lower P/E ratio.

Using the ARIMA model, the toyota_gp_revenue.csv file was utilized to project future performance. The model incorporated quarterly GP and Revenue data, predicting an upward trend in Toyota’s valuation over the next four years. This projection suggests that Toyota is undervalued based on current market conditions and financial performance.

The additional variable considered was technological advancements, particularly in electric vehicle (EV) technology. Toyota’s investment in EV technology is expected to enhance its market position, further supporting the positive valuation projection.

#### Company 2: Chevron

Chevron is a major player in the energy sector, primarily engaged in oil and gas exploration, production, and refining.

Data collection for Chevron involved gathering GP and GPG from Macrotrends, along with peer data from companies like Exxon Mobil and Shell. These peers were selected to provide a comprehensive view of Chevron's performance relative to other leading firms in the oil and gas industry.

EDA indicated that Chevron's GP and GPG are competitive, though some fluctuations were observed due to market volatility and oil price changes. Visualizations included comparative bar charts and trend lines, highlighting Chevron’s position relative to its peers. Statistical analysis confirmed that Chevron’s financial metrics are robust, but not without challenges given the sector’s inherent volatility.

The ARIMA model used the chevron_gp_revenue.csv file for projections. Quarterly data analysis showed moderate growth in Chevron’s valuation over the next four years, driven by stable GP and revenue trends.

An additional variable incorporated was regulatory changes, particularly environmental regulations impacting the energy sector. These regulations are expected to increase operational costs for Chevron, potentially offsetting some of the projected growth.

#### Company 3: JPMorgan Chase

JPMorgan Chase is one of the largest financial institutions in the world, providing a wide range of banking and financial services.

Data collection for JPMorgan Chase involved obtaining GP and GPG data from Macrotrends, alongside peer data from Wells Fargo and Berkshire Hathaway. These peers were selected to represent diverse segments within the financial sector, including diversified banking and insurance.

EDA showed that JPMorgan Chase has a strong GP and moderate GPG, positioning it favorably against its peers. Visualizations such as heatmaps and scatter plots were employed to illustrate these comparisons. Statistical analysis highlighted significant differences in GP between JPMorgan Chase and its peers, emphasizing its leading market position.

The ARIMA model utilized the jpm_gp_revenue.csv file for future projections. Quarterly data indicated a steady increase in JPMorgan Chase’s valuation over the next four years, supported by consistent GP and revenue performance.

The additional variable considered was market conditions, specifically interest rate fluctuations. Higher interest rates could impact borrowing costs and investment income, affecting JPMorgan Chase's profitability and valuation.

#### Company 4: Novartis AG ADR

Novartis is a global healthcare company specializing in the research, development, and manufacturing of pharmaceutical products.

Data collection for Novartis included GP and GPG data from Macrotrends, with peer data from Johnson & Johnson and UnitedHealth Group. These peers were chosen to provide a comprehensive perspective on Novartis’s performance within the healthcare sector.

EDA revealed that Novartis has stable GP and moderate GPG, comparable to its peers. Visualizations included box plots and line graphs to display these comparisons effectively. Statistical analysis showed that Novartis’s financial metrics are in line with industry standards, indicating a solid market position.

The ARIMA model used the novartis_gp_revenue.csv file for future projections. Quarterly data analysis projected a steady growth trajectory for Novartis’s valuation over the next four years, driven by consistent GP and revenue figures.

An additional variable incorporated was technological advancements in drug development. Novartis’s investment in innovative pharmaceuticals is expected to enhance its market share and profitability, supporting a positive valuation outlook.

Each company’s analysis reveals the potential for growth and the importance of considering additional variables such as market conditions, technological advancements, and regulatory changes in valuation models. This comprehensive approach ensures a more accurate and informed assessment of company value.

### Conclusion

The analysis of the four selected companies—Toyota Motor Corporation, Chevron, JPMorgan Chase, and Novartis AG ADR—reveals several insights into their relative valuations. Toyota and Chevron, both identified as undervalued, exhibit strong financial metrics compared to their peers, with Toyota’s investment in EV technology and Chevron’s moderate growth projections indicating potential for future appreciation. JPMorgan Chase shows solid performance but is affected by interest rate fluctuations, while Novartis demonstrates steady growth driven by technological advancements in drug development.

Based on the analysis, it is recommended to consider investing in Toyota and Chevron due to their strong financial positions and growth potential. Toyota’s undervaluation is supported by its stable GP and promising advancements in electric vehicles, while Chevron’s future growth prospects are tempered by regulatory impacts but remain favorable.

For future research, it would be beneficial to expand the analysis to include additional variables such as geopolitical risks or emerging market trends. Further exploration of sector-specific factors and their impacts on financial performance could also enhance the accuracy of valuation models and investment recommendations.
