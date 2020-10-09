---
title: 'Weekly Exercises #5'
author: "Kalvin Thomas "
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(gifski)
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial  

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
beet_total_harvest <- garden_harvest %>%
  mutate(str_to_upper(variety),
         weight_in_lbs = weight*0.00220462) %>% 
  filter(vegetable == "beets") %>% 
  group_by(date, variety) %>% 
  summarize(total_weight_in_lbs = sum(weight_in_lbs)) %>% 
  group_by(variety) %>% 
  mutate(cum_weight_in_lbs = cumsum(total_weight_in_lbs)) %>% 
    ggplot(aes(x = date,
             y = cum_weight_in_lbs,
             color = variety)) +
      geom_line()+
      labs(x = "Date",
           y = "Cummlative Weight (lbs)")+
      ggtitle(label = "Total Harvests of Beet Varieties")

ggplotly(beet_total_harvest)
```

<!--html_preserve--><div id="htmlwidget-0f6c0b0a0940f47013d0" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-0f6c0b0a0940f47013d0">{"x":{"data":[{"x":[18450,18451,18463,18470,18487],"y":[0.13668644,0.3196699,0.55556424,0.88405262,7.0217147],"text":["date: 2020-07-07<br />cum_weight_in_lbs: 0.13668644<br />variety: Gourmet Golden","date: 2020-07-08<br />cum_weight_in_lbs: 0.31966990<br />variety: Gourmet Golden","date: 2020-07-20<br />cum_weight_in_lbs: 0.55556424<br />variety: Gourmet Golden","date: 2020-07-27<br />cum_weight_in_lbs: 0.88405262<br />variety: Gourmet Golden","date: 2020-08-13<br />cum_weight_in_lbs: 7.02171470<br />variety: Gourmet Golden"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"Gourmet Golden","legendgroup":"Gourmet Golden","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18424,18431,18432,18434],"y":[0.01763696,0.07275246,0.09700328,0.22266662],"text":["date: 2020-06-11<br />cum_weight_in_lbs: 0.01763696<br />variety: leaves","date: 2020-06-18<br />cum_weight_in_lbs: 0.07275246<br />variety: leaves","date: 2020-06-19<br />cum_weight_in_lbs: 0.09700328<br />variety: leaves","date: 2020-06-21<br />cum_weight_in_lbs: 0.22266662<br />variety: leaves"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,186,56,1)","dash":"solid"},"hoveron":"points","name":"leaves","legendgroup":"leaves","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18450,18452,18455,18461,18470,18473,18487],"y":[0.0220462,0.17416498,0.37037616,0.7495708,0.85759718,1.0802638,6.38678414],"text":["date: 2020-07-07<br />cum_weight_in_lbs: 0.02204620<br />variety: Sweet Merlin","date: 2020-07-09<br />cum_weight_in_lbs: 0.17416498<br />variety: Sweet Merlin","date: 2020-07-12<br />cum_weight_in_lbs: 0.37037616<br />variety: Sweet Merlin","date: 2020-07-18<br />cum_weight_in_lbs: 0.74957080<br />variety: Sweet Merlin","date: 2020-07-27<br />cum_weight_in_lbs: 0.85759718<br />variety: Sweet Merlin","date: 2020-07-30<br />cum_weight_in_lbs: 1.08026380<br />variety: Sweet Merlin","date: 2020-08-13<br />cum_weight_in_lbs: 6.38678414<br />variety: Sweet Merlin"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(97,156,255,1)","dash":"solid"},"hoveron":"points","name":"Sweet Merlin","legendgroup":"Sweet Merlin","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":31.4155251141553},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Total Harvests of Beet Varieties","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18420.85,18490.15],"tickmode":"array","ticktext":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"tickvals":[18428,18444,18458,18475,18489],"categoryorder":"array","categoryarray":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Date","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.332566927,7.371918587],"tickmode":"array","ticktext":["0","2","4","6"],"tickvals":[0,2,4,6],"categoryorder":"array","categoryarray":["0","2","4","6"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Cummlative Weight (lbs)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"variety","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1679302dc934":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"1679302dc934","visdat":{"1679302dc934":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



```r
vegetable_daily_harvests <- garden_harvest %>% 
  mutate(weight_in_lbs = weight*0.00220462) %>%
  ggplot() +
  theme(axis.text.x = element_text(angle = 90)) +
  geom_boxplot(aes(y = weight_in_lbs,
                   x = vegetable))+
  labs(x = "Vegetable",
       y = "Weight (lbs)")+
  ggtitle(label = "Daily Harvest for Each Vegetable")

ggplotly(vegetable_daily_harvests)
```

<!--html_preserve--><div id="htmlwidget-01864c82ebfea2e34487" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-01864c82ebfea2e34487">{"x":{"data":[{"x":[17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,23,23,23,23,23,23,23,23,23,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,8,27,27,27,27,27,27,27,27,27,27,2,28,28,28,28,28,28,28,28,28,28,28,28,28,28,9,9,9,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,24,24,24,24,24,24,24,24,24,24,24,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,18,18,18,18,18,18,18,18,18,18,18,18,18,14,14,14,14,14,14,14,14,14,14,13,13,13,13,13,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,20,20,20,20,20,20,20,20,20,20,20,20,6,6,6,6,6,21,21,21,21,21,21,21,21,21,21,12,12,12,12,10,10,10,10,10,10,10,10,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,16,1],"y":[0.0440924,0.11464024,0.0330693,0.0220462,0.17857422,0.02645544,0.30203294,0.41667318,0.24471282,0.04188778,0.14770954,0.23589434,0.10582176,0.1873927,0.0661386,0.02866006,0.18077884,0.11464024,0.10361714,0.13448182,0.70988764,0.03527392,0.08598018,0.1763696,0.08377556,0.04850164,0.26896364,0.03968316,0.21605276,0.0992079,0.16093726,0.20282504,0.1322772,0.31746528,0.16093726,0.2094389,0.1433003,0.47840254,0.12345872,0.05070626,0.01763696,0.18298346,0.27116826,0.32407914,0.17416498,0.03968316,0.29541908,0.14770954,0.47619792,0.21825738,0.20062042,0.21825738,0.2094389,0.20723428,0.19621118,0.05070626,0.12786796,0.12345872,0.19180194,0.24471282,0.04188778,0.13448182,0.2866006,0.30644218,0.11684486,0.08598018,0.03086468,0.1653465,0.07936632,0.08598018,0.14770954,0.110231,0.19400656,0.08157094,0.03527392,0.11684486,0.09479866,0.21825738,0.04629702,0.12786796,0.15652802,0.1763696,0.08598018,0.1322772,0.11243562,0.09700328,0.09038942,0.0551155,0.01984158,0.08157094,0.03527392,0.12786796,0.06834322,0.13007258,0.04850164,0.03086468,0.08598018,0.04188778,0.0661386,0.23589434,0.37919464,0.18298346,0.10802638,0.19621118,0.12566334,0.0220462,0.32848838,5.45863912,4.87000558,0.22266662,0.43651476,0.15211878,0.13668644,0.01763696,0.0551155,0.67902296,0.02425082,0.25794054,0.1322772,0.6724091,0.28219136,0.6172936,0.23809896,0.13448182,0.26675902,0.06172936,0.38139926,0.28219136,0.35935306,0.15652802,0.0220462,0.84436946,0.24912206,0.04188778,0.6283167,0.26675902,0.74075232,0.55556424,0.32628376,0.15652802,1.00751134,0.0881848,0.1653465,0.10582176,0.01763696,0.45635634,0.3637623,0.0881848,0.0881848,1.3778875,0.07495708,1.74826366,1.23679182,0.95460046,0.9369635,0.3086468,1.63803266,0.73413846,1.15963012,1.75928676,0.01763696,0.16975574,0.04188778,0.19400656,0.1873927,0.20502966,0.24912206,0.03747854,0.08157094,0.0881848,0.05070626,0.0440924,0.07054784,0.56438272,0.50265336,1.02735292,0.51147184,0.21164352,1.13978854,0.06393398,0.66579524,0.39242236,0.05291088,0.04188778,0.02866006,0.68122758,0.03747854,0.00440924,0.07275246,0.02645544,0.01543234,0.0110231,0.330693,0.0551155,0.03747854,0.30203294,0.02425082,0.00661386,0.05952474,0.07495708,0.05291088,0.03527392,0.02866006,0.01984158,0.1322772,0.16975574,0.13448182,0.07054784,0.28880522,0.0661386,0.06393398,0.2314851,0.33510224,0.06393398,0.30203294,3.39952404,0.81791402,0.3858085,2.91230302,0.3196699,1.61157722,1.4550492,0.17857422,0.86641566,0.16755112,0.29541908,0.36155768,0.97664666,0.75838928,3.91099588,2.866006,2.53751762,1.08467304,0.9810559,0.55556424,2.5904285,0.2425082,1.00751134,7.15178728,5.37045432,7.23997208,2.6786133,2.47358364,2.68743178,1.89376858,6.24127922,2.0282504,0.94137274,1.83865308,0.5180857,1.14419778,0.1433003,0.220462,0.771617,0.77382162,1.60496336,0.6724091,0.41005932,0.72311536,0.39242236,0.04629702,0.26896364,0.20723428,0.28439598,0.63272594,1.4550492,1.26104264,0.4960395,0.5401319,1.52780166,0.2314851,1.30513504,1.63803266,0.3527392,0.97664666,0.09038942,0.26896364,0.7054784,1.2015179,0.51588108,0.17196036,1.03176216,0.3086468,1.54543862,0.23809896,0.17857422,1.6644881,0.64815828,0.38360388,1.32056738,2.33028334,1.13317468,2.4912206,2.92553074,1.64685114,0.17196036,0.39462698,6.36694256,0.27998674,1.98636262,1.1574255,1.4440261,1.7306267,1.17065322,0.10361714,0.76500314,1.38009212,2.19800614,0.33510224,2.26855398,0.39903622,0.39462698,0.2866006,0.77382162,0.16755112,0.1543234,2.5463361,3.74124014,0.51367646,0.43431014,0.0881848,2.42949124,0.67902296,1.39552446,0.34392072,1.2345872,0.46076558,0.32628376,0.64154442,0.69886454,0.52469956,2.27737246,0.46517482,0.79145858,0.4850164,0.29982832,1.19710866,1.32056738,0.35714844,0.26014516,0.87523414,0.40565008,1.12215158,0.67902296,1.24120106,0.5070626,0.75838928,1.30293042,0.17857422,1.24340568,0.68784144,2.2266662,0.23368972,1.88935934,0.74075232,0.74736618,0.32628376,0.66579524,0.67681834,3.52959662,1.08687766,0.33730686,0.78043548,0.3527392,1.23899644,0.67681834,1.55205248,0.96121432,1.27647498,0.220462,0.86641566,0.98326052,0.6393398,0.6724091,1.60275874,0.9590097,0.91050806,0.7054784,1.36465978,0.21384814,0.11904948,1.34702282,0.14770954,0.99428362,1.23899644,0.48060716,2.18918766,0.67681834,0.52469956,1.1574255,0.5291088,0.58642892,1.0802638,0.27778212,0.22487124,1.09569614,1.11553772,0.6944553,1.56748482,2.64995324,0.56879196,1.02073906,0.50926722,0.16093726,0.74736618,0.8377556,1.62480494,2.19800614,2.41846814,0.58201968,1.4770954,2.3038279,0.30203294,0.73193384,0.73413846,1.29631656,1.68432968,0.96121432,0.6393398,1.72180822,0.49163026,0.84216484,0.47840254,1.29852118,0.18959732,1.83644846,4.42246772,3.086468,0.3858085,0.06834322,0.39903622,0.54454114,1.48591388,1.85629004,3.39070556,0.2314851,0.79145858,0.86641566,0.80248168,0.05291088,3.38850094,1.76810524,0.64374904,1.76590062,1.92242864,2.49342522,1.3558413,0.46737944,0.7385477,0.59304278,4.7619792,0.67461372,0.80248168,1.76590062,6.46835508,1.06483146,1.39331984,0.39462698,1.92022402,0.2535313,1.38670598,5.24038174,0.14109568,0.44753786,3.63541838,1.3558413,0.28880522,1.80558378,1.45284458,0.47619792,0.97444204,1.52559704,2.29721404,1.30733966,0.47619792,0.94357736,0.53572266,0.7275246,0.20062042,1.70417126,2.72050108,1.5983495,3.39952404,1.05160374,0.72311536,1.11553772,3.46786726,1.3448182,1.57409868,0.37037616,0.50265336,6.39119338,0.97444204,1.65787424,2.59704236,0.1653465,1.41536604,1.27427036,1.89817782,0.67461372,6.46835508,0.96121432,0.68122758,1.07585456,0.52029032,4.01902226,0.5842243,3.59573522,0.92814502,2.26194012,4.15791332,1.46827692,1.34040896,0.53131342,3.06883104,2.7888443,1.7747191,0.57540582,1.80558378,2.31926024,0.51367646,0.44312862,0.85318794,1.24781492,2.33248796,0.7936632,1.81219764,1.0141252,1.32056738,0.3417161,0.39242236,2.90348454,0.78484472,1.12215158,0.3086468,0.5621781,4.30562286,0.66799986,0.69665992,1.66228348,4.36073836,6.03624956,0.9479866,2.73152418,0.59745202,1.5652802,0.91050806,1.57409868,0.2094389,1.05160374,0.67020448,0.22487124,0.28439598,0.63713518,0.24912206,0.27778212,0.20062042,0.26014516,0.4299009,0.30203294,0.40124084,0.07275246,0.110231,0.04188778,0.43431014,1.12215158,0.09479866,0.16314188,0.77602624,0.35714844,0.12786796,0.0440924,0.22487124,0.19180194,0.0881848,0.06834322,0.02645544,0.05291088,1.23238258,0.37037616,1.94667946,1.95770256,0.5621781,0.38360388,0.48722102,1.00751134,0.3527392,0.97444204,0.3527392,0.39242236,0.98987438,0.1763696,0.25573592,0.36155768,0.23589434,0.37258078,0.12345872,0.42328704,0.8157094,0.2535313,0.5952474,0.24691744,1.38229674,0.18518808,0.1543234,0.14991416,0.1322772,0.55556424,0.17857422,0.29541908,0.1653465,0.48281178,0.82011864,0.22487124,0.37037616,0.61288436,0.96782818,0.57761044,0.69886454,1.38670598,1.85849466,0.71209226,1.57850792,0.59965664,0.24030358,1.06483146,1.16183474,3.62439528,0.47178868,0.84436946,0.7275246,3.54282434,1.45725382,3.44802568,1.75928676,0.75838928,5.92822318,10.48958196,16.203957,6.46174122,15.542571,5.16322004,6.35371484,7.58609742,3.84265266,2.66318096,2.44492358,2.26634936,2.49342522,2.87041524,3.4612534,2.99607858,3.54502896,5.01991974,2.54413148,2.89025682,13.778875,1.18388094,1.55646172,0.87523414,2.60806546,3.71698932,2.59704236,1.06703608,1.00089748,0.69225068,1.08908228,0.64815828,0.96341894,1.0582176,0.55556424,0.67681834,3.43479796,3.9352467,7.11430874,11.353793,5.1257415,2.58381464,4.23948426,4.6737944,4.24830274,4.04327308,3.6486461,0.42108242,0.34392072],"hoverinfo":"y","type":"box","fillcolor":"rgba(255,255,255,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"showlegend":false,"xaxis":"x","yaxis":"y","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":98.6301369863014,"l":37.2602739726027},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Daily Harvest for Each Vegetable","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,30.6],"tickmode":"array","ticktext":["apple","asparagus","basil","beans","beets","broccoli","carrots","chives","cilantro","corn","cucumbers","edamame","hot peppers","jalapeño","kale","kohlrabi","lettuce","onions","peas","peppers","potatoes","pumpkins","radish","raspberries","spinach","squash","strawberries","Swiss chard","tomatoes","zucchini"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30],"categoryorder":"array","categoryarray":["apple","asparagus","basil","beans","beets","broccoli","carrots","chives","cilantro","corn","cucumbers","edamame","hot peppers","jalapeño","kale","kohlrabi","lettuce","onions","peas","peppers","potatoes","pumpkins","radish","raspberries","spinach","squash","strawberries","Swiss chard","tomatoes","zucchini"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-90,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Vegetable","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.805568148,17.013934388],"tickmode":"array","ticktext":["0","5","10","15"],"tickvals":[0,5,10,15],"categoryorder":"array","categoryarray":["0","5","10","15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Weight (lbs)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"16795a3207b9":{"x":{},"y":{},"type":"box"}},"cur_data":"16795a3207b9","visdat":{"16795a3207b9":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
cumtrips_station <- small_trains %>% 
  filter(!is.na(service)) %>% 
  group_by(departure_station, year) %>% 
  mutate(cumtrips100 = cumsum(total_num_trips)/100) %>% 
  arrange(desc(cumtrips100))
 
ggcum <- cumtrips_station %>% 
  ggplot(aes(x = year,
             y = cumtrips100,
             fill = departure_station,
             group = departure_station)) +
  labs(title = "Cummulative Number of Departures (100's) for Each Station by Year",
       subtitle = "Departure Station: {closest_state}",
       x = "",
       y = "") +
  geom_col(position = "dodge") +
  theme(legend.position = "none") +
  transition_states(departure_station,
                    transition_length = 4,
                    state_length = 2) 
```


```r
animate(ggcum, duration = 45) 
```

```
## Error in animate(ggcum, duration = 45): object 'ggcum' not found
```

```r
anim_save("ggcum.gif")
```

```
## Error: The animation object does not specify a save_animation method
```

```r
knitr::include_graphics("ggcum.gif")
```

![](ggcum.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
tomato_cum_harvest <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>%
  mutate(day_of_week = wday(date, label = TRUE)) %>% 
  ungroup() %>%
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>% 
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb)) %>% 
  mutate(variety = fct_reorder(variety, cum_harvest_lb, sum, .desc = TRUE))

ggtomato <- tomato_cum_harvest %>% 
  ggplot(aes(x= date,
             y = cum_harvest_lb,
             fill = variety)) +
  geom_area() +
  scale_fill_brewer(palette = "Set3") +
  geom_line(position = "stack") +
  geom_text(aes(label = variety),
            position = "stack",
            color = "darkred") +
  labs(title = "Cumulative Harvest (lbs) of Tomato Varieties Over Time",
       subtitle = "Date: {frame_along}",
       x = "",
       y = "") +
  theme(legend.position = "none") +
  transition_reveal(date)
```


```r
animate(ggtomato, duration = 20)
```

```
## Error in animate(ggtomato, duration = 20): object 'ggtomato' not found
```

```r
anim_save("ggtomato.gif")
```

```
## Error: The animation object does not specify a save_animation method
```

```r
knitr::include_graphics("ggtomato.gif")
```

![](ggtomato.gif)<!-- -->


## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.


```r
lisa_mallorca <- get_stamenmap(
  bbox = c(left = 2.3, bottom = 39.52, right = 2.73, top = 39.713),
  maptype = "terrain",
  zoom = 10)

gglisa <- ggmap(lisa_mallorca) +
  geom_path(data = mallorca_bike_day7, 
            aes(x = lon, y = lat, color = ele),
            size = 2) +
  geom_point(data = mallorca_bike_day7,
             aes(x = lon, y = lat),
             color = "red",
             size = 2) +
  geom_text(data = mallorca_bike_day7,
            aes(label = round(ele, 2)),
            hjust = 0,
            vjust= 0,
            check_overlap = TRUE,
            color = "brown4") +
  scale_color_viridis_c(option = "inferno") +
  theme_map() +
  theme(legend.background = element_blank()) +
  labs(title = "Lisa's Bike Path in Mallorca",
       subtitle = "Time: {frame_along}",
       color = "Elevation") +
  transition_reveal(time)
```


```r
animate(gglisa, duration = 15)
```

```
## Error in animate(gglisa, duration = 15): object 'gglisa' not found
```

```r
anim_save("gglisa.gif")
```

```
## Error: The animation object does not specify a save_animation method
```

```r
knitr::include_graphics("gglisa.gif")
```

![](gglisa.gif)<!-- -->
  
- Comments: I like the animated map more than the static map because it is easier to visualize the route taken and having a point labeled with the elevation in conjunction with the colored path based on elevation helps to add more easily accessible information to the map. 
  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
heather_trimap <- get_stamenmap(
  bbox = c(left = -79.6, bottom = 8.9, right = -79.4, top = 9.0),
  maptype = "terrain",
  zoom = 12)

heather_tri <- bind_rows(list(panama_bike, panama_run, panama_swim)) 

ggheather <- 
  ggmap(heather_trimap) +
  geom_path(data = heather_tri, 
            aes(x = lon, y = lat), 
            size = 1) +
  geom_point(data = heather_tri,
             aes(x = lon, y = lat,
                 color = event), 
             size = 2) +
  theme_map() +
  theme(legend.background = element_blank()) +
  labs(title = "Heather's Triathalon Path in Panama",
       subtitle = "Time: {frame_along}") +
transition_reveal(time)
```


```r
animate(ggheather)
```

```
## Error in animate(ggheather): object 'ggheather' not found
```

```r
anim_save("ggheather.gif")
```

```
## Error: The animation object does not specify a save_animation method
```

```r
knitr::include_graphics("ggheather.gif")
```

![](ggheather.gif)<!-- -->
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the x-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe


```r
ggcovid <- covid19 %>% 
  group_by(state) %>% 
  mutate(new_cases = cases - lag(cases, n = 7, order_by = date)) %>% 
  replace_na(list(new_cases = 0)) %>% 
  filter(cases > 19) %>% 
  ggplot(aes(x = cases,
             y = new_cases,
             group = state)) +
  scale_y_log10(labels = scales:: comma) +
  scale_x_log10(labels = scales:: comma) +
  geom_point(aes(group = state)) +
  geom_path(aes(group = state,
                color = state)) +
  geom_text(aes(label = state), check_overlap = TRUE) +
  labs(title = "Cumulative Cases on the Order of New Cases in the Past Week on Log10 Scale",
       subtitle = "Date: {frame_along}",
       x = "Cumulative Cases",
       y = "New Cases in the Past Week") +
  theme(legend.position = "none") +
  transition_reveal(date)
```


```r
animate(ggcovid, nframes = 200, duration = 30)
```

```
## Error in animate(ggcovid, nframes = 200, duration = 30): object 'ggcovid' not found
```

```r
anim_save("ggcovid.gif")
```

```
## Error: The animation object does not specify a save_animation method
```

```r
knitr::include_graphics("ggcovid.gif")
```

![](ggcovid.gif)<!-- -->
- Observations: The labels are hard to read among the 50 lines in the background, but changing the color of the line slightly helps with this. I also noticed that some of the states' labels, CA, TX, and FL in particular, would overlap each other, causing some of the names to disappear and reappear after a few moments. This could cause issues in readability, however the main message of the plot is still communicated: all states are still continuing to observe increases in cases.

  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. Put date in the subtitle. Comment on what you see. The code below gives the population estimates for each state. Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state)) 

states_map <- map_data("state")

covid19map <- covid19 %>% 
  mutate(state = str_to_lower(state),
         day_of_week = wday(date, label = TRUE)) %>% 
  filter(day_of_week == "Fri") %>% 
  group_by(state, fips) 
  #top_n(n =1, wt= date) 

covid19_with_2018_pop_est <-
  covid19map %>% 
  left_join(census_pop_est_2018,
            by = c("state" = "state")) %>% 
  group_by(state, date) %>% 
  mutate(cases_per_10000 = (cases/est_pop_2018)*10000)

gg2018covid <- covid19_with_2018_pop_est %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = cases_per_10000)) +
  facet_wrap(vars(date)) + #I had to 'facet_wrap' the map here to get it to actually show a change in the amount of cases. For whatever reason, the transition would not show any change in the number of cases per state, which was odd because 'facet_wrap'-ing the data showed a change in number of cases each week per state. This was the best I could do to work around this issue while still using an animation to tell the story of the increase in covid cases in each state.
  expand_limits(x = states_map$long, y = states_map$lat) + 
  labs(title = "COVID Cases per 10000 People in the United States Over Time by Week",
       subtitle = "Date: {frame_along}",
       caption = "Created by Kalvin Thomas") +
  theme_map() +
  theme(legend.background = element_blank(),
        legend.position = "none") +
  transition_reveal(date)
```


```r
animate(gg2018covid)
```

```
## Error in animate(gg2018covid): object 'gg2018covid' not found
```

```r
anim_save("gg2018covid.gif")
```

```
## Error: The animation object does not specify a save_animation method
```

```r
knitr::include_graphics("gg2018covid.gif")
```

![](gg2018covid.gif)<!-- -->

## Your first `shiny` app

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
  -[covid_app link here](https://kthomas54.shinyapps.io/covid_app/)
    
    # This app does not funciton in the specified way; shiny would not allow for multiple selecitons to be made. I set the multiple = TRUE option, but this didn't work. I have tried looking up dozens of websites and help communities as well as looking through the cheatsheet, but I could not find anything to rectify this issue. Everything else works as specified.
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.

  - [Main GitHub Page](https://github.com/kthomas54)

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
