## Set Up ##

# Packages
library(tidyverse)
library(tidymodels)
library(modelr)
library(stringr)

# Enter your own WD
setwd("/Users/claytonwalther/Desktop/24S/STAT ML/Project")

# Data for 2018-19
scores<-read.csv("raw_scores.csv")
vegas<-read.csv("vegas.csv")
teams<-read.csv("teams.csv")

## Cleaning ##

# Fixing team name/city issues
vegas$Team<-sub("Oklahoma City","OklahomaCity",vegas$Team)
vegas$Team<-sub("Golden State","GoldenState",vegas$Team)
vegas$Team<-sub("New York","NewYork",vegas$Team)
vegas$Team<-sub("New Orleans","NewOrleans",vegas$Team)
vegas$Team<-sub("San Antonio","SanAntonio",vegas$Team)
vegas$Team<-sub("L.A. Clippers","LAC",vegas$Team)
vegas$Team<-sub("L.A. Lakers","LAL",vegas$Team)

vegas$OppTeam<-sub("Oklahoma City","OklahomaCity",vegas$OppTeam)
vegas$OppTeam<-sub("Golden State","GoldenState",vegas$OppTeam)
vegas$OppTeam<-sub("New York","NewYork",vegas$OppTeam)
vegas$OppTeam<-sub("New Orleans","NewOrleans",vegas$OppTeam)
vegas$OppTeam<-sub("San Antonio","SanAntonio",vegas$OppTeam)
vegas$OppTeam<-sub("L.A. Clippers","LAC",vegas$OppTeam)
vegas$OppTeam<-sub("L.A. Lakers","LAL",vegas$OppTeam)

teams$Team<-str_replace_all(teams$Team,'\\*','')
teams$Team<-sub("Oklahoma City","OklahomaCity",teams$Team)
teams$Team<-sub("Golden State","GoldenState",teams$Team)
teams$Team<-sub("New York","NewYork",teams$Team)
teams$Team<-sub("New Orleans","NewOrleans",teams$Team)
teams$Team<-sub("San Antonio","SanAntonio",teams$Team)
teams$Team<-sub("Los Angeles Clippers","LAC",teams$Team)
teams$Team<-sub("Los Angeles","LAL",teams$Team)
teams$Team<-sub(" .*","",teams$Team)

vegas<-vegas[,c(3,6,49,50,58)]

# Merging Data
data<-merge(vegas,teams,by="Team")
data<-data[,-1]

data2<-data%>%
  group_by(GameId)%>%
  summarise(across(everything(),sum))

data2$Average_Line_OU<-data2$Average_Line_OU/2
data2$Average_Odds_OU<-data2$Average_Odds_OU/2
data2$Total<-data2$Total/2

data2$y<-ifelse(data2$Average_Line_OU>data2$Total,0,1)

odds<-data2[,c(1,2,3,4)]

data3<-data2[,-c(1,2,3,4)]
data3$y<-as.factor(data3$y)

# Removing correlated variables
data3<-data3[,-c(12,15,27,31)]
cordata<-cor(data3[,-29])

## Model Building ##

modfull<-glm(y~.,data = data3,family = "binomial"(link = "logit"))
modnull<-glm(y~1,data = data3,family = "binomial"(link = "logit"))

## Backward Selection

bw = step(modfull, 
          scope=list(lower=formula(modnull),upper=formula(modfull)),
          direction="backward")
summary(bw) 

## Forward Selection

fw = step(modnull, 
          scope=list(lower=formula(modnull),upper=formula(modfull)),
          direction="forward")
summary(fw) 

## Both Selection
both1 = step(modnull, 
             scope=list(lower=formula(modnull),upper=formula(modfull)),
             direction="both")
summary(both1) 

both2 = step(modfull, 
             scope=list(lower=formula(modnull),upper=formula(modfull)),
             direction="both")
summary(both2) 

## Model Evaluation 1 ##

LogReg.add1 <- data3 %>%
  gather_predictions(both1, type = "response")  %>%
  rename(prob_y = pred) %>%
  mutate(pred_y = as.factor( if_else( prob_y >= 0.5, 1, 0 )),
         y = as.factor( y ))

LogReg.add1 %>%
  filter(model == "both1") %>%
  conf_mat(truth = y, estimate = pred_y)

all_metrics <- metric_set(accuracy, precision, recall)

LogReg.add1 %>%
  group_by(model) %>%
  all_metrics(truth = y, estimate = pred_y)

## Model Evaluation 2 ##

LogReg.add2 <- data3 %>%
  gather_predictions(both2, type = "response")  %>%
  rename(prob_y = pred) %>%
  mutate(pred_y = as.factor( if_else( prob_y >= 0.5, 1, 0 )),
         y = as.factor( y ))

LogReg.add2 %>%
  filter(model == "both2") %>%
  conf_mat(truth = y, estimate = pred_y)

all_metrics <- metric_set(accuracy, precision, recall)

LogReg.add2 %>%
  group_by(model) %>%
  all_metrics(truth = y, estimate = pred_y)

## Odds Evaluation 1 ##

# compare our odds to average sports books odds

## Odds Evaluation 2 ##

# compare our odds to average sports books odds
