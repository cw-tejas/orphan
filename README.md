# Scenario
We have N commits  on main branch. We want main branch to be empty and have another branch called features where we want to have all those commits. So we can make PR from features branch into main branch for code review and documentation purposes.

# Main Branch Cleanup and Feature Branch for PR

### Create orphan branch
```git checkout --orphan orphanbranch```

### This branch is tracking all files but no commits on it. So remove all files.
```git rm -rf .```

### Commit this change in the branch. Use --allow-empty to make commit without any file.
```git commit --allow-empty -m "empty"```

### Push this branch on remote
```git push origin orphanbranch```

### Copy the only commit id of the orphanbranch from log
```git log```

### Checkout to main branch and rebase it to the only commit id of the orphanbranch
```
git checkout main
git rebase <commit id>
```

### With this command we have rewritten the commit history of the main branch. Where first commit is the now 'empty' that has same \<commit id\> that we copied from orphanbranch and then all N commits that it already had. Verify it!
```git log```

At this point of time we have orphanbranch that has only one commit 'empty' and features branch that has 'empty' commit + N commits. 
Note that rebase only rewrites the commit history of the current branch, keeping the base branch unchanged

### Update the remote main branch 
```git push origin --force main```

### Rename main => features and orphanbranch => main
```
git branch -m main featues
git push origin features
git branch -m orphanbranch main
git push origin --force main
```
### You can also delete one extra branch (orphanbranch) that is present on the remote
```git push origin --delete orphanbranch```


## And all set!
Now make a PR from features branch into main branch to compare empty branch with branch that has all commits.
