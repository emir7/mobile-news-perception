# Mobile news perception: Datasets & Models

Folder */data* contains two seperate datasets representing contextual parameters, UI related parameters, and the correspodning user's answers gathered through the ESM questionnaires. Previously mentioned datasets are organized in the following folder structure:

* The first subfolder named as *Study1* contains two seperated *.csv* files which were used in our statistical Multilevel model analysis followed by building multiple general predictive models. 

* Since our second study was divided into two different parts, the second subfolder named as *Study2* also contains two seperate subfolders named as *Part1* and *Part2*. Both folders contain files named as *data.csv* which represent datapoints gathered throughout our second study. 

## Description of datasets collected in our first study

File *rawData.csv* consists of 836 rows each containing 13 columns:  
* Column *user_activity* is represented by the value of current user activity whilst reading the news labled as *STILL*, *ON_FOOT*, and *IN_VEHICLE*. User's activity was sensed using Google's activity recognition API. 
* Column *environmental_brightness* is represented by the value of ambient brightness measured in lux. Datapoints were gathered using the environmental sensors that are supported on the Android platform.  
* Column *screen_brightness* is represented by the current brightness level of the screen whilst users were reading the news. It can take a value ranging from 0 to 255.
* Column *time_of_day* holds an integer value which represents the current hour whilst users were reading the news.
* Column *internet_speed* is represented by a value ranging from 0 to 2. Datapoints were gathered using the Android's WifiManager class containing a method called as *calculateSignalLevel(int rssi)* which returns the RSSI signal quality rating using the system default RSSI quality rating thresholds. 
For the users connected to the cellular network we checked the internet connectivity type (in our case 2G network represented the lowest possible interneed speed, whilst the 4G network was labled as the highest one).
* Column *batery_level* is represented with the value of user's current battery percentage. 
* Column *images* contains a value representing whether the user turned the image rendering on or off. 
* Column *theme* contains a value represending whether the user was using dark or light theme whilst reading the news.
* Column *layout* represents currently used layout by the user whilst reading the news. It can take four different values: *largeCards*, *gridView*, *miniCards*, and *xLargeCards*.  
* Column *font_size* contains two different values (*small-font/large-font*) which represents the currenlty used font size whilst users were reading the news. 
* The following three columns *preference*, *readability*, *informativness* are holding an integer value ranging from -2 to 2 and represent which point on the five-point Likert scale a user has selected. 

File *mlData.csv* is built upon the *rawData.csv* file and contains 6 feature columns with one binarized output label. Dimensionality of parameters such as: *layout* and *images* were reduced since we filtered out labels of the UI parameters which were generally not prefered by users. Additionally column labled as *likert_sum* represents the final aggregated score that was calculated by suming up the values of each likert-scale questionnaire values. Finally the *binarized_output* column represents whether user was satisfied with the configured UI or not (if the value of *likert_sum* column is equal to 6 then we label instance as positive otherwise as negative). 

## Description of datasets collected in the second study
The goal of the first part of our second study was to personalize existing general predictive models using multiple active learning strategies and the UCB algorithm, whilst the goal of the second part was to evaluate whether the personalized model achieved greater performance or not. 

Both datasets (of the first and the second part of our study) named as *data.csv* are inside subfolders */Part1* and */Part2*. They conists of the following columns:

* Column *user_id* represents an identifier of a specific user
* Columns: *user_activity*, *environmental_brightness*, *theme*, *layout*, *fontSize* are the same as in our *mlData.csv* file.
* Coulmn *feedback* represents whether the user was satified with the configured UI or not (values labled as *Y* are positive, whilst instances marked as *N* are negative).
The *feedback* column is also the only one that differ for both *data.csv* files inside subfolders */Part1* and */Part2* due to the fact that two different methodolgies of gathering the data were used. 
In the first part of the second study the UCB algorithm with the help of a selected active learning strategy decided whether to query a user or not. In cases where a specific active learning strategy decided that querying a user is not neccessary and the user did not change the configured UI we could not retrieve users feedback and consequentially we labeled such instances with the question mark, meaning that we can not guarantee the user was satisfied with the configured UI correspodning to the current context of use. However, in the second part of our study with the probability of *50%* we decided whether querying the user is required, therefore there are only two options for labeling the *feedback* column (Y - user is satisfied with the configured UI and N - user is not satisfied with the configured UI).  