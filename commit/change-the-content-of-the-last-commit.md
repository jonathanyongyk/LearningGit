# Change the last commit message
After you commit, you realize that you forgot to include some files in the commit. You can amend the commit to include these additional files that you forgot earlier.
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
