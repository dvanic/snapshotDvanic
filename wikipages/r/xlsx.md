---
layout: default
title: Export to xlsx
---

    library(xlsx)
	# First one
	write.xlsx(x = dataframe1, file = "filename.xlsx" ,sheetName = "sheetname1", row.names = FALSE, append=FALSE)
	# 2nd and all subsequent sheets
	write.xlsx(x = dataframe1, file = "filename.xlsx" ,sheetName = "sheetname1", row.names = FALSE, append=TRUE)