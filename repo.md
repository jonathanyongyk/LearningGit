- [Repo](#repo)
  - [Start repo from Azure DevOps, then clone to client.](#start-repo-from-azure-devops-then-clone-to-client)
  - [Start repo from server, init repo from client, link to remote](#start-repo-from-server-init-repo-from-client-link-to-remote)
  - [Show VS Code](#show-vs-code)
  - [Shallow clone](#shallow-clone)
  - [Shallow branch - clone specific branch only](#shallow-branch---clone-specific-branch-only)
  - [Showing repo size.](#showing-repo-size)
  - [Migrate from one repo to another.](#migrate-from-one-repo-to-another)
  - [Mirror repo](#mirror-repo)
  - [Clone using PAT](#clone-using-pat)
  - [Update remote link](#update-remote-link)
# Repo
## Start repo from Azure DevOps, then clone to client.
1. Create a repo in Azure DevOps, add a README and Visual Studio .gitignore file.
2. Clone to client, by running **git clone \<url to git repo\>**
3. Add a new file *file1.txt* at client side
4. Run **git status** to show the untrack new file.
5. Run **git add \*** and **git status** again to show the file in tracking state.
6. Run **git commit -m "\<some comment\>"** to commit change
7. Run **git push** to push to server
8. Go to Azure DevOps server repo, and refresh the page, you can see the new file.
9. Add a new file *file2.txt* in Azure DevOps server repo.
10. On client side, run **git pull** to download the new commit.
11. Run **git remote –v** to show how local repo is map to a Azure DevOps repo.

## Start repo from server, init repo from client, link to remote
1. Create a repo in Azure DevOps
2. Create a repo in client by running **git init**.
3. Add a new file *file1.txt* to the local repo.
4. Run **git add \*** and **git commit -m "\<some comment\>"** to commit change
5. Run **git remote –v** to show there is currently no remote repo connected.
6. Run **git remote add origin https://\<url to azure devops repo\>**
7. Run **git remote –v** again to show there is now a remote repo.
8. Run **git pull origin master --allow-unrelated-histories** to merge remote repo into local repo.
   * Need to do this because both local and remote repo contain unrelated commit history that need to be merge.
9. Run **git push --set-upstream origin master** to push local commit to server. Now both local and server should be in sync. You can refresh the repo in Azure DevOps to verify this.
10. Add a new line to *file1.txt* and commit it.
11. Run **git push** to push the new commit to server. You can verify this in the server repo.
 
> Note: If your local and remote branch has different name, use the convention \<local branch name\>:\<remote branch name\>

## Show VS Code
1. Open the local repo folder in VS Code.
2. Add another file *file3.txt*.
3. Using the Source Control extension, commit and push the file to Azure DevOps
4. Open a Repo in both VS and VS Code to show how the status is synced between both the tools and Tortoise Git also.
   * Add a new file and show the status in 3 places.
   * Create a new branch and switch to it, show the status in 3 places.



## Shallow clone
If you have a large repo, it may take a lot of space in local disk. When you clone the repo, you may just want parts of the recent history only to save space.
1. Create a server side repo, name it *largerepo*.
2. In the local folder, clone from *largerepo*.
3. Run following Powershell scripts to create a file and do a few commits.
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

Add-Content -Path .\file1.txt -Value 'line4'
git add file1.txt
git commit -m "add line 4"

Add-Content -Path .\file1.txt -Value 'line5'
git add file1.txt
git commit -m "add line 5"
```
4. Run **git log** to show how many commits it has. It should have 6 commits.
5. Run **git push** to push the changes to server.
6. Create another local folder.
7. In that new local folder, we will clone from *largerepo* but will only take the latest 2 commits as history. Run **git clone --depth 2 \<url to largerepo\>** .
8. Run **git log** now and you will see that the repo only has the latest 2 history.


## Shallow branch - clone specific branch only
1. Before start, complete step 1 to 6 in [Shallow clone](#shallow-clone) demo.
2. In the server repo of *largerepo*, create two branch *b1* and *b2* from *master*.
3. In local folder, clone from *largerepo*.
4. Run **git branch --all** and see that you have all the branches from the server repo.
5. We will do another clone, but we just want to bring down the **master** branch only.
6. Create and go to another new local folder.
7. In the new local folder, we will clone from *largerepo* but will only take the *master* branch. Run **git clone \<url to largerepo\>  --branch master --single-branch**.
8. Run **git branch --all** now and you will see that you only have the *master* branch.

## Showing repo size.
1. Initialize a local git repo
2. Run following Powershell scripts to create a file and do a few commits.
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

Add-Content -Path .\file1.txt -Value 'line4'
git add file1.txt
git commit -m "add line 4"
  ```
3. Also add a large file(file larger than 1MB) and commit it to repo.
4. Run **git count-objects -vH** to show the size of the repo. You can read what the output mean in the following links. Make sure you can explain what is pack and loose file.
   * https://git-scm.com/docs/git-count-objects
   * http://alblue.bandlem.com/2011/09/git-tip-of-week-objects-and-packfiles.html
 
 
## Migrate from one repo to another.
1. Create a server side repo, name it *migratedemo*.
2. Create another server side repo, name it *newrepo*. When create the repo, make sure don't add any file including the readme and .gitignore.
3. In the local folder, clone from *migratedemo*.
4. Run following Powershell scripts to create a file and do a few commits and create a new branch. It will then push to the server.
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

Add-Content -Path .\file1.txt -Value 'line4'
git add file1.txt
git commit -m "add line 4"

git push

git checkout -b feature1

Add-Content -Path .\file2.txt -Value 'file2-L1'
git add file2.txt
git commit -m "add file2 Line 1"

git push -u feature1:feature1
git checkout main

  ```
4. Run **git log** to show how many commits it has. It should have 6 commits and 2 branches.
5. **git push** is not really needed. We just act as if we are pulling existing code from server.
6. We will now push this repo to the *newrepo*. Run **git push --all \<clone url of newrepo\>**.
7. Now you can see the *newrepo* has exact same copy of the *migraterepo* including the history and branches.

## Mirror repo
This demo show how you can push a git repo to another repo. This is similar to the demo **Migrate from one repo to another**. This is useful if you have a repo that need to be mirror in another location and updated continuously.
1. Create a server side repo, name it *mainrepo*.
2. Create another server side repo, name it *mirrorrepo*. When create the repo, make sure don't add any file including the readme and .gitignore.
3. In the local folder, clone from *mainrepo*.
4. Run following Powershell scripts to create a file and do a few commits and create a new branch. It will then push to the server.
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

Add-Content -Path .\file1.txt -Value 'line4'
git add file1.txt
git commit -m "add line 4"

git push

  ```
4. **git push** is not really needed. We just act as if we are pushing to the server as in real scenario.
5. We will now setup another upstream link to our mirror repo. Run the following command: **git remote add mirror \<clone url of mirrorrepo\>**.
6. Run **git remote**. You should see there is a new upstream link named *mirror*.
7. We will now push from **mainrepo** to the **mirrorrepo**. Run **git push mirror**.
8. Now you can see the **mirrorrepo** has exact same copy of the **mainrepo**.
9. Now let's make some changes in **mainrepo**. Run the following powershell script to create 2 new commits and push it to the **mainrepo**. 
   ```Powershell
   Add-Content -Path .\file1.txt -Value 'line5'
   git add file1.txt
   git commit -m "add line 5"

   Add-Content -Path .\file2.txt -Value 'File2 - line1'
   git add file2.txt
   git commit -m "add file 2 line 1"

   git push
   ```
10. We will now push from **mainrepo** to the **mirrorrepo** again to update the code. Run **git push mirror**.
    ```text
    Note: If you want to push to a branch in the mirrored repo, you need to create a branch on the mirrored repo first, then run git push mirror main:mirrored-branchname.
    ```
11. Now you can see the **mirrorrepo** has exact same copy of the **mainrepo**.

## Clone using PAT
```
$pat = "<your PAT>"
git clone https://token:$pat@<repo clone url>
```
Note: The user name ('token' in this example) is insignificant. it can be of any value.


## Update remote link
You may need to update the remote link of a repo. This is useful if you have a repo that need to be moved to another location. Or the PAT you used to clone the repo has expired.
Run the following command to remove the current remote link and add a new remote link.
```Powershell
$newPAT = "<PAT>"
$newOrigin = "https://token:$newPAT@github.com/owner/repo.git"
git remote remove origin
git remote add origin $newOrigin
```