---
layout: default
title: Volcano
---

```
library(ggplot2)
setwd("something")
data <- read.table("AllResults.txt", sep="\t",header=TRUE)
data$GeneShort <- NULL

# Get the significance threshold
data$threshold <- as.factor(data$padj <= 0.1)

# Now do the plotting
baseplot <- ggplot(data, aes(x = log2FoldChange,y = -log10(padj), colour = threshold)) + theme_bw() + geom_point(alpha = 1, size=2) + theme(legend.position = "none") + xlab("Log2Fold change") + ylab("-log10adjustedPvalue") + theme(axis.text.x=element_text(size=18)) + theme(axis.text.y=element_text(size=18))+ theme(axis.title.x=element_text(size=18)) + theme(axis.title.y=element_text(size=18))

# Now add anything I want to show
coders1plotArc<- baseplot # + ...

```
