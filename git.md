# GIT
## Setup
```
cd /path/to/my/repo
git config core.sshCommand "ssh -i /c/Users/xxx/.ssh/id_rsa_xxx"
git config user.name "your name"
git config user.email "your@email.com"

git config --global user.name "your name"
git config --global user.email "your@email.com"
```
## undo your changes
[gitlab Undo Samples](https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/)
```
git reset HEAD tools/~$gular4tutorial.docx

# Unstage the file to current commit (HEAD)
git reset HEAD <file>
# Unstage everything - retain changes
git reset
# Discard all local changes, but save them for later
git stash
git stash -u ;(the --include-untracked option)

# Discard everything permanently
git reset --hard

git reset --hard HEAD   # reset your files to the HEAD of the branch
git checkout HEAD -- path/to/file # reset a single file
git reset --soft HEAD~1  # committed changes but want to revert back

git reset --hard HEAD^     #unstage 1 commit 
git reset --hard HEAD~2    #unstage 2 commits
```
#For a list of files to be pushed, run:
```
git diff --stat --cached [remote/branch]
e.g. git diff --stat --cached origin/master
For the code diff of the files to be pushed, run:
git diff [remote repo/branch]
To see full file paths of the files that will change, run:
git diff --numstat [remote repo/branch]
```
# Who changed a file
```
git blame -w  # ignores white space
git blame -M  # ignores moving text
git blame -C  # ignores moving text into other files
```
## resolve binary conflict
```
# to use that version of the file. 
$ git checkout --theirs -- path/to/conflicted-file.txt
# Likewise, want your version (not the one being merged in)
$ git checkout --ours -- path/to/conflicted-file.txt
```
## [undelete](https://makandracards.com/makandra/45654-git-undo-delete)
```
# delete local change (not commit yet)
# restores the file status in the index
git reset -- <file>
git reset -- src/app/shared/payload.service.ts
# or check out a copy from the index
git checkout -- <file>
git checkout -- src/app/shared/payload.service.ts
```
## development
```
git clone git@url:test/docker.git

git checkout -b [new branch] #create
git status
git add .
git reset HEAD [file]
git pull origin develop
git commit -m "fix(): (#)"
ng lint mybenefits

git push -u origin {feature_branch_name}
```
## branch
```
git checkout -b [new branch] #create
git branch -d [branch]  #delete
git checkout -f [branch]   #switch without local changes
git checkout - #checkout to previous branch

# short cut
git checkout master
git checkout -b new-branch 
# OR
git checkout -b new-branch master  # new branch, base branch

# delete all tracking branches locally that are not in remote repo
git remote prune origin --dry-run #view result, not prune yet

 remote prune origin 

# Deleting a remote branch:
git push origin --delete <branch>  # Git version 1.7.0 or newer
git push origin -d <branch>        # Shorter version (Git 1.7.0 or newer)
git push origin :<branch>          # Git versions older than 1.7.0

#Deleting a local branch:
git branch --delete <branch>
git branch -d <branch> # Shorter version
git branch -D <branch> # Force delete un-merged branches

#Deleting a local remote-tracking branch:
git branch --delete --remotes <remote>/<branch>
git branch -dr <remote>/<branch> # Shorter

git fetch <remote> --prune # Delete multiple obsolete tracking branches
git fetch <remote> -p      # Shorter
```
## config multiple ssh 
```
# go to [git folder]/.git/config and add the following ...
[core]
    sshCommand = ssh -i /c/Users/[userid]/.ssh/id_rsa_onca
[user]
name = [your name]
email = [your email]

# ./gitconfig
[alias]
  co = checkout
  ci = commit
  br = branch
# [users/username]\.gitconfig (please sync with V:\.gitconfig) or .git/config 
git config --list --show-origin
git config --global alias.mylog 'log --oneline --graph --decorate'
git config --global alias.unstage 'reset HEAD --'
$ git config --global alias.last 'log -1 HEAD'
```

## checkout a repository
```
git clone /path/to/repository

#when using a remote server, your command will be
git clone username@host:/path/to/repository
```

## workflow
your local repository consists of three "trees" maintained by git. 
* the first one is your Working Directory which holds the actual files. 
* the second one is the Index which acts as a staging area 
* and finally the HEAD which points to the last commit you've made.

## add & commit
You can propose changes (add it to the Index) using
```
git add <filename>
git add *
```
This is the first step in the basic git workflow. To actually commit these changes use
```
git commit -m "Commit message"
```
Now the file is committed to the HEAD, but not in your remote repository yet.

## pushing changes
Your changes are now in the HEAD of your local working copy. 
To send those changes to your remote repository, execute 
```
git push origin master
```
Change master to whatever branch you want to push your changes to. 

If you have not cloned an existing repository and want to connect your repository to a remote server, 
you need to add it with
```
git remote add origin <server>
```
Now you are able to push your changes to the selected remote server

## branching
Branches are used to develop features isolated from each other. 
The master branch is the "default" branch when you create a repository. 
Use other branches for development and merge them back to the master branch upon completion.

### create a new branch named "feature_x" and switch to it using
```
git checkout -b feature_x
```
switch back to master
```
git checkout master
```
and delete the branch again
```
git branch -d feature_x
```
a branch is not available to others unless you push the branch to your remote repository
```
git push origin <branch>
```

## update & merge
to update your local repository to the newest commit, execute 
```
git pull
```
in your working directory to fetch and merge remote changes.
to merge another branch into your active branch (e.g. master), use
```
git merge <branch>
```
in both cases git tries to auto-merge changes. Unfortunately, this is not always possible 
and results in conflicts. You are responsible to merge those conflicts 
manually by editing the files shown by git. After changing, you need to mark them as merged with
```
git add <filename>
```
before merging changes, you can also preview them by using
```
git diff <source_branch> <target_branch>

#abort
git merge --abort
```

## tagging
it's recommended to create tags for software releases. this is a known concept, which also exists in SVN. You can create a new tag named 1.0.0 by executing
```
git tag -a v2.1.4 -m "pom.xml v2.1.4Release"
git tag -l -n3

git tag -l "prefix" #list
git tag -a [tagname eg. v1.4] -m [message eg."my version 1.4"] #create
git checkout [tagname] #change pointer
git checkout -b [new branch] #create branch from this tagname

git tag #list
git show v1.4 #show details
git tag v1.4-lw #light weight less details
git tag 1.0.0 1b2e1d63ff

git log --pretty-oneline
git tag -a v1.2 9fceb02 #list commit history and tag by id

git push origin [tagname] #share 

git tag -d v1.4-lw #delete tag local
git push [remote] : refs/tags/[tagname]
git push origin :refs/tags/v1.4-lw #delete in remote

git checkout [tagname] 
```
the 1b2e1d63ff stands for the first 10 characters of the commit id you want to reference with your tag. You can get the commit id by looking at the... 
log
in its simplest form, you can study repository history using.. git log
You can add a lot of parameters to make the log look like what you want. To see only the commits of a certain author:
```
git log --author=bob
```
To see a very compressed log where each commit is one line:
```
git log --pretty=oneline
```
Or maybe you want to see an ASCII art tree of all the branches, decorated with the names of tags and branches: 
```
git log --graph --oneline --decorate --all
```
See only which files have changed: 
```
git log --name-status
```
These are just a few of the possible parameters you can use. For more, see git log --help
replace local changes
In case you did something wrong, which for sure never happens ;), you can replace local changes using the command
```
git checkout -- <filename>
```
this replaces the changes in your working tree with the last content in HEAD. Changes already added to the index, as well as new files, will be kept.

If you instead want to drop all your local changes and commits, fetch the latest history from the server and point your local master branch at it like this
```
git fetch origin
git reset --hard origin/master
```
## Compare commits
```
# Compare a file
git diff $start_commit..$end_commit -- path/to/file
# Compare changes between two commits
git diff $start_commit..$end_commit

git config --global diff.tool git-meld

```
## useful hints
built-in git GUI
```
gitk
```
use colorful git output
```
git config color.ui true
```
show log on just one line per commit
```
git config format.pretty oneline
```
use interactive adding
```
git add -i
```

## Stash
```
git stash save "Your stash message"

git stash save -u
or
git stash save --include-untracked

git stash list

git stash apply stash@{0} #default
git stash apply stash@{1} #choose other
git reset --hard #remove stash changes

git stash pop stash@{1} #apply and delete from stash

git stash show -p
git stash show stash@{1}

# not revertable
git stash clear
git stash drop stash@{1}
```
## Revert
```
#Revert fourth last commit in HEAD and create a new commit with the reverted changes.
git revert HEAD~3
#Revert from the fifth last commit (included) to the third last commit in master (included)
#but do not create any commit with the reverted changes
git revert -n master~5..master~2

```
## Rebase
```
git config --global rebase.autosquash true
[rebase]
  autosquash = true
or
git rebase -i --autosquash
# Did you know @ is the same as HEAD? Using it during a rebase is a life saver:
git rebase -i @~2
```

## mybenefits git flow
```
git commit -m "feat(seo): update meta tags (#15898)"
git pull origin develop
git add .
git checkout 1a72b3726be #roll back to last commit point (my change)
git last
git stash save -u "pre merge version"
git checkout -b develop-seometatags
```
[Tutorial](http://rogerdudler.github.io/git-guide/)
##  Updating a feature branch
$ git checkout [master]

Merge the changes from origin/master into your local master branch. This brings your master branch in sync with the remote repository, without losing your local changes. If your local branch didn't have any unique commits, Git will instead perform a "fast-forward".
$ git fetch -p origin [master]

Check out the branch you want to merge into
$ git merge origin/[master]

Merge your (now updated) master branch into your feature branch to update it with the latest changes
$ git checkout <feature-branch>

Depending on your git configuration this may open vim. Enter a commit message, save, and quit vim:
Press a to enter insert mode and append text following the current cursor position.
Press the esc key to enter command mode.
Type :wq to write the file to disk and quit.
This only updates your local feature branch. To update it on GitHub, push your changes.
$ git merge [master]

$ git push origin <feature-branch>
```
# workflow
```
# feature_branch
git checkout -b feature_branch
git add .
git stash
git pull origin master
git stash pop
... resolve conflict here
git commit -m "feat(xxx): message"
git push -u origin feature_branch
... merge request ...
```

```
#2a - good
git pull # master
git checkout -b opsbr2a
git add .
git commit -m "opsbr2a"
# 
git pull --rebase origin master
# with conflict
...
Patch failed at 0001 opsbr2a
git am --show-current-patch   # to see the failed patch
git add/rm <conflicted_files> # Resolve all conflicts manually, mark them as resolved with
git rebase --continue
# You can instead skip this commit: run "
git rebase --skip
# To abort and get back to the state before "git rebase", run "
git rebase --abort

# before you push
git rebase -i
...
from 
pick ceb9162 opsbr2a - commit 3
pick 35176f5 opsbr2a - commit 4
to
reword ceb9162 opsbr2a - new combined commit message
squash 35176f5 opsbr2a - commit 4
```

# workflow - no good
```
# at feature branch 
git fetch origin
git rebase origin master
```
# delete remote branch
```
git push --delete origin macbr1

git add <file>  # to update what will be committed)
git checkout -- <file> #to discard changes in working directory
```
# error
```
fatal: git-write-tree: error building trees
Cannot save the current index state

git reset --mixed
```

#unstage file
```
git reset -- src/app/shared/fake-backend.ts
git checkout -- src/app/app.module.ts
```
# Merge
```
https://cysvigdcapmdw37.service.cihs.gov.on.ca/samobile/ms-support/merge_requests/new?merge_request%5Bsource_
branch%5D=sandbox 
``` 
# List
``` 
git diff-tree --no-commit-id --name-only -r bd61ad98
git show --stat --oneline HEAD
git show --stat --oneline b24f5fb
git show --stat --oneline HEAD^^..HEAD

# Keep ignored files out of git status
git update-index --assume-unchanged <file>
git rm --cached <file>
```

# Apply gitignore againt repo
```
git rm -r --cached .
git add .
git commit -m "fix .gitignore"
```

# Cherry-pick
``` 
#from feature branch to master
git checkout master
git cherry-pick [sha at feature branch]
git push
```

# Add cert to git/curl
```
Briefly:
1.Get the self signed certificate
2.Put it into some (e.g. ~/git-certs/cert.pem) file
3.Set git to trust this certificate using http.sslCAInfo parameter

In more details:

Get self signed certificate of remote server

Assuming, the server URL is repos.sample.com and you want to access it over port 443.

There are multiple options, how to get it.

Get certificate using openssl
$ openssl s_client -connect repos.sample.com:443


Catch the output into a file cert.pem and delete all but part between (and including) -BEGIN CERTIFICATE- and -END CERTIFICATE-

Content of resulting file ~/git-certs/cert.pem may look like this:
-----BEGIN CERTIFICATE-----
MIIDnzCCAocCBE/xnXAwDQYJKoZIhvcNAQEFBQAwgZMxCzAJBgNVBAYTAkRFMRUw
EwYDVQQIEwxMb3dlciBTYXhvbnkxEjAQBgNVBAcTCVdvbGZzYnVyZzEYMBYGA1UE
ChMPU2FhUy1TZWN1cmUuY29tMRowGAYDVQQDFBEqLnNhYXMtc2VjdXJlLmNvbTEj
MCEGCSqGSIb3DQEJARYUaW5mb0BzYWFzLXNlY3VyZS5jb20wHhcNMTIwNzAyMTMw
OTA0WhcNMTMwNzAyMTMwOTA0WjCBkzELMAkGA1UEBhMCREUxFTATBgNVBAgTDExv
d2VyIFNheG9ueTESMBAGA1UEBxMJV29sZnNidXJnMRgwFgYDVQQKEw9TYWFTLVNl
Y3VyZS5jb20xGjAYBgNVBAMUESouc2Fhcy1zZWN1cmUuY29tMSMwIQYJKoZIhvcN
AQkBFhRpbmZvQHNhYXMtc2VjdXJlLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEP
ADCCAQoCggEBAMUZ472W3EVFYGSHTgFV0LR2YVE1U//sZimhCKGFBhH3ZfGwqtu7
mzOhlCQef9nqGxgH+U5DG43B6MxDzhoP7R8e1GLbNH3xVqMHqEdcek8jtiJvfj2a
pRSkFTCVJ9i0GYFOQfQYV6RJ4vAunQioiw07OmsxL6C5l3K/r+qJTlStpPK5dv4z
Sy+jmAcQMaIcWv8wgBAxdzo8UVwIL63gLlBz7WfSB2Ti5XBbse/83wyNa5bPJPf1
U+7uLSofz+dehHtgtKfHD8XpPoQBt0Y9ExbLN1ysdR9XfsNfBI5K6Uokq/tVDxNi
SHM4/7uKNo/4b7OP24hvCeXW8oRyRzpyDxMCAwEAATANBgkqhkiG9w0BAQUFAAOC
AQEAp7S/E1ZGCey5Oyn3qwP4q+geQqOhRtaPqdH6ABnqUYHcGYB77GcStQxnqnOZ
MJwIaIZqlz+59taB6U2lG30u3cZ1FITuz+fWXdfELKPWPjDoHkwumkz3zcCVrrtI
ktRzk7AeazHcLEwkUjB5Rm75N9+dOo6Ay89JCcPKb+tNqOszY10y6U3kX3uiSzrJ
ejSq/tRyvMFT1FlJ8tKoZBWbkThevMhx7jk5qsoCpLPmPoYCEoLEtpMYiQnDZgUc
TNoL1GjoDrjgmSen4QN5QZEGTOe/dsv1sGxWC+Tv/VwUl2GqVtKPZdKtGFqI8TLn
/27/jIdVQIKvHok2P/u9tvTUQA==
-----END CERTIFICATE-----


Get certificate using your web browser

I use Redmine with Git repositories and I access the same URL for web UI and for git command line access. This way, I had to add exception for that domain into my web browser.

Using Firefox, I went to Options -> Advanced -> Certificates -> View Certificates -> Servers, found there the selfsigned host, selected it and using Export button I got exactly the same file, as created using openssl.

Note: I was a bit surprised, there is no name of the authority visibly mentioned. This is fine.

Having the trusted certificate in dedicated file

Previous steps shall result in having the certificate in some file. It does not matter, what file it is as long as it is visible to your git when accessing that domain. I used ~/git-certs/cert.pem

Note: If you need more trusted selfsigned certificates, put them into the same file:
-----BEGIN CERTIFICATE-----
MIIDnzCCAocCBE/xnXAwDQYJKoZIhvcNAQEFBQAwgZMxCzAJBgNVBAYTAkRFMRUw
...........
/27/jIdVQIKvHok2P/u9tvTUQA==
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
AnOtHeRtRuStEdCeRtIfIcAtEgOeShErExxxxxxxxxxxxxxxxxxxxxxxxxxxxxxw
...........
/27/jIdVQIKvHok2P/u9tvTUQA==
-----END CERTIFICATE-----


This shall work (but I tested it only with single certificate).

Configure git to trust this certificate
$ git config --global http.sslCAInfo /home/javl/git-certs/cert.pem


You may also try to do that system wide, using --system instead of --global.

And test it: You shall now be able communicating with your server without resorting to:
$ git config --global http.sslVerify false #NO NEED TO USE THIS


If you already set your git to ignorance of ssl certificates, unset it:
$ git config --global --unset http.sslVerify


and you may also check, that you did it all correctly, without spelling errors:
$ git config --global --list


what should list all variables, you have set globally. (I mispelled http to htt).

```

# pull before push
```
git stash save "my change"
git pull origin sandbox
git stash apply 
-- resolve any conflict
git add .
git commit -m "my message"
git push
```

# show what will be push
```
git diff --stat --cached origin/sandbox
git diff --numstat origin/sandbox
git difftool origin/sandbox
```

# Change commit message
```
git commit --amend
git push --force example-branch
 
git rebase -i HEAD~3 # Displays a list of the last 3 commits on the current branch
pick e499d89 Delete CNAME
pick 0c39034 Better README
pick f7fde4a Change the commit message but push the same commit.

# Rebase 9fdb3bd..f7fde4a onto 9fdb3bd
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
pick e499d89 Delete CNAME
reword 0c39034 Better README
reword f7fde4a Change the commit message but push the same commit.

git push --force
```

## Revert to previous commit 
```
git checkout 66ada4cc61d62afc .; 
git add .; 
git commit -m "foo" 
git checkout version brings you info detached head, 
git checkout version filename gets the files from the history

git checkout <hash> . will not remove files though. 
git checkout <hash> 
git reset --soft master 
git commit
```

```
# lost history
git reset --hard <commit>
git push --force origin sandbox
```

## Create branch from a Tag
```
git checkout tags/springboot_2.0-20191030 -b fix/revert-sandbox-spring2

git push -u origin fix/revert-sandbox-spring2

git branch -D fix/revert-sandbox-spring2

```

## force to refresh from remote
```
git fetch --all
git reset --hard origin/sandbox
```

## rollback (permanently)
```
git reset --hard <commit>
git push --force origin sandbox
```

## git master
```
git add :/ (adding the entire working tree)
git add . (adding the current directory)
git add --all
git status 
```

## Un-tracking and re-tracking files
```
git rm --cached <file>   ;ignoring a file that was formerly tracked
git add -f ;retracking or ignored file

```

## Essentials
```
git reset HEAD -- myFile.txt  ;unstage a file
git reset --soft HEAD~1 ;undo last ongoing commit

git log -1 HEAD  ;last commit
git log --oneline --graph --decorate --all 

git diff --cached HEAD^  ;diff against last commit

git commit --amend -m "New commit message"  ;for not push commit only

git blame <filename>  ;look at change of file
git gui blame <filename>

git show <commit-hash>

git init --bare <NewRepo.git>
git clone --bare myProject myProject.git

git archive master --format=zip --output=../repoBackup.zip
git archive HEAD --format=zip --output=../headBackup.zip

```

## Unstage a file
```
git restore --staged src/main/java/org/mcss/samobile/api/batch/service/CuramRest2Service.java
```

## Misc notes
```
-- commited all your changes to feature-branch
git checkout sandbox
git pull --rebase
git rebase sandbox my_new_feature

git checkout master
git merge my_new_feature
git push


$ git checkout sandbox
$ git fetch -p origin sandbox   #or git fetch origin
$ git merge origin/sandbox
$ git checkout <feature-branch>
$ git merge sandbox
$ git push origin <feature-branch>

Method 
1)
git checkout feature
git fetch origin
git rebase origin/dev

2)
git checkout feature
git pull --rebase origin/dev

3)
git checkout dev
git pull
git checkout feature
git rebase dev

git pull upstream develop
git push origin

git checkout -b new-feature
git push -u origin new-feature

git reset --hard origin/master


#Cherry Pick
git log # take note of the commit IDs, which are 32 digit hexadecimal strings

git checkout master # switch back to the real master branch
git cherry-pick [ID] # repeat for each ID
git cherry -v # doublecheck what will actually be pushed
git push origin master

git checkout my_master # safe again


##Common git commands##

upstream - the github repository.

fetch and merge changes from upstream:

git pull
commit all local changes:

git commit -a -m “message”
commit changes in specific files:

git add <files…>

git commit -m “message”
get patch files for last N commits

git format-patch -N
get patch files for all commits that are in your current branch but not in upstream:

git format-patch origin/master
undo (delete) all current non-commited changes (Watch out !):

git reset --hard HEAD
undo last commit, but leave the changes in the working tree

git reset --soft HEAD^1
undo (delete) all commits since last time one was pulled from upstream (Watch out !). This should be done while in the master branch:

git reset --hard origin/master
undo all commits since the last time one was pulled from upstream but leave the changes in the working tree. This should be done while in the master branch:

git reset --soft origin/master
show uncommited local changes:

git diff [file]
discard local changes instead of commiting them:

git checkout — <file>
add interactively

git add -i
To revert local commits, revert will create another commit that undoes the commit provided. Unlike reset the history is not erased.

git revert <commit ID>
show changes compared to upstream:

git diff origin/master    # only diff

git show origin/master..  # log and diff

git log origin/master..   # only log
show latest log in upstream:

git log origin/master
see status of changes:

git status
```