
- [Git tag](#git-tag)
  - [Create tag from server side.](#create-tag-from-server-side)
  - [Create tag from local and push to server.](#create-tag-from-local-and-push-to-server)

# Git tag
## Create tag from server side.
1. Create a server side repo, name it *tagdemo*.
2. In the local folder, clone from *tagdemo*.
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
4. Run **git push** to push it to server.
5. On the server side, Under **Repos | Files**, click on *file1.txt*, and go to the **History** tab.
6. To the right of "add line 3" commit, click on the triple dot and select **Create Tag**.
7. Give the tag name '*release1*' and enter a description, click **Create**.
8. If you go to **Repos | Tags**, you can see the tag listed there.
9.  Go to the local folder, and **run git tag -l** and you see no tag yet.
10. Run **git pull** to update the local repo.
11. run **git tag -l** again and you see the tag.
12. We can check the contents of the tag by running **git checkout release1**.
13. Run **git log** and show the content of *file1.txt*.
14. You can also create a new branch from this tag. This is useful for scenario which you wwant to make a code change or bug fix baed on specific commit or "release". Run **git switch -c r1patch1**.
15. Open *file1.txt* and change "line2" to "line2a".
16. Commit the change.
17. Push the commit to server. Since this branchh does not exist on server yet, we need to run **git push -u origin r1patch1:r1patch1**.
18. Go to the server repo and chech that you have the new branch with the expected commit. You can now proceed to do Pull Request to merge in a normal workflow, but we will not do that for this demo.
  
## Create tag from local and push to server.
Complete the demo [Create tag from server side](#create-tag-from-server-side) to setup the backdrop. If not at least perform step 1 to 4.
1. In the local repo, run **git log** to find the commit hash for "add line 4" .
2. We will create a tag based on the commit for "add line 4". Run **git tag -a alpha1 \<hash for add line 4\> -m "This is alpha test"**.
3. Run **git tag -l** to make sure the tag is created.
4. Run **git push origin alpha1** to push the tag to server. (syntax: **git push \<repo_name\> \<tagname\>**)
5. Go to server repo and you should see the tag you just pushed.
6. Side note: You can also push all tags by **git push --tags \<repo-name\>**. But this is uncommon because it is unlikely you will create a bunch of random tags and push it to server.
