# How to reset the initial git commit?
Credit [here](https://stackoverflow.com/questions/6632191/how-to-revert-initial-git-commit).
The repository has one initial commit at the moment but we find something wrong in that commit and we want to undo/reset it. We often try `git reset --soft HEAD~1`, but we will get the error message `fatal: ambiguous argument 'HEAD~1': unknown revision or path not in the working tree.`. How to solve it? We just delete the branch we are on because the repo has only one commit by the time being. However, we cannot use `git branch -D` as this has a safety check against doing so. We can use `update-ref` to archive the purpose.
```bash
git update-ref -d HEAD
```
Notes: Do not use `rm -rf .git` of anything like this because it will completely wipe the entire repository including all other branches as we as the branch that we are trying to reset. Also, an issue was discussed [here](https://github.com/microsoft/vscode/issues/44776).


https://stackoverflow.com/questions/2739376/definition-of-downstream-and-upstream

# How to undo the last commit
To undo the last commit means to delete it and move the repository HEAD to the previous commit. The command is `git reset --soft HEAD~1` or `git reset --hard HEAD~1`. The former is used to keep changes in the staged while the latter will delete all latest changes. Read more discussions about it at [https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git/](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git/).

# How to remove committed files after adding them to .gitignore
When discovering a file (containing sensitive information) should be untracked but it was already committed, we usually add it to gitignore but it is still tracked. How to tell git do not track changes made on  it anymore? I read 
[this question](https://superuser.com/questions/1397199/how-to-ignore-a-tracked-file-in-git-without-deleting-it) and find [the answer](https://superuser.com/a/1655712/193851) work as my expectation.
```bash
git update-index --assume-unchanged <file>
```
But if we want to start tracking changes again, we can undo the previous command by using:
```bash
git update-index --no-assume-unchanged <file>
```
