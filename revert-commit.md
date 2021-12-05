# Reverting Commits.
 
Incase you accidentally uploaded Personal Information to your github repository, or you just want to remove a commit, there is two options:

- [Remove ***FOREVER***](#remove-forever).
- [Remove](#remove-only).


## Remove Forever
 
So.  You accidentally posted personal information to your github repo.  How do you get rid of it?

Here's the guide:

- Run `git log` and find the commit you want to reset to, and then copy it's commit ID.
- Run `git stash` if you want to keep the current changes so you can remove the personal info, and reupload the files.
- Run `git reset <commit_ref>` where `<commit_ref>` is the commit ID you copied.
- Run `git gc` to get rid of unaccessible commits from the git logs, so no one can ever access the commit again (in our case, the commit you want to get rid of).
- Run `git push -f` in order to force-push your new changes to the Github Repository.
- Run `git stash apply` in order to bring the files before you reverted the commit back, so you can remove the files that had personal information.
- After removing the files, Run `git add .` in order to add all files.
- Commit the changes.
- Push.

## Remove Only

- Run `git log` and find the commit you want to rever to, and then copy it's commit ID.
- Run `git stash` to save your changes to a stash, if you want to keep them.
- Run `git push origin +<commit_ID>:<main_branch_name>` (where `<commit_ID>` is the commit ID you copied, and `<main_branch_name.` is the name of the main branch on your github repo) in order to force push the reset using only one command.
- Run `git stash apply` to get your changes back, if you wanted to keep them.
