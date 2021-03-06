﻿
##     Getting & Cleaning Data - Course Project    


###   Creating a Tidy Data Set from Raw Data

This project entails creating a tidy data set from the raw data sets provided.  The raw data was collected from a Human Activity Recognition experiment using Samsung  Galaxy X Smartphones.  Sensor signals generated by the Accelerometer and Gyroscope embedded in the smartphones were proceesed to create the raw data presented.

This document describes the sequence of steps taken to create the tidy data set.  It is is organized into the following sections.

1) The Raw Data: A description of the files in the raw data set.

2) Tidying the Data: A description of the steps taken to process the raw data.

3) The Script: Instructions for running the analysis script (in R code) developed to tidy the data.


####  1. The Raw Data

The raw data for this project is contained in space-separated text files (.txt) as follows:

The measured variables data provided are in two data sets, “train” and “test”, each with its own, eponymous directory.  Each data set has three files named beginning with “subject_”, “y_” and “X_” and appended with train or test for their respective data sets:

1) subject_ : Identifies the individual on whom the observations were made.

2) y_ : Codes for the activity performed by the individual during the observations.

3) X_ : The measured variables of the observation, in time domain and FFT form, obtained by processing the raw signals.

The analysis script use two other files in order to process the raw data in the subject_, y_ and X_ files into tidy data:

1) features.txt : A list with the names of the measured variables, used by the script because the X_ files do not have column names identifying the measured variables.

2) activity_labels.txt : Descriptive activity names to the codes present in the y_ files.

The test and train data sets also have a directory named “Inertial Signals” with files that contain the raw signal data.  These were not used in creating the tidy data [1].

The data set also has README.txt and features_info.txt files that provide information on the experiment. 

####  2. Tidying the Data

The analysis script creates a tidy data set through the following steps:

1) Create a single data set by merging the raw data in the training and test data sets.  The merged data set contains 10,299 obervations of 563 variables. One (1) fixed variable each from the subject_ and y_files, and 561 measured variables from the X_ files.

2) Extract only the measurements on the mean and standard deviation for each measurement.  The features.txt file is scanned for variables with "mean", "Mean"
or "std" present in their name [1]. Only these variables are retained from the merged data set from step 1. This reduces the merged data set to 86 variables.

3) Give descriptive activity names to the activities in the data set.

4) Label the data set with appropriate, descriptive variable names.  The required labels for the variable names are obtained from the features.txt file and processed to create syntactically correct R variable name.  The characters "-", ",", "(" and ")" are replaced by the "_", with not more than one "_" in sequence, for tidiness.  If the string "BodyBody" is present in a variable name, it is replaced by "Body".

If variable (column) names are not syntactically correct, R will substitute a “.” for any disallowed character when reading the data file – creating an untidy and possibly meaningless name to code with.  This can be suppressed by setting check.names = FALSE in the read command, but this will lead to a variable name that cannot be used in many R statements.

5) Create a second, independent tidy data set with the average of each variable for each activity and each subject, from the data set in step 4 [2].  The summarized data in the tidy data set has 180 observations on 86 variables – one observation for each of 6 Activities x 30 Subjects.  Each observation has 2 fixed variables and 84 measured variables.

6) The tidied data set form step 5 is tidied further to make it conform to tidy data norms. In tidy data [3]:

- Each observation forms a row.

- Each variable forms a column.

- Each observational unit forms a table.

The tidy data from step 5 violates the first two norms.

From the README.txt and features_info.txt files we know that the data in each row of the X_ files comes from the raw signals of an Accelerometer and a Gyroscope, both embedded in a Smartphone.  Each row therefore has data from 2 measurements – one from an Accelerometer and one from a Gyroscope – thereby violating the first norm.  They should be split into 2 separate rows.

In other words, this experiment does not have 2 Fixed variables, it has 3 – Activity, Subject and Device (Acc or Gyro).  A Fixed variable is one that describes the experimental design and is known before the observation is made.  A measured variable is known only after the observation is made.  

Since we clearly know whether the raw signal is from the Accelerometer or the Gyroscope before the measurement is done, we need to introduce a third fixed variable – Device, with 2 categorical values of Acc and Gyro.

A look at the variable names from features.txt will clarify matters further. The variable names can divided into the groups below, based on their prefixes.  The count includes only the 86 measured variables extracted in Step 2 (those with "mean", "Mean" or "std" in their name).

- tBodyAcc  : 16 variables

- tBodyGyro : 16 variables

- fBodyAcc  : 24 variables

- fBodyGyro : 15 variables

- tGravity  : 8 variables

- angle     : 7 variables

The number of tBodyAcc and tBodyGyro variables is the same.  This is because for every Accelerometer measurement there is a corresponding Gyroscope measurement. They are separate measurements, each measuring the acceleration of the same Subject performing the same Activity. Linear acceleration signals are from the Accelerometer, angular acceleration signals are from the Gyroscope. (The number of fBodyAcc and fBodyGyro should also be the same; discussed later.)

They form one observational unit since the measurements are of the same Subject performing the same Activity, at the same time.  Hence, it is correct to keep them in the same table. i.e. the third norm is not violated.

Consider the names of the variables themselves (after the processing in Step 4).

For example, "tBodyAcc_mean_X" and "tBodyGyro_mean_X".  These are more accurately (and tidily) termed (1) “tBody_mean_X”, Device = Acc, and (2) “tBody_mean_X”, Device = Gyro, and placed in separate rows. Likewise for all tBody, fBody and angle_tBody variables.

Combining the Accelerometer and Gyroscope measurements in one row causes one variable, in the above case “tBody_mean_X”, to be split across 2 columns – a violation of the second norm. 

It also leads to a variable VALUE to be present in a column variable NAME, a common problem in messy data sets.  In this case, a fixed variable value, Acc or Gyro, is present in all tBody and fBody column variable names.

The only Accelerometer variables that do not have a Gyroscope counterpart are the eight tGravity variables and the three angle_XYZ variables. This is as it should be since (a) the gravity signal was obtained from the Accelerometer via a 0.3 Hz low-pass filter, and (b) gravity causes linear acceleration, not angular acceleration (at least in local, non-relativistic environments such as this experiment).

Thus, out of the 86 selected measured variables, only 11 are Accelerometer measurements that do not have a Gyroscope counterpart.  So, by splitting a row of the Step 5 data set into an Acc row and a Gyro row, we reduce the number of columns to : 3 x Fixed + 16 x tBody + 24 x fBody + 8 x Gravity + 2 x angle_tBody + 3 x angle_XYZ = 56.  This is a reduction of 32 columns out of 88, while making the data set conform to tidy data norms.

Out of these 56 variables, only 11 (less than 20%) will properly have NA values in the Gyro rows, because they are “not applicable” to Gyroscope measurement.

The missing fBodyGyro variables: The fBody variables are derived from a Fast Fourier Transform (FFT) of the tBody variables.  One would expect, therefore that every tBody variable has an fBody counterpart. (There are also Fbody meanFreq variables that do not have a Time domain counterpart.)  So, since every tBodyAcc variable has a tBodyGyro counterpart, every fBodyAcc variable should have an fBodyGyro counterpart.  But, there are 9 fBodyAccJerk variables that do not have corresponding fBodyGyroJerk variables.

The features_info document does omit fBodyGyroJerk-XYZ from the list of FFT variables derived – no explanation is given.  But, the same document also omits fBodyAccMag from this list and it is present later on and in the raw data provided.  So, perhaps an error has taken place.  If so, the NA entries in the Gyro rows of these variables will serve as an indication that there are missing values that should have been measured. (Does it make senseto measure the FFT of an angular jerk magnitude and not the axial components?  Both FFTs are measured for linear jerk in the raw data.  Surely, rotation is as important for Human Activity Recognition as linear motion.)  In any case, the raw Gyro signals and relevant tBody Gyro measurements are available in the data if these missing values are needed for future study.  The NAs filled in this exercise serve as a placeholder and a reminder that these measurements are missing. 

The mis-named angle_tBodyAccMean_gravity variable:  There should be no such thing.  The angle is measured between 2 vectors, one of which could be gravityMean.  In the features_info document, gravity is not on the list of vectors from which the angle measurements are derived.  Moreover, all the other angle variables have gravityMean at the end.  So, this variable name has been amended to tBodyAccMean_gravityMean, which also gives it a Gyro counterpart.

The order of the variables: A tidy data set should have related variables placed near each other. The order of the variables has been modified accordingly (with Acc and Gyro measurements in 2 separate, alternating rows):

- tBody     : 16 columns

- fBody     : 24 variables

- tGravity  : 8 variables

- angle     : 7 variables

Further, the tBody... and fBody... variables all have XYZ variables with a single Mag (magnitude) variable which is calculated from their values (the Euclidean norm).  They are separated by intervening columns of other XYZ variables.  The position of the Mag variables has been altered to be immediately after their XYZ variables.

The tidy data set from this step has 360 observations on 56 variables, of which 3 are fixed variables and 53 are measured variables.  The new fixed variable is named Device.  It has 2 values - “Acc” and “Gyro”.  The data set is grouped by Activity, Subject and Device, with summarized measurements for each Activity and each Subject.

The “Gyro” rows each have 20 x NA values, 9 of which (“fBodyJerk...”) may be missing values that should be measured and placed in the data set.

Step 6 is labelled Step 5 (Part B) in the analysis script.


#### 3.  The Script

The script that creates the tidy data described in Step 6 of Tidying the Data is named run_analysis.R and is available at the following GitHub repository:

https://github.com/rswamidasan/Data_Science_GCD

To run the script successfully:

1. Place the script in your working directory.

2. Your working directory should contain the following files and folders:

         -  /train (with the X_, y_ and subject_ data files)

         -  /test  (with the X_, y_ and subject_ data files)

         -  features.txt

         -  activity_labels.txt

3. Run the following command in your R console: source(“run_analysis.R”)

4. The script will create the file “tidy_data.txt” in your working directory.

5. It takes less than 20 seconds to complete on a Windows 8.1 PC with Intel i3-4130 3.4 GHz CPU,
   8 GB RAM, 180 GB SSD (for Program files), 7200 rpm HDD (for Data files).

6. The tidy data file can be read in using the following R command:
   data <- read.table(“tidy_data.txt”, header = TRUE)

####  References :

[1] Hood, David: “David's Project FAQ”, Getting and Cleaning Data Discussion Forum,
    https://class.coursera.org/getdata-013/forum/thread?thread_id=30

[2] Hood, David: "Tidy data and the Assignment", Getting and Cleaning Data Discussion Forum,
    https://class.coursera.org/getdata-013/forum/thread?thread_id=31

[3] Wickham, Hadley: “Tidy Data”, Journal of Statistical Software, Vol. VV, Issue II, pp 3 – 8,
    http://www.jstatsoft.org/v59/i10/paper
