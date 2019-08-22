---
title: "[Git] Git Command Cheat Sheet / Git 명령어 정리"
date: 2019-07-27T21:17:55+09:00
draft: false
---




## Git Command Cheat Sheet



### 1. Basics

+ check out your git version   

  ```bash
  git --version
  ```

+ change username and email

  ```bash
  git config --global user.name "A-one_Day1"
  git config --global user.email "dwdsuh@gmail.com"
  ```

+ help

  ```bash
  git help config
  git config --help
  ```

  config is just an example. you can type clone, merge etc



+ initialize git

  ```bash
  git init
  ```

+ remove git

  ```bash
  rm -rf .git
  ```

+ check out the state

  ```bash
  git status
  ```

+ make some file not tracked by git(not to push to repository)

  ```bash
  touch .gitignore
  #open with text editor
  #add the file name which you want to get ignored
  ```

+ git overview 

![git_overview](../images/git_overview.png?raw=true)


+ add files to the staging area

  ```bash
  git add -A #add all files
  git status #check what is in the staging area
  ```

+ remove files from the staging area

  ```bash
  git reset <file name> #remove <file name> from the staging area
  git reset #remove all files from the staging area
  ```

+ make commit

  ```bash
  git commit -m "Commit Message"
  ```

+ check the history

  ```bash
  git log
  ```

+ cloning a remote repostitory

  ```bash
  git clone <url> <where to clone>
  #example
  git clone https://github.com/superwonderfulrepo.git .   # . means current directory 
  ```

+ viewing information about the remote repository

  ```bash
  git remote -v #info about the remote repository
  git branch -a #list every branch of the repository
  ```

+ pushing changes

  ```bash
  git diff #show the changes that I made
  git status
  git add -A
  git commit -m "commitment message"
  
  git pull origin master #pull every change from the remote repo
  git push origin master #origin: name of the remote repo / master: branch
  ```

+ common workflow

  ```bash
  git branch <branch_name> #make a branch. people usually set the branch name with the task they gon do within that branch
  
  git branch #check the list of the branches
  git checkout <branch_name> #moving on to the other branch 
  # make changes like editing your python script or bash script
  git add -A
  git commit -m "commitment message" #we successfully commit the change to the local branch. no effect on our remote repo
  git push -u origin <branch_name> #origin: the name of the remote repo
  git branch -a #see all of our branch
  
  git checkout master
  git pull origin master
  
  git branch --merged #checkout the branched that we merged. Nothing but master would show up at the first time
  git merge <branch_name> #you should be in the master branch to merge the other branch
  git push origin master #push the change to the remote repo
  ```

+ Delete the branch

  ```bash
  git branch --merged #check out the branched which have already been merged. If the branch name showes up, it means the branch would be deleted since it is already merged to the master branch.
  
  git branch -d <branch_name> #but the branch exists in the remote repo
  git push origin --delete <branch_name> #delete the branch in the remote repo
  ```

  

+ example code (test yourself if you understand what's going on with those codes)

  ```bash
  git branch subtract
  git checkout subtract
  ## do something with the subtract function in your file
  git status
  git add -A
  git commit -m "editted substract function"
  git push -u origin subtract
  git checkout master
  git pull origin master
  git merge subtract
  git push origin master
  git branch --merged
  git branch -d subtract
  git push origin --delete subtract
  ```

### 2. Fixing Mistakes and Undoing Bad Commits 

+ Before you made any commit: Undo what you have done

  ```bash
  git checkout <file_name>
  ```

+ After you made commit: edit the commit message of the very last commit

  ```bash
  git commit --amend -m "commit message of the commit that you want to undo"
  
  git log #check out you edited it properly
  ```

+ Adding the change to the last commit

  ```bash
  #example: assume that you want to .gitignore to the last commit
  
  touch .gitignore
  git status
  git add .gitignore 
  
  git commit --amend ## this show interactive editer/ :wq
  git log #check out that the number of commits has not chaged
  git log --stat 
  ```

+ Cherry-Pick (when you made commit in the wrong branch)

  ```bash
  git log #grab the log of the commit
  git checkout <branch_name>
  git log
  git cherry-pick <hash of the log> #brought the commit from the new branch from the master branch
  git log #now we can see the commit is added to our new branch 
  git checkout master
  git log #but we still have the commit in our master branch
  
  
  # Three ways to undo the commit(soft, mixed, hard)
  
  git reset --soft <hash of the log>
  git log
  git status #still have changes in the staging area
  
  git reset <hash of the log> #reset mixed is the default
  git status #still have changes in the working directory
  
  git reset --hard <hash of the log> #Be Careful!! It gon remove all the changes in the tracked files. But if you made a new file during the commit, the file would not be deleted. 이미 있던 파일의 수정사항은 다 없어지고, 새로 만든 파일을 없어지지 않는다. 
  
  git clean -df #remove every untracked "d"irectories and "f"iles
  ```

+ Show commits by Chronological Order (This can be a Life Saver)

  ```bash
  git reflog #walk through what you have doen
  
  git checkout <hash of the log> #we are in the detached head state --> this will be garbage-collected in the future. 
  git log
  git branch backup #make <backup> branch
  git branch
  git checkout master
  git branch
  git checkout backup
  git log
  ```

+ When Others Already Pulled the Changes

  ```bash
  git log
  git revert <hash of the commit>
  git log
  
  
  git diff <hash of commit 1> <hash of commit 2> #this showed the difference between two commits
  ```



### 3. Stash Commend

Commends when you want to save the current  status in the temporary space.

stash: v. store (something) safely and secretly in a specified place.



+ stash our changes

  ```bash
  #working on something in some branch
  git stash save "type some message worked on add function"
  git diff #nothing showes up
  git status #nothing to commit
  git stash list #shows the stashes 
  git stash apply <first col that appeared in the list above> 
  git stash list # stash is still there. we are taking the change but not getting rid of it. 
  git checkout -- . #reset to where we were
  
  git stash list
  git stash pop #grap the very first stash in the list, apply the change, and drop the stash 
  
  git stash save "example message"
  git stash list
  #do something
  git stash save "did something else"
  git stash list
  git stash pop
  
  git stash drop <name of the stash> #drop the stash
  git stash list
  
  git stash clear #drop all stashes
  git stash list
  ```

+ workflow

  + Senerio: We were working on a master branch and the works have not commited yet. You realize the works should be commited in the other branch. You can stash the changes, checkout the branch, and apply the changes. 

  ```bash
  #on master branch work on something
  git stash save "Oops. I was working on the master branch"
  git status
  git checkout add #moving to add branch
  git stash pop 
  git diff
  git add .
  git commit -m "commit message"
  ```

### 4. git add

```bash
git add -A
git add --all #those two commands are identical. those add every change to the staging area even though you are in a subdirectory,

git add -A my_dir/ #add changes within the my_dir to the staging area. Leave the changes in the parent directory intact. 
git add my_dir/ #this do the exactly the same thing. 

git add --no-all my_dir #Not staging the deleted files in my_dir

git add -u #same as --update. Stages "deleted", "modified" files but not untracked files(newly touched).

git add -u my_dir/ 

git add . # . means current directory
#if you are in the top directory it is same with "git add -A"

git add -A # add all changes in the entire tree no matter where you are currnetly in. 

git add * #not recommended. * is shell command. do not include hidden files. 
```






