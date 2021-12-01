# Final-Report
---
title: 'Nutritions Facts of Cereals'
author: "Juritza, Chineze, Julie"
date: "Fall 2021"
output: slidy_presentation
---


```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

---

## Introduction:

Ever heard of the phrase you are what you eat?

Today, we will be looking at the nutrition facts of different kinds of cereal. When you first hear breakfast, what is the first meal that comes to your mind? Maybe eggs, and bacon, with some toast, pancakes, or waffles, or perhaps some cream cheese on a bagel. Or if you are on a time crunch you may want a quick meal, you'll probably settle for some oatmeal or cereal?
Yesss!!
Cereal! A quick, fast and delicious meal for breakfast or as a late-night "snack." There are 100's of cereals out on the market that varies from different types of manufacturers to sugar-free and keto-friendly. So today, we are going to be looking at the nutrition facts of 80 kinds of cereal.

We found a dataset on Kaggle crated updated on October 24, 2017, by Christ Crawford . But stated in his acknowledgments section the dataset was "gathered and cleaned up by Peta Isenberg, Pierre Dragicevic and Yvonne Jansen," so credit must be given to them as well. But the original source was by "Stat.Lib- 1993 expo." Which is then moved to kaggle.

This dataset has many different fields to look at, including the manufacturers, type (Hot or Cold), Calories, Rating, and so much more.
We have created a few interesting questions to help us understand this dataset. By the end of this presentation you may change your mind about what best ceral it is and your thoughts might change on the best cereal you are going to consume.
look at data of ceral and the nutirein facts.
 
---

## Library

```{r}
#install.packages("treemap")
library(treemap, warn.conflicts = F)    #Used to biuld a treemap
library(readr, warn.conflicts = F)      #Used to read the csv file
library(tidyverse, warn.conflicts = F)  #Used to clean up the dataset 
library(dplyr, warn.conflicts = F)      #Used to manipulate datasets
library(ggplot2, warn.conflicts = F)    #Used to create plots from data frames
```

```{r}
#Read in and rename the csv file
cereal <- read_csv("C:/Users/chunw/OneDrive/Data2401/cereal.csv", na = "-1")
glimpse(cereal)
```


# Clean up:

 There where a few things that we noticed needed sum tiding up with the dataset.

 1. While looking at the data we saw that there was negatives in the data, and knowing its not
possible to have negative potassium in anything, we needed to remove it.

 2. We wanted to get a more precise weight of the cereals, so we converted the ounces to pounds, also we changed the name of `weight` to `weight_ounces.`
 
 3. The third was a bit odd, we noticed that there are actually 77 cereals listed in this dataset, but still we carried along.

# ounces to grams:
```{r message = FALSE, warning = FALSE}
#Renaming and adjusting the columns 
cereal <- cereal %>% 
  mutate(weight_grams = round(weight*28.3495, digits = 0), weight_ounces = weight, rating = ceiling(rating)) %>% 
  subset(select = -weight)
glimpse(cereal)
```


# Closer Look:

 We were curious to know what the meant by hot and cold type cereal. Did they mean What type of cereal is best hot or cold? Lets look at the data

### Types of cereal: Hot vs Cold
```{r message = FALSE, warning = FALSE}
cereal %>% 
  count(type)
```

 Cool! Now we know that there are only 3 types of hot cereal...but now I feel like we need to know what the 3 cereals are.
 
### What are the 3 hot cereals?
```{r}
cereal %>% 
  select(name, type) %>% 
  filter(type == "H")
```

 Interesting, it seams that the three cereals are oatmeal.


# Lets talk calories
 
 We wanted to know which cereal has the most calories, because duhh, we need to count are calories....jk, you don't. But still lets find out. 

### Which cereal has the highest calories per serving? Which cereals have the lowest calories per serving?
```{r message = FALSE, warning = FALSE, fig.dim = c(16, 10)}
#The max calories
cereal$name[cereal$calories == max(cereal$calories)]

#The min calories
cereal$name[cereal$calories == min(cereal$calories)]

cereal %>%
  ggplot(aes(x = reorder(name, calories), y = calories))+
  geom_col() +
  coord_flip()+ 
  labs(title = "Calories per Serving", x = "Name", y= "Calories")
  
```

 It seams that highest is the Muesilx Crispy Blend with 160 calories per serving.
 
 The lowest is a three way tie between Puffed Wheat, Puffed Rice, and All- Bran with extra fiber, with 50 grams per serving
 
# Now proteins

 Proteins are very beneficial, the more the better. Its probably the one nutrition fact that you'll see a high number and not be too concerned.

### Which has the highest protein per serving?
```{r message = FALSE, warning = FALSE, fig.dim = c(15, 10)}
cereal %>% 
  select(name, calories, protein) %>% 
  arrange(desc(protein)) %>% 
  head(5)

cereal %>% 
  arrange(desc(protein)) %>% 
  ggplot(aes(x = protein, y = name, color = protein, fill = protein ))+
  geom_col() +
  labs(title = "Protein per Serving", x = "Protein", y= "Name")

```

 The winner for the `Cereal Nutrions` in the protein category is.....drumroll please.......
 gasp its a tie, coming in at 6 proteins per serving, congratulations to  *Cheerios, and Special K*
 
# Moving on

 OK, OK that was fun... I guess. Lets move on, we want to see what the relationship between the sugar and the calories.
 
 According to, Diabetes Education Online: [https://dtc.ucsf.edu/living-with-diabetes/diet-and-nutrition/understanding-carbohydrates/demystifying-sugar/] , "A teaspoon of sugar is about 20 calories." So in addition to the set calories, there's additional calories from the sugar.

### What is the relationship between sugar and calories? 
```{r message = FALSE, warning = FALSE}
cereal %>% 
  ggplot(aes(x = calories, y = sugars)) +
  geom_point()+
  geom_smooth()+ 
  theme_get()+
  labs(title = "Sugar vs Calories", x = "Calories", y= "Sugar")
```

 So by the looks of it, as the calories increase, so does the sugar intake.
 Its actually kind of scary, so make sure you are aware when you go to pick your next midnight snack, you may want to think twice before reaching for your favorite cereal.
 
# Fiber Time!!!

 Wait... what is fiber?! If your like me, and don't know what it is, a quick Google search will help clarify.
 
 According to Harvard: The Nutrition Source- [https://www.hsph.harvard.edu/nutritionsource/carbohydrates/fiber/], "Fiber is a type of carbohydrate that the body can’t digest." Also Taylor Norris, a writter form healthline wrote in an article, that the recomended daily fiber intake is 25 for women and 38 for men. [https://www.healthline.com/health/soluble-vs-insoluble-fiber]
 
 So, that has us thinking which cereal has the most fiber, also do cereals with lower calories have more or less fiber in them? 

### Which has the most fiber? Will high fiber cereals have lower calories per serving?  
```{r message = FALSE, warning = FALSE}
# The Highest fiber
cereal$name[cereal$fiber == max(cereal$fiber)]

cereal %>% 
  select(name, fiber) %>%  
  arrange(desc(fiber)) %>% 
  head(1)

cereal %>% 
  ggplot(aes(x = calories, y = fiber, color= fiber)) +
  geom_smooth() +
  geom_point()+ 
  theme_get()+
  labs(title = "Fiber vs Calories", x = "Calories", y= "Fiber")

```

 By the looks of the graph. We find that *All- Bran with Extra Fiber* has the most fiber, with a whopping 14 grams per serving. Thats already more than half of the recommended daily intake for women. But I mean it was kind of a given being that its in the name of the cereal.
 
 

# Keto- friendly and low carb

 Lets look at the worlds new favorite dietary obsession. Keto!
 Keto is a diet that consist of high fat and low carb.

### Are there any keto-friendly cereals? Which are low-carb? 
```{r message = FALSE, warning = FALSE, fig.dim = c(12, 10)}
cereal %>% 
  select(name, carbo) %>%  
  arrange(carbo) %>% 
  head(5)

cereal %>% 
  ggplot(aes(x = carbo, y = name, color = carbo, fill = carbo))+
  geom_col() +
  labs(title = "Carbohydrates per Serving", x = "Carbohydrates", y= "Name")
```
 
 100% Bran has the lowest carbs therefor is the most keto-friendly option in this dataset.

# Ratings
 Lets look at the rating, Crawford, the kaggle creator of the dataset, believes that the ratings was produced by "Customer Reports". We want to see what factors determine the ratings. 

# What factors determine the ratings of the cereals? What affects the ratings? 
```{r message = FALSE, warning = FALSE, fig.dim = c(10, 10)}
par(mfrow=c(3,3))
plot(cereal$calories, cereal$rating,  
 xlab="Calories",
 ylab="Rating",
 main="Rating vs Calories")
abline(lm(cereal$rating~cereal$calories), col="red")

plot(cereal$fat, cereal$rating, 
 xlab="Fat",
 ylab="Rating",
 main="Rating vs Fat")
abline(lm(cereal$rating~cereal$fat), col="red")

plot(cereal$sodium, cereal$rating,
 xlab="Sodium",
 ylab="Rating",
 main="Rating vs Sodium")
abline(lm(cereal$rating~cereal$sodium), col="red")

plot(cereal$carbo, cereal$rating,
 xlab="Carbohydrates",
 ylab="Rating",
 main="Rating vs Carbohydrates")
abline(lm(cereal$rating~cereal$carbo), col="red")

plot(cereal$fiber, cereal$rating,
 xlab="Fiber",
 ylab="Rating",
 main="Rating vs Fiber")
abline(lm(cereal$rating~cereal$fiber), col="red")

plot(cereal$sugars, cereal$rating,
 xlab="Sugar",
 ylab="Rating",
 main="Rating vs Sugar")
abline(lm(cereal$rating~cereal$sugars), col="red")

plot(cereal$protein, cereal$rating,
 xlab="Protein",
 ylab="Rating",
 main="Rating vs Protein")
abline(lm(cereal$rating~cereal$protein), col="red")

plot(cereal$potass, cereal$rating,
 xlab="Potassium",
 ylab="Rating",
 main="Rating vs Potassium")
abline(lm(cereal$rating~cereal$potass), col="red")


```

# Shelf

 In this data there is information about the cereals shelf placement, lets look at it compared the the calories sugar and carbs. Will the the higher the nutrition fact the higher on the self it is? 
 
 Lets find out. 

### What are the calories grouped by shelfs? What is the relation between the shelf display and nutritional information? Which self display(1, 2, or 3) have the highest/lowest ratings? 

```{r message = FALSE, warning = FALSE}
cereal %>% 
  ggplot(aes(calories, shelf, group = shelf, fill = shelf)) + 
  geom_violin()         
  
cereal %>% 
  ggplot(aes(sugars, shelf, group = shelf, fill = shelf)) + 
  geom_violin()    

cereal %>% 
  ggplot(aes(carbo, shelf, group = shelf, fill = shelf)) + 
  geom_violin()  
```


```{r message = FALSE, warning = FALSE}
treemap(cereal, index = c("rating", "name"), vSize = "rating", 
        fontsize.labels=c(8,12),               
        fontcolor.labels=c("Black","Navy blue"),    
        fontface.labels=c(8,9))


cereal %>% 
  ggplot(aes(x= rating, y = calories)) +
  geom_col(color = "blue") +
  facet_wrap(~ shelf)
cereal %>% 
  ggplot(aes(x= rating, y = sugars)) +
  geom_col(color = "blue") +
  facet_wrap(~ shelf)


```

```
**Which shelf display has the highest/lowest ratings?**

```{r, message = FALSE, warning = FALSE}
boxplot(cereal$rating~cereal$shelf,
 xlab="Shelf",
 ylab="Rating",
 main="Rating vs Shelf",
 col = rainbow(3))
```


**Based on the nutrition elements and ratings, the lower the sugar and calories, the higher the rating. Will shelf 3 have lower calories and lower sugar? Will shelf 2 have higher calories and higher sugar? **


```{r,message = FALSE, warning = FALSE, fig.dim = c(12, 10)}
par(mfrow=c(2,2))
boxplot(cereal$sugars~cereal$shelf,
 xlab="Shelf",
 ylab="Sugars",
 main="Sugar vs Shelf",
 col = rainbow(3))
boxplot(cereal$calories~cereal$shelf,
 xlab="Shelf",
 ylab="Calories",
 main="Calories vs Shelf",
 col = rainbow(3))
```

**Which manufacturers have the higher ratings? Are the high-rating manufacturers producing low-sugar and low-calorie cereals?** 

```{r message = FALSE, warning = FALSE}
#mfr: Manufacturer of cereal
# A: American Home Food Products
# G: General Mills
# K: Kelloggs
# N: Nabisco
# P: Post
# Q: Quaker Oats
# R: Ralston Purina
cereal %>% 
  count(mfr)
```

```{r message = FALSE, warning = FALSE, fig.dim = c(12, 10)}
par(mfrow=c(2,3))
boxplot(cereal$rating~cereal$mfr,
 xlab="Manufacture",
 ylab="Rating",
 main="Rating vs Manufacture",
 col = rainbow(7))
boxplot(cereal$sugars~cereal$mfr,
 xlab="Manufacture",
 ylab="Sugar",
 main="Sugar vs Manufacture",
 col = rainbow(7))
boxplot(cereal$calories~cereal$mfr,
 xlab="Manufacture",
 ylab="Calories",
 main="Calories vs Manufacture",
 col = rainbow(7))
```


# Conclusion

 To conclude, looking at the nutrition facts of these, we can conclude that cereal isn't the healthiest option in the market. Hopefully, we have convinced you to be aware the next time you go to pick your cereal of choice you will take a second look at the nutrition facts. Although yummy and easy to prepare, its not the healthiest to consume as an everyday meal. Weather it be for breakfast or for a mid-day snack. If you do decide to gobble down that bowl of cereal at 2 in the morning, maybe not something that is high in fiber. Maybe every now-and-then treat yourself with some avocado toast or some acai bowl.
 
 ## Resources 

- 80 Cereals Dataset: https://www.kaggle.com/crawford/80-cereals
- Source of the Dataset: http://lib.stat.cmu.edu/datasets/1993.expo/
- Cheat Sheet Data Visualization with ggplot2 : https://ggplot2.tidyverse.org/
- How to plot, and how to plot multiple graphs:https://www.youtube.com/watch?v=Z3V4Pbxeahg
- Protein and its importance: https://www.youtube.com/watch?v=Fe5wHw3pnmY
- Ketogenic lifestyle: https://www.everydayhealth.com/diet-nutrition/ketogenic-diet/ , https://www.everydayhealth.com/ketogenic-diet/diet/types-targeted-keto-high-protein-keto-keto-cycling-more/
- More information on Fiber: https://www.hsph.harvard.edu/nutritionsource/carbohydrates/fiber/ , https://www.healthline.com/health/soluble-vs-insoluble-fiber
