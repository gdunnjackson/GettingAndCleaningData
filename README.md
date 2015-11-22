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



The "run.analysis.R" script performs the steps below to perform analysis on the data collected from the accelerometers from the Samsung Galaxy S smartphone:

The data files must loaded to the working directory prior to running the script.

##  load libraries
library(data.table)
library(dplyr)

##  read input data files ####
X_test.txt
ytest.txt
X_train.txt
ytrain.txt
activity_labels.txt
subjtest.txt
subjtrain.txt
features.txt

##merge activity labels with the activities in ytestd and ytraind 

#subset ytestd and ytraind back to one column

##  rename columns in ytest,ytrain,subjtest,subjtrain to descriptive names (requirement 3)
The activity default column name V2 is renamed to activity in ytest and ytrain.
The subject default column name V1 is renamed to subject in subjtest and subjtrain.

##  add activity and subject columns to end of test and train data tables

## Merge the traning and test sets to create one data set (requirement 1)

##  select features required for measurements (mean and standard deviation)

##  appropriately label the column names for selected measures in testtraind (requirement 4)

## create a dataset containing only the selected measures (requirement 2)
                        
##  create a tidy dataset with the average of each variable for each activity and each subject (requirement 5)

The tidy dataset is named HAR_averages.txt, and it is sorted by subject,activity.


