# Condition Based Maintenance

## Problem Statement

Traditional maintenance concept is time/schedule based and are reactive type of maintenance.  On the other hand, condition based maintenance is a pro-active type of maintenance that relies on early detection and prediction of potential failure to pre-empt sudden breakdowns and prolonged downtime.  Predicting early failures in maintenance will mitigate prolonged downtime and reduce maintenance cost while keeping equipment operationally ready and available.

Military equipment is often operated in harsh environment and are required to be readily available and operational 24/7.  Having the capability to predict potential equipment failures before they actually occur will enable planners to pro-actively maintain the equipment to be ready 24/7. There is considerable cost savings (for example by avoiding unscheduled maintenance and by increasing equipment usage) and operational safety improvements.

## Aim of Project

The aim this project is to build a model capable of forecasting the remaining useful life (the total amount of engine cycle that are left before failure).

For an engine degradation scenario an early prediction is preferred over late predictions. Therefore, the scoring algorithm for this project could be asymmetric around the true time of failure (provided we know or have this information in the dataset) such that late predictions will be more heavily penalized than early predictions.  

For this project, we do not have the actual or true time of failure, so the standard evaluation metric R2 score for regression modelling will be used.

## Data Source

Data source is from NASA. Dataset is on sensor measurements of turbofan engines (total of 708 engines).  The dataset is modelled after real life engine data and was presented in a technical paper reference “Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation”.

data source : https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan

## Data Description

Data sets consist of multiple multivariate time series. Each data set is further divided into training and test subsets. Each time series is from a different engine ñ i.e., the data can be considered to be from a fleet of engines of the same type.

Each engine starts with different degrees of initial wear and manufacturing variation which is unknown to the user. This wear and variation is considered normal, i.e., it is not considered a fault condition. There are three operational settings that have a substantial effect on engine performance. These settings are also included in the data. The data are contaminated with sensor noise.

The engine is operating normally at the start of each time series, and starts to degrade at some point during the series. In the training set, the degradation grows in magnitude until a predefined threshold is reached beyond which it is not preferable to operate the engine. In the test set, the time series ends some time prior to complete degradation.


## Data Dictionary

|S/No|Features|Description|Units|Data Type|
|---|---|---|---|---|
|1|engine_no|Engine number|-|int64|
|2|time_in_cycles|Time when engine is running|hours|int64|
|3|op_setting_1|Throttle|Resolver Angle|degrees	float64|
|4|op_setting_2|Mach| number|-|float64|
|5|op_setting_3|Altitude|	ft	|float64|
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
|27|RUL	|Remaining useful life|	cycle	|int64|


## Potential Challenges

The project is challenging enough and will allow me to utilise my understanding and knowledge of the various models that i have learnt during the course, with real practical application to modern military maintenance requirements.

The datasets are divided into 4 subsets and can be combined together with the assumption that the engines are not linked in the 4 sub-datasets. The strategy is to develop develop the model for the simplest test set (eg set 1 consisting of 100 engines with single test condition and single fault mode) and then test it on the combined dataset of 700+ engines that has various combinations of conditions and fault modes.  This way, it may be easier to understand the interaction and importance of the various features in a smaller scale.

The other alternative approach is to model on the combined data and derive the best model. This could potentially take up more time and iterations because of the complexity of the combinations of conditions and fault modes.



##
Is this a reasonable project given the time constraints that you have?

Yes. The project is challenging enough and will
