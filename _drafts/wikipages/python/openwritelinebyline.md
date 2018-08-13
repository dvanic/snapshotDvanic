---
layout: default
---

## Open file and write line by line
    
    with open('file.txt') as f:
        for line in f:
            print line

    from __future__ import print_function
    print("hi there", file=f) 

## Dict create or append

    bookdict.setdefault(author,[]).append(book)
