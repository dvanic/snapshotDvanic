---
layout: default
title: Order data frame
---

	df <- read.table(text = "   gene_biotype	One	Two	Three	Four
	1	3prime_overlapping_ncrna	6	2	11	6
	2	antisense	14992	3278	25077	4333
	                 3	IG_V_gene	3	7	2	5
	",header = TRUE,sep = "")
	df <- df[order(df$One, -df$Two) , ]
