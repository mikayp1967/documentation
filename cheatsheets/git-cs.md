# GIT cheatsheet



## Revert commits/pushes

Revert "git add"

    git reset
	
Revert last local commit (Files are still tehre and added)

    git reset --soft HEAD~1

The following approach will preserve the git history

    git log
    git revert <commit id>
    git push


## Merging

MERGE MASTER LATEST TO CURRENT LOCAL BRANCH
    https://www.atlassian.com/git/tutorials/using-branches/git-merge

    git merge master
    git merge report
    git merge --abort
    git merge master -X ours/theirs
	
Combine 2 branches:
	git pull origin <branch> where branch is the one you want to put on top
	


## GPG Keys
https://docs.gitlab.com/ee/user/project/repository/gpg_signed_commits/

```
    List available keys
        gpg --list-secret-keys --keyid-format LONG <your_email>
    Export key:
        gpg --armor --export <key ID>
    Add key to github

    Associate key to git 
        git config --global user.signingkey <key ID>


    Sign Commits
        git commit -S -m "message".....

    Amend last commit
        git commit -S --amend

```



## Misc

	git log

	
