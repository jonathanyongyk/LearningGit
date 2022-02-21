- [History](#history)
  - [Viewing history](#viewing-history)
  - [Stashing](#stashing)
  - [Unstage a change](#unstage-a-change)
  - [Showing evolution of a file](#showing-evolution-of-a-file)
# History
## Viewing history
1. Run **git init** to initialize a new repo
2. Run the following Powershell script. This will create 7 commits with one new file added in each commit.
   ```powershell
   Add-Content -Path .\file1.txt -Value 'file1'
   git add file1.txt
   git commit -m "add file 1"

   Add-Content -Path .\file2.txt -Value 'file2'
   git add file2.txt
   git commit -m "add file 2"

   Add-Content -Path .\file3.txt -Value 'file3'
   git add file3.txt
   git commit -m "add file 3"

   Add-Content -Path .\file4.txt -Value 'file4'
   git add file4.txt
   git commit -m "add file 4"

   Add-Content -Path .\file5.txt -Value 'file5'
   git add file5.txt
   git commit -m "add file 5"

   Add-Content -Path .\file6.txt -Value 'file6'
   git add file6.txt
   git commit -m "add file 6"

   Add-Content -Path .\file7.txt -Value 'file7'
   git add file7.txt
   git commit -m "add file 7"
   ```
3. Run **git log**. This will show the history.
4. By default, it will show the latest log first, if you want to see the oldest log first, run **git log --reverse**.
5. If your log is long enough, git log will pause and you may need to press <space> to continue or 'q' to exit. If you don\'t want the pause run **git --no-pager log**.
6. If you want to see the files affected in each commit, run **git log --name-only**.  
7. Run **git log --name-status**. This will show the history with what file is affected, and the status of the file.
8. If you want a simple view with one line for each commit, run **git log --oneline**.
9. You can also run **git log --format=oneline**. The difference is --format-oneline will print the full hash of the commit
10. If you want to see the file affected between two commits, run **git log --name-only \<start_commit\>..\<end_commit\>**. Try replace the start and end commit with the commit hash of "add file 3" and add file 5". This will show you file4.txt and file5.txt are changed between this commit.
11. If you have a long history of log, you can also specify you want log that is only since certain date. Run **git log --since="yyyy-mm-dd"**.
12. More interestingly, you can also ask for log that is just X days go. For example, to view log that is 3 days ago, run **git log --since="3 days ago"**.

## Stashing
Stashing allow you to temporary "clean up" those dirty files (work in progress) and revert to a clean state. You can do other work then and when complete, restore those dirty files into the workspace.
1. Add *file9.txt* and commit
2. Add *file10.txt* but don't commit
3. Modify *file9.txt* but don't commit
4. Now, we are asked to resolve some urgent issue and need to undo our work-in-progress to work on the urgent issue. Run **git stash push --include-untracked -m "got disturpt"** to stash the file. You can verify *file10.txt* is disappear and *file9.txt* is un-modified (the un commit change is undo).
5. Run **git stash list** to show all stash.
6. Run **git stash show 0** to show detail of the first stash.
7. We have not complete our urgent work and ready to resume back our previous work. Run **git stash pop** to get back the changes. You can verify *file10.txt* and *file9.txt* is same as step 2 and step 3.

## Unstage a change
1. Add a new file *file5.txt*.
2. Run **git add \***.
3. Run **git status** to show the staging.
4. Run **git rm –cached file5.txt**
   * Or run **git restore –staged file5.txt**
5. Run git status again to show that the file is not staged anymore.

## Showing evolution of a file
You can understand the evolution of a file by seeing who make what change by when into which commit.
1. Run the following script to setup a demo repo.
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
2. Run **git blame file1.txt**. This will show the content of the file along with the commit ID, who make the change and when.