# 005-missing-data_Pengantar-data-mining
Tugas pengantar data mining
#### DECISION TREE ####
library(rpart)
library(rpart.plot)
library(party)
dataku <- read.delim("clipboard")
View(dataku)
dataku$city <- as.factor(dataku$city)
dataku$kelayakan <- as.factor(dataku$kelayakan)
summary(dataku)
str(dataku)
dat <- sample (2, nrow(dataku), replace = TRUE, prob = c(0.7, 0.3) )
trainData <- dataku[dat==1, ]
testData <- dataku[dat==2, ]
myFormula <- city ~ sqft_lot+ sqft_above+sqft_living+price+floors+city
data_ctree <- ctree (myFormula, data = trainData)
table(predict(data_ctree), trainData$sqft_basement)
print(data_ctree)
plot(data_ctree)
prediksi <- predict(data_ctree, trainData)
cm <- table(trainData[, 5], prediksi)
cm
accuracy <- (sum(diag(cm)))/sum(cm)
accuracy
summary(prediksi)

### ASSOCIATION RULES ###
library(arules)
library(arulesViz)
mydata <- read.delim("clipboard")
str(mydata)
class(mydata)
mydata
SD <- eclat (mydata, parameter = list(supp = 0.07, maxlen = 15)) 
inspect(SD)
rules1 <- apriori (mydata, parameter = list(supp = 0.001, conf = 0.5))
rules1_conf <- sort (rules1, by="confidence", decreasing=TRUE)
inspect(head(rules1_conf))
rules_lift <- sort (rules1, by="lift", decreasing=TRUE)
inspect(head(rules_lift))
rules <- apriori(mydata, parameter = list (supp = 0.001, conf = 0.5, maxlen=3))
plot(rules1)
plot(rules1, method="grouped")
plot(rules1, method="graph")
plot(rules1, method="graph", control=list(type="items"))
plot(rules1, method="paracoord", control=list(reorder=TRUE))
