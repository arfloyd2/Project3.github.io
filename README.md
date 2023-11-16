# Project 3 Group 15 Angelice Floyd and Michael Dolan

The purpose of this repo is to hold the R Markdown code, visualizations, and rendered output files for modelling and data analyses using the Diabetes Binary data from Diabetes Health Inidicators Data set. The output file are produced based on various educational levels of the survey participants that range from elementary school or before to college graduates. Each file will contain exploratory data analysis that observe the interaction between health, age lifestyle indicators and the binary response of whether or not a subject has been told by a physician that they have diabetes. Later, a series of predictive models will be fit using statistical and machine learning methods. For the exploratory data analysis, some of the main predictors used were body mass index level, age group, lifestyle habits such as physical healh scores, nutritous food consumption and whether or not a subject engaged in physical activity in the past 30 days, and pre-existing health issues and indcators such as whether or not a subject has hih blodd pressure, if they have had a stroke, have high cholesterol, and if they have had a heart attack. This analysis also observes the interaction between the diabetes response, age and number of days a participant reported having poor or concerning mental health. The predictive and machine learning models consider all of the varibles.  

Below is the list of R Packages used  
  - tidyverse  
  - caret  
  - GGally  
  - shiny  
  - rmarkdown  
  - MASS  
  - glmnet  
  - randomForest  
  - logbin

Below is the code used for the output

initial_dta <- read_csv('diabetes_binary_health_indicators_BRFSS2015.csv') %>% 
          mutate(Education_1 = ifelse(Education %in% c(1,2),1,Education)) %>%
          mutate(Education_Level = ifelse(Education_1==1, "Elementary or less",
                                    ifelse(Education_1==3, "Some high school",
                                    ifelse(Education_1==4, "High school graduate",
                                    ifelse(Education_1==5, "Some college or technical school",
                                    ifelse(Education_1==6, "College graduate", "ERROR"))))))

Education_params <- unique(initial_dta$Education_Level)
output_file <- paste0("Project 3, ", Education_params, ".md")
params <- lapply(Education_params, FUN = function(x){list(Education_Level=x)})
reports <- tibble(output_file, params)

apply(reports, MARGIN = 1,
      FUN = function(x){render(input = "Project3.Rmd",
                               output_file = x[[1]], params = x[[2]],
                               output_options = list(
                                name_value_pairs = "value", 
                                toc = TRUE,
                                toc_depth = 4, 
                                number_of_sections = TRUE, 
                                df_print = "paged",
                                html_preview = FALSE))})  


Finally, below are the links to the html files rendered by github pages 

[Elementary or less]()
[Some high school]()
[High school graduate]()
[Some college or technical school]()
[College graduate]()


