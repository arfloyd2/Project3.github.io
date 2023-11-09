Angelice_Project3 Education Level: 1
================
Angelice Floyd

- [Reading in the Data](#reading-in-the-data)
- [Exploratory Data Analysis](#exploratory-data-analysis)
  - [Frequency of Diabetes Patients and Pre-Existing
    Conditions](#frequency-of-diabetes-patients-and-pre-existing-conditions)

Introduction : Insert Michael’s intro here

``` r
library(tidyverse)
library(caret)
library(GGally)
library(shiny)
```

# Reading in the Data

Below reads in the binary heart disease data, converts the appropriate
variables into factor level variables, and splits the data into training
and test set data.

``` r
initial_dta <- read_csv('diabetes_binary_health_indicators_BRFSS2015.csv') %>% mutate( Education1 = ifelse(Education %in% c(1,2),1,Education)) %>% filter(Education1 == params$edlvl)
```

    ## Rows: 253680 Columns: 22
    ## ── Column specification ──────────────────────────────────
    ## Delimiter: ","
    ## dbl (22): Diabetes_binary, HighBP, HighChol, CholCheck...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
cols <- c("Diabetes_binary","PhysActivity","AnyHealthcare","Education1","HighBP",
          "Smoker","Fruits","NoDocbcCost","Income","DiffWalk","HighChol","Stroke",
          "Veggies","GenHlth","Sex","CholCheck","HeartDiseaseorAttack","HvyAlcoholConsump",
          "MentHlth")
  
initial_dta[cols] <- lapply(initial_dta[cols],factor)

#set seed for reproducability 

set.seed(90)

#create a training index that uses 70% of the data for training data

trainindex <- createDataPartition(initial_dta$Diabetes_binary,p = 0.70, list= FALSE)

#Now, split the data into 70% training and 30% testing

initial_dta_train <- initial_dta[trainindex, ]
initial_dta_test <- initial_dta[-trainindex, ]

#To help make our predictions valid, we are now going to standardize the numeric variables

preprocval <- preProcess(initial_dta_train,method = c("center","scale"))

trainTransformed <- as_tibble(predict(preprocval,initial_dta_train))


testTransformed <- as_tibble(predict(preprocval,initial_dta_test))
```

# Exploratory Data Analysis

Because this data is in binary form, contingency tables and categorical
visuals will be helpful since it will show how the different levels of
those with and without diabetes compare to each other over levels of
other categorical variables or how values for the numerical variables
conpare at different levels for those with or without diabetes.

## Frequency of Diabetes Patients and Pre-Existing Conditions

This frequency table will show the number of patients with and without
diabetes who have( or do not have) high blood pressure, high
cholestroral, have had stroke or have had a heart attack or heart
disease.

``` r
PARAMS <- initial_dta %>% filter(Education1 == params$edlvl) 

BP <- data.frame(table(PARAMS$Diabetes_binary,PARAMS$HighBP)) %>% rename(Diabetes_binary="Var1", High_BP = "Var2", BP_Freq = "Freq")
#CHOL <- data.frame(table(initial_dta$Diabetes_binary, initial_dta$HighChol,initial_dta$params$edlvl)) %>% rename(Diabetes_binary="Var1", High_Chol = "Var2", HI_CHOL_Freq = "Freq" )
#STRKE <- as.data.frame(table(initial_dta$Diabetes_binary, initial_dta$Stroke,initial_dta$params$edlvl)) %>% rename(Diabetes_binary="Var1", Stroke = "Var2", sTROKE_Freq = "Freq" )
#HATCK <- as.data.frame(table(initial_dta$Diabetes_binary, initial_dta$HeartDiseaseorAttack,initial_dta$params$edlvl)) %>% rename(Diabetes_binary="Var1", Heart_Disease_Attack = "Var2", Heart_Disease_Attack_Freq = "Freq" )

#Diabetes_disease_frequency <- list("BP" = BP,"CHOL" = CHOL,"STRKE" = STRKE,"HATCK" = HATCK)
```
