
- [Git tag](#git-tag)
  - [Create tag from server side.](#create-tag-from-server-side)
  - [Viewing the contents of a tag and create a branch from the tag](#viewing-the-contents-of-a-tag-and-create-a-branch-from-the-tag)
  - [Create tag from local and push to server.](#create-tag-from-local-and-push-to-server)
  - [Delete tag from local repo and push to server.](#delete-tag-from-local-repo-and-push-to-server)
  - [Listing tag](#listing-tag)

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
9.  Go to the local folder, and **run git tag -l** to list all the tags and you see no tag yet.
10. Run **git pull** to update the local repo.
11. run **git tag -l** again and you see the tag.
```text
Note: Step 5 to 8 assume you are using Azure DevOps. If you are using GitHub, you can also create and view tag using the GitHub web or desktop.
```
    
## Viewing the contents of a tag and create a branch from the tag
Complete the demo [Create tag from server side](#create-tag-from-server-side) to setup the backdrop.
1. We can check the contents of the tag by running **git checkout release1**.
2. Run **git log** and show the content of *file1.txt*. In *file1.txt*, you should only see up to 'line3'.
3. You can also create a new branch from this tag. This is useful for scenario which you want to make a code change or bug fix baed on specific commit or "release". Run **git switch -c r1patch1**.
4. Open *file1.txt* and change "line2" to "line2a".
5. Commit the change.
6. Push the commit to server. Since this branchh does not exist on server yet, we need to run **git push -u origin r1patch1:r1patch1**.
7. Go to the server repo and check that you have the new branch with the expected commit. You can now proceed to do Pull Request to merge in a normal workflow, but we will not do that for this demo.
  
## Create tag from local and push to server.
Complete the demo [Create tag from server side](#create-tag-from-server-side) to setup the backdrop. If not at least perform step 1 to 4.
1. In the local repo, make sure you are in the main/master branch.
2. Run **git log --oneline** to find the commit hash for "add line 4" .
3. We will create a tag based on the commit for "add line 4". Run **git tag -a alpha1 \<hash for add line 4\> -m "This is alpha test"**.
4. Run **git tag -l** to make sure the tag is created.
5. Run **git push origin alpha1** to push the tag to server. (syntax: **git push \<repo_name\> \<tagname\>**)
6. Go to server repo and you should see the tag you just pushed.
```text
Side note: You can also push all tags by **git push --tags \<repo-name\>**. But this is uncommon because it is unlikely you will create a bunch of random tags and push it to server.
```
```text
Note 2: If you want to create a tag from the latest commit, omit the commit hash in 'git tag', run 'git tag -a <tag_name> -m "<message>"'.
```

## Delete tag from local repo and push to server.
Complete the demo [Create tag from local and push to server.](#create-tag-from-local-and-push-to-server) to setup the backdrop. 
1. We will now delete the tag *alpha1* we just created and also delete it at the server repo.
2. To delete a tag, run `git tag -d alpha1`. (syntax: **git tag -d \<tagname\>**)
3. Run `git tag -l` and *alpha1* should be gone.
4. Now let's delete the tag at the server repo by pushing this to the server repo.
5. Run `git push origin --delete alpha1`. (syntax: **git push origin --delete  \<tagname\>**)
6. Go to the server repo and refresh the tag page. *alpha1* should not be visible anymore.

## Listing tag
When you list a tags with `git tag`, you can search for tag that match some pattern.
1. Run the following script to setup a demo repo. (This script run in Bash and Powershell)
   ```bash
    git init
    echo "line1" >> file1.txt
    git add *
    git commit -m "add file"
   ```
2. Run the following to create a five tags. (The tags are all created on the same commit, but is good enough for our demo) 
   ```bash
    git tag -a v1.0.0 -m "v100"
    git tag -a v1.0.1 -m "v101"
    git tag -a v1.0.2 -m "v102"
    git tag -a v1.2.0 -m "v120"
    git tag -a v1.2.1 -m "v121"
    git tag -a v2.0.0 -m "v200"
    git tag -a v2.0.1 -m "v201"    
   ```
3. Let start with basic. To list all the tags, run `git tag` or `git tag -l`.
4. To list tags that are major and minor version 1.0, run `git tag -l "v1.0.*"`.
5. To list tags that are the first major version, run `git tag -l "v*.0.0"`.
6. To list tags that are the patch version 1, run `git tag -l "v*.*.1"`. (Note: There could be no reason why you would want to list this way, it is just a illustration of what is possible)