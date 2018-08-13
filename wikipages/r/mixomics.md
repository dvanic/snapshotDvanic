---
layout: default
title: Mixomics PCA
---

	## Compare Sam's with Illumina body map -----
	IBM <- read.table("~/Documents/A_A_MyProjects/01_ActivityStudy/CounttablesForQuickExploration/IBM/bigIBM.table", col.names=c("Geneadipose", "adipose","Geneadrenal", "adrenal","Genebrain", "brain","Genebreast", "breast","Genecolon", "colon","Geneheart", "heart","Genekidney", "kidney","Geneliver", "liver","Genelung", "lung","Genelymphnode", "lymphnode","Geneovary", "ovary","Geneprostate", "prostate","Geneskeletalmuscle", "skeletalmuscle","Genetestes", "testes","Genethyroid", "thyroid","Gene", "whiteblood"), sep="\t")
	IBM$Geneadipose <- NULL
	IBM$Geneadrenal <- NULL
	IBM$Genebrain <- NULL
	IBM$Genebreast <- NULL
	IBM$Genecolon <- NULL
	IBM$Geneheart <- NULL
	IBM$Genekidney <- NULL
	IBM$Geneliver <- NULL
	IBM$Genelung <- NULL
	IBM$Genelymphnode <- NULL
	IBM$Geneovary <- NULL
	IBM$Geneprostate <- NULL
	IBM$Geneskeletalmuscle <- NULL
	IBM$Genetestes <- NULL
	IBM$Genethyroid <- NULL
	row.names(IBM) <- IBM$Gene
	#IBM$Gene <- NULL
	counttable.sam <- backup.sam
	IBMUs <- merge.data.frame(IBM, counttable.sam, by.x="Gene", by.y="Gene")
	tmp <- merge.data.frame(IBMUs, counttable.C32,by.x="Gene", by.y="Gene" )
	IBMUs <- tmp
	rm(IBM, tmp)
								
	conds <- factor(c( "adipose", "adrenal", "brain", "breast", "colon", "heart", "kidney", "liver", "lung", "lymphnode", "ovary", "prostate", "skeletalmuscle", "testes", "thyroid", "whiteblood","C11d0", "C11d0", "C11d0", "MC3d0", "MC3d0", "MC3d0","C11d34", "C11d34", "C11d34", "MC3d34", "MC3d34", "MC3d34", "C32d42", "C32d42", "C32d42"))
	row.names(IBMUs) <- IBMUs$Gene
	IBMUs$Gene <- NULL													
	d <- newCountDataSet(IBMUs, conds)
	d <- estimateSizeFactors(d)
	d <- estimateDispersions(d,method = "blind")
	vsdFull = varianceStabilizingTransformation( d )
	library("RColorBrewer")
	library("gplots")
	# heatmap of top 10000
	select = order(rowMeans(counts(d)), decreasing=TRUE)[1:10000]
	hmcol = colorRampPalette(brewer.pal(9, "GnBu"))(100)
	heatmap.2(exprs(vsdFull)[select,], col = hmcol, trace="none", margin=c(10, 6))


	# clustering by distance
	dists = dist( t( exprs(vsdFull) ) )
	mat = as.matrix( dists )
	rownames(mat) = colnames(mat) = with(pData(d), paste(condition))
	heatmap.2(mat, trace="none", col = rev(hmcol), margin=c(13, 13))
	# PCA
	library(mixOmics)
	forpca <- as.data.frame(t(exprs(vsdFull)))
	dim(forpca)
	# [1]    31 46734
	pcatune(forpca, center = TRUE, scale. = FALSE)
	# 29 percent is explained by dim1, 9 for dim2, 7 for dim3
	# remove the columns (genes) for which the variance is the same
	forpca2 <- forpca[,apply(forpca, 2, var, na.rm=TRUE) != 0]
	# the proportions of explained variance do no change
	result <- pca(forpca2, ncomp = 3, center = TRUE, scale. = TRUE)
	# Make it pretty
	## Now lets add colours to each type of cell line
	tmp1 <- as.data.frame(c( "adipose", "adrenal", "brain", "breast", "colon", "heart", "kidney", "liver", "lung", "lymphnode", "ovary", "prostate", "skeletalmuscle", "testes", "thyroid", "whiteblood", "C11d0", "C11d0", "C11d0", "C11d34", "C11d34", "C11d34", "MC3d0", "MC3d0", "MC3d0", "MC3d34", "MC3d34", "MC3d34", "C32d42", "C32d42", "C32d42" ))
	names(tmp1) <- c("class")
	# tmp <- cbind(tmp1,forpca2)


	col.drug = as.numeric(as.factor(tmp1$class))
	col.drug[col.drug == 1] <- 'antiquewhite'
	col.drug[col.drug == 2] <- 'mediumaquamarine'
	col.drug[col.drug == 3] <- 'magenta1'
	col.drug[col.drug == 4] <- 'azure4'
	col.drug[col.drug == 5] <- 'blue'
	col.drug[col.drug == 6] <- 'blueviolet'
	col.drug[col.drug == 7] <- 'brown'
	col.drug[col.drug == 8] <- 'aquamarine'
	col.drug[col.drug == 9] <- 'chartreuse'
	col.drug[col.drug == 10] <- 'darkgreen'
	col.drug[col.drug == 11] <- 'cornflowerblue'
	col.drug[col.drug == 12] <- 'cyan'
	col.drug[col.drug == 13] <- 'darkblue'
	col.drug[col.drug == 14] <- 'black'
	col.drug[col.drug == 15] <- 'deeppink'
	col.drug[col.drug == 16] <- 'lightpink'
	col.drug[col.drug == 17] <- 'yellow'
	col.drug[col.drug == 18] <- 'navy'
	col.drug[col.drug == 19] <- 'red'
	col.drug[col.drug == 20] <- 'darkolivegreen'
	col.drug[col.drug == 21] <- 'darkorchid4'

	plotIndiv(result, comp = 1:2, ind.names = FALSE, col = col.drug, pch = 16)
	legend(20, 270, unique(tmp1$class), col = unique(col.drug), pch = 16, title = "Dataset", ncol = 3, cex = 0.6)

	# plot3dIndiv(result, comp = 1:3, ind.names = FALSE, col = col.drug, cex=6, axes.box = "both")
	# rgl.postscript('cs_multi_plotIndiv_final.ps')
	# Clean-up
	rm(mat, d, dists, hmcol, select, vsdFull)
