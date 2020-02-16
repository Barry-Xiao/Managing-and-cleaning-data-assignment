#Codebook
##Variables
###ID:The ID of the subjects. Its range is from 1 to 30
###Typeofactivity: The name of the activities
###totalaccxmean:the mean of the acceleration signal from the smartphone accelerometer X axis. (units: standard gravity units "g")
###totalaccxsd:the standard deviation of the acceleration signal from the smartphone accelerometer X axis. (units: standard gravity units "g")
###bodyaccxmean:the mean of the body acceleration signal from X axixobtained by subtracting the gravity from the total acceleration.
###bodyaccxsd: the standard deviation of the body acceleration signal from X axixobtained by subtracting the gravity from the total acceleration.
###bodygryoxmean: the mean of the angular velocity vector measured by the gyroscope for each window sample.(units: radians/second)
###bodygryoxsd: the standard deviation of the angular velocity vector measured by the gyroscope for each window sample.(units: radians/second)
#### substituting "x" in above six variable names with "y" and "z" are the variable names for the same measurements from smartphone accelerometer Y axis and Z axis
###setmean: the mean of train/test set
###setsd: the sd of train/test set

##Work on raw data
###Get the mean and sd for each measurement variable from the 128 measurements, the final tidy data is the average of the mean and sd of each subject and each activity.
