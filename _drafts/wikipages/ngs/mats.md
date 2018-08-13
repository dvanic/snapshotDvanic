---
layout: default
title: MATS
---

Pull out significant events

SE

    awk '{if ($20 < 0.05 || $20 == "FDR") print $0}'

MXE

    awk '{if ($22 < 0.05 || $22 == "FDR") print $0}'