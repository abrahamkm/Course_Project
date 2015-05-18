Getting and Cleaning Data: Course ProjectThe course project in GETTING AND CLEANING DATA course.
CREATES DESTINATION DIRECTORY AND DOWNLOAD THE DATASET
UNZIPS THE DOWNLOADED FILE FROM THE COURSE PROJECT DATA DIRECTORY
SETS THE WORKING DIRECTORY FOR CONVENIENCE 

######################################I. Merges the training and the test sets to create one data set ########################
SETS THE GLOBAL FILE PATHS FOR EASE OF REFERENCE
READING THE ACTIVITY FILES FIRST FROM THE TEST AND TRAIN DATASETS
READING THE SUBJECT FILES FIRST FROM THE TEST AND TRAIN DATASETS
READING THE FEATURES FILES FIRST FROM THE TEST AND TRAIN DATASETS
MERGING THE DATA FROM TRAIN AND TEST DATA FOR ACTIVITY, SUBJECT AND FEATURES
COMBINE DATA TO GET THE COMPLETE DATA SET F

############################# ##################### II. Extracts only the measurements on the mean and standard deviation for each measurement.#####
ASSIGN NAMES TO VARIABLES IN THE DATASETS
CREATE SUBSET OF THE NAMES FROM THE NAMES OF FEATURES WHICH CONTAINS THE EXPRESSION “MEAN” AND “STD”

############################################################# III. Uses descriptive activity names to name the activities in the data set###############


############################################## IV. Appropriately labels the data set with descriptive variable names###########################


###################### V. Creates a tidy data set with the average of each variable for each activity and subject.
LOADS THE LIBRARY PLYR l
AGGREGATES THE DATA
SORTS THE DATA
WRITES THE DATA INTO A FILE

