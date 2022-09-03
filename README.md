# Beer-recipe-analysis

## Introduction

Tableau and R analysis of recipes from brewer’s friend using a dataset found on kaggle: https://www.kaggle.com/jtrofe/beer-recipes

What does a typical beer look like?

## Tableau

### Treemap

<img width="712" alt="image" src="https://user-images.githubusercontent.com/72087263/188254306-1e594c00-91d2-417c-9268-a5462aa62f0b.png">

This treenap shows that American IPAs and American Pale Ales are by far the most popular styles in the dataset.

### ABV Histogram

<img width="716" alt="image" src="https://user-images.githubusercontent.com/72087263/188254329-1cf75ca6-c6fc-46c9-8219-3f48399785d8.png">

Plotting the ABV of beers in the dataset in a histogram shows that the majority of beers are between 4.5-7.5% ABV.

### Color vs IBU by Style

<img width="524" alt="image" src="https://user-images.githubusercontent.com/72087263/188254367-d7411d99-c101-4cc6-b3b6-0fc4c83873de.png">

Average IBU vs average color does not reveal any meaningful trend. Most beers regardless of color tend to fall between 20 and 40 IBUs.

### Brew Method Packed Bubble Chart

<img width="408" alt="image" src="https://user-images.githubusercontent.com/72087263/188254396-c8b6990b-16bf-49d3-be04-08ffcdd98cb7.png">

This is a simple visual that shows different brewing methods of the recipes. All grain is the overwhelming favorite of users. This is followed by brew in a bag (BIAB), extract, and then partial mash. This is somewhat surprising given the surge in popularity of the BIAB method of brewing.

## R Visualizations

### Brew Method Pie Chart

<img width="376" alt="image" src="https://user-images.githubusercontent.com/72087263/188254463-88bb412a-9792-4bbd-8ace-f4ac091216ca.png">

### ABV by Brewing Method Boxplot

<img width="571" alt="image" src="https://user-images.githubusercontent.com/72087263/188254486-d160e57b-4eff-4833-8b2a-4bc532bee88c.png">

The pie chart of brewing methods is an alternative to the packed bubble chart created in Tableau. The pie chart better shows the proportion of recipes for each method - for instance, here it is seen that all grain comprises close to ⅔ of all recipes, with the remaining third being comprised of brew in a bag, extract, and lastly, partial mash. Brew methods are then used to analyze ABV using a boxplot to see if the brewing method has any impact on ABV. There does not appear to be a very large difference among the styles, however, all grain, extract, and partial mash do seem to achieve higher ABVs, which is an expected outcome due to the presence of more fermentable sugars per volume of grain using these methods.

## Conclusion

The most popular styles are American IPA and American Pale Ale. A typical recipe is all grain and is created to achieve an ABV of 4.5-7.5%, IBUs of about 20-40, and color of 20 SRMs and below.

## R Code
```
library(tidyverse)
library(readr)

options(max.print=2000)
beer=read_csv("recipeData.csv")
as_tibble(beer)
head(beer)

beer2<-na.omit(beer)

#Pie chart
brewmethodpie = ggplot(beer2, aes(x = factor(1), fill = BrewMethod)) +
  geom_bar(color = "white") 

brewmethodpie + coord_polar(theta = "y") +  
  labs(title = "Brew Method") + 
  scale_fill_brewer(palette = 10,"YIGnBu") + 
  scale_fill_brewer("") +
  theme_void() + 
  theme(plot.title = element_text(hjust = 0.5)) 

#ABV boxplot
ggplot(beer2, aes(x = BrewMethod, y = ABV)) + 
  geom_boxplot(alpha = .5, outlier.size = .5, fill="lightblue1", color="dodgerblue") + 
  labs(title = "Boxplot of ABV by Brewing Method", x = "Brewing Method", y = "ABV") + 
  theme_minimal() + 
  theme(plot.title = element_text(hjust = 0.5, vjust = 3), axis.text.x = element_text(vjust = 7),
        axis.title.y = element_text(vjust=7))

#bad box plot
ggplot(beer2, aes(x = BrewMethod, y = Efficiency)) + 
  geom_boxplot(alpha = .5, outlier.size = 0, fill="lightblue1", color="dodgerblue",outlier.shape=100) + 
  labs(title = "Boxplot - Efficiency by Brewing Method", x = "Brewing Method", y = "Efficiency") + 
  theme_minimal() + 
  theme(plot.title = element_text(hjust = 0.5, vjust = 3), axis.text.x = element_text(vjust = 7),
        axis.title.y = element_text(vjust=4))

#better violin
ggplot(beer2, aes(x = BrewMethod, y = Efficiency)) + 
  geom_violin(alpha = .80, fill="lightblue1", color="dodgerblue", bw = 2, draw_quantiles = c(0.25, 0.5, 0.75)) + 
  labs(title = "Violin Plot", x = "Brewing Method", y = "Efficiency") + 
  theme_minimal() + 
  theme(plot.title = element_text(hjust = 0.5, vjust = 3), axis.text.x = element_text(vjust = 7), 
        axis.title.y = element_text(vjust=4))
```
