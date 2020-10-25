# Analysis-of-English-Premier-League-using-qplot-and-ggplot
Analyzing the 2019 premier league player datasets using qplots and qqplot.
---
title: "English Premier league 2019"
output:
  pdf_document: default
  html_document: default
  word_document: default
date: ""
theme: cerulean
---


## Name: PROJECT 7

### Analyzing the 2019 premier league player datasets using qplots and qqplot.

```{r}
### Importing the datasets. Remenber ggplot2 is part of tidyverse so i will load tidyverse instead.
library(tidyverse) 
Philant.players <- read.csv("Players-1920.csv", header = TRUE, sep = ",")

### The player and columns contain double entries so i try to take the one of each out. 
Philant.players1<- Philant.players %>% separate(Player, into = c("Player", "Player2"), sep = "\\\\") %>% select (-Player2)
```

```{r}
### qplot for Matches Played vr Starts and Goals
qplot(MP, Starts, data = Philant.players1, xlab = "Matches played", ylab = "Starts", main = "Scatter plot for Matches played vs Starts")
qplot(MP,Gls, data = Philant.players1, xlab = "Matches played", ylab = "Goals", main = "Scatter plot  for Matches played vs Goals")
```

```{r}
### Same scatter plot but changing the colour according to the Position.  
qplot(MP,Gls, data = Philant.players1, colour=Pos, xlab = "Matches played", ylab = "Goals", main = " Scatter plot for Matches played vs Goals")
```

```{r}
### Same scatter plot but changing the size according to the Position. 
qplot(MP,Gls, data = Philant.players1, size=Pos, xlab = "Matches played", ylab = "Goals", main = " Scatter plot for Matches played vs Goals")
```

```{r}
### Regression analysis for matches played vs Goals
mod<-lm(MP~Gls, data = Philant.players1)
qplot(fitted(mod), resid(mod))
```

```{r}
### Changing the colors to red.
qplot(MP, Starts, data = Philant.players1, colour=I("red"), xlab = "Matches played", ylab = "Starts", main = "Matches played vs Starts")
```

```{r}
### Histogram of the players age and changing the colour according to the players positions
qplot(Age, data = Philant.players1, colour=Pos)
```

```{r}
### Using geom to change the type of plots
qplot(MP, Starts, data = Philant.players1, geom = "path", xlab = "Matches played", ylab = "Starts", main = "Matches played vs Starts")
qplot(MP, Starts, data = Philant.players1, geom = "line", xlab = "Matches played", ylab = "Starts", main = "Matches played vs Starts")
qplot(MP, data = Philant.players1, geom = "dotplot", xlab = "Matches played", ylab = "Starts", main = "Matches played vs Starts")
```

```{r}
### Plots, Boxplots for using factors on the x variables. 
qplot(factor(Pos), Gls, data = Philant.players1)
qplot(factor(Pos), Gls, data = Philant.players1, geom = "boxplot")
qplot(factor(Pos), Gls, data = Philant.players1, geom = c("boxplot", "jitter"))
```

```{r}
### Density plots for Matches played for the players
qplot(MP, data = Philant.players1, geom = "density")
qplot(MP, data = Philant.players1, geom = "density", fill=Pos, alpha=I(0.5), main = "Density plot for matches played")
```

```{r}
### Regression om matches played on Goals with colors and points
qplot(MP, Gls, data = Philant.players1, geom = c("point", "smooth"), method="lm", colour=Pos, main = "Regression on Matches Played on goals")

### Without methods
qplot(MP, Gls, data = Philant.players1, geom = c("point", "smooth"), colour=Pos, main = "Regression on Matches Played on goals")

### same but box plot.
qplot(MP, Gls, data = Philant.players1, geom = "boxplot", colour=Pos, main = "Regression on Matches Played on goals")
```

### Using ggplot2 

```{r}
 ### Scatter plot for matches played and Red Card and Yellow Cards
### Yellow card
pat.yellow<- ggplot(Philant.players1, aes(x=MP, y=CrdY))
pat.yellow + geom_point()

### Red Card
pat.red<- ggplot(Philant.players1, aes(x=MP, y=CrdR))
pat.red + geom_point()
```

```{r}
### Another way for matches played and Minutes played
ggplot(Philant.players1, aes(x=MP, y=Min)) + geom_point()

### Same as above but changing the color according to the positions.
ggplot(Philant.players1, aes(x=MP, y=Min)) + geom_point(aes(colour=Pos))

### Another way.
pat<- ggplot(Philant.players1, aes(x=MP, y=Min))
pat +  geom_point(aes(colour=Pos))

### Histogram for the players age
histo<- ggplot(Philant.players1, aes(x=Age))
histo + geom_histogram()

### Another way and changing the colour according to thier Positions
ggplot(Philant.players1, aes(x=Age)) + geom_histogram(aes(colour=Pos))
```

```{r}
### Variation of histograms for Minutes played with stats_bin
Jerm<- ggplot(Philant.players1, aes(x=Min))
Jerm + stat_bin(geom = "bar")
Jerm + stat_bin(geom = "area")
Jerm + stat_bin(geom = "point")
Jerm + stat_bin(geom = "line")
Jerm + geom_histogram(aes(fill=Pos))
Jerm + geom_histogram(aes(y=..density..))
Jerm + geom_histogram(aes(fill=..count..))

```

```{r}
### Variation of Scatter plots for Matches played on mins played. Note the charts well.
pat<- ggplot(Philant.players1, aes(x=MP, y=Min))
pat + geom_point(aes(colour="green"))
pat + geom_point(colour="green")
```

```{r}
### Linetype arguments for the same data.
pat<- ggplot(Philant.players1, aes(x=MP, y=Min))
pat + geom_line()

### another way
ggplot(Philant.players1, aes(x=MP, y=Min)) + geom_line()

### changing line type.
pat + geom_line(linetype=2)
```

```{r}
### Histogram of goals and age with mappings
ggplot(Philant.players1, mapping = aes(x= Gls)) + geom_histogram(binwidth = 0.5)
ggplot(Philant.players1, mapping = aes(x= Age)) + geom_histogram(binwidth = 0.5)

### Retaining those with age less than 30. 
young<- Philant.players1 %>% filter(Age<30)
ggplot(young, mapping = aes(x=Age)) + geom_histogram(binwidth = 0.5)
```

 
```{r}
### Scatter plot for matches played and goals with mappings
ggplot(Philant.players1, mapping = aes(x=MP, y= Gls)) + geom_point()

### Box plot of Mins  played and Age with mappings.
ggplot(Philant.players1, mapping = aes(y=Min)) + geom_boxplot()
ggplot(Philant.players1, mapping = aes(y=Age)) + geom_boxplot()

### for teams and goals
ggplot(Philant.players1, mapping = aes(x=Squad, y=Gls)) + geom_boxplot()

### For positions and goals
ggplot(Philant.players1, mapping = aes(x=Pos, y=Gls)) + geom_boxplot()
```
