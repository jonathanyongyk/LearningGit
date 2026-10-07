# Change the content of the last commit
After you commit, you realize that you forgot to include some files in the commit, or you make some mistakes in the committed files. You can amend the commit to include these additional files that you forgot earlier.

## Demo steps
Run these in a terminal from a parent working folder.

1. Create and enter demo folder.
```powershell
mkdir demo
cd demo
```
2. Initialize repository.
```powershell
git init
```

3. Add initial app file and commit.
```powershell
echo "console.log('App started');" > app.js
git add app.js
git commit -m "Initial commit - add app.js"
```

4. Add sensitive file and another feature file, then commit only the secret file. We intentionally leave out the other file for now.
```powershell
echo "API_KEY=super_secret_key_12345" > secrets.txt
echo "console.log('feature 2 added');" > feature2.js
git add secrets.txt
git commit -m "Add secrets and feature2 file"
```

Now we realize we make two errors:
- We forgot to include the *feature2.js* file in the last commit.
- We accidentally committed the *secrets.txt* file which contains sensitive information.

We need to correct this before we push the commit to the remote repository.

5. Open *secrets.txt* and remove the sensitive information. The *secrets.txt* should look like the following
```
API_KEY="<PLACEHOLDER>"
```

6. Stage secrets.txt for commit
```powershell
git add secrets.txt
```

7. Stage the *feature2\.js* for commit
```powershell
git add feature2.js
```

8. Run **git commit --amend**. This will open the last commit message in a editor. You can see the original commit message.
9. Add "missing some stuff" after the original commit message. save and close the edit.
10. Run **git log**. You can see the commit message has been updated and *feature2\.js* is added to the commit.
11. You can also update the content, commit it without a new commit message. To do so, run ****git commit --amend --no-edit**.
12. Run **git log**. You should not see any change in the commit log. But if you run **git diff HEAD..\<previous_commit_id\>**, you can see the change in the content.
