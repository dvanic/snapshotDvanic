---
layout: page
---

```bash
# For BSD or GNU grep you can use -B num to set how many lines before the match and -A num for the number of lines after the match.
grep -B 3 -A 2 foo README.txt 

# If you want the same amount of lines before and after you can use -C num.
grep -C 3 foo README.txt 

# If you want to report only 5 characters before the match and five after
grep -E -o ".{0,5}test_pattern.{0,5}" test.txt
```
