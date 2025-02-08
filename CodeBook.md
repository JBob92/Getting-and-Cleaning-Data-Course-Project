CodeBook.md - Samsung Data
Data Set Overview
This dataset is a collection of sensor data captured from smartphones. The data consists of accelerometer and gyroscope measurements collected during activities performed by subjects. It includes training and testing data, each with the following variables:

Subjects: Identifiers for the subjects who performed the activities.
Activities: The activity that was performed by the subject (e.g., walking, sitting, etc.).
Sensor Data: Measurements related to the body movements captured by the smartphone's accelerometer and gyroscope.
The data was split into two sets (training and testing) and combined into a single dataset for analysis.

Variables in the Data Set
Here’s a summary of the variables present in the tidy dataset after cleaning:

1. Subject
Description: Identifies the subject who performed the activity.
Type: Integer
Values: Integer values representing unique subject IDs (1 to 30).
2. Activity
Description: Describes the activity performed by the subject.
Type: Factor (categorical)
Values:
Walking
Walking Upstairs
Walking Downstairs
Sitting
Standing
Laying
3. Sensor Measurements (Example Features)
These columns correspond to the accelerometer and gyroscope measurements, cleaned and renamed for clarity. Each feature represents a measurement of mean or standard deviation.

TimeBodyAccelerationMeanX: Mean of the body acceleration in the X direction (Time domain).
TimeBodyAccelerationMeanY: Mean of the body acceleration in the Y direction (Time domain).
TimeBodyAccelerationMeanZ: Mean of the body acceleration in the Z direction (Time domain).
TimeBodyGyroscopeMeanX: Mean of the gyroscope measurements in the X direction (Time domain).
TimeBodyGyroscopeSTDZ: Standard deviation of the gyroscope measurements in the Z direction (Time domain).
... (There are many additional sensor measurement variables representing different time and frequency domain features)
4. Measurement Types
Time Domain Features: These are directly collected from the smartphone’s sensors (e.g., accelerometer and gyroscope) in the time domain. They represent values like body acceleration and angular velocity.
Frequency Domain Features: These features are based on the Fast Fourier Transform (FFT) applied to the time-domain signals, which give insights into the frequency characteristics of the sensor data.
Data Cleaning and Transformation
The following transformations were applied to the raw data to clean and organize it into a tidy format:

1. Merging the Training and Test Sets
The training and test data sets (train and test) were merged to create one unified dataset.
The data consists of the following:
subject_train.txt and subject_test.txt: Contains subject IDs.
X_train.txt and X_test.txt: Contains sensor measurements.
y_train.txt and y_test.txt: Contains activity labels.
The rbind() function was used to combine rows for each dataset, and cbind() was used to combine these with subject IDs and activity codes.
2. Extracting Mean and Standard Deviation
The dataset was filtered to include only the columns that contain the mean and standard deviation values for each measurement.
The grep() function was used to identify features containing mean() and std(). These were extracted from the complete dataset using select().
3. Descriptive Activity Labels
Numeric activity codes (from the y_train.txt and y_test.txt files) were replaced with descriptive activity names (e.g., "Walking", "Sitting") using a left join with the activity_labels.txt file.
The left_join() function was used to map activity codes to their descriptive labels.
4. Descriptive Variable Names
The column names were updated to more descriptive names that are easier to understand:
Replaced t with Time to indicate time-domain measurements.
Replaced f with Frequency to indicate frequency-domain measurements.
Replaced Acc with Acceleration and Gyro with Gyroscope to describe the sensor data.
Replaced mean() with Mean and std() with STD to indicate the type of calculation performed.
5. Creating the Tidy Data Set
A second tidy dataset was created by calculating the average of each variable for each subject and activity.
The group_by() function was used to group the data by Subject and Activity, and the summarise() function was used to compute the mean of each measurement for each group.
Final Tidy Dataset
The final dataset contains one row per subject and activity combination, with the average of each variable. This tidy dataset is ready for further analysis and can be used to study the relationship between activities and sensor measurements.

Variables in the Final Dataset
Subject: Identifier for the subject (1 to 30).
Activity: Descriptive name of the activity performed.
Sensor Measurements: The mean of each variable for each subject-activity combination (e.g., TimeBodyAccelerationMeanX, FrequencyGyroscopeSTDZ).
Summary of Data Transformation
Merged training and test data sets.
Extracted mean and standard deviation columns.
Replaced numeric activity codes with descriptive names.
Renamed variables for clarity (e.g., t → Time, Acc → Acceleration).
Created a second tidy dataset with the average of each variable for each subject and activity.
