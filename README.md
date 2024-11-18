# KT2.2
install.packages("readxl")
library(readxl)

D<-read_excel("problems_1.xlsx")
View(D)

Q <- data.frame(D[2:828,]$AdmArea,
                D[2:828,]$Month,
                as.numeric(D[2:828,]$Year),
                as.numeric(D[2:828,]$TotalAmount))
colnames(Q) <- c("AdmArea","Month","Year","TotalAmount")
View(Q)

install.packages("rpivotTable")
#1.1
T <- Q[Q$Year >= 2016 & Q$Year <= 2019, ]
rpivotTable::rpivotTable(T, rows = c("AdmArea"),
                         cols = c("Year"),
                         vals = c("TotalAmount"), 
                         aggregatorName = c("Average"))
#1.2
rpivotTable::rpivotTable(Q, rows = c("Month"), c("Year") == "2021",
                         vals = c("TotalAmount"), 
                         aggregatorName = "Sum")
#1.3
rpivotTable::rpivotTable(Q, rows = c("AdmArea"), 
                         cols = c("Year"),
                         vals = c("TotalAmount"), 
                         aggregatorName = ("Average"))
#2
P <- data.frame(Q[Q$AdmArea == "Центральный административный округ", ])
View(P)
#3
SREDNY <- aggregate(TotalAmount ~ Year, P, mean)
colnames(SREDNY)[2] <- "AvgTotalAmount"
View(SREDNY)

P_SREDNY <- merge(x = P, y = SREDNY, by = "Year")

M <- subset(P_SREDNY, TotalAmount > AvgTotalAmount)
View(M)

install.packages("ggplot2", dep=T)

install.packages('ggplot2', repos='http://cran.us.r-project.org')
library(ggplot2)

rpivotTable::rpivotTable(Q, rows = c("AdmArea"), 
                         cols = c("Year"),
                         vals = c("TotalAmount"), 
                         aggregatorName = ("Average"))

# graf
ggplot(data = Q, aes(x = Year, y = TotalAmount, color = AdmArea)) +
  geom_line() +
  geom_point() +
  labs(title = "Средние ежегодные начисления по всем административным округам")
#Из исходных данных отобрать только центральный административный округ, создав из него новый датафрейм.
P <- data.frame(Q[Q$AdmArea == "Центральный административный округ", ])
#В созданном датафрейме отобрать только те месяца, начисления в которых оказались выше среднего уровня начислений в течение соответствующего года.
M <- subset(P_SREDNY, TotalAmount > AvgTotalAmount)
View(M)

