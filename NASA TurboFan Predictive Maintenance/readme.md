# Condition Based Maintenance

## Problem Statement

Prognostics is an engineering discipline focused on predicting the time at which a system or a component will no longer perform its intended function. The predicted time then becomes the remaining useful life (RUL), which is an important concept in decision making for contingency mitigation. Prognostics predicts the future performance of a component by assessing the extent of deviation or degradation of a system from its expected normal operating conditions.

Traditional maintenance concept is time/schedule based and is a reactive type of maintenance.  On the other hand, condition based maintenance is prognostics-based and a pro-active type of maintenance that relies on early detection and prediction of potential failure of the equipment or component to pre-empt sudden breakdowns and prolonged downtime.  Predicting early failures in a maintenance regime will mitigate prolonged downtime and reduce maintenance cost while keeping equipment operationally ready and available.

Military equipment is often operated in harsh environment and are required to be readily available and operational 24/7.  Having the capability to predict potential equipment failures before they actually occur will enable planners to pro-actively maintain the equipment to be ready 24/7. There is considerable cost savings (for example by avoiding unscheduled maintenance and by increasing equipment usage) and operational safety improvements.

The aim this project is to build a model capable of forecasting the remaining useful life of turbofan engines. For an engine degradation scenario an early prediction is preferred over late predictions. Therefore, the scoring algorithm for this project could be asymmetric around the true time of failure such that late predictions will be more heavily penalized than early predictions. The baseline aggregate score will be based on the Linear Regression model.


## Data Source

The data source is obtained from NASA. This prognostics dataset is on sensor measurements of turbofan engines (a total of 708 engines).  The dataset is modelled after real life engine data and was presented in a technical paper reference “Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation”.

data source : https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan

## Data Description

The datasets consist of multiple multivariate time series. Each dataset is further divided into training and test subsets. Each time series is from a different engine i.e., the data can be considered to be from a fleet of engines of the same type.

Each engine starts with different degrees of initial wear and manufacturing variation which is unknown to the user. This wear and variation is considered normal, i.e., it is not considered a fault condition. There are three operational settings that have a substantial effect on engine performance. These settings are also included in the data. The data are contaminated with sensor noise.

The engine is operating normally at the start of each time series, and starts to degrade at some point during the series. In the training set, the degradation grows in magnitude until a predefined threshold is reached beyond which it is not preferable to operate the engine. In the test set, the time series ends some time prior to complete degradation.


There are 4 sets each of training and testing data and true (ground-truth) remaining useful life values for model evaluation. Dataset 1 contains 100 engines' data, dataset 2 contains 260 engines' data, dataset 3 contains 100 engines' data and dataset 4 contains 249 engines' data.  The engines can be considered to be from a fleet of the same type.

The engine is operating normally at the start of each time series, and develops a fault at some point during the series. In the training set, the fault grows in magnitude until system failure. In the test set, the time series ends some time prior to system failure.

### Datasets

The data are provided as text files with 26 columns of numbers, separated by spaces. Each row is a snapshot of data taken during a single operational cycle, each column is a different variable. The columns correspond to:

    1)	unit number
    2)	time, in cycles
    3)	operational setting 1
    4)	operational setting 2
    5)	operational setting 3
    6)	sensor measurement  1
    7)	sensor measurement  2
    ...
    26)	sensor measurement  26


|Dataset|No. of rows|No. of Engines|
|---|---|---|
|train_FD001|20631|100|
|train_FD002|53759|260|
|train_FD003|24720|100|
|train_FD004|61249|249|
|test_FD001|13096|100|
|test_FD002|33991|259|
|test_FD003|16596|100|
|test_FD004|41214|248|
|RUL_FD001|100|100|
|RUL_FD002|259|259|
|RUL_FD003|100|100|
|RUL_FD004|248|248|

### Data Dictionary

|S/No|Features|Description|Units|Data Type|
|---|---|---|---|---|
|1|engine_no|Engine number|-|int64|
|2|time_in_cycles|Time when engine is running|hours|int64|
|3|op_setting_1|Throttle|Resolver Angle|degrees	float64|
|4|op_setting_2|Mach| number|-|float64|
|5|op_setting_3|Altitude|	per 1000 ft	|float64|
|6|sensor_1|Total temperature at fan inlet|	degrees R|	float64|
|7|sensor_2|Total temperature at LPC outlet|degrees R|float64|
|8|sensor_3|	Total temperature at HPC outlet|	degrees R	|float64|
|9|sensor_4|	Total temperature at LPT outlet	|degrees R	|float64|
|10|sensor_5|	Pressure at fan inlet	|psia	|float64|
|11|sensor_6|	Total pressure in bypass-duct|	psia|	float64|
|12|sensor_7|	Total pressure at HPC outlet|	psia|	float64|
|13|sensor_8|Physical fan speed	|rpm|	float64|
|14|sensor_9|	Physical core speed|	rpm|	float64|
|15|sensor_10|	Engine pressure ratio|	-	|float64|
|16|sensor_11|Static pressure at HPC outlet|	psia|	float64|
|17|sensor_12|	Ratio of fuel flow to Ps30|	pps/psi|	float64|
|18|sensor_13	|Corrected fan speed|	rpm|	float64|
|19|sensor_14|	Corrected core speed|	rpm	|float64|
|20|sensor_15|	Bypass ratio|	-	|float64|
|21|sensor_16|	Burner fuel-air ratio|	-	|float64|
|22|sensor_17|	Bleed Enthalpy|	-	|int64|
|23|sensor_18|	Demanded fan speed|	rpm|	int64|
|24|sensor_19|	Demanded corrected fan speed|	rpm|	float64|
|25|sensor_20|	HPT coolant bleed| 	lbm/s	|float64|
|26|sesnor_21|	 LPT coolant bleed|	lbm/s	|float64|
|27|RUL	|Remaining useful life|	-	|int64|


## Potential Challenges

From my research, a number of papers have been published on these datasets and the common finding is that these datasets represent a multi-dimensional response from a complex non-linear system. There is also a lot of noise in the data and the effects of faults are masked by the 3 operating conditions, making analysis not so straightforward.  

The datasets are divided into 4 subsets and can possibly be combined together since the engines are not linked in the 4 sub-datasets. The strategy is to develop a model and test it against the 4 sub  datasets first before testing it on the combined dataset of 700+ engines that has various combinations of conditions and fault modes.  Various models can be explored and scored progressively, coupled with fine tuning of parameters and feature engineering.  This way, it may be easier to understand the interaction and importance of the various features in a smaller scale before going full swing on model selection.

The other alternative approach is to model on the combined data and derive the best model. This could potentially take up more time and iterations because of the complexity of the combinations of conditions and fault modes.

## Project Implementation

My broad plan for project implementation :

1. Understand the dataset [*completed*]

2. Decide what to solve - find the RUL (regression), or determine whether engine fails or not (classification). Finding RUL is the preferred option for practical application[*for now regression, if there is time i will try classification*]

3. Data preparation - sensor selection, feature engineering, noise filtering(?)[*completed preparing train,test,validation, scaling*]

4. Model creation and prediction - LR, GLM, SMV, NN? [*Ran the baseline LR model*]

5. Performance measurement - what metrics to use - R2, NASA's metrics ? Comparison against NASA benchmark and other technical publications.[*will use NASA benchmark. Created function to compute scoring metrics and computed the baseline LR metrics with it*]

6. Analysis

7. Conclusion & Recommendations


### Data inspection and cleaning (Completed)

The data is relatively clean with only two columns of 100% missing values. The 4 sub-datasets are combined into single dataset, with the engines renumbered in running order as each sub-dataset engine number starts with 1.

### Exploratory Data Analaysis (Completed)

#### Calculate RUL
The  datasets do not provide the remaining useful life RUL (feature/target y).  This has to be calculated from the dataset.

#### Heatmap & Distribution plots analysis

Analysis from heatmaps and distribution plots was done.
From the single engine and train datasets heatmaps, it was observed that there are some features that do not have correlation at all.
The op_settings in the train dataset do not have significant correlation with the sensors.

However for the combined dataset, there seems to be a large number of features that are highly correlated. The ops-settings are also negatively correlated to the sensors, which is not the case for a single engine EDA.  The distribution plots for the combined dataset also showed skewed and bimodal distributions. This could be due to some of the sub-datasets having more number of fault modes, allowing more possible state of failure and a possible continuation of the data when expecting failure.

#### Scaling/Normalizing Features

From the initial EDA, it can be seen that the sensor data ranges from min 0.02 to max 8243. There are also negative values for the op_settings data.  The op_setting and sensors features are thus normalized using StandardScaler.  The engine, cycle and computed RUL are not scaled.

#### Define Function to Calculate Aggregate Score for Model

NASA provided the scoring metrics formula, in which the final score is a weighted sum of RUL errors. The scoring function is an asymmetric function that penalizes late predictions more than the early predictions.  The scoring metrics has been created from the formula as a function.

## Create Baseline Regresson Model on Dataset 1 (Completed)

As this is a regression problem, and there is no naive base score available, a Linear Regression model is constructed to baseline the score, without removing any features or conducting feature engineering.

Linear, Ridge and Lasso regressions were used and compared. They were tested against the 4 sets of test datasets and Linear Regression was found to have the best aggregate scores among the 3 models :

|Dataset|No. of Engines|No. of Faults and Conditions|Aggregate Score|
|---|---|---|---|
|test_FD001|100|1 Fault, 1 Condition|2016.90|
|test_FD002|259|1 Fault, 6 Conditions|370047.87|
|test_FD003|100|2 Fault, 1 Condition|92182.30|
|test_FD004|248|2 Fault, 6 Condition|309479.21|

The poor scores for the 2nd to 4th test datasets are expected, as the model is only trained on the 1st dataset which has only 1 fault and 1 condition.

As a reference, the NASA top 20 aggregate scores range from 436.84 to 2430.42 for a unseen test set of 435 engines.

## Review / Seek Alternative Models

Tasks for the next 1 week :

1. Some possible models to work on include : SVM, Random Forest

2. To work on feature selection / extraction, feature engineering

3. If there is time, to look at Health Index (HI) methodology to predict RUL.  This methodology assumes that the HI of a system (engine in our case) degrades linearly over time. The RUL is predicted in 2 steps : 1) from input signals (sensors) to HI; and then 2) mapping HI to RUL.

## Feature engineering

## Benchmarking

## Analysis
