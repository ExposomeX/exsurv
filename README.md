# exsurv: Exposome Survival Module 
### Author: Changxin Lan (lancx_pku@foxmail.com)
### Date: 2022-12-01
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
#### 1. Initial Survival module
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
#### 3. Tidy procedures(optional)
```R
res3 = TransImput(PID=res$PID,
                  Group="F",
                  Vars="all.x",
                  Method="lod")
res4 = TransType(PID=res$PID,
                 Vars="C1",
                 To="factor")
				 
res5 = TransScale(PID=res$PID,
                  Group="T",
                  Vars="all.x",
                  Method="normal")

res6 = TransDistr(PID=res$PID,
                  Vars="all.x",
                  Method="ln")
			 
res7 = TransDummy(PID=res$PID,
                   Vars="default")
```
See (https://github.com/ExposomeX/extidy) for more information.

#### 4. Find out covariates
```R
res8 = FindCovaSurv(PID=res$PID,OutPath=OutPath, TimeY = "Y1", EventY= 'Y2',
                    VarsC_Prior = "default", VarsC_Fixed = NULL, 
                    Method = "single.factor", Thr = 0.1)

res8$FindCovaSurv
```
#### 5.Association analyses
```R
res9 = SurvAsso(PID=res$PID,OutPath=OutPath, TimeY = "Y1", EventY= 'Y2',VarsX='all.x',
                VarsN="single.factor",VarsSel=T,IncCova=T)

res9$coxph_res
```
#### 6. Visualize association results
```R
res10 = VizSurvAsso(PID=res$PID,OutPath=OutPath,VarsN="single.factor",Layout="forest",
                   Brightness= "light",Palette = "default1")
res10$ForestPlot

res11 = VizSurvAsso(PID=res$PID,OutPath=OutPath,VarsN="single.factor",Layout="volcano",
                   Brightness= "light",Palette = "default1",
                   ColorFor= "p.value",SizeFor= "p.value")
res11$VolcanoPlot
```
#### 7.Predict the survival curve
```R
res12 = SurvPred(PID=res$PID,OutPath=OutPath, TimeY = "Y1", EventY= 'Y2',VarsX='all.x',
                IncCova=T,RsmpMethod="cv",Folds=3)

res12$bmrout
```
#### 8.Visualize prediction results
```R
res13 = VizSurvPred(PID=res$PID,OutPath=OutPath,Layout="all",Brightness="light",
                   Palette='default1')
res13$PredSurvCurvPlot
res13$BmrBarPlot
```

#### 9.Compare the survival curves of two groups
```R
res14 = VizSurvCompGroup(PID=res$PID,OutPath=OutPath,TimeY="Y1",EventY="Y2",VarsG="C3",
                  Model="coxph",VarsAdj='X1,X2,X3,X4,X5',AdjMethod='average',
                  Brightness="light",Palette='default1')
res14$Comp_Coxph_Plot[[1]]
```
#### 10.Exit
```R
FuncExit(PID = res$PID)
```
