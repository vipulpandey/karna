---
title: "Kaabil vs Raees( Bollywood Movies comparison)"
author: "Vipul Pandey"
date: "February 14, 2017"
status: publish
published: true
output: html_document
---
 

 
## R Markdown
 
I am not a big fan of usual bollywood movies, but in a country that is obssesed with bollywood and bollywood stars, it's really hard for anyone to be separate from this industries activities.
 
Recently, Sharukh's and Hritik's ( bollywood superstars) fans were fighing all over the social media about who earned more money, apparently that's the paramenter of success in India big screens. Various sources of Media reported different facts and figures to declare a winner.
 
However, I was really pissed at the parameter of comparison without the regard for all the facts and its relative cause on the outcome. So, I scrapped the data  from a famous media source and tried to find the real winner.
 
Getting the data:

{% highlight r %}
library(rvest)
{% endhighlight %}



{% highlight text %}
## Loading required package: xml2
{% endhighlight %}



{% highlight text %}
## Warning: package 'xml2' was built under R version 3.3.1
{% endhighlight %}



{% highlight r %}
library(ggpubr)
{% endhighlight %}



{% highlight text %}
## Warning: package 'ggpubr' was built under R version 3.3.2
{% endhighlight %}



{% highlight text %}
## Loading required package: ggplot2
{% endhighlight %}



{% highlight r %}
url <- "http://indiatoday.intoday.in/story/raees-vs-kaabil-box-office-collection-shah-rukh-hrithik-jolly-llb-2/1/882219.html"
 
webpage <- read_html(url)
collect <- html_nodes(webpage, "p b")
collect_both <- collect %>% html_text()
 
 
raees_v <- collect_both[8:27]
kaabil_v <- collect_both[30:49]
 
raees <- strsplit(raees_v, "-")
kaabil <- strsplit(kaabil_v,"-")
# Raees 
Days <- as.character(sapply(raees, function(x) x[[1]]))
Date <- as.character(sapply(raees, function(x) x[[2]]))
Collection_crores <- as.character(sapply(raees, function(x) x[[3]]))
 
raees_df <- data.frame(Days, Date,Collection_crores,stringsAsFactors = FALSE)
 
l <- sapply(strsplit(x = raees_df$Collection_crores, split = " "), function(x) as.numeric(x[[3]]))
l[20] = .66
raees_df$Collection_crores <- l
 
# Budget recovery for Raees
#budget <- sum(kaabil_df$Collection_crores)
budget <- 85
b <- c()
for (i in raees_df$Collection_crores){
  budget <- budget - i;
  b <- c(b,budget)
}
 
raees_df$diffbudget <- b
raees_df$diffbudget <- -raees_df$diffbudget
 
# Kaabil 
 
 
Days <- as.character(sapply(kaabil, function(x) x[[1]]))
Date <- as.character(sapply(kaabil, function(x) x[[2]]))
Collection_crores <- as.character(sapply(kaabil, function(x) x[[3]]))
 
kaabil_df <- data.frame(Days, Date,Collection_crores, stringsAsFactors = FALSE)
t <- sapply(strsplit(x = kaabil_df$Collection_crores, split = " "), function(x) as.numeric(x[[3]]))
t[20] = 0.8
kaabil_df$Collection_crores <- t
# converting lakh to crores
 
# Budget recovery for kaabil
#budget <- sum(kaabil_df$Collection_crores)
budget <- 50
b <- c()
for (i in kaabil_df$Collection_crores){
  budget <- budget - i; 
  
  b <- c(b,budget)
  }
 
kaabil_df$diffbudget <- b
kaabil_df$diffbudget <- -kaabil_df$diffbudget
 
# final data frame in long format
final <- rbind(cbind(kaabil_df,Movies = "Kaabil"),cbind(raees_df,Movies = "Raees"))
{% endhighlight %}
 
Plotting the final data frame:
 
# 1) Comparison of total earnings:
![plot of chunk collection](/figures/collection-1.png)
 
But, is it right to declare the winner wrt to earnings? What about budget? Number of screen's the movies were released?
Let's look further and decide for ourselves:
 
# 2) Comparison of total net profit:
![plot of chunk profit](/figures/profit-1.png)
 
 
# 3) Number of Screes worldwide
 
It is again an important factor to consider as more screens means more business. There are lot of figures available from different sources, so it's hard to get the exact data. However rougly speaking, Raees was released in lot more screeens.
 
### Number of Screens( Kaabil): 2800+ worldwide
### Number of Screens(Raees) : 3000 Screens + worldwide
 
## 4) Final Verdict
 
Clearly, when it came to better returns on investment :
 
# Kaabil did beat Raees
 
##I mean,that's what the data speaks!!
 
What do you think?
