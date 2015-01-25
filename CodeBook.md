

##     Getting & Cleaning Data - Course Project

###	Code Book


####  Introduction

This Code Book describes the methodology used in arriving at a tidy data set for an experiment in Human Activity Recognition using Smartphones.  The experiments were conducted by Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio and Luca Oneto of the Smartlab - Non Linear Complex Systems Laboratory at DITEN - Università degli Studi di Genova, Genoa, Italy in November 2012.  

The raw data collected in this experiment were processed to create the tidy data set using a script written in R code.

The Code Book is comprised of the following sections:

1. Study Design

2. Raw Data Generation

3. Data Summarization and Cleaning

4. Data Dictionary



#### 1.  Study Design [1, 2]

The experiments were carried out with 30 volunteers who performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) while wearing a Samsung Galaxy S II smartphone.  The smartphone sensor emitted 3-axial linear acceleration and angular velocity signals  (tAcc-XYZ and tGyro-XYZ) at 50Hz from an embedded accelerometer and gyroscope. The experiments were video-recorded to label the activity data manually.

The resulting dataset was randomly partitioned into a training data set (71%) and a test data set (29%)).

Frequencies above 20 Hz were filtered out in both signals to remove noise. Samples of the clean signals were collected in windows of 2.56 seconds with 50% overlap.  At 50 Hz this results in 128 readings per window for each signal.  The tAcc-XYZ signal was then split into body signals (above 0.3 Hz) and gravity acceleration signals, to give 3 base signals:

1. tBodyAcc-XYZ	: 3 XYZ components
2. tGravityAcc-XYZ	: 3 XYZ components
3. tBodyGyro-XYZ	: 3 XYZ components

The following 7 signals were derived from these base signals:

1. tBodyAccJerk-XYZ	: time derivative of tBodyAcc-XYZ, 3 XYZ components
2. tBodyGyroJerk-XYZ	: time derivative of tBodyAcc-XYZ, 3 XYZ components
3. tBodyAccMag		: Euclidean norm of tBodyAcc-XYZ
4. tGravityAccMag	: Euclidean norm of tGravityAcc-XYZ
5. tBodyAccJerkMag	: Euclidean norm of tBodyAccJerk-XYZ
6. tBodyGyroMag	: Euclidean norm of tBodyGyro-XYZ
7. tBodyGyroJerkMag	: Euclidean norm of tBodyGyroJerk-XYZ

Then, a Fast Fourier Transform (FFT) was applied to some of the signals giving 7 more signals:

1. fBodyAcc-XYZ	: 3 XYZ components
2. fBodyAccJerk-XYZ	: 3 XYZ components
3. fBodyGyro-XYZ	: 3 XYZ components
4. fBodyAccMag
5. fBodyAccJerkMag
6. fBodyGyroMag
7. fBodyGyroJerkMag

The naming convention uses the prefix “t” to denote a time domain signal, while the “f” prefix denotes a frequency domain signal produced by an FFT. “-XYZ” denote a 3-axis signal in the X, Y and Z directions.  “Mag” denotes the Euclidean norm of a 3-Axis signal.


#### 2.  Raw Data Generation

A set of statistical variables estimated from these signals are the measurements presented in the raw data set.  First, it would help to list the signals as follows:

1. t-XYZ signals	: 5 signals, each with 3 x XYZ components
2. t-Mag signals	: 5 signals, no XYZ components
3. f-XYZ signals	: 3 signals, each with 3 x components
4. f-Mag signals	: 4 signals, no XYZ components

The statistical variables in the lists below are estimated from the signals.

The following generate 3 variables for t-XYZ and f-XYZ signals, one per axial component, and 1 variable for t-Mag and f-Mag signals.

1. mean	: Mean value
2. std	: Standard deviation
3. mad	: Median absolute deviation 
4. max	: Largest value in array
5. min	: Smallest value in array
6. sma	: Signal magnitude area
7. energy: Energy measure. 
8. Iqr	: Interquartile range 
9. entropy: Signal entropy

The following generates 4 variables for each axial component of t-XYZ signals and 4 variables for t-Mag signals. (Not applicable to f-XYZ and f-Mag.)

1. arCoeff(): Auto-regresion coefficients with Burg order equal to 4

The following generates 3 variables for t-XYZ signals.

1. correlation(): correlation coefficient between two signals

The following generates 3 variables for f-XYZ signals and 1 variable for f-Mag signals.

1. MaxInds	: index of the frequency component with largest magnitude
2. meanFreq	: Weighted average of the frequency components
3. skewness	: skewness of the frequency domain signal 
4. kurtosis	: kurtosis of the frequency domain signal 

The following generates 42 variables for f-XYZ signals.  (There is a discrepancy here – The document says 64 bins [2], but the data shows only 42 variables [3].

1. bandsEnergy	: Energy of a frequency interval in the 64 bins.

From the above, different categories of signals give rise to a differing number of measured variables, as follows:

1. t-XYZ signals	: 40 variables each
2. t-Mag signals	: 13 variables each
3. f-XYZ signals	: 79 variables each
4. f-Mag signals	: 13 variables each

Additional vectors obtained by averaging the signals in a signal window sample:

1. gravityMean
2. tBodyAccMean
3. tBodyAccJerkMean
4. tBodyGyroMean
5. tBodyGyroJerkMean

The angle between the gravityMean vector and each of the other four vectors in the list above gives rise to 4 angle variables.

Further, there are 3 additional angle variables between XYZ vectors (presumably referring to XYZ axes of the smartphone) and gravityMean that are present in the data [3] but not referred to in the document [2].

Thus we arrive at the following total number of variables:

1. t-XYZ : 5 signals x 40 variables 
2. t-Mag : 5 signals x 13 variables 
3. f-XYZ : 3 signals x 79 variables 
4. f-Mag : 4 signals x 13 variables 
5. angle	: 4 + 3 = 7 variables

The end result is a data set with the following characteristics


1. The total number of measured variables = 561 
2. Number of fixed variables = 2 (Activity and Subject) 
3. Number of observations = 10,299 
4. Number of observations in the training data set = 7,352
5. Number of observations in the test data set = 2,947

The measured variable values have been normalized to fit within [-1, 1].

This raw data is contained in space-separated text files (.txt) contained in a directory named “UCI HAR Dataset”.

Each data set is placed in a separate directory - “train” and “test”.  Each directory has three files named beginning with subject_, y_ and X_ and appended with train or test for their respective data sets: 

1. subject_ : Identifies the individual on whom the observations were made. 

2. y_ : Codes for the activity performed by the individual during the observations. 

3. X_ : 561 measured variables for each the observation. 

Two other data files are present in the UCI HAR Dataset directory: 

1. features.txt : A list with the names of the measured variables. 

2. activity_labels.txt : Descriptive activity names for the codes present in the y_ files. 

The test and train data sets also have a directory named “Inertial Signals” with files that contain the raw signal data.  These were not needed to create the tidy data [4]. 

The “UCI HAR Dataset” directory also has README.txt and features_info.txt files that provide information on the experiment.


#### 3.  Data Summarization and Cleaning

An analysis script in R code was written to create a tidy data set through the following steps: 

1. Merge the data:  A single data set was formed by merging the raw data in the training and test data sets.  

2. Extract summary data: Only the mean and standard deviation for each observation were retained from the merged data set of step 1.  The features.txt file is scanned for variables with "mean", "Mean" or "std" present in their name to identify the required variables[4]. This reduced the merged data set to 86 variables. 

3. Name the Activities:  The activities were given  descriptive activity names by matching and replacing the codes with the labels in the activity_labels.txt file. 

4. Give the variables descriptive names:  The variable names from the features.txt file were processed to create tidier, syntactically correct R variable names.  The characters "-", ",", "(" and ")" are replaced by the "_", with not more than one "_" in sequence.  Mistakes such as "BodyBody" in a variables names were corrected to “Body”.  The mis-named angle_tBodyAccMean_gravity variable was appended with “Mean”.

5. Summarize the data by activity and subject:  The data set was summarized to show average of each variable for each activity and each subject.  The summarized data in the tidy data set has 180 observations on 86 variables  one observation for each of 6 Activities x 30 Subjects.

6. Ensure that the data set complies with tidy data norms. The data set was found to violate the following tidy data norms [5]. 

i. Each observation forms a row

ii. Each variable forms a column.

The measurements in each row of the data set comes from 2 raw signals  – one from an Accelerometer and the other from a Gyroscope.   These are 2 separate observations, one from each device, which should be split into 2 separate rows to conform with the first norm listed above.  Since we know whether the raw signal is from the Accelerometer or the Gyroscope before the measurement is done the device is a third fixed variable in this experiment.

Placing Gyro and Acc variables in the same row also cause the second norm above. Consider the variable names.  Out of 86 measured variables in the current summarized data set, 66 measured variables can be paired with another having the same name except for the “Acc” or “Gyro” string.  

For example, "tBodyAcc_mean_X" and "tBodyGyro_mean_X".  These are more accurately  termed (1) tBody_mean_X, Device = Acc, and (2) tBody_mean_X, Device = Gyro, and placed in separate rows.  There are 33 such instances where a variable is split across 2 columns, with the added problem that a variable value (in this case “Acc” or “Gyro”) is present in a variable name.

For these reasons, each row has been split into 2 – one row with Accelerometer data, the other with Gyroscope data.  A third fixed variable, “Device” has been added to identify “Acc” rows and “Gyro” row.  Refer to the accompanying README.md file for a detailed discussion of this issue [6].

This row splitting action creates a data set  with 360 observations on 56 variables, of which 3 are fixed variables and 53 are measured variables.  The Gyro rows each have 20 x NA values.  Some of them are clearly inapplicable to Gyroscope measurements.  These are the 8 tGravity variables, which are derived entirely from the tAcc-XYZ raw signal, and the 3 angle_XYZ variables.

The other 9 NA values in the variables starting with fBodyJerk, since, as noted in section 1 of this document, FFTs were applied to tBodyAccJerk-XYZ but not to tBodyGyroJerk-XYZ.  In all other cases when an FFT was applied to a tBodyAcc signal, it was also applied to the corresponding tBodyGyro signal.  No explanation is given for this anomaly and the features_info.txt file is inconsistent in other FFT matters.  For instance, fBodyAccMag is not mentioned in the list of frequency domain variables produced by FFT, but it appears later in the same document and in the data files.

Perhaps an error has taken place and these missing values should be measured and placed in the data set.  If so, the NA values will bring these missing values to attention.  In any event, the signals for generating these variables is present in the “Inertial Signals” directory.

Finally, the order of the columns has been altered so that related variables are grouped together, in this order:

1. tBody 	: 16 columns 

2. fBody	: 24 variables 

3. tGravity	: 8 variables 

4. angle	: 7 variables 

The position of the Mag variables has been altered to be immediately after their respective XYZ variables.  The order of the variables can be seen in the Data Dictionary in section 4. 

The tidy data set as a result of these operations has 360 observations on 56 variables, of which 3 are fixed variables and 53 are measured variables.  The new fixed variable is named Device.  It has 2 values - Acc and Gyro.  The data set is grouped by Activity, Subject and Device, with summarized measurements for each Activity and each Subject.

The new measured variable names reflect the fact that Acc and Gyro variables have been merged into one (with the values split across 2 rows).  The strings “Acc” and “Gyro” have been stripped out, giving  shorter, tidier names.

An analysis script in R code was developed to perform the operations described in this section.  The script writes the tidy data set to a disk file in the working directory named “tidy_data.txt”.  Instructions on running the script are given in the README file accompanying this Code Book [6].



#### 4.  Data Dictionary
 

The data file contains the following 56 fields in space-separated format.  There are 3 Fixed variables, in columns 1 through 3, and 53 measured variables (columns 4 through 56).

Each measured or variable is normalized to lie within [-1, 1] and is stored in the data file as a numeric with fifteen (15) decimal places.  Since the data are normalized, they do not have units.

This data set contains measured variable values that have been summarized by Activity and Subject. Every measured variable below must be understood to be the mean of all measurements of that variable for a given combination of Activity and Subject.


1	Activity	:	Character; A Fixed variable. 
				The activity for which this observation was collected, 				identified by the following Activity Label values. 

				WALKING
				WALKING_UPSTAIRS
				WALKING_DOWNSTAIRS
				SITTING
				STANDING
				LAYING 

2	Subject	:	Integer, Range: 1 to 30; A Fixed variable.
				An integer between 1 and 30 used to identify the 					individual who performed the Activity.
			
3 	Device	:	Character; A Fixed variable
				The device from which the raw signal was captured

				Acc 	: Accelerometer measurement
				Gyro 	: Gyroscope measurement

4 	tBody_mean_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean X-Axis acceleration

5 	tBody_mean_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean Y-Axis acceleration

6 	tBody_mean_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean Z-Axis acceleration

7 	tBodyMag_mean	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean Euclidean norm of X, Y & Z-Axis acceleration

8 	tBody_std_X		:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of X-Axis acceleration

9 	tBody_std_Y		:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of Y-Axis acceleration

10 	tBody_std_Z		:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of Z-Axis acceleration

11 	tBodyMag_std	:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of the Euclidean norm of 
					X, Y & Z-Axis acceleration

12 	tBodyJerk_mean_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean X-Axis Jerk

13 	tBodyJerk_mean_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean Y-Axis Jerk

14 	tBodyJerk_mean_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean Z-Axis Jerk

15 	tBodyJerkMag_mean	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean Euclidean norm of X, Y & Z-Axis Jerk

16 	tBodyJerk_std_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of X-Axis Jerk

17 	tBodyJerk_std_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of Y-Axis Jerk

18 	tBodyJerk_std_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of Z-Axis Jerk

19 	tBodyJerkMag_std	:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of the Euclidean norm of 
					X, Y & Z-Axis Jerk

20 	fBody_mean_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean of FFT of X-Axis Acceleration

21 	fBody_mean_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean of FFT of Y-Axis Acceleration

22 	fBody_mean_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean of FFT of Z-Axis Acceleration

23 	fBodyMag_mean	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean of FFT of Euclidean norm of X, Y & Z-Axis 						Acceleration 

24 	fBody_std_X		:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of FFT of X-Axis Acceleration

25 	fBody_std_Y		:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of FFT of Y-Axis Acceleration

26 	fBody_std_Z		:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of FFT of Z-Axis Acceleration

27 	fBodyMag_std	:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of FFT of Euclidean norm of
					X, Y & Z-Axis Acceleration

28 	fBody_meanFreq_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					Weighted Average of Frequency components of FFT
					of X-Axis Acceleration

29 	fBody_meanFreq_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					Weighted Average of Frequency components of FFT
					of Y-Axis Acceleration

30 	fBody_meanFreq_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					Weighted Average of Frequency components of FFT
					of Z-Axis Acceleration

31 	fBodyMag_meanFreq	:	Numeric, 15 decimal places, normalized [-1, 1]
					Weighted Average of Frequency components of FFT
					of Euclidean norm of X, Y & Z-Axis Acceleration	

32 	fBodyJerk_mean_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Mean of FFT of X-Axis Jerk

33 	fBodyJerk_mean_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Mean of FFT of Y-Axis Jerk

34 	fBodyJerk_mean_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Mean of FFT of Z-Axis Jerk

35 	fBodyJerkMag_mean	:	Numeric, 15 decimal places, normalized [-1, 1]
					Mean of FFT of Euclidean norm of X, Y & Z-Axis
					Jerk

36 	fBodyJerk_std_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Standard Deviation of FFT of X-Axis Jerk

37 	fBodyJerk_std_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Standard Deviation of FFT of Y-Axis Jerk

38 	fBodyJerk_std_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Standard Deviation of FFT of Z-Axis Jerk

39 	fBodyJerkMag_std	:	Numeric, 15 decimal places, normalized [-1, 1]
					Standard Deviation of Euclidean norm of X, Y & 
					Z-Axis Jerk

40 	fBodyJerk_meanFreq_X	: Numeric, 15 decimal places, normalized [-1, 1]
					  NA for Device = Gyro
					  Weighted Average of Frequency components of FFT
					  of X-Axis Jerk

41 	fBodyJerk_meanFreq_Y	: Numeric, 15 decimal places, normalized [-1, 1]
					  NA for Device = Gyro
					  Weighted Average of Frequency components of FFT
					  of Y-Axis Jerk

42 	fBodyJerk_meanFreq_Z	: Numeric, 15 decimal places, normalized [-1, 1]
					  NA for Device = Gyro
					  Weighted Average of Frequency components of FFT
					  of Z-Axis Jerk

43	fBodyJerkMag_meanFreq	: Numeric, 15 decimal places, normalized [-1, 1]
					  Weighted Average of Frequency components of FFT 					  of Euclidean norm of X, Y & Z-Axis Jerk

44 	tGravity_mean_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Mean Gravity Acceleration in X-Axis

45 	tGravity_mean_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Mean Gravity Acceleration in Y-Axis

46 	tGravity_mean_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Mean Gravity Acceleration in Z-Axis

47 	tGravityMag_mean	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Mean of Euclidean norm of Gravity Acceleration 
					in X, Y & Z-Axes

48 	tGravity_std_X	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Standard Deviation of Gravity Acceleration in 
					X-Axis

49 	tGravity_std_Y	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Standard Deviation of Gravity Acceleration in 
					Y-Axis

50 	tGravity_std_Z	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Standard Deviation of Gravity Acceleration in 
					Z-Axis

51 	tGravityMag_std	:	Numeric, 15 decimal places, normalized [-1, 1]
					NA for Device = Gyro
					Standard Deviation of Euclidean norm of Gravity 					Acceleration in X, Y & Z-Axes

52 	angle_tBodyMean_gravityMean	: Numeric, 
						  15 decimal places, normalized [-1, 1]
						  Angle between the Mean Acceleration and 						  Mean Gravity vectors in a sampling window

53 	angle_tBodyJerkMean_gravityMean	: Numeric, 15 decimal places,
							  normalized [-1, 1]
						  Angle between the Mean Jerk and Mean					  		  Gravity vectors in a sampling window

54 	angle_X_gravityMean	: Numeric, 15 decimal places, normalized [-1, 1]
					  NA for Device = Gyro
					  Angle between the X-Axis and Mean	Gravity vectors		  		 	  in a sampling window				

55 	angle_Y_gravityMean	: Numeric, 15 decimal places, normalized [-1, 1]
					  NA for Device = Gyro
					  Angle between the Y-Axis and Mean	Gravity vectors		  		 	  in a sampling window				

56 	angle_Z_gravityMean	: Numeric, 15 decimal places, normalized [-1, 1]
					  NA for Device = Gyro
					  Angle between the Z-Axis and Mean	Gravity vectors 					  in a sampling window




##### References

[1] Reyes-Ortiz, Jorge L., et al: README.txt file given with the data set.

[2] Reyes-Ortiz, Jorge L., et al: features_info.txt file given with the data
                                  set.

[3] Reyes-Ortiz, Jorge L., et al: features.txt data file in the data set.

[4] Hood, David: David's Project FAQ, Getting and Cleaning Data Discussion Forum, https://class.coursera.org/getdata-010/forum/thread?thread_id=49 

[5] Wickham, Hadley: Tidy Data, Journal of Statistical Software, Vol. VV, Issue II, pp 3  8, http://www.jstatsoft.org/v59/i10/paper 

[6] Swamidasan, Rabindra:  README.md file accompanying this project.




				
