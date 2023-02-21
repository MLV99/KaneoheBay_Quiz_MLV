# Quiz 1: Kāne'ohe Bay, Hawai'i Chemistry Data

For this assignment, you will clone this repository, edit this README.md file with code and your responses in text following each question, and knit it to create an HTML file that you will push to your GitHub page. Your final submission will be an email to me with a link to your GitHub repository. No changes to this repository should be made after the end of class. Show all code and results needed to support your answer.

## 1) Is the data tidy? If not, explain how you could make the data tidy.
The data could be more tidy by eliminating the sample numbers and possibly separating the 'Recovery' and 'Bleached' observations in two distinct columns. The variable named 'ISO_DateTime_Local' could be eliminated as the observations are already presented in other columns named 'date' and 'time'. 

## 2) Which survey date had the highest mean seawater temperature? Report mean ± standard deviation seawater temperature for each survey date to support your answer.
````
library(tidyverse)
library(dplyr)

KB=read_csv("carbonate_chem_Kaneohe.csv")

KB_date= KB %>% 
select(reefstatus, date, temp) %>% 
group_by(reefstatus, date) %>% 
summarise_at(vars(temp),
list(mean=mean, 
sd=sd)) %>% 
as.data.frame()

view(KB_date)
````
Answer: The Bleached reefstatus from survey date 31-Oct-15 had the highest mean seawater  temperature, being 27.63697 with a standard deviation of 0.2733986. 


## 3) Create a "publishable" plot of TA vs DIC for the "bleached" survey data.
````
library(ggplot2)
library(ggthemes)
library(RColorBrewer)
library(extrafont)

KB_TA_DIC= 
KB %>% 
filter(reefstatus=='Bleached') %>% 
select(reefstatus, ta, dic) 
KB_TA_DIC

KB_plot= ggplot(data=KB_TA_DIC, aes (x=dic, y=ta)) +
geom_point((aes(color=reefstatus, shape=reefstatus)), 
              size=5, alpha=0.7)+
scale_color_brewer(palette = "Dark2")+
 xlab("\n DIC (µmol/kg)")+
  ylab("TA (µmol/kg)\n")+
  ggtitle("Seawater Total Alkalinity (TA) vs Seawater Dissolved Inorganic Carbon (DIC) for the Bleached Reef Status Survey Data")+
  scale_x_continuous(limits=c(1730,2029), breaks=seq(1730,2029,30))+
  scale_y_continuous(limits=c(2018,2314), breaks=seq(2018,2314,30))+
  abline(lm(ta~dic,data=KB_TA_DIC),col='black')+ # regression line not functioning
  theme_few()+
  theme(legend.position="none", text=element_text(size=13,  family="Georgia"))
  KB_plot
````
````
FIGURE1= KB_plot
````

## Column names for the data in the attached file
Sample ID Number = "sample"             
Bleached or Recovery = "reefstatus"         
Date of sampling = "date"               
Time of samplng = "time"              
Latitude of sampling = "lat"                
Longitude of sampling = "long"               
Seawater temperature (°C) = "temp"               
Seawater salinity = "sal"               
Seawater total alkalinity (µmol/kg) = "ta"                 
Seawater dissolved inorganic carbon (µmol/kg) = "dic"                
ISO time of sampling = "ISO_DateTime_Local"

