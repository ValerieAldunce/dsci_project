# dsci_project



Title: Analysis of Heart Disease Data to Predict Likelihood of Disease in an Individual.


Introduction: Heart disease is the second most common cause of death in Canada. Coronary artery disease is one of the most prevalent kind of heart disease and can result in a heart attack. It has been proven that lifestyle modifications and, in certain situations, medication, can significantly lower your chance for developing heart disease. In this analysis we will aim to answer the question: what factors can help us predict whether an individual will have heart disease? To answer this question we will be using a heart disease dataset that has information on 303 patients with 14 columns containing data about different medical information. The class we want to predict is wether or not heart disease is present in an individual based on evaluating three predictors: cholesterol levels, blood sugar levels and age, which are all known factors that effect the probability of a patient being diagnosed with heart disease.

```R
#Demonstrate that the dataset can be read from the web into R
 #read data 
library(tidyverse)
library(tidymodels)
heart_disease <- read.csv("https://archive.ics.uci.edu/ml/machine-learning-databases/heart-disease/processed.cleveland.data")
    #rename columns
    names(heart_disease) <- c("age", "sex", "chest_pain", "blood_pressure", "cholesterol", "blood_sugar", "EKG", "heart_rate", "angina", "ST_depression", "ST_slope", "fluro", "thallium", "presence_of_disease")
disease_data <- heart_disease |>
    #make wether or not a patient has the diseaes a fct
    mutate (disease_presence =as_factor(presence_of_disease))|>
    #select the three predictors that we want to examine
    select (age, blood_pressure, cholesterol, disease_presence)
 #display the first 9 rows of the data
head(disease_data, n = 9)

```

```R
#Demonstrate that the dataset can be read from the web into R
 #read data
library(tidyverse)
library(tidymodels)
heart_disease <- read.csv("https://archive.ics.uci.edu/ml/machine-learning-databases/heart-disease/processed.cleveland.data")
    #rename columns
    names(heart_disease) <- c("age", "sex", "chest_pain", "blood_pressure", "cholesterol", "blood_sugar", "EKG", "heart_rate", "angina", "ST_depression", "ST_slope", "fluro", "thallium", "presence_of_disease")
disease_data <- heart_disease |>
    #make wether or not a patient has the diseaes a fct
    mutate (disease_presence =as_factor(presence_of_disease))|>
    #select the three predictors that we want to examine
    select (age, blood_pressure, cholesterol, disease_presence)
 #display the first 9 rows of the data
head(disease_data, n = 9)

```

```R
#Clean and wrangle your data into a tidy format
    #make column of disease presence binary (by making all values bigger than 1 equal to 1)
#? make 2=1
#? make 3=1
#? make 4=1
#? make 5=1
#    head(disease_data, n = 9)    

    #split data into training and testing so we can use training data seperatly
disease_split <- initial_split(disease_data, prop = 0.75, strata = disease_presence)  
disease_training <- training(disease_split)   
disease_testing <- testing(disease_split)
  head(disease_training, n = 3)
  head(disease_testing, n = 3)
```

```R
#Using only training data, summarize the data in at least one table (this is exploratory data analysis). An example of a useful table could be one that reports the number of observations in each class, the means of the predictor variables you plan to use in your analysis and how many rows have missing data.
#We created a table that groups the data into individuals who present heart disease and those who don't, and allows us to see the difference between the means of the three data columns that we are intereted in. 
disease_summary <- disease_training |> 
    group_by(disease_presence)|>
    summarize(avg_age=mean(age), avg_blood_pressure=mean(blood_pressure), avg_cholesterol=mean(cholesterol))
disease_summary
```

```R
#Using only training data, visualize the data with at least one plot relevant to the analysis you plan to do (this is exploratory data analysis). An example of a useful visualization could be one that compares the distributions of each of the predictor variables you plan to use in your analysis.
#We created a plot that shows the difference in the 
disease.age_plot <- ggplot(disease_training, aes(x=age, fill=disease_presence, color=disease_presence)) +
          geom_histogram(bins=15)+
          xlab("age") +
          ylab("number of individuals") + 
          theme(text = element_text(size = 12)) 
disease.age_plot
 
disease.bloodpressure_plot <- ggplot(disease_training, aes(x=blood_pressure, fill=disease_presence, color=disease_presence)) +
          geom_histogram(bins=15)+
          xlab("blood pressure level") +
          ylab("number of individuals") + 
          theme(text = element_text(size = 12)) 
disease.bloodpressure_plot

disease.cholesterol_plot <- ggplot(disease_training, aes(x=cholesterol, fill=disease_presence, color=disease_presence)) +
          geom_histogram(bins=15)+
          xlab("cholesterol level") +
          ylab("number of individuals") + 
          theme(text = element_text(size = 12)) 
disease.cholesterol_plot
```

<!-- #region -->
Method:
Create a knn model of the training data


Expected Outcome/Significance:
<!-- #endregion -->

```R

```
