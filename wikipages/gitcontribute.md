---
layout: page
---

### Contributing to other repositories

## Keeping A GitHub Fork Updated

1\. Fork repository someone/coolproject to dvanic/coolproject.

2\. Now clone it down to your desktop:

```
git clone git@github.com:dvanic/coolproject.git
cd coolproject
git remote add upstream git@github.com:someone/coolproject.git
```

3\. Make some changes

4\. Then, when you want to incorporate the changes the primary developer has made:

```
git fetch upstream
git rebase upstream/master
```

The last command interleaves your commits with theirs chronologially, thereby reducing the number of branches in the final project repository.
