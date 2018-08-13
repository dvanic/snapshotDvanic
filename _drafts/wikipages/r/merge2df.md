---
layout: default
title: Merge more than 2 data frames
---

    binned <- merge(merge(control.bins1, control.bins2, by.x="bin", by.y="bin"), merge(depol.bins1, depol.bins2, by.x="bin", by.y="bin"), by.x="bin", by.y="bin")