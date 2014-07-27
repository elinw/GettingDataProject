---
title: "Readme"
output: html_document
---

The documents the exercise data set found in the /data/tidyexercisedata.txt file.

This file is based on the data found here:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones#

It was originally collected as part of this project:
Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. 
Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support 
Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). 
Vitoria-Gasteiz, Spain. Dec 2012

The original data consist of measurements of 30 individuals over 6 activity types. 
THe measurements are called features and there were multiple measures of each feature for each individual-activity combination. 


Features

Feature refers to a measurement or derived measurement of movements that individuals made. This includse a set of 8 for x,y,z vectors (24 total) and 9 other base measures. Some deatures representent magnitude and jerk measures  A variety of statistics were included for each. The data set here includes only the mean and standard deviation and the transformations done on those two statistics. For more detail please see either the codebook or the features_info.txt file from the original study.

Each statistic was normalized to have a mean of 0, maximum of 1, and minimum of -1.

The records in this file represent the means of all measures (that represent means or standard deviations) on each feature type for each combination of subject and activity type. That is, the values are calculated by taking the mean of all of the observations for a subject-activity type combination for all of the features that represent mean or standard deviation measurements.


To make the data easier to work with in R, the following transformations to the original feature names were made
1. Change all ascii characters to lower case.
2. Delete the following characters; "(", ")", "-".
This process is found in the createFeatures() function in the handlefeatures.R file.

Train and Test
The original data were separated into train and test data sets. Two separate data frames were created using the createSubsetData() function.  These are merged to then make the combined 
data. This process includes merging in the subject id and the activity name for each record.

At this point the feature names were parsed to break them down to their constituent elements.

Domain, Accelerationsignal, Source, Statistic, Jerk, magnitude, frequency, dimesnions.

Note that this could be more detailed but for the purposes of this project more detailed parsing was not necessary.

The parsing processing is done with the parseFeatureNames() function in the handlefeatures.R file. This produces the `expandedFeatures` dataframe.

At that point the list of features that included the strings 'mean" or "std' were extracted.
`statswanted<-c("mean","std")` was used to define the specific statistics wanted (you could define a different list if desired; this is set near the top of the file for ease of use). These rows were then extracted subsetting the expandedFeatures dataframe.

Next, these selected rows are used to subset the combined data so that it only contains the desired data. 
The `melt()` function from reshape2 is used to create long data for each combination of subject, activity and feature. The `summarize` method from the HMisc package is used calculate the means of each variable by `subjectid`,`activityname`, and featurename (stored as variable from the `melt()` output).  

Finaldata is created by merging these summaries with the `expandedFeatures` dataframe, which provides the new feature names as well as the variables breaking down the meaning of the feature name.

Finally a set of functions are provided which save the data in text files with different formats.

`getLongNarrowData()` saves one row per combination of subject, activity, and feature. With the means that makes the data frame 4 variables wide.  This file is the one uploaded in coursera since it was like what most people in the coursera forums thought was appropriate. However please note that this includes only the 4 necessary columns and no repeated 
information. The value column provides the mean.

`getLongNarrowPlusData` is the same as getLongNarrowData but adds the attribute variables.

`getWideData()` presents the data in 180 rows representing 30 subjects*6 activities. Each data  column represents themeans for each subject-action combination on a specific feature. This is an easier to read compact presentation.

`getReallyTidy` presents a very long four column dataset. Each row represents a unqique combination of subjectid, activity name, and feature. The value column provides the mean.

Reproducing the analysis.

THree analysis files are included.

 run_analysis.R
 getdata.R
 handlefeatures.R

and run_analysis calls the other two.

By default run_analysis will create the long, narrow data set and store the created data in a folder called data under your working directory. The file is called `tidyexercisedata.txt`.

To run the analysis put the files in your desired location.  In R run run_analysis.R (for example by using source(run_analysis.R) with the appropriate path).

This analysis will create some saved R data sets. These will improve performance if you run the script repeatedly.  If not needed you may delete them. 