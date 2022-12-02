# exsurv: Exposome Survival Module 
### Author: Changxin Lan (lancx_pku@foxmail.com)
### DATE: 2022-12-01
ExpoSurvival module is designed to conduct the survival analysis of the censored data. It mainly aims to evaluate the associations between exposure factors and health outcome, as well as predicting the survival probability. Cox proportional hazards regression model is used to calculate the hazard ratio (HR). Various machine learners are used to conduct the prediction analysis, including coxph, coxboost, xgboost, ranger, etc., from mlr3 R package.

Users can install the package using the following code:
```R
if (!requireNamespace("devtools", quietly = TRUE)){

	install.packages("devtools")

	devtools::install_github('ExposomeX/exsurv',force = TRUE)

	devtools::install_github('ExposomeX/extidy',force = TRUE) #"extidy" package is optional if the data file has been well prepared. However, the it is recommended as users may need tidy the data to meet the modeling requirement, such as deleting varaibles with low variance, transforming data type, classifying variable into several level, etc.
}

library(exsurv)
library(extidy) 
```

### Tips:
&nbsp;&nbsp;&nbsp;&nbsp;1.Before using the package, a user defined physical output path (i.e., OutPath) is recommended. For example
```R
OutPath = "D:/test" #The default path is the current working directory of R. Users can use this code to set the preferred path.
```
&nbsp;&nbsp;&nbsp;&nbsp;2.For each step, the returned values can be named as users' like by following R language requirement.<br>

&nbsp;&nbsp;&nbsp;&nbsp;3.All the PID must be the same with the one provided by InitMedt function, e.g., res$PID.

### Example codes:
#### 1. Initial Survival module:
```R
res = InitSurv()
res$PID
```
#### 2. Load data for survival analyses
```R
res2 <- LoadMedt(PID=res$PID,
	            UseExample = "example#1",
	            DataPath = NULL,
	            VocaPath = NULL)
res2$Expo$Data
res2$Expo$Voca
```
