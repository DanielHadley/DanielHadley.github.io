---
layout: post
title: The Political and Public Opinion of Same-Sex Marriage, by State
---

As 2014 draws to a close and we prepare for a new set of politicians to take office, I thought I would take advantage of all of the data that exists to create this map of current opponents of same-sex marriage:

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/GMapOpposedFinal.png)

There are six states where the governor and all of the congress members are on record opposing gay marriage, Oklahoma being the largest:

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/plot03.png)

I scraped these data from [two](http://en.wikipedia.org/wiki/List_of_supporters_of_same-sex_marriage_in_the_United_States) [excellent](http://en.wikipedia.org/wiki/List_of_opponents_of_same-sex_marriage_in_the_United_States) wikipedia lists, which seem rather complete for the political offices I analyzed. 

As you might expect, the map of supporters reads almost as the inverse of opponents, with Hawaii, Delaware and parts of New England electing 100% supporters to congress and the governor's office:

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/Map1.png)

### Public Opinion

Here is a map of recent polling data, which were also scraped from wikipedia and [analyzed](https://github.com/DanielHadley/SameSexMarriage/blob/master/MungeData.R) in R:

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/Map4.png)

Political opposition is correlated to public opinion, but not perfectly. Interesting things happen at ends of this chart, such as states with 100% of politicians opposed to gay marriage, even though opposition is not a majority opinion among voters. 

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/plot02.png)


### Public vs. Political

Given these data, I was somewhat surprised by the number of governors who oppose same-sex marriage.

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/Map6.png)

There are six places where the governor's views would seem to run counter to his or her constituents' opinion:

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/plot07.png)

The same is true of senators in certain states.

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/Map7.png)

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/plot08.png)

The only place I could find where the opposite is true - a senator approves while the majority opposes - is Louisiana, where Sen. Landrieu is careful to point out that she honors the voters' choice to ban gay marriage. 


### Aside: Religiosity and Gender Balance

Out of curiosity, I plotted how often residents of a state say they attend church alongside the more recent polling data. Although the usual caveat applies, I was not surprised to see these variables correlated and would not be amazed to learn that the link is causal. 

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/plot09.png)

This final chart is a little more difficult to explain. I noticed that states who elected all-male delegations to congress also had a larger portion of their delegates oppose gay marriage. Is this because more conservative states tend to favor male candidates? Are females more likely to support gay marriage (e.g., Landrieu)? Or is it a coincidence? 

![_config.yml](https://raw.githubusercontent.com/DanielHadley/SameSexMarriage/master/plotsForBlog/plot10.png)


### Conclusion

In light of this data, I think it is fair to conclude that in 2014 statewide politicians appear to have been more conservative about gay marriage than their constituents. 

If you found this analysis interesting, I would encourage you to edit the wikipedia articles on the incoming governors and 114th Congress members. When those appear to be complete, I will update the data and analysis using code I created [here](https://github.com/DanielHadley/SameSexMarriage). In the meantime, feel free to replicate my results and expand on them. 


