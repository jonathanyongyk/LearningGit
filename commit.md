- [Overview](#overview)
- [Commit](#commit)
  - [Show commit pending to push](#show-commit-pending-to-push)
  - [Discard last commit and revert to the previous commit (reset)](#discard-last-commit-and-revert-to-the-previous-commit-reset)
  - [Revert a specific commit (go back to the commit before it)](#revert-a-specific-commit-go-back-to-the-commit-before-it)
  - [Revert un-commited change](#revert-un-commited-change)
  - [Unstage a file](#unstage-a-file)
  - [Restore a deleted file](#restore-a-deleted-file)
  - [Restore a deleted file that was commited with other changes.](#restore-a-deleted-file-that-was-commited-with-other-changes)
  - [Show content of a commit](#show-content-of-a-commit)
  - [View what has change in each commit](#view-what-has-change-in-each-commit)
  - [View commits history in GUI tool](#view-commits-history-in-gui-tool)
- [Rewriting commit](#rewriting-commit)
  - [Change the last commit message](#change-the-last-commit-message)
  - [Change the content of the last commit.](#change-the-content-of-the-last-commit)
- [Tracing/Listing/Following commits](#tracinglistingfollowing-commits)
  - [Follow and list commits](#follow-and-list-commits)
  - [Following commits which is unique in a branch.](#following-commits-which-is-unique-in-a-branch)
  - [Following merge commits.](#following-merge-commits)

# Overview
This demo show common commands related to working with commit.

# Commit
## Show commit pending to push
1. Clone a repo from the server.
2. Make some changes locally and commit.
3. Run **git diff origin/main..HEAD**, it will show pending commit to be push to server

## Discard last commit and revert to the previous commit (reset)
1. Add a new file *file4.txt*.
2. Commit *file4.txt*.
3. Edit *file4.txt* by adding a new line
4. Commit *file4.txt*
5. Run **git reset --hard HEAD^**. This will revert the HEAD back to one commit before the current commit(current HEAD).
   * This will discard the last commit you did, essentially undo the last commit.
   * Other reading: https://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard

## Revert a specific commit (go back to the commit before it)
1. Add a new file *file8.txt*.
2. Commit *file8.txt*.
3. Add a new line into file8 and commit. Do this 3 time. (line2, line3, line 4)
4. Run **git log**.
5. Take the commit hash for the commit that add line 3
6. Run **git revert \<commit hash\>**.
7. You should get a conflict.
8. Undo the revert by running **git revert –-abort**
   * We are doing this intentionally to show how you can abort a revert
9. Run step 6 again. This time we will resolve the conflict manually in the file.
10. Then run** git add** and **git commit** to commit the changes
11. Run **git log** again and you will see that the revert create a new commit.
 
## Revert un-commited change
1. Create a file *file6.txt*.
2. Commit *file6.txt*.
3. Modify *file6.txt*, but don't commit it yet.
4. Add *file7.txt*.
5. Run **git restore file6.txt** to undo the changes on *file6.txt*.
6. Run **git clean -n** to do a dry run. Dry run will simulate what will happen without actually performing the action. In this case, it should show that it would remove file7.txt
7. To actually remove file7.txt, run **git clean -f** to remove untrack file *file7.txt*. Or you can run **git clean -f \<filename\> if you just want to remove one specific file.


## Unstage a file
1. You can unstage a file back to working directory by usng the **git reset** command.
2.  Run the following powershell script to setup a demo repo.
    ```powershell
    git init

    Add-Content -Path .\file1.txt -Value 'line1'
    git add file1.txt
    git commit -m "add line1"

    Add-Content -Path .\file1.txt -Value 'line2'
    git add file1.txt
    git commit -m "add line2"

    Add-Content -Path .\file1.txt -Value 'line3'
    Add-Content -Path .\file2.txt -Value 'line3'
    git add file1.txt
    git add file2.txt
    git commit -m "add line3"
    ```
3. Run the following command to 
   * make a change and stage file1.txt.
   * make a change but not stage file2.txt.
   * add a new file3.txt and stage.
   * add a new file4.txt but not stage.
   ```powershell
    Add-Content -Path .\file1.txt -Value 'line4'
    git add file1.txt

    Add-Content -Path .\file2.txt -Value 'line4'

    Add-Content -Path .\file3.txt -Value 'line4'
    git add file3.txt

    Add-Content -Path .\file4.txt -Value 'line4'
   ```    
4. Run **git status** to check on the status of the files. file1.txt and file3.txt is in staging area.   
5. Run **git reset** to unstage the files back to working directory. Run **git status** to check on the current repo status.
6. If you want to undo all changes and go back to its original state, run **git reset --hard**.
   

## Restore a deleted file
1. Initialize a git repo. Run **git init**.
2. Add file1 and do a few commits.
```Powershell
Add-Content -Path .\file1.txt -Value 'line1'
git add file1.txt
git commit -m "add line 1"

Add-Content -Path .\file1.txt -Value 'line2'
git add file1.txt
git commit -m "add line 2"
```
2. Add file2 and do a few commits.
```Powershell
Add-Content -Path .\file2.txt -Value 'file2-line1'
git add file2.txt
git commit -m "file2 add line 1"

Add-Content -Path .\file2.txt -Value 'file2-line2'
git add file2.txt
git commit -m "file2 add line 2"
```
3. Delete file1.txt and commit
```Powershell
del file1.txt
git add file1.txt
git commit -m "delete file1"
```
4. Add file3 and do a few commits.
```Powershell
Add-Content -Path .\file3.txt -Value 'file3-line1'
git add file3.txt
git commit -m "add file3.txt"

Add-Content -Path .\file3.txt -Value 'file3-line2'
git add file3.txt
git commit -m "file3 add line 2"
```
4. Now run **git log --name-status**. This show the history and the status of the affected file.
5. Take the hash of the commit that delete file1.txt
6. Run **git revert --no-commit \<hash of the commit that delete file1.txt\>**. This will bring back file1.txt into the staging area. If you run **git status**, you can see file1.txt is added.
7. Commit the change, run **git commit -m "restore file1"**.

## Restore a deleted file that was commited with other changes.
Real life can be complicated. the previous scenario was simple because the deleted file was in its own commit. What if the deleted file was commited with other changes? how do we bring just that one file back?
1. Initialize a git repo. Run **git init**.
2. Add file1 and do a few commits.
```Powershell
Add-Content -Path .\file1.txt -Value 'line1'
git add file1.txt
git commit -m "add line 1"

Add-Content -Path .\file1.txt -Value 'line2'
git add file1.txt
git commit -m "add line 2"
```
2. Add file2 and do a few commits.
```Powershell
Add-Content -Path .\file2.txt -Value 'file2-line1'
git add file2.txt
git commit -m "file2 add line 1"

Add-Content -Path .\file2.txt -Value 'file2-line2'
git add file2.txt
git commit -m "file2 add line 2"
```
3. Delete file1.txt
```Powershell
del file1.txt
git add file1.txt

```
4. Add file3.txt
```Powershell
Add-Content -Path .\file3.txt -Value 'file3-line1'
git add file3.txt
```
5. Run **git status**. You should see file1.txt is deleted and file3.txt is added.
6. Run **git commit -m "delete file1 and add file3"**.
7. Run **git log --name-status** and take note of the status of file in each commit and the hash of commit that delete file1.txt
8. Add additional content to file3.txt
```Powershell
Add-Content -Path .\file3.txt -Value 'file3-line2'
git add file3.txt
git commit -m "file3 add line 2"
```
9. We will now restore *file1.txt*, Run **git checkout \<hash of the commit that delete file1.txt\>^ file1.txt**. (take note of the caret (^) sign after the hash. This mean take one commit before the commit specified). 
10. This will bring back *file1.txt* into the staging area. If you run **git status**, you can see *file1.txt* is added. You can also display the contents of all files to ensure the contents are all intact.
11. Commit the change, run **git commit -m "restore file1"**.
13. Run **git log** again to show the history.
    
## Show content of a commit
You can get details of a commit such as who , when, commit message and commit parent.
1. Run the following powershell script to setup a demo repo.
   ```powershell
    git init

    Add-Content -Path .\file1.txt -Value 'line1'
    git add file1.txt
    git commit -m "add line1"


    Add-Content -Path .\file1.txt -Value 'line2'
    git add file1.txt
    git commit -m "add line2"


    Add-Content -Path .\file1.txt -Value 'line3a'
    Add-Content -Path .\file1.txt -Value 'line3b'
    git add file1.txt
    git commit -m "add line3"

    Add-Content -Path .\file1.txt -Value 'line4'
    git add file1.txt
    git commit -m "add line4"   
   ```
2. Run **git log** to get the commit ID for "add line3"
3. Run **git cat-file -p \<commit id\>**. 
   
## View what has change in each commit
We can trace back what are the changes in each commit. This enable you to quickly browse back the the logs to identify when you commit a specific change.
1. Run the following Powershell script to initialize a repo and make a few changes.
   ```powershell
      git init

      Add-Content -Path .\file1.txt -Value 'file 1 line1'
      git add file1.txt
      git commit -m "add file1 line1"

      Add-Content -Path .\file1.txt -Value 'file 1 line2'
      Add-Content -Path .\file2.txt -Value 'file 2 line1'
      git add file1.txt
      git add file2.txt
      git commit -m "Update file 1 line2, add file 2 line1"


      Add-Content -Path .\file1.txt -Value 'file 1 line3'
      git add file1.txt
      git commit -m "Update file1 line3"

      Add-Content -Path .\file1.txt -Value 'file 1 line4'
      Add-Content -Path .\file2.txt -Value 'file 2 line2'
      git add file1.txt
      git add file2.txt
      git commit -m "Update file 1 line4, Update file 2 line2"

      Add-Content -Path .\file1.txt -Value 'file 1 line5'
      git add file1.txt
      git commit -m "Update file1 line5"

      Add-Content -Path .\file1.txt -Value 'file 1 line6'
      Add-Content -Path .\file2.txt -Value 'file 2 line3'
      git add file1.txt
      git add file2.txt
      git commit -m "Update file 1 line6, Update file 2 line3"
   ```
2. Run ```git log --oneline``` to get the commits ID.
3. Get the commit ID for "Update file 1 line2, add file 2 line1" and "Update file1 line5".
4. To view the file changes from one commit to another, run ```git log -p <commit ID of 'Update file 1 line2'>..<commit ID of 'Update file1 line5'>```. This will show you all the files involved in the commit.
5. If we are only interested in one file, We can specify the filename as parameter. Run ```git log -p <commit ID of 'Update file 1 line2'>..<commit ID of 'Update file1 line5'> -- file1.txt```.
6. You may notice that the log display the latest commit first and go in time descending order, not very intuitive if our purpose is to view the evolution in chronological order. To view by time in ascending order, we can use the **--reverse** flag. Run ```git log -p --reverse <commit ID of 'Update file 1 line2'>..<commit ID of 'Update file1 line5'> -- file1.txt```.

## View commits history in GUI tool
1. On Windows, you can run ```gitk``` to view the commit history in a GUI tool.
      
# Rewriting commit
## Change the last commit message
1. Run **git init** to initialize a repo.
2. Create a new file *file1.txt*
3. Add 3 lines to *file1.txt* and commit each line separately
```Powershell
Add-Content -Path .\file1.txt -Value 'line1'
git add file1.txt
git commit -m "add line 1"

Add-Content -Path .\file1.txt -Value 'line2'
git add file1.txt
git commit -m "add line 2"

Add-Content -Path .\file1.txt -Value 'line3'
git add file1.txt
git commit -m "add line 3"
```
4. Run **git log**. you should see 3 commmits.
5. Now **run git commit --amend**. This will open the last commit message in a editor. You can see the original commit message.
6. Add "it got some cool stuff" behind the original commit message. save and close the edit.
7. Run **git log**. You can see the commit message has been updated.
```text
After the --amend complete, a new commit id will be generated replacing the previous one. This is because the commit id hash is calculated based on the committed changes, commit message, and timestamp. If any one of this change, the commit id hash will change also.
```
## Change the content of the last commit.
1. Make sure you have run the previous demo "Change the last commit message".
2. edit file1.txt by adding "(forgot something)" to the end of "line 3"
3. Stage the change by run **git add file1.txt**.
4. Now run **git commit --amend**. This will open the last commit message in a editor. You can see the original commit message.
5. Add "missing some stuff" after the original commit message. save and close the edit.
6. Run **git log**. You can see the commit message has been updated and file1.txt is added to the commit.
7. You can also update the content, commit it without a new commit message.
8. Edit *file1.txt* by making some change.
9. Stage the change by run **git add file1.txt**.
10. Now run **git commit --amend --no-edit**. This will update the commit without prompt you for a commit messgae.
11. Run **git log**. You should not see any change in the commit log. But if you run **git diff**, you can see the change in the content.

# Tracing/Listing/Following commits
Some times you may want to follow the historical commits to understand how the repo or even just a particular file has evolved over time. This can be times where you may want to know when a change was made to a file, or just in general what are the list of commits that touches a specific file. To do this, you can use the ```git rev-list``` command. This command list commits that are reachable by following the parent links from the given commit(s).

The tutorial in this section will be a bit lengthly and you will need to perform in the sequence as per the following topics.

## Follow and list commits
1. Run the following Powershell command to initialize the repo and make a few commits.
   ```powershell
   git init

   Add-Content -Path .\file1.txt -Value 'file 1 line1'
   git add file1.txt
   git commit -m "add file1 line1"

   Add-Content -Path .\file1.txt -Value 'file 1 line2'
   Add-Content -Path .\file2.txt -Value 'file 2 line1'
   git add file1.txt
   git add file2.txt
   git commit -m "Update file 1 line2, add file 2 line1"

   Add-Content -Path .\file1.txt -Value 'file 1 line3'
   git add file1.txt
   git commit -m "Update file1 line3"

   Add-Content -Path .\file1.txt -Value 'file 1 line4'
   Add-Content -Path .\file2.txt -Value 'file 2 line2'
   git add file1.txt
   git add file2.txt
   git commit -m "Update file 1 line4, Update file 2 line2"

   Add-Content -Path .\file1.txt -Value 'file 1 line5'
   git add file1.txt
   git commit -m "Update file1 line5"

   Add-Content -Path .\file1.txt -Value 'file 1 line6'
   Add-Content -Path .\file2.txt -Value 'file 2 line3'
   git add file1.txt
   git add file2.txt
   git commit -m "Update file 1 line6, Update file 2 line3"

   Add-Content -Path .\file1.txt -Value 'file 1 line7'
   git add file1.txt
   git commit -m "Update file1 line7"

   Add-Content -Path .\file1.txt -Value 'file 1 line8'
   Add-Content -Path .\file2.txt -Value 'file 2 line4'
   git add file1.txt
   git add file2.txt
   git commit -m "Update file 1 line8, Update file 2 line4"

   Add-Content -Path .\file1.txt -Value 'file 1 line9'
   git add file1.txt
   git commit -m "Update file1 line9"

   Add-Content -Path .\file1.txt -Value 'file 1 line10'
   Add-Content -Path .\file2.txt -Value 'file 2 line5'
   git add file1.txt
   git add file2.txt
   git commit -m "Update file 1 line10, Update file 2 line5"
   ``` 
2. Before you proceed further, understand the script above to understand exactly the commit history of the files involved. In simple words, we made 10 commits to *file1.txt*. For every other commmit to *file1.txt*, we also made a commit to *file2.txt*.   
3. Run ```git log --oneline``` to get the list of all commits. Copy and paste it into a text file. We will need to refer to the commit ID in the later steps.
4. Get the commit ID for "Update file 1 line8, Update file 2 line4". Run ```git rev-list <commit ID for "Update file 1 line8, Update file 2 line4">```. You should see there is 8 commits listed, from the commit you specified leading back to the very first commit in the repo.
5. The above command will target all commits in general. Next we want to list the commits just for *file2.txt*. As you may understand from the earlier script, file2.txt has much less commit.
6. Get the commit ID for "Update file 1 line8, Update file 2 line4". Run ```git rev-list <commit ID for "Update file 1 line8, Update file 2 line4"> -- file2.txt```. You should see there is only 4 commits listed, from the commit you specified leading back to the very first commit in the repo that involved *file2.txt*.
7. If you just want to list only a specifc range of commits, you can also do that. Get the commit ID for "Update file 1 line8, Update file 2 line4" and "Update file 1 line4, Update file 2 line2".
8. Run ```git rev-list <commit ID for "Update file 1 line8, Update file 2 line4"> ^<commit ID for "Update file 1 line4, Update file 2 line2">```. You should see there is only four commits, from "Update file 1 line8, Update file 2 line4" to "Update file1 line5". The caret (^) sign indicate we want up-to but exclude the commit. You can also achive the same with ```git rev-list <commit ID for "Update file 1 line4, Update file 2 line2">..<commit ID for "Update file 1 line8, Update file 2 line4">```.
   
## Following commits which is unique in a branch.
This technique is useful when your branches has diverted and you want to inspect the commits in each branch before you merge, or even just to get a sense of how far behind your branch is. Before you continue, make sure you have complete the tutorial before.
1. Run the following Powershell script to create a branch and add 3 commits to a new file, *file3.txt*.
   ```powershell
   git checkout -b f1
   Add-Content -Path .\file3.txt -Value 'file 3 line1'
   git add file3.txt
   git commit -m "Update file3 line1"

   Add-Content -Path .\file3.txt -Value 'file 3 line2'
   git add file3.txt
   git commit -m "Update file3 line2"

   Add-Content -Path .\file3.txt -Value 'file 3 line3'
   git add file3.txt
   git commit -m "Update file3 line3"   
   ```
2. Still in *f1* branch, Run ```git log --oneline``` to get the list of commits. If you just want to get the list of new commits you made in *f1*, run ```git log --oneline master..HEAD```. Copy the commits to a text file where you have the commits from the *master* branch earlier.
3. We now switch back to *master* branch and made two more commits. After which, there will be a diversion between *master* and *f1* branch. Run the following Powershell script.
   ```powershell
   git checkout master
   Add-Content -Path .\file1.txt -Value 'file 1 line11'
   git add file1.txt
   git commit -m "Update file1 line11"

   Add-Content -Path .\file1.txt -Value 'file 1 line12'
   git add file1.txt
   git commit -m "Update file1 line12"   
   ```
4. Run ```git log --oneline``` to get the new commit list in *master* branch.
5. We now know we have some some changes in *master* branch. Let say now we want to find out what are the new commits in *master* that is not in *f1*. The general logic is to take all commits that are reachable in *master*, but exlcude the commits which are also reachable from *f1*. To do this, first make sure you are still in *master* branch, run ```git rev-list HEAD ^f1```. You should only see two commits in the output which match the two latest commits you made in *master*.
6. Similarly, if you want to get the list of new commits you made in *f1* since you branch out from *master*, run ```git rev-list f1 ^HEAD```. You should see three commits.

## Following merge commits.
1. Make sure you are still in *master* branch, run ```git merge f1``` to merge the three changes from *f1*. Since *master* has also new commits, git cannot perform a fast-forward merge. Instead it will do a three way merge and create a new merge commit.
2. Run ```git log --oneline``` and we can see the updated commit log. There is a new merge commit "Merge branch 'f1'". You can also run ```gitk``` to get a visualization of the commit log.
3. Run the following powershell script to create two more commits in *master* branch.
   ```powershell
   Add-Content -Path .\file1.txt -Value 'file 1 line13'
   git add file1.txt
   git commit -m "Update file1 line13"

   Add-Content -Path .\file1.txt -Value 'file 1 line14'
   git add file1.txt
   git commit -m "Update file1 line14"
   ```
4. Run ```git log --oneline``` to get the new commit log.
5. Run ```git rev-list <latest commit ID> --merges```. This time you should only see one commit in the output. This commit is the new commit when you merge from f1 ("Merge branch 'f1'"). The *--merges* flag tell git to list only merge commit.
6. If you want to know the two commits which are merged in this commit, run ```git cat-file -p <commmid ID of "Merge branch 'f1'">```. You should see there are two parent commits.