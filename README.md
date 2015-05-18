# Course_Project
The course project  in GETTING AND CLEANING DATA course.
#CREATES DESTINATION DIRECTORY AND DOWNLOAD THE DATASET if(!file.exists(“./courseprojectdata”)){dir.create(“./courseprojectdata”)} fileUrl <- “https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip” download.file(fileUrl,destfile=“./courseprojectdata/Dataset.zip”,method=“curl”)

#UNZIPS THE DOWNLOADED FILE FROM THE COURSE PROJECT DATA DIRECTORY unzip(zipfile=“./courseprojectdata/Dataset.zip”,exdir=“./courseprojectdata”)

#SETS THE WORKING DIRECTORY FOR CONVENIENCE setwd(“~/R_WORK”)

##################################################################################################################### ###########################I. Merges the training and the test sets to create one data set ######################## #####################################################################################################################

#SETS THE GLOBAL FILE PATHS FOR EASE OF REFERENCE cp_path <- file.path(“./courseprojectdata” , “UCI HAR Dataset”) cp_files<-list.files(cp_path, recursive=TRUE)

#READING THE ACTIVITY FILES FIRST FROM THE TEST AND TRAIN DATASETS Path_Activity_from_Test <- file.path(cp_path, “test” , “Y_test.txt” ) Activity_from_Test_data <- read.table(Path_Activity_from_Test,header = FALSE)

Path_Activity_from_Train <- file.path(cp_path, “train” , “Y_train.txt” ) Activity_from_Train_data <- read.table(Path_Activity_from_Train,header = FALSE)

#READING THE SUBJECT FILES FIRST FROM THE TEST AND TRAIN DATASETS Path_Subject_from_Test <- file.path(cp_path, “test” , “subject_test.txt” ) Subject_from_Test_data <- read.table(Path_Subject_from_Test,header = FALSE)

Path_Subject_from_Train <- file.path(cp_path, “train” , “subject_train.txt” ) Subject_from_Train_data <- read.table(Path_Subject_from_Train,header = FALSE)

#READING THE FEATURES FILES FIRST FROM THE TEST AND TRAIN DATASETS Path_Features_from_Test <- file.path(cp_path, “test” , “x_test.txt” ) Features_from_Test_data <- read.table(Path_Features_from_Test,header = FALSE)

Path_Features_from_Train <- file.path(cp_path, “train” , “X_train.txt” ) Features_from_Train_data <- read.table(Path_Features_from_Train,header = FALSE)

#MERGING THE DATA FROM TRAIN AND TEST DATA FOR ACTIVITY, SUBJECT AND FEATURES Merged_Activity_data<- rbind(Activity_from_Train_data, Activity_from_Test_data) Merged_Subject_data <- rbind(Subject_from_Train_data, Subject_from_Test_data) Merged_Features_data<- rbind(Features_from_Train_data, Features_from_Test_data)

#COMBINE DATA TO GET THE COMPLETE DATA SET Full_data <-cbind(Merged_Subject_data,Merged_Activity_data,Merged_Features_data)

##################################################################################################################### ##################### II. Extracts only the measurements on the mean and standard deviation for each measurement.##### #####################################################################################################################

#ASSIGN NAMES TO VARIABLES IN THE DATASETS temp_Features_Names <- read.table(file.path(cp_path, “features.txt”),head=FALSE) names(Full_data)<-c(“SUBJECT”,“ACTIVITY”,as.character(temp_Features_Names$V2))

#CREATE SUBSET OF THE NAMES FROM THE NAMES OF FEATURES WHICH CONTAINS THE EXPRESSION “MEAN” AND “STD” subset_of_temp_Features_Names<-temp_Features_Names$V2[grep(“mean|std”, temp_Features_Names$V2)] temp_Selected_Names<-c(“SUBJECT”,“ACTIVITY”,as.character(subset_of_temp_Features_Names)) Mean_and_Std_data<-subset(Full_data,select=temp_Selected_Names)

##################################################################################################################### ######################### III. Uses descriptive activity names to name the activities in the data set############### #####################################################################################################################

tmp_act_Labels <- read.table(file.path(cp_path, “activity_labels.txt”),header = FALSE)

#FACTORIZES THE ACTIVITY VALUES Mean_and_Std_data$ACTIVITY<-factor(Mean_and_Std_data$ACTIVITY,labels=tmp_act_Labels$V2)

##################################################################################################################### ################### IV. Appropriately labels the data set with descriptive variable names########################### #####################################################################################################################

names(Meanand_Std_data)<-gsub(“\()”, “”, names(Mean_and_Std_data)) names(Mean_and_Std_data)<-gsub(“t”, “Time”, names(Meanand_Std_data)) names(Mean_and_Std_data)<-gsub(“f”, “Frequency”, names(Meanand_Std_data)) names(Mean_and_Std_data)<-gsub(“Acc”, “_Accelerometer”, names(Meanand_Std_data)) names(Mean_and_Std_data)<-gsub(“Gyro”, “_Gyroscope”, names(Meanand_Std_data)) names(Mean_and_Std_data)<-gsub(“Mag”, “_Magnitude”, names(Meanand_Std_data)) names(Mean_and_Std_data)<-gsub(“BodyBody”, “Body”, names(Mean_and_Std_data)) names(Mean_and_Std_data)<-gsub(“-”, “”, names(Meanand_Std_data)) names(Mean_and_Std_data)<-gsub(“\”, “\”, names(Mean_and_Std_data)) names(Mean_and_Std_data)<-gsub(“mean”, “MEAN”, names(Mean_and_Std_data)) names(Mean_and_Std_data)<-gsub(“std”, “STD”, names(Mean_and_Std_data))

##################################################################################################################### ########### V. Creates a tidy data set with the average of each variable for each activity and subject. #####################################################################################################################

#LOADS THE LIBRARY PLYR library(plyr);

#AGGREGATES THE DATA Mean_and_Std_data_aggregates<-aggregate(. ~SUBJECT + ACTIVITY, Mean_and_Std_data, mean)

#SORTS THE DATA Mean_and_Std_data_aggregates<-Mean_and_Std_data_aggregates[order(Mean_and_Std_data_aggregates$SUBJECT,Mean_and_Std_data_aggregates$ACTIVITY),]

#WRITES THE DATA INTO A FILE write.table(Mean_and_Std_data_aggregates, file = “course_project.txt”,row.name=FALSE)
