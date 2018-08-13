---
layout: default
title: Mode and not in
---

## Mode

	Mode <- function(x) {
	  ux <- unique(x)
	  ux[which.max(tabulate(match(x, ux)))]
	}

## Not in

    `%ni%` <- Negate(`%in%`)