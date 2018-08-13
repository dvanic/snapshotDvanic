---
layout: default
title:  Beautiful outlier test and visualization in R
---

    library(outliers)
    library(ggplot2)
    
    X <- c(152.36,130.38,101.54,96.26,88.03,85.66,83.62,76.53,
           74.36,73.87,73.36,73.35,68.26,65.25,63.68,63.05,57.53)
    
    grubbs.flag <- function(x) {
      outliers <- NULL
      test <- x
      grubbs.result <- grubbs.test(test)
      pv <- grubbs.result$p.value
      while(pv < 0.05) {
        outliers <- c(outliers,as.numeric(strsplit(grubbs.result$alternative," ")[[1]][3]))
        test <- x[!x %in% outliers]
        grubbs.result <- grubbs.test(test)
        pv <- grubbs.result$p.value
      }
      return(data.frame(X=x,Outlier=(x %in% outliers)))
    }
    
    
    # Plot the outliers highlighted in colour:
    
    ggplot(grubbs.flag(X),aes(x=X,color=Outlier,fill=Outlier))+
      geom_histogram(binwidth=diff(range(X))/30)+
      theme_bw()
    
    
Taken from [here](http://stackoverflow.com/questions/22837099/how-to-repeat-the-grubbs-test-and-flag-the-outliers)
