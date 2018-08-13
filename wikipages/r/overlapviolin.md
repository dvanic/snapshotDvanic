---
layout: default
title: Creating overlapping violin plots using ggplot2 
---

## Creating overlapping violin plots using ggplot2
Based on [this]() StackOverflow question:

I would like to plot two series of ten violin plots one over the second :

    library(ggplot2)
    #generate some data
    
    coco1<-rnorm(10000,0,1)
    coco2<-c(runif(10000))
    decile<-rbinom(10000,9,1/2)+1
    coconut<-data.frame(coco1,coco2,decile)
    #draw the violin plots of the coco1 serie
    p <- ggplot(coconut, aes(factor(decile), coco1))
    p<-p + geom_violin(aes(alpha=0.3,colour="#1268FF"))
    p

    #draw the violin plots of the coco2 serie
    q <- ggplot(coconut, aes(factor(decile), coco2))
    q<-q + geom_violin(aes(alpha=0.3,colour="#3268FF"))
    q

I would like to plot the violin plot "p" and "q", on the same graph, and I want each violin plot of "q" to be over the corresponding violin plot of "p".

You can just add the geom_violin of the second plot to your first one:

    p <- ggplot(coconut, aes(factor(decile), coco1))
    p <- p + geom_violin(aes(colour = "#1268FF"), alpha = 0.3)
    q <- p + geom_violin(aes(y = coco2, colour = "#3268FF"), alpha = 0.3)

Now, q contains both versions of the violins. NB: If you want to get rid of the colour legend, you have to specify the colour outside of `aes`.
