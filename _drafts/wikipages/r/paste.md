---
layout: default 
title: Paste tab-delimited text into data table in R
---

	a <- "Ensembl Gene ID HGNC symbol
	ENSG00000198576	ARC"
	b <- read.table(textConnection(a), header=TRUE, sep="\t")
