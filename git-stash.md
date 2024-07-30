# Git Stash

Git stash allows me to store uncommitted changes in y working directory

This is useful when having to switch to a different branch for a task and you don't want to commit your changes yet, or your code is in a non-commit friendly state.

By this I mean perhaps your code changes are in progress or otherwiese incoplete, meaning code may be broken in your working directory and so you don't want to commit the changes yet untill you're completely done.

I had a sccenario in which because i had closed a ticket in JIRA, i wanted to close the relevant branch as well by merging it.

i had also started work according to a new ticket, but had not created a new branch yet.

So I wanted to stage and commit only those changes which should be part of branch 1, and then create a new branch for the new ticket, and port over changes from branch 1 to the new branch that i did not want to commit in branch 1.

Git stash helped me here.

I used git add and git commit (git add through command line and git comit through VSCode;s git commit box).

I git add -ed those files i wanted ot commit in branch 1. and commited and synced using git push via the VSCode interface.

I then used git stash -u to stash the remaining files which I wanted to commitin the new branch only. The -u ensures that I stashed the untracked files as well rather than just the staged files.

>>Actually I use git stash at first then was confused when my untracked files were not stashed. I looke up how to save untracked files and found the u parameter.

I created a new branch for the new ticket via JIA, using branch 1 as the source branch.

I then closed branch 1 via a pull request on JIRA and deleted it on merge.

Next locally I switched to the new branch,  and used git stash apply to port over my untracked changes from branch 1

>> Actually, since I created the branch as a fork of branch 1 before pushing the latest changes to branch1, I first used git pull <branch1> while in the new branch to fetch the latest changes. I could have also used git pull on the local branch1, but this way felt more appropriate.

Now that my untracked changes and files are present in the new branch, I deleted branch1 locally as the pull request had completed on the remote/origin.

Now I am in my new branch containing changes related to that branch.

Now I can continue working, committing and pushing from the new branch, keeping in sysnc with the relevant task on JIRA as well.

commands learned:

- git stash list
- git stash show
- git stash show -p
- git stash clear
- git stash save "my changes"
- git stash drop <id>
