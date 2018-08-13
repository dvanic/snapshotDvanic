---
layout: page
---

### Customize how git log is displayed

###Nice default:

     git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short

Explanation:

--pretty="..." defines the format of the output.

%h is the abbreviated hash of the commit

%d are any decorations on that commit (e.g. branch heads or tags)

%ad is the author date

%s is the comment

%an is the author name

--graph informs git to display the commit tree in an ASCII graph layout

--date=short keeps the date format nice and short

Other approaches:

    git log --pretty=oneline --max-count=2

    git log --pretty=oneline --since='5 minutes ago'

    git log --pretty=oneline --until='5 minutes ago'

    git log --pretty=oneline --author=<your name>

    git log --pretty=oneline --all

    git log --all --pretty=format:'%h %cd %s (%an)' --since='7 days ago'

From [here](http://gitimmersion.com/lab_10.html)
