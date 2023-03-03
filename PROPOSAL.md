# Analysis of Heart Disease Data to Predict Likelihood of Disease in an Individual.


# Introduction
Heart disease is the second most common cause of death in Canada. Coronary artery disease is one of the most prevalent kind of heart disease and can result in a heart attack. It has been proven that lifestyle modifications and, in certain situations, medication, can significantly lower your chance for developing heart disease. In this analysis we will aim to answer the question: what factors can help us predict whether an individual will have heart disease? To answer this question we will be using a heart disease dataset that has information on 303 patients with 14 columns containing data about different medical information. The class we want to predict is wether or not heart disease is present in an individual based on evaluating three predictors: cholesterol levels, blood sugar levels and age, which are all known factors that effect the probability of a patient being diagnosed with heart disease.


# Exploratory Data Analysis

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

We renamed the columns with names that we can understand and therfore manipulate easier.
Then we selected only the four columns that we are interested in examining, cholesterol levels, blood sugar levels, age and presence of heart disease.

```R
#Clean and wrangle your data into a tidy format
    #make column of disease presence binary (by making all values bigger than 1 equal to 1) 
disease_data$disease_presence <-ifelse(disease_data$disease_presence=="0",0,1)
disease_data <- disease_data |>
    #make disease presence a factor again since it was transformed into a double case
    mutate (disease_presence =as_factor(disease_presence))
    head(disease_data, n = 9)   

    #split data into training and testing so we can use training data seperatly
disease_split <- initial_split(disease_data, prop = 0.75, strata = disease_presence)  
disease_training <- training(disease_split)   
disease_testing <- testing(disease_split)
  head(disease_training, n = 3)
  head(disease_testing, n = 3)
```

Having the presence of heart disease be a binary factor will allow us to better visualise the data later on. 
Spliting the data is helpful because it allows us to ...

```R
#Using only training data, summarize the data in at least one table (this is exploratory data analysis). An example of a useful table could be one that reports the number of observations in each class, the means of the predictor variables you plan to use in your analysis and how many rows have missing data.
#We created a table that groups the data into individuals who present heart disease and those who don't, and allows us to see the difference between the means of the three data columns that we are intereted in. 
disease_summary <- disease_training |> 
    group_by(disease_presence)|>
    summarize(avg_age=mean(age), avg_blood_pressure=mean(blood_pressure), avg_cholesterol=mean(cholesterol))
disease_summary
```

This summary of the data shows the mean age, blood pressure levels, and cholesterol levels of individuals where heart disease is absent (the 0 row) and where it is present (the 1 row), which allos us to compare the information and draw an idea about how these variables might play a role in increasing the risk of heart disease for an individual.

```R
#Using only training data, visualize the data with at least one plot relevant to the analysis you plan to do (this is exploratory data analysis). 
#An example of a useful visualization could be one that compares the distributions of each of the predictor variables you plan to use in your analysis.
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

These three graphs help us to visulalise the distribution of patient data and allows us to compare the ages , cholesterol levels and blood sugar levels of individulas with and without a diagnosis for heart disease. 


# Method
We will use the variables age, cholesterol level, and blood pressure level because they are all known factors in the likelihood of an individual to develope heart disease. The older a patient is the higher the risk of heart disease becomes especially above the age of 65. The higher a patients cholesterol levels, the likely it is that thier arteries can get damaged and cloged which can result in heart disease. Similarly, if patients have high blood sugar levels, they are more likely to have damaged blood vessels and nerves that control their heart, which increases the risk of heart disease. 

Sources: https://memorialhermann.org/services/specialties/heart-and-vascular/healthy-living/education/heart-disease-and-age , https://familyheart.org/cholesterol-is-key , https://www.cdc.gov/diabetes/library/features/diabetes-and-heart.html

Using the data from above that we have loaded into R, we can now conduct our analyses by forming 3 main plots: age v.s. presence of heart disease, cholesterol levels v.s. presence of heart disease, and blood sugar levels v.s. presence of heart disease. These plots will have to be separated into something such as facet grids to factor in things such as sex, whether or not the patient has angina, thalach (maximum heart rate achieved), etc. Using the data, we will form scatterplots to give us a clear visualization of any common trends. 

To analyse this data we will preprocess the data and make it suitable for use in a classifier, so that we can use our observed data to make predictions. First we will ... 

One way we will visualize the results ... 

(Explain how you will conduct either your data analysis
Describe at least one way that you will visualize the results)


# Expected Outcome/Significance
What do you expect to find?
What impact could such findings have?
What future questions could this lead to?

```R

```
