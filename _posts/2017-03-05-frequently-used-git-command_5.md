---
layout: post
title: Frequently Used Git Command
date: '2017-03-05T07:50:00.001-08:00'
author: Ivan
tags: Git
categories:
- Others
modified_time: '2017-03-05T08:14:57.877-08:00'
thumbnail: https://4.bp.blogspot.com/-2z36zSf7b88/WLw2Gzqu0pI/AAAAAAAAKkI/bbLfQ6OVb6A7kvEkqgi-lg8nhvh9abo7ACK4B/s72-c/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%2B2017-03-05%2B%25E4%25B8%258B%25E5%258D%258810.55.23.png
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-822634844834151404
blogger_orig_url: http://invictuscode.blogspot.com/2017/03/frequently-used-git-command_5.html
---

Recently, I use Git a lot and it’s really helpful when you are different kind of situation. In my opinion, I think Git is more useful than Subversion because it provides local version control and multiple features to deal with different branches. Let me walk you through how to use Git.  
  
  

### Clone repositoy

First, you can clone remote repository, for example, we clone Retrofit repository.  

    

    $git clone https://github.com/square/retrofit

  
  

### Check the history log

    $git log

  
or you can show it in graph  

    

    $git log --graph

  
type q to leave log.  
you will see commits and messages in a list…  
  
  

### Show status

Assume that you modified README.MD, and then you can try “git status”.  

    

    $git status

    

    

  
We use git status a lot to check file status.  
  
  

### Add file to staging area

    $git add README.MD

  
If you got too many modified files, you can add them all:  

    

    $git add .

  
but normally, we don’t do that because sometimes you don’t wanna to commit all files.  
  
  

### Unstage file

    $git reset HEAD README.MD

  
  

### Discard changes

Discard changes by checkout files  
  
$git checkout README.MD  
  
  

### Checkout file differences

    $git diff

    

    

  
Press ESC, then type :q to leave viewer.  
  
  

### Commit changes

    $git commit

  
Then edit message in vim editor.  
Press ESC, then type :wq to save message after you finish editing message.  
But there is a faster way, you can write one line commit message.  

    

    $git commit -m "Your commit message here."

  
  

### Show history

    $gitk

  
A window will prompt out, and the rest… you know what to do ;)  
  
  

### Create a branch

For example, I create a branch called “hola”.  

    

    $git branch hola

  
  

### Switch to another branch

In this example, I switch to master branch.  

    

    $git checkout master

  
  

### Push changes

Push your commits to server  

    

    $git push

  
  

### Git stash

This is really useful. For example, you are developing a new feature, however, your boss tells you to fix a bug immediately. You can use git stash to temporarily store your changes.  

    

    $git stash

    $git checkout debug

  
And now you can switch to a debug branch, once you fix the bug, you commit changes, and then go back to your developing branch.  

    

    $git checkout dev

  
then pop your previous changes, and go back to develop feature.  

    

    $git stash pop

  
  

### Cherry Pick

you can pick commit from a branch, and then put it into another branch by commit id. (commit id can be found in git log)  

    

    $git cherry-pick c8d652089

  
  

### Quickly modify last commit

    $git commit --amend

  
  

### Drop your commit

    $git reset HAED^

  
  

### Rebase a branch

In general, you develop in another branch, you want to push your commits to master.  
In this case, you can use git rebase to update your developing branch with master branch.  

    

    $git rebase master

  
After you done developering, you can merge two branches under master branch:  

    

    $git merge dev