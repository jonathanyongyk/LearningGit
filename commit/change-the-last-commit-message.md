# Change the last commit message
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