## Analysis of an individual's participation and contributions to open source and factors affecting them. 
The Github Survey Data 2017 consisits of questions and responses asked to a particular user regarding open source contributions
and participations. The survey responses are in a csv file format with 93 variables and user responses from 6029 people.
Here, I have made an attempt to analyse and understand factors affecting an individual's contribution to open source and learn some ways to analyse given. This 
analysis had been done using R.

#### Shape of Data 
Firstly, in order to understand the nature of data, the csv file was loaded into R and number of rows and columns were found.
Further, head() command was used to see the first few rows and get an idea about various attributes.

```R
library(ggplot2) #for plots 
library(reshape2) #for reshaing data 
df <- read.csv("/home/reen/Downloads/data_for_public_release/data_for_public_release/survey_data.csv",header = TRUE,na.strings=" ",sep = ",")
#dataframe df, header=True implies that the first row of the csv file is to be taken as header for various columns.
rows<-nrow(df) #number of rows
cols<-ncol(df) #number of columns
head(df) #outputs the first 6 rows of data and all columns(attributes) associated with it

Output:
> rows
[1] 6029
> cols
[1] 93
```

#### Distribution of participants categorized by gender
Here, I wanted to find the gender wise participation for each category including Man, Non-Binary or Other, refer Not to Say
and Woman. I used a pie chart to visualise this data(below).

```R
gender_part<-(df[-which(df$GENDER == ""), ]) #Select rows without blanks in gender
gender_part<-gender_part['GENDER'] #Make a column for only the gender variable
table(gender_part) #Outputs the frequency for each category.
#make a pie chart using pie command
pie(table(gender_part), labels = paste(round(prop.table(table(gender_part))*100), "%", sep = ""), 
    col = heat.colors(5), main = "distribution of gender of participators") 
#label the pie chart and various categories using legend
legend("topright", legend = c("-", "Man", "Non Binary/other", "Prefer Not To Say", "Woman"), 
       fill = heat.colors(5), title = "Categories", cex = 0.5)
```

![alt text](https://github.com/avneet14027/Github-Survey-analysis/blob/master/pie.png)

#### Distribution of types of particpations done by various users
Users participated in various activities like following, applications,dependecies, contributing , or other. Here is a distribution for the
same which was analysed using a bar-graph.

```R
#subset a few columns from the larger dataframe for analysis
types_participations = subset(df, , select = c("PARTICIPATION.TYPE.FOLLOW", "PARTICIPATION.TYPE.USE.APPLICATIONS","PARTICIPATION.TYPE.USE.DEPENDENCIES","PARTICIPATION.TYPE.CONTRIBUTE","PARTICIPATION.TYPE.OTHER","GENDER"))
n_follow<-length(types_participations$PARTICIPATION.TYPE.FOLLOW[types_participations$PARTICIPATION.TYPE.FOLLOW == "1"]) #number of participants of type follow
n_applications<-length(types_participations$PARTICIPATION.TYPE.USE.APPLICATIONS[types_participations$PARTICIPATION.TYPE.USE.APPLICATIONS=="1"]) #number of participants of type appplications
n_dependencies<-length(types_participations$PARTICIPATION.TYPE.USE.DEPENDENCIES[types_participations$PARTICIPATION.TYPE.USE.DEPENDENCIES=="1"]) #number of participants of type dependencies
n_contribute<-length(types_participations$PARTICIPATION.TYPE.CONTRIBUTE[types_participations$PARTICIPATION.TYPE.CONTRIBUTE=="1"]) #number of participants of type contribute
n_other<-length(types_participations$PARTICIPATION.TYPE.OTHER[types_participations$PARTICIPATION.TYPE.OTHER=="1"]) #number of participants of type other

#plot for above
gdf <- data.frame(type_of_particiation=c("FOLLOW", "APPLICATIONS", "DEPENDENCIES","CONTRIBUTE","OTHER"),
                 Number=c(n_follow,n_applications,n_dependencies,n_contribute,n_other))
p<-ggplot(data=gdf, aes(x=type_of_particiation, y=Number)) + geom_bar(stat="identity",fill="steelblue") + geom_text(aes(label=Number), vjust=1.6, color="white", size=3.5)

```
![alt text](https://github.com/avneet14027/Github-Survey-analysis/blob/master/bar-graph.png)





