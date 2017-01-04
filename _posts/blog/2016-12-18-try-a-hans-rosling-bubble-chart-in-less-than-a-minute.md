---
layout: post
title: Try a Hans Rosling Bubble chart in less than a minute
excerpt: "gganimate"
modified: 2016-12-18
categories: blog
tags: [R, ggplot2, gganimate]
image:
  feature: so-simple-sample-image-4.jpg
  credit: pixabay
  creditlink: https://pixabay.com/en/wave-atlantic-pacific-ocean-huge-1913559/
comments: true
share: true
---

Last few weeks were very stressful with all the projects and deadlines. Many thanks to Keith Hultman for suggesting me the wonderful “gganimate” package in R. I immediately tried and tested the examples from the “gganimate” github page [github.com/dgrtwo/gganimate](github.com/dgrtwo/gganimate).

It was interesting to do the Hans Rosling GDP bubble using gganimate with just few lines of code. I could use the gapminder data from 
gapminder package.There are a lot of options and customizations that can be done using gganimate. Internally it actually creates a GIF 
image from a “for loop” of “ggplot” plots [yhat](github.com/yhat/ggplot). 

For advanced controls and fancier user interface you might need Shiny.

Many thanks to David Robinson for his work.

The example code I tried is below. ( Please refer [gganimate](https://github.com/dgrtwo/gganimate) ) for more options.

```r
setwd("/tmp/working")
devtools::install_github("dgrtwo/gganimate")
library(gapminder)
library(ggplot2)
theme_set(theme_bw())
```

```r
p <- ggplot(gapminder, aes(gdpPercap, lifeExp, size = pop, 
            color = continent, frame = year)) +  
            geom_point() +  scale_x_log10()
library(gganimate)
gganimate(p)
```

The R gif output is below.

<img src="/datascientist/images/file_show.gif" alt="image" align="center">

