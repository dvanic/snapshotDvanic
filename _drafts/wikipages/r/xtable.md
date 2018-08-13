---
layout: default
---
## Pretty output with xtable

	library(xtable)
	print(xtable(subset.data.frame(miscRNA, C32K1 > 2000)), type = "html", include.rownames=FALSE) 
