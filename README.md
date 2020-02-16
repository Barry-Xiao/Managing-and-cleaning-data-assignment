# Run_analysis.R script

##set workdirectory
if(!file.exists("./data")){dir.create("data")}
setwd("./data/UCI HAR Dataset")

##list all test group files
testfilelist <- list.files(pattern = ".test.txt$",recursive = T)

##read the data of test group into R
testalllist <- lapply(testfilelist,function(x){read.table(x,header = F)})

##extract data of ID and Type of activity
testid <- do.call("cbind",testalllist[c(10,12)])
colnames(testid) <- c("ID","Typeofactivity")

##extract measurements for the test group
testdatalist <- testalllist[-c(10,12)]

##calculate mean and sd for each measurement and produce a dataframe collecting these
testsumlist <- lapply(testdatalist,function(x){
                sumdf <- data.frame(mean = rowMeans(x),sd = apply(x,1,sd))
})
testdata <- do.call("cbind",testsumlist)

##produce variable names for the dataframe
testvariable <- c(unlist(lapply(c("bodyacc","bodygyro","totalacc"),function(x){
        paste(x,c("x","y","z"),sep = "")
})),"set")
colnames(testdata) <- unlist(lapply(testvariable,function(x){
        paste(x,c("mean","sd"),sep = "")
}))

##combine the id and measurements mean&sd for test group
test <- cbind(testid,testdata)

##list all train group files
trainfilelist <- list.files(pattern = ".train.txt$",recursive = T)

##read the data of train group into R
trainalllist <- lapply(trainfilelist,function(x){read.table(x,header = F)})

##extract data of ID and Type of activity
trainid <- do.call("cbind",trainalllist[c(10,12)])
colnames(trainid) <- c("ID","Typeofactivity")

##extract measurements for the train group
traindatalist <- trainalllist[-c(10,12)]

##calculate mean and sd for each measurement and produce a dataframe collecting these
trainsumlist <- lapply(traindatalist,function(x){
        sumdf <- data.frame(mean = rowMeans(x),sd = apply(x,1,sd))
})
traindata <- do.call("cbind",trainsumlist)

##produce variable names for the dataframe
trainvariable <- c(unlist(lapply(c("bodyacc","bodygyro","totalacc"),function(x){
        paste(x,c("x","y","z"),sep = "")
})),"set")
colnames(traindata) <- unlist(lapply(trainvariable,function(x){
        paste(x,c("mean","sd"),sep = "")
}))

##combine the id and measurements mean&sd for train group
train <- cbind(trainid,traindata)

##merge test and train
fulldataset <- rbind(test,train)

##label activity with activity names
library(dplyr)
activitylevel <- c("WALKING","WALKING_UPSTAIRS","WALKING_DOWNSTAIRS","SITTING","STANDING","LAYING")
activityfactor <- as.factor(fulldataset$Typeofactivity)
levels(activityfactor) <- activitylevel
fulldataset <- fulldataset %>% mutate(Typeofactivity = activityfactor)

##Produce the final dataset with the average of each variable for each activity and each subject
library(reshape2)
meltedfulldata <- melt(fulldataset,id.vars = c("ID","Typeofactivity"))
avefulldata <- dcast(meltedfulldata,ID + Typeofactivity~ variable, mean)
write.table(avefulldata,file = "tidydata.txt",row.names = FALSE)
