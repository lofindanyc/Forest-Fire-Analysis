# Forest Fire Analysis:

## Predicting Burned Area in Northeast Portugal

##### [TEPP] Phase 2 Portfolio Project

##### TKH Innovations Fellowship Program

##### Data Analytics Track 2026


<img width="122" height="124" alt="image" src="https://github.com/user-attachments/assets/15353f95-f2ea-4055-a7bb-9927bdda281b" />



<img width="340" height="280" alt="image" src="https://github.com/user-attachments/assets/c62e015f-645b-4c09-bcb0-f70c9f49fb80" />


Montesinho Natural Park, Portugal


## Amir Benston

## Lofinda B. Beynis

## Palo Becerra, author

## August 26, 2026 1

Table of Contents

0. Data Dictionary . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 3
1. Detailed EDA & preprocessing decisions . . . . . . . . . . . . . . . . . . . . . . . pg. 4 - 8
2. Unsupervised learning approach & findings . . . . . . . . . . . . . . . . . . . . pg. 9 - 12
3. Supervised models tested & compared . . . . . . . . . . . . . . . . . . . . . . . . . pg. 12 - 13
4. Model selection & rationale . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 13
5. Detailed results . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 13 - 14
6. Dashboard interpretation . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 14 - 15
7. Limitations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 15 - 16
8. Recommendations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 16
Figures
Section 1 Figures . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .pg. 5 - 8
Section 2 Figures . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 9 - 11
Section 5 Figure . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . pg. 14
August 26, 2026 2

Data Dictionary

(Sourced from original dataset, supplemental sources, engineered and transformed features)

1. X : X-axis spatial coordinate within the Montesinho park map: 1 to 9
2. Y : Y-axis spatial coordinate within the Montesinho park map: 2 to 9
3. Month : Month of the year: "jan" to "dec". One-hot encoded and numerically encoded for binary
modeling and filtering in the project. (“month_feb, month_num”)
4. Day : Day of the week: "mon" to "sun". One-hot encoded and numerically encoded for binary
modeling and filtering in the project. (“day_mon, day_num”)
5. FWI : Fire Weather Index. Engineered feature.
ISI and BUI scores combined into the ultimate numerical rating to evaluate for general fire
intensity potential. Originally created by the National Canadian Forest Service.
6. BUI : Buildup Index. Engineered feature.
DMC and DC combined into a numerical rating to evaluate for the cumulative effects of drought
and moisture in relation to the FWI System overall. It responds to fuel accumulation as well.
7. FFMC : index from the FWI system: 18.7 to 96.20
Previous FFMC, temp, RH, wind speed and rain combined into a numerical rating of the moisture
content of litter and other cured fine fuels. Used to represent the potential for human-caused
ignition. It responds to temperature, relative humidity, wind and rain, and indicates the relative ease
of ignition and flammability of fine fuels.
8. DMC : index from the FWI system: 1.1 to 291.3
Previous DMC, temp, RH, rain and month of the year combined into a numerical rating of the
average moisture content of loosely compacted organic layers of moderate depth. Used to assess the
potential for lightning-caused ignition. It responds to temperature, relative humidity and rain, and
provides an indication of fuel consumption in moderate duff layers and medium-sized woody
material.
9. DC : index from the FWI system: 7.9 to 860.6
Previous DC, temp, rain and month of the year combined into a numerical rating of the average
moisture content of deep, compact organic layers. It responds to temperature and rain and indicates
the effects of seasonal drought on forest fuels and the potential for smouldering in deep duff layers
and large logs.
10. ISI : index from the FWI system: 0.0 to 56.10
FFMC and wind speed combined into a numerical rating of the expected rate of fire spread. Higher
values indicate faster spread rates. It combines wind speed with the FFMC without accounting for
the quantity of fuel available for combustion.
11. Temp : Temperature in Celsius degrees: 2.2 to 33.30
12. RH : Relative humidity in %: 15.0 to 100
13. Wind : Wind speed in km/h: 0.40 to 9.40
14. Rain : Outside rain in mm/m2 : 0.0 to 6.4
15. Area : The burned area of the forest (in ha): 0.00 to 1090.84
16. Log_area : Engineered feature. Original ‘Area’ feature was extremely skewed towards 0.00, thus
leading to logarithmic transformation for better modeling and evaluation.
August 26, 2026 3
1. Detailed EDA & Preprocessing Decisions
With three team members, this project began with independent exploratory data analyses conducted
by each member. This decision was made as a group to gather many various findings and after
discussion, compile the most valuable findings into one singular plan of action for preprocessing. The
dataset was sourced from the raw UCI Montesinho Natural Park forest fires dataset. All three analyses
converged on similar core findings, which gave our team confidence that the patterns identified were
genuine and verifiable. This collaborative approach continued throughout the entire process.
1.1 Data Quality
● The initial dataset consisted of 517 rows and 13 columns.
● No missing values were found present anywhere in the dataset.
● 4 exact duplicate rows were identified in the raw data. These were removed by Lofinda during
her own preprocessing stage, but ultimately left in the consolidated dataset used for supervised
learning. Amir’s clustering and supervised modeling pipeline used the full 517 rows in addition
to his engineered features and remained consistent in his analysis.
● The target variable ‘area’ was extremely right-skewed to equal 0.00.
● A small number of outlier fires burned several hundred to over 1,000 hectares.
1.2 Target Transformation
Because of this skew, log_area = log(area + 1) was created and used as the main regression target for
both visualizations and modeling. The log1p form was chosen specifically to handle the large number
of zero-area observations, since log(o) is undefined. This transformation was also recommended by the
dataset’s original authors (Cortez & Morais, 2007).
1.3 Feature Engineering
Amir engineered BUI (Buildup Index) and FWI (Fire Weather Index) from the existing composite
columns by following the official Canadian Forest Fire Weather Index System’s formulas. These
composite indices were not present in the original dataset and became a central point for his modeling
processes tested and built down the line.
‘Month’ and ‘day’ were also encoded in two ways for separate downstream use: ordinal numeric
encoding (month_num, day_num) for use in regression/tree models by Amir, and one-hot encoding
for exploratory grouping and filtering by Lofinda.
1.4 Supplemental Visual Findings
Beyond the target transformation, each group member’s EDA produced a set of exploratory
histograms and scatterplots examining the raw weather variables and fire weather indices individually.
While these did not all lead to further dedicated analysis given the project’s time constraints, several
patterns were worth nothing and illustrated in 1.5 EDA Figures.
August 26, 2026 4
Fire observations were heavily concentrated in August and September (Figure 1). While August
recorded more fires than September, the fires in September produced a greater total burned area,
highlighting how frequency and severity did not track perfectly together.
The extreme skew of the target variable (area) is visible directly in its distribution (Figure 2). There is
almost no visible spread beyond the zero mark. Applying the log transformation does reveal more
spread and a lower peak, confirming that this transformation was necessary for meaningful analysis and
model building.
Histograms of the relationship between frequency and the feature variables (Figure 3) shown reflect
the fire-friendly conditions when these fires occurred. FFMC, DC and BUI skew heavily toward
high-risk ranges, while temp shows the only relatively normal distribution centered around 18-22 °C.
Rain shows almost no variation. Overall the weather variables reflect logical and expected behavior.
Early scatterplots of the raw FWI indices against Log(Area+1) (Figure 4) indicate that FFMC was the
only variable demonstrating a weak relationship to fire size. DMC, DC and ISI showed no real
patterns, with large and small fires recorded across the range.
A separate pair of scatterplots (Figure 5) examined how the overall FWI related to two of it’s
combined inputs, temperature and wind. Temperature showed a clear, positive relationship with FWI,
while wind showed no apparent relations. This was an early signal that some raw weather variables
were more valuable than others for modeling purposes.
The correlation matrix (Figure 6) included the raw weather variables and composite indices, as well as
the engineered composite features built by Amir, and displays their relationships to burned area. None
of the variables showed a strong correlation with area. The highest score was temperature at only 0.10.
This foreshadowed a reoccurring theme of our analysis: no matter how composited, the individual
variables offered weak support for predicting burned area.
1.5 EDA Figures
August 26, 2026 5
Figure 1.1 Number of forest fires by month (top) and total burned area by month (bottom).
Figure 1.2 Raw burned area vs. Frequency (left) and log(area + 1) vs. Frequency (right).
August 26, 2026 6
Figure 1.3 Distributions of FFMC, DMC, DC, ISI, BUI, FWI, temperature, relative humidity, wind,
and rain. (from left to right).
Figure 1.4 FFMC, DMC, DC, and ISI vs. Log(Burned Area+1).
August 26, 2026 7
Figure 1.5 Temperature vs. FWI (left) and wind vs. FWI (right).
Figure 1.6 Correlation matrix across weather variables, fire-weather indices, and burned area.
1.6 Collaborative Plan of Action
After our group compared initial EDA findings, we split the remaining work into roles. Amir and
Lofinda continued with their own preprocessing and unsupervised learning, with Amir being the sole
member to continue with supervised learning. I took on the role of team lead, holding myself
responsible for guiding and consolidating our team’s work, building the final Tableau dashboard, and
authoring this report. Lofinda created the Github repository and was responsible for its maintenance.
Lofinda also built a Tableau dashboard and after group review, mine was selected as the version for
submission.
August 26, 2026 8
2. Unsupervised Learning Approach & Findings
To help prevent bias, both Amir and Lofinda independently applied K-Means clustering to test
whether fire-weather conditions naturally separated into distinct groups, without using burned area to
define the clusters. The two approaches used different feature sets and arrived at a different number of
clusters, but reached otherwise compatible conclusions.
2.1 Lofinda’s Approach (K = 2)
Lofinda clustered on raw meteorological data and fire-risk variables (temperature, wind, humidity, and
fire-risk indicators). The elbow method and silhouette score supported a two-cluster solution. The
resulting groups separated into a hotter/drier cluster and a cooler/higher-humidity cluster.
Key finding: The hotter/drier cluster contained 77% of all fires and had a substantially higher average
burned area (14.97 ha) than the cooler/more humid cluster (5.94 ha). Lofinda concluded that hotter,
drier conditions tend to be associated with larger fires, but that the two groups still had considerable
overlap. In short, weather conditions alone do not fully explain fire size, and some other unmeasured
factors likely play a role.
2.2 Amir’s Approach (K = 3)
Amir clustered on a six feature set chosen to balance four categories of information given in the dataset:
surface fuel state (FFMC), fuel accumulation (BUI), composited Fire Weather Index (FWI), and raw
weather (temp, wind, RH). This differed from Lofinda’s approach in that he substituted his
engineered composite indices (BUI, FWI) for the raw underlying composites (DMC, DC, ISI). Amir’s
elbow method bent somewhere between K=2 and K=4, leading him to select K=3 as the middle point.
This resulted in 3 distinct clusters:
Cluster n Mean
Area (ha)
Median
Area (ha)
Avg FFMC Avg BUI Avg
FWI
Avg Temp (°C)
0 — Low Risk 114 6.08 0.00 85.23 37.42 10.36 12.51
1 — Hot/Dry High
Risk
300 17.07 1.04 92.47 167.80 37.13 22.29
2 — High Fuel,
Humid, Contained
103 8.05 0.00 91.33 183.58 34.33 16.04
2.3 Unsupervised Visual Figures
August 26, 2026 9
Figure 2.1 Lofinda’s Elbow Method for K-Means results.
Figure 2.2 Lofinda’s K=2 Clusters charted by Average Fire-Weather Conditions.
August 26, 2026 10
Figure 2.3 Lofinda’s K=2 Clusters PCA Visualization.
Figure 2.4 Amir’s Elbow Method for K-Means Results.
Figure 2.5 Amir’s K=3 Clusters charted by Average FWI, RH and Temp °C.
August 26, 2026 11
2.4 Benefits of Two Unsupervised Model Approaches
Both Lofinda and Amir used different feature sets(raw components vs. engineered composites) and
different scaling/elbow interpretations, which organically produced different elbow curves and cluster
counts. Both were legitimate and well-justified in the choices they made while constructing their
pipelines. Since their findings reached a convergent conclusion - that hot, dry, high-fuel conditions
were associated with more fire spread, but not enough for determination - our group viewed this as a
strength rather than a weakness.
3. Supervised Models Tested & Compared
Motivated by his prior experience as a math teacher with regression, and a determination to improve
upon the model found in the source literature, Amir led the supervised learning portion. Amir had a
goal of building a model that would improve meaningfully on the previous correlations. Eight model
configurations were tested in total, moving from simple interpretable baselines towards more
progressively informed feature sets.
3.1 Amir’s Ambitious Approach
Models A and B (Linear Regression) served as the baselines, assuming linear relationships which fire
risk and behavior likely violated. Models C, D and E (RandomForest) tested whether capturing
nonlinearity and feature interactions could improve upon the baseline. Fwi_compile (RandomForest)
tested the full FWI System chain with no raw weather at all. Model F (RandomForest) further tested
by adding wind and rain on top of the best base model found so far, and Model G tested adding the
unsupervised cluster feature on top of the same base.
Model Features Type MAE (area) RMSE (log) R²
A FWI, temp, FFMC, RH Linear Regression 19.83 1.48 -0.001
B FWI, FFMC, RH, wind Linear Regression 19.81 1.48 0.011
C FFMC, DMC, DC, temp Random Forest 19.68 1.49 -0.020
D temp, ISI, BUI, FWI Random Forest 19.98 1.52 -0.054
E FFMC, DMC, DC, ISI, BUI, FWI,
temp
Random Forest 19.75 1.49 -0.018
fwi_compile FFMC, DMC, DC, ISI, BUI, FWI
(no raw weather)
Random Forest 19.45 1.45 0.041
F fwi_compile features + wind + rain Random Forest 19.75 1.46 0.024
G fwi_compile features + cluster Random Forest 19.42 1.44 0.057
3.2 Notable Patterns Across Regression Models
● All three RandomForest variants built from single/overlapping raw variables (Models C, D, E)
produced negative R² scores, underperforming even the weak but positive Model B (0.011).
Increasing model flexibility did not improve predictive performance. If anything, it hurt
performance, suggesting that the limitation here was caused by the dataset rather than any
model complexity.
August 26, 2026 12
● Fwi_compile (full FWI chain, no raw weather) outperformed every model built specifically for
the redesign (C, D, E), despite being a simpler feature set. This was the first positive R² during
the supervised modeling sequence.
● Adding raw weather back on top of ‘fwi_compile’ consistently hurt performance: Adding
temp alone (Model E) dropped R² from 0.041 to -0.018; adding wind and rain (Model F)
dropped it to 0.024. This is one of the more consistent and interesting findings throughout
our analysis. The FWI system’s indices appear to already carry whatever weather signal is
usable, and raw weather variables layered on top mostly just added noise.
● Model D (composite indices and temp) was the weakest of the RandomForest variants. Despite
having four named features, this set offered few genuinely independent directions for the
model to split on. With a training set of only 413 records, this redundancy likely made the
model more prone to overfitted noise rather than learning structure.
4. Model Selection & Rationale
Model G (fwi_compile feature set combined with unsupervised cluster label) was selected as the final
model. It was the best performing model across the entire testing sequence (A-G), and the only
weather-derived addition to the analysis that improved rather than degraded predictive performance.
4.1 Why Model G Stood Out
Unlike every previous model, which tested every raw weather variable in addition to the FWI indices,
Model G improved every metric over the base model by adding the cluster label (MAE(area) = 19.42,
RMSE(log) = 1.44, R² = 0.057). This supported the central hypothesis behind the clustering analysis:
individual raw weather variables carried too little independent signal, and tended to introduce
overfitted noise, but the same underlying conditions compressed into a single risk category via
unsupervised clustering, captured a combined multivariate structure that no single raw variable could
express on its own. The improvement from fwi_compile (R² = 0.041) to Model G (R² = 0.057) was
modest at best, and should be cautiously interpreted given the limited test size (n = 104). Still, it was
the first and only analysis that improved upon predictive performance, which is why we chose it as our
final model.
5. Detailed Results
5.1 Predicted vs. Actual (Model G)
Most points in the predicted vs actual plot sat well off the perfect-prediction line, especially the
handful of large actual fires (log(area+1) above 4), which the model consistently underpredicted. The
largest fire in the test set (actual log(area+1) ≈ 7.0) is predicted at only ≈3.3, and a second large fire
(actual ≈ 5.3) is predicted at only ≈2.1. There is also a dense cluster of points at Actual = 0, with
predicted values ranging from 0 up to ≈2.6, highlighting how the model remained noisy even on the
fires with no measurable spread.
August 26, 2026 13
Even with the clusters added, Model g’s core weakness persists. It cannot reliably predict large fires.
Figure 5.1 Predicted vs. Actual Log(Area+1) - Model G.
5.2 Overall Metric Summary
Across all eight models tested (A-G plus the fwi_compile baseline), R² values ranged from -0.054 to 0.057.
Even the best model could only explain about 5-6% of the variance in burned area. This was consistent with the
weak univariate correlations observed in the heatmap (side: strongest single-variable correlation was ‘temp’ at 0.10),
and reflected the well documented difficulty of this dataset from the original source.
6. Dashboard Interpretation
Both Lofinda and I independently built our own Tableau dashboards to communicate our group’s
findings to a non-technical audience. Lofinda’s dashboard effectively highlighted key metrics and
earlier analysis points, but didn’t include the supervised learning as part of the narrative. After group
review, and keeping presentation in mind, our group selected my own dashboard as the version used
for submission and presentation. Despite this being my first time using Tableau, I was able to
effectively tell a cohesive story across our analytical journey.
The selected dashboard included 8 linked visuals that mirror the structure in this report:
● Fires by Month / Burned Area by Month: Established the concentration of fire activity in
August and September, and the distinction between fire frequency and fire severity.
● Correlation Heatmap: Visualized the core challenge of the project: no individual weather
variable or fire index correlated strongly with burned area.
● FFMC vs. Log(Area+1): Highlighted the one raw variable that showed a real (nonlinear,
threshold-like) pattern with fire size, and noted that FFMC is itself a composite of prior
FFMC, temperature, humidity, wind, and rain — motivating Amir’s later engineering of BUI
and FWI.
August 26, 2026 14
● One Cluster Stood Alone / K-3 Clusters: Amir’s clustering results, showing that Cluster 1
is the only group where measurable fire spread is the typical outcome, and that humidity —
not fuel or danger scores alone — differentiated Cluster 1 from Cluster 2.
● Eight Models, One Modest Win: Model G: Summarized the full A–G model spectrum and
highlighted Model G's modest but real improvement.
● Predicted vs. Actual: Model G: Completed the dashboard with an honest reality check on
the limits of the best model produced.
Note: Since Amir was the sole member of our group able to deliver the presentation in person, I moved
around the order of the visuals into a structure that he felt most comfortable presenting the dashboard
and telling the story of our process. Both versions will be listed in the ‘Forest_Fire_Analysis’ Repo,
filed under /Dashboards.
7. Limitations
Our group analysis confirmed what the dataset’s original authors (Cortez & Morais, 2007) and
subsequent published works have consistently found: predicting burned area from meteorological and
fire-index data alone is an incredibly difficult regression problem. Across every model configuration
tested (linear and nonlinear, raw and composite features, with and without K clusters), no model could
strongly predict burned areas for Montesinho National Park.
● Weak underlying signals: No single raw weather variable or FWI indice correlated strongly
with burned area. The strongest correlation was temp with only 0.10.
● Small dataset: The raw dataset held less than 1,000 rows (518). After preprocessing there were
only 513 usable observations, with approximately 400 used for training and 100 saved for
testing after the train/test split. This limited the statistical power available for the final model
(G) to only predict with mild success.
● Extreme skew of target variable: Nearly half of all the observations had an area=0, with the
largest fires being outliers among the dataset. Even after compensating with Log(Area+1),
every model tested vastly underpredicted the large fires.
● Feature redundancy across composite indices: FWI, BUI, FFMC, and many more indices
were features used in this project. Because of their over-lapping nature, this reduced their
usefulness in tree-based model building and instead led to increasing noise and potential
overfitting.
● Missing possible additional variables: The original dataset did not include any other
meaningful factors related to forest fire risk, such as: fuel types, ignition cause, terrain, fire
fighters response time, or more. Any supplemental data could have changed the direction of
this project and potentially improved analysis or prediction.
7.1 Time - Structural Limitations Examined
Taking a closer look at the limitations of this dataset, time was an underutilized factor collected for
further analysis. The original Montesinho dataset observations only include records for month and day
of each fire, with weather and fire risk indices representing the daily conditions. In a recent publication
August 26, 2026 15
from the Canadian Forest Fire Danger Rating System (FWI25-26) , the update explicitly recommends
moving from once-per-day weather observations to hourly weather inputs, noting that this allows fire
agencies to “better account for changes throughout the daily weather cycle” (Natural Resources
Canada, 2025, Section 4.4&5). The same publication acknowledges that while hourly data increases
analytical complexity, it also enables more accurate fire potential assessments than daily and monthly
data allows.
This directly ties back to our current predicament: the raw weather and composite FWI indices used
throughout this project vary meaningfully within a single day. Temperature, humidity and wind in
particular can have variations that swing across the spectrum of interpretation in a rapid hourly
fashion. It is entirely plausible that some of the weak correlations and modest model performance
could be remedied if we had access to a more rich and complex amount of hourly data over daily data.
Hourly data would also include an overall larger dataset. More data could lead to better testing and
stronger models, thus leading to better evaluation scores and clearer prediction.
8. Recommendations
● Acquire hourly-resolution weather and fire index data. Per Section 7.1, the current
national guidance for the FWI system itself has moved towards adopting hourly inputs. In
addition, the FWI2025 now includes support and guidance on monitoring sunrise, sunset and
sunlight hours, as well as peak burn hours. “While hourly outputs may be an overwhelming
amount of data from the daily regional planning perspective, this real-time monitoring can
highlight discrepancies and potential shifts in fire behavior…this functionality could provide
early indications of unexpected activity…” (Natural Resources Canada, 2025, Section 4.4&5).
● Incorporate missing contextual variables not currently present in the dataset.
Terrain/elevation, vegetation and fuel types, ignition cause, fire watch response time, and more
could help bring more clarity or correlation to a small and weak dataset.
● Treat forest fire potential and fire size as two separate modeling problems to target.
Given that roughly half of the data had zero burned area, perhaps a two - stage approach could
provide a better structure for interpreting and assessing the data at hand. For example:
classification followed by regression, rather than solely regression.
Sources
UCI Machine Learning Repository — Forest Fires Dataset: https://archive.ics.uci.edu/dataset/162/forest+fires
Cortez, P., & Morais, A. (2007). A Data Mining Approach to Predict Forest Fires using Meteorological Data.
Natural Resources Canada (2025). Canadian Forest Fire Danger Rating System FWI2025 Update, Section 4.4
"New daily summaries of hourly inputs" and Section 5 "Use and Interpretation."
https://publications.gc.ca/collections/collection_2026/rncan-nrcan/Fo123-2-42-2025-eng.pdf
Github Repository: https://github.com/lofindanyc/Forest-Fire-Analysis
August 26, 2026 16
