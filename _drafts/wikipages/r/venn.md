---
layout: default
title: Venn
---

Gplots library

    library(gplots)
    VennList<-list(A=list1, B = list2, C = list3)
    venn(VennList)

VennDiagram library

	library(VennDiagram)
	pdf("VennDiagram.pdf", width=20, height=14)
	venn.plot <- draw.pairwise.venn(area1 = 609, area2 = 1821, cross.area=250, c("Day 0", "Day 34"), euler.d = TRUE)
	#grid.newpage()
	dev.off()
