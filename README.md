# GettingAndCleaningData
This repository contains the files used in the Getting and Cleaning Data course

Introduction:

The purpose of this project is to demonstrate my ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis.  The project files include: 1) a tidy data set as described below, 2) a link to a Github repository with my script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that I performed to clean up the data called CodeBook.md. I will also include a README.md in the repo with my scripts. This repo explains how all of the scripts work and how they are connected.  

The data was sourced from the course website, and represents data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

 I created one R script called run_analysis.R that performs the following:
1	Merges the training and the test sets to create one data set.
2	Extracts only the measurements on the mean and standard deviation for each measurement. 
3	Uses descriptive activity names to name the activities in the data set
4	Appropriately labels the data set with descriptive variable names. 
 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity    and each subject.



A copy of the "run.analysis.R" script is shown below with comments to describe each step.

###  BEGIN SCRIPT
### NAME OF SCRIPT:  run_analysis.R
### DESCRIPTION:  This script is used to perform analysis on the data collected from the  
### accelerometers from the Samsung Galaxy S smartphone.
### AUTHOR: Gloria Jackson
### DATE:   November 20, 2015

## set working directory
## NOTE: set working directory prior to executing script

##  load libraries
library(data.table)
library(dplyr)

##  read input data files ####
xtestd <- data.table(read.delim("X_test.txt", header = FALSE, sep = ""))
ytestd <- data.table(read.delim("ytest.txt", header = FALSE, sep = ""))
xtraind <- data.table(read.delim("X_train.txt", header = FALSE, sep = ""))
ytraind <- data.table(read.delim("ytrain.txt", header = FALSE, sep = ""))
activityd <- data.table(read.delim("activity_labels.txt", header = FALSE, sep = ""))
subjtestd <- data.table(read.delim("subjtest.txt", header = FALSE, sep = ""))
subjtraind <- data.table(read.delim("subjtrain.txt", header = FALSE, sep = ""))
featuresd <- data.table(read.delim("features.txt", header = FALSE, sep = ""))

##  rename columns in ytest,ytrain,subjtest,subjtrain to descriptive names
ytestd <- rename(ytestd, activity = V1)
ytraind <- rename(ytraind, activity = V1)
subjtestd <- rename(subjtestd, subject = V1)
subjtraind <- rename(subjtraind, subject = V1)

##  add activity and subject columns to end of test and train data tables
addtestc <- cbind(xtestd,ytestd,subjtestd)
addtrainc <- cbind(xtraind,ytraind,subjtraind)

## Merge the traning and test sets to create one data set (step 1)
testtraind <- rbind(addtestc,addtrainc)

## Use descriptive activity names to name the activities in the dataset (step 3)
## assign descriptions to activities
testtraind$activity[testtraind$activity == "1"] <- "WALKING"
testtraind$activity[testtraind$activity == "2"] <- "WALKING_UPSTAIRS"
testtraind$activity[testtraind$activity == "3"] <- "WALKING_DOWNSTAIRS"
testtraind$activity[testtraind$activity == "4"] <- "SITTING"
testtraind$activity[testtraind$activity == "5"] <- "STANDING"
testtraind$activity[testtraind$activity == "6"] <- "LAYING" 

##  select features required for measurements (mean and standard deviation)
selectmeasures <- featuresd[grep("mean|std", featuresd$V2),]

##  appropriately label the column names for selected measures in testtraind (step 4)
setnames(testtraind,selectmeasures$V1,as.character(selectmeasures$V2))

## create a dataset containing only the selected measures (step 2)

extract_testtraind <- select(testtraind,contains("mean"),contains("std"),contains("activity"),contains("subject"))
                        
##  create a tidy dataset with the average of each variable for each activity and each subject (step 5)
by_var <- extract_testtraind %>% group_by(activity,subject)  ## define group by
avg_testtraind <- by_var %>% summarize_each(funs(mean))      ## calculate average
avg_testtraind <- setorder(avg_testtraind,"subject","activity")  ## sort results
write.table(avg_testtraind,file = "HAR_averages.txt",row.name = FALSE)  ## write results

## END SCRIPT

