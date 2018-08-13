---
layout: default
title: Sed with newline and quotes
---

## Sed with newline and quotes (that actually works!)

Very simple problem - very simple solution:

- if you put the command into single quotes and actually physically use return at the command line, the substitution actually works, for example:

        sed -i 's/STRCUFFLINKS/STAROPTIONS="--clip3pNBases 50"\
        STRCUFFLINKS/g' runconfig_*
        
        
 Taken from [here](http://stackoverflow.com/a/12324899/3424793)
