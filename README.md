## ReadMe - Smartphone Data Analysis
In view of the Coursera Getting and Cleaning Data course, as part of a specialization.

#### Provided Data

The data was downloaded from using a remote URL provided on the coursera websites. This URL is:
> https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

Further, once this file was unzipped and relocated to the correct working directory, several files and 2 sub-directories were present. The sub-dirs corresponded to the train and test datasets. In addition, a file names "features.txt" contained information of the variables present in the final dataset. Details about the data present in the two datasets of training and testing, were named as "y_test" and "y_train", while the datasets were labelled "X_test" and "X_train".

#### Loading the Data

The following files were loaded using the read.table() command, and saved in variables that were appropriately labelled:
> subject_test, 
> X_test, 
> y_test, 
> subject_train, 
> X_train, 
> y_train

#### Merging the Data

A function **run_analysis.R** is used to accomplish several tasks. This script is used for:
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

The data is present in a format that is difficult to understand. The multiple test and train files must be merged. For this, the rbind() command is used. This command combines and binds 2 datasets along the row (hence the name **r** bind). The fllowing files were merged to get completed datasets:

```
> dataSub <- rbind(dataSubTrain, dataSubTest)
> dataAct <- rbind(dataActTrain, dataActTest)
> dataFea <- rbind(dataFeaTrain, dataFeaTest)
```

The next step was to assign variable names, as this makes the data easier to understand. For this, the following command was used:
```
> names(dataFea)<- dataFeaNames$V2
```
And then, using cbind() for column binding, the three individual datasets are combined. As identification, the variable names are used.
```
> dataComb <- cbind(dataSub, dataAct)
> Data <- cbind(dataFea, dataComb)
```

#### Tidying the Data

The first step is to assign understandable names to the various terms present in the data. This includes time, frequency etc. This is accomplished using the following commands:
```
> names(Data)<-gsub("^t", "time", names(Data))
> names(Data)<-gsub("^f", "frequency", names(Data))
> names(Data)<-gsub("Acc", "Accelerometer", names(Data))
> names(Data)<-gsub("Gyro", "Gyroscope", names(Data))
> names(Data)<-gsub("Mag", "Magnitude", names(Data))
> names(Data)<-gsub("BodyBody", "Body", names(Data))
```

Finally, the data is aggregated, and then ordered into a neat format using the order() and aggregate() commands. For this, the package **dplyr** is installed, run, and then invoked.
```
> install.packages("dplyr")
> library(dplyr)
> Data2<-aggregate(. ~subject + activity, Data, mean)
> Data2<-Data2[order(Data2$subject,Data2$activity),]
```

And lastly, to print the tiday dataset into a nex file, the following command is used:
```
> write.table(Data2, file = "tidydata.txt",row.name=FALSE)
```

Lastly, the codebook and README were created in GitHub itself.
