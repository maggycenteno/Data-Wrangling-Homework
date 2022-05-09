# Data-Wrangling-Homework
Showcasing an assignment from my Data Wrangling class.

# Data Wrangling Homework 5
> For this assignment, we answered a series of questions using RStudio code.

## General Information
For this assignment, we were asked to utilize a variety of RStudio functions we had learned in class. We executed tasks such as creating data frames, proportional tables, utilizing correlation coefficients, and more.

## Technologies Used
RStudio
Microsoft Excel

##Code used
####Question 1
food<-read.csv("food.txt", na.strings = c("n/a"), header = TRUE, stringsAsFactors = TRUE, sep = "\t")
food$Total<-food$Sushi + food$Alligator + food$FrogLegs + food$Caviar + food$Fugu + food$Lutefisk
food$Continent<-factor(food$Continent, labels = c("Other", "Other", "Other", "North America", "Other", "Other"))

####Question 2
caviar<-prop.table(table(food$Caviar, food$Continent), margin = 2)
alligator_test<-chisq.test(table(food$Gender, food$Alligator))
#p-value is 0.1669 so we cannot reject the null hypothesis (having eaten alligator and gender are independent)

####Question 3
age_total<-cor(food$Age, food$Total)
#correlation coefficient is 0.25 which is a weak/positive linear relationship

####Question 4
#install.packages("RSocrata")
library(RSocrata)

url<-"https://noaa-fisheries-afsc.data.socrata.com/resource/vsba-nbxa.json?$select=date,habitat,treatment,carapace_width&$where=experiment='2011'"
crab<-read.socrata(url)
crab$carapace_width<-as.numeric(crab$carapace_width)

#Question 5
library(dplyr)

treatment_summary<-summarize(group_by(crab, treatment), Total_Crabs = n(), Median_Size = median(carapace_width))
treatment_test<-t.test(crab[crab$treatment == "5", "carapace_width"], crab[crab$treatment== "20", "carapace_width"])

#p-value is 7.93e-08, so we can reject the null hypothesis (avg carapace width is = for the treatment conditions)
hist(crab$carapace_width)

## Usage
To utilize the code I wrote to answer these questions, I could either send you the Rdata file or the Rstudio file for you to run the code on your own computer.

## Room for Improvement
Somewhere I could improve in my work is to use #comments to describe exactly what my code is going to do so that the reader knows/understands my thought process.

## Acknowledgements
This assignment was created by Mike Colbert for the University of Iowa's Data Wrangling Class.
