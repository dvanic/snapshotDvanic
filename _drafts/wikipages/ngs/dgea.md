---
layout: default
title: Notes on differential gene expression analysis 
---
## Voom to RPKM
( from the Bioconductor mailing list; Gordon Smythe)

voom() outputs normalized log-cpm values.  Just plot them.
Ex.: 

    v <- voom(y, design)

the log-cpm values are in v$E. To get RPKM:

Take log2CPM values from voom, and subtract log2(GeneLength/1000) from each row, where GeneLength is the effective length of each gene in bases.  This will convert the log2CPM values into log2 FPKM.



## Testing for no change in RNA-seq differential gene expression
( from the Bioconductor mailing list; Simon Anders)

DESeq2  provides a empirical- Bayes style shrinkage estimates of coefficients (i.e., log fold changes) and also estimates standard errors for these coefficients (taken from the the reciprocal curvature of the posterior). Your task is one of the application we had in mind for this.You want to know whether the fold change is zero or nearly zero, and you also want to ascertain that this estimate of a nearly-zero fold change is a precise one. So, to find genes whose absolute log fold change is _reliably_ smaller than some threshold, take all those genes for which the estimated abs log fold change is below the threshold and the standard error is well below the threshold, too.The reason, why I write "below a threshold" rather than "equal to zero"is that it's biologically unreasonable to assume that a treatment has exactly zero effect on a gene's expression. Gene regulation is such an interconnected network that, at least in my view, every gene will react ever so slightly to every perturbance, but often, this reaction is too small to be measurable. This is why you should define a threshold for"biologically unlikely to be relevant". Let's say, we don't believe that an expression change of less than 15% (0.2 on a log2 scale) is worth bothering. So, if you might take all genes with abs log2 fold change below 0.2, and furthermore require that the standard error of this log2fold change estimate is below, say, 0.05, than you will get gene with true values well below at least 0.3 or so.In the end, the thresholds won't matter too much but if you want real p values, this is possible, too: Simply divide the log2 fold change estimates by their standard errors to get z scores (this works because the sampling distribution of our fold change estimates is reasonably close to normal), and do two one-sided tests (TOST) as suggested by Ryan above.
