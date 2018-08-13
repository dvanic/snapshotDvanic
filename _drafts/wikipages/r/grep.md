---
layout: default
title: grep
---

Primitive grep function
	
	grepd <- function(searchstring, df, column){
	if(column=="row.names") {
	      a = df[searchstring,]
	    } else {
	      a <-   df[grep(searchstring, df[[column]], ignore.case=T),]
	    }    
	return(a)
	}

General grep syntax

    counttable.complete[grep("ENSG00000252985", counttable.complete$Gene, ignore.case=T),]
