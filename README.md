# Getting-and-Cleaning-Data-Course-Project
Getting and Cleaning Data Course Project, Coursera

# Step 1: Download and load data
file_url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(file_url, "dataset.zip")
unzip("dataset.zip")

# Read data
features <- read.table("UCI HAR Dataset/features.txt")
activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("Code", "Activity"))
subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt")
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt")
x_train <- read.table("UCI HAR Dataset/train/X_train.txt")
x_test <- read.table("UCI HAR Dataset/test/X_test.txt")
y_train <- read.table("UCI HAR Dataset/train/y_train.txt")
y_test <- read.table("UCI HAR Dataset/test/y_test.txt")

# Step 2: Merge training and test sets
subject <- rbind(subject_train, subject_test)
x <- rbind(x_train, x_test)
y <- rbind(y_train, y_test)
colnames(subject) <- "Subject"
colnames(y) <- "ActivityCode"
colnames(x) <- features$V2  # Assign column names using features
merged_data <- cbind(subject, y, x)

# Step 3: Extract mean and standard deviation measurements
mean_std_columns <- grep("mean\\(\\)|std\\(\\)", features$V2)  # Get column indices
tidy_data <- merged_data[, c(1, 2, mean_std_columns + 2)]  # Include Subject and ActivityCode columns

# Step 4: Use descriptive activity names
tidy_data$ActivityCode <- activities[tidy_data$ActivityCode, 2]  # Replace activity codes with names
colnames(tidy_data)[2] <- "Activity"  # Rename ActivityCode to Activity

# Step 5: Label data with descriptive variable names
colnames(tidy_data) <- gsub("^t", "Time", colnames(tidy_data))
colnames(tidy_data) <- gsub("^f", "Frequency", colnames(tidy_data))
colnames(tidy_data) <- gsub("Acc", "Acceleration", colnames(tidy_data))
colnames(tidy_data) <- gsub("Gyro", "Gyroscope", colnames(tidy_data))
colnames(tidy_data) <- gsub("Mag", "Magnitude", colnames(tidy_data))
colnames(tidy_data) <- gsub("-mean\\(\\)", "Mean", colnames(tidy_data))
colnames(tidy_data) <- gsub("-std\\(\\)", "STD", colnames(tidy_data))
colnames(tidy_data) <- gsub("\\()", "", colnames(tidy_data))

# Step 6: Create second tidy dataset
final_data <- aggregate(. ~ Subject + Activity, tidy_data, mean)

# Write output files
write.table(final_data, "tidy_dataset.txt", row.names = FALSE)

# check work
source("run_analysis.R")
write.table(final_data, "tidy_dataset.txt", row.names = FALSE)
tidy_data <- read.table("tidy_dataset.txt", header = TRUE)
head(tidy_data)
