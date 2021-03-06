
- [Tools](#tools)
- [Config](#config)
  - [Setting user name and email](#setting-user-name-and-email)
  - [Listing command](#listing-command)
  - [Alias](#alias)
  - [Setting Editor](#setting-editor)
- [Move/Rename and delete file](#moverename-and-delete-file)
  - [Move/Rename file](#moverename-file)
  - [Delete file](#delete-file)
  - [Delete a file from repo but keep it in working directory.](#delete-a-file-from-repo-but-keep-it-in-working-directory)
- [Squash](#squash)
  - [Squash multiple commits into one during merge](#squash-multiple-commits-into-one-during-merge)
  - [Squash multiple commits into one within the same branch](#squash-multiple-commits-into-one-within-the-same-branch)
- [Bisect](#bisect)
  - [Using Bisect to find bad commit](#using-bisect-to-find-bad-commit)
 
# Tools
1. Show Git for windows
2. Show POSH git in Powershell and download site
3. Show TortoiseGit Windows Explorer integration
 
# Config
You can set git config either globally by using the ```--global``` flag or local to the current repo only by using the ```--local``` flag.
## Setting user name and email
1. Run the following command:
   ```bash
    git config --global user.name "Your Name"
    git config --global user.email "name@example.com"   
   ```
 ## Listing command
 1. Run following. This will show the configuration and where the confguration is specified.
    ```bash
    git config --list --show-origin
    ```
2. To list only either global or local config, run ```git config --global --list``` or ```git config  --local --list```.
       
## Alias
Alias enable you to create shorthand to common commands you use to save typing time.
1. Run the following Powershell script to initialize a repo and commit a file.
   ```powershell
   git init
   Add-Content -Path .\file1.txt -Value 'line1'
   git add file1.txt
   git commit -m "add line 1"
   ```
2. Alias can be created for global use or local to your current repo only by using **--global** or **--local** flag.
3. In this example we will create an alias "co" as a shorthand to the "checkout" command and only be effective for the current repo. Run ```git config --local alias.co checkout```.
4. Run ```git config --local --list``` to check that the alias is created.
5. Run ```git co -b f1``` to create and switch to a new branch named **f1**. Run ```git branch``` to ascertain that you are in the **f1** branch.
6. In git, most of the time we will be working in branch and often need to switch back to the main or master branch. The command to switch to master branch is ```git checkout master``` which is quite some typing.
7. Let's create an alias to make this easier. Run ```git config --local alias.m 'checkout master'```.
   ```text
   Note: If your master branch is named 'main', change the 'checkout master' to 'checkout main'.
   ```
8. Now run ```git m``` to switch to master branch, and run ```git branch``` to ascertain that you are in the **f1** branch.

## Setting Editor
1. When you make a commit using `git -m "some messages"`, you can only conveniently enter a simple one liner message. If you need to enter multi-line commit message, you need to use command `git commit` instead. This will open up a editor.
2. You can change what the default editor git will use when it need to open up a new editor.
3. To change the default editor globally, run `git config --global core.editor "Path to editor"`. Example: `git config --global core.editor "C:\\<path>\\notepad2.exe"`. In this example, I am using notepad2 on my Windows laptop.
4. Now if you run `git commit`, notepad2.exe will open. Git will wait until you save and close notepad2.exe.

# Move/Rename and delete file
## Move/Rename file
1. Run the following Powershell script to initialize a git repo and commit two files into repo.
   ```powershell
    git init

    Add-Content -Path .\file1.txt -Value 'This is file 1'
    git add file1.txt
    git commit -m "add file 1"

    Add-Content -Path .\file2.txt -Value 'This is file 2'
    git add file2.txt
    git commit -m "add file 2"
   ```
2. To rename or move a file, run the following git command
   ```powershell
   git mv file1.txt newname.txt
   ```    
3. The above command will rename the file and stage the file for commit (aka update index). Run ```git status``` to check the status of the index.
   ```text
   Note: You can also use the regular shell command to rename a file, but you need to stage the change manually.
   ```
4. Run ```git commit -m "rename file1"``` to commit the change.  
5. Run ```git log --oneline -- newname.txt```. You only see one commit listed, and is the one that rename the file. This is because git only look at the commit which involved *newname.txt*.
6. It is very useful to trace back the entire history of the file, even back to the previous file name before it is renamed. To do this, run ```git log --follow --oneline -- newname.txt```. Now, you can see all the commits which involve *newname.txt* (or its old name).
## Delete file
1. This demo continue from above. Make sure you perform the* Move/Rename file* demo first.
2. To delete a file, run the following git command
   ```powershell
   git rm file2.txt
   ```    
3. The above command will delete the file and stage the file for commit (aka update index). Run ```git status``` to check the status of the index.
   ```text
   Note: You can also use the regular shell command to delete a file, but you need to stage the change manually.
   ```
4. Run ```git commit -m "delete file2"``` to commit the change. 

## Delete a file from repo but keep it in working directory.
1. Run the following Powershell script to initialize a repo and add *file3.txt* to the repo
   ```powershell
   git init

   Add-Content -Path .\file1.txt -Value 'This is file 1'
   git add file1.txt
   git commit -m "add file 1"

   Add-Content -Path .\file3.txt -Value 'This is file 3'
   git add file3.txt
   git commit -m "add file 3"  
   ```
2. Run ```git rm --cached file3.txt``` to delete file3.txt.
3. Run ```git status``` and you will see that *file3.txt* is deleted and is also untracked.
4. Run ```git commit -m "delete file3"``` to commit the change. 
5. Run ```git status``` again and you will see that *file3.txt* is untracked.
6. If you run ```git log --name-status --oneline```, you will see that *file3.txt* is deleted from the repo.


# Squash
## Squash multiple commits into one during merge
1. Run **git init** to initialize a git repo.
2. Create a new file *file1.txt*.
3. Add a new line "line 1" and commit it with message "add line 1"
4. Create a new branch call *f1*, run **git checkout -b f1**
5. Add 3 lines and commit each line separately.
```Powershell
Add-Content -Path .\file1.txt -Value 'line 2'
git add file1.txt
git commit -m "add line 2"

Add-Content -Path .\file1.txt -Value 'line 3'
git add file1.txt
git commit -m "add line 3"

Add-Content -Path .\file1.txt -Value 'line 4'
git add file1.txt
git commit -m "add line 4"
```
6. Run **git log** and you should see 4 commits.
7. Swith back to *master* branch, run **git checkout master**.
8. Now we want to merge *f1* branch into *master*. Run **git log master..f1** and you should see that there are 3 commits to be merge.
9. In this case, we don't really care about the interim history in the *f1* branch. What we want is when we merge, the 3 commits will be squash into one single commit. 
10. Run **git merge --squash f1** to take the 3 commits and merge as one commit.
11. Run **git commit -m "merge from f1"** to commit the merge.
12. Run **git log** to verify that there is only one commit.


## Squash multiple commits into one within the same branch
1. Run **git init** to initialize a git repo.
2. Create a new file *file1.txt*.
3. Add 4 lines and commit each line separately.
```Powershell
Add-Content -Path .\file1.txt -Value 'line 1'
git add file1.txt
git commit -m "add line 1"

Add-Content -Path .\file1.txt -Value 'line 2'
git add file1.txt
git commit -m "add line 2"

Add-Content -Path .\file1.txt -Value 'line 3'
git add file1.txt
git commit -m "add line 3"

Add-Content -Path .\file1.txt -Value 'line 4'
git add file1.txt
git commit -m "add line 4"
```
4. Run **git log** and you should see 4 commits.
   
   ![commits](img/squash-01.png)
5. Now we want to combine the last 3 commit ('add line 2' to 'add line 4') into a single commit.
6.  Run **git rebase -i HEAD~3**. This tell git we want to interactively rebase the last 3 commit from the HEAD. This will open in editor.
   * If you have Visual Studio Code, this may open a rebase/squash editor in VS Code.
   ![squash editor](img/squash-editor.png)
7. Keep the line for 'add line 2' as it is. Change the 'pick' to 'squash' for 'add line 3' and 'add line 4'.
    ![squash commit](img/squash-editor-02.png)
8. Save and close the editor.
9. Another editor will prompt up for you to enter a commit message. Enter a commit message, save and close the editor.
10. Run **git log** and you should only see 2 commits now.
    ![After squash](img/squash-postsquash.png)



# Bisect
## Using Bisect to find bad commit
There is times when you commit a bad code without realizing it until ways later. You will have to backtrack each commit to find out which commit introduce the bad code. Git Bisect give you a much better way to do this. Bisect use a binary tree approach to divide and conquer and lead you to the bad commit much faster. It divide the commits log into two and ask you whether the current commit is good or bad, and then eliminate the right half of the tree and continue the process until you identify the bad commit.
1. Run the following Powershell script to setup the backdrop. It initialize a git repo and make ten commits to file1.txt. The third commit is where we introduce the bad code.
   ```powershell
    git init
    1..10 | ? {
      $n = $_
      
      if($n -eq 3)
      {
        Add-Content -Path .\file1.txt -Value "line $n - buggy"
      }
      else
      {
        Add-Content -Path .\file1.txt -Value "line $n"
      }
      git add file1.txt
      git commit -m "add line $n"
    }   
   ```
2. For simplicity, assuming that we know the first commit was a good one and the latest commit contain bad code. Our mission is to find out which commit introduce the bad code. Run ```git log --oneline``` to get the commit history. Take note the commit ID of "add line 1" (first commit) and "add line 10" (contain bad commit).
3. Run ```git bisect start``` to start the bisect interactive command. It will help you through the elimination process to find the source of the bad commit until you run ```git bisect reset```. After you run ```git bisect start```, it will return the control back to the command prompt.
4. We first specify the good commit. Run ```git bisect good <commit ID of add line 1>```.
5. Then we specify the bad commit. The bad commit is the one we know contain the bad code, but not necessary the one that introduce it. Run ```git bisect bad <commit ID of add line 10>```.
6. Git tell you how many more commits to check and roughly how many iteration it still need to complete this. Since there is ten commits between the good and bad, it tell us there are four more to check in two steps (remember it divide the list of commits to two). You should see that you are now on the fifth commit (add line 5). Git automatically checkout this commit for us. This commit is halfway between the good and latest bad commit.
7. Run ```cat file1.txt```. You should see that it contain five lines including the buggy code. These represent the first five commits in the log.
8. Since this commit also contain the bad code, we will tell bisect to take the current five commits and divide it into half again. Run ```git bisect bad```. You should see a similar message as in step-6. You should now be on commit "add line 3".
   ```text
   Note: If the current commit does not contain the bad code, you run "git bisect good". Git will take the other half of the commit logs.
   ```
9.  Run ```cat file1.txt```. You should see that it contain three lines including the buggy code. These represent the first three commits in the log.
10. It still contain the buggy code and we are still unsure whether the current commit is the source of the bug. 
11. Run ```git bisect bad``` again. Run ```cat file1.txt``` to view the content of file1.txt. This time it does not contain the buggy code.
12. Since we now know the current commit is a good one, run ```git bisect good``` to tell git this commit does not contain the buggy code. Once you do this, git tell you the commit that introduce the bad code (add line 3).
13. You can now do the neccessary code fixing or take the neccessary action.
14. When you are done, run ```git bisect reset``` to bring you back to the HEAD.

```text
Exercise: Run the following code to create a new repo. But this time the buggy code is much closer
 to the latest commit. Use Bisect to identify the commit that introduce the error ("add line 9").

  git init
  1..10 | ? {
    $n = $_
    
    if($n -eq 9)
    {
      Add-Content -Path .\file1.txt -Value "line $n - buggy"
    }
    else
    {
      Add-Content -Path .\file1.txt -Value "line $n"
    }
    git add file1.txt
    git commit -m "add line $n"
  }
```