# Git Filter-Repo Caveats and Watch Outs

This note highlights common pitfalls when rewriting history with `git filter-repo` and how to avoid them.

## 1. History Is Rewritten (Commit SHAs Change)
- Every rewritten commit gets a new SHA.
- Open pull requests, commit links, and automation that depends on old SHAs may break.
- Plan a migration window and communicate the cutover time.

## 2. Force Push Is Required
- After rewrite, a normal push is rejected.
- You must use force push (for example: `git push origin --force-all --prune`).
- If branch protection blocks force push, temporarily adjust protection settings.

## 3. Team Must Re-Sync Correctly
- Teammates should usually reclone after a major rewrite.
- If they cannot reclone, they must hard reset to the new remote history.
- If they keep old local history, they can accidentally reintroduce removed content.

## 4. Backups Are Mandatory
- Create a backup branch, mirror clone, or repository snapshot before rewriting.
- Keep backup until you verify all checks and remote push are complete.
- Do not delete backup immediately.

## 5. Reflog and Garbage Collection Matter
- Rewritten objects can still exist locally through reflog and loose objects.
- Run cleanup if your goal is complete local removal:
  - `git reflog expire --expire=now --all`
  - `git gc --aggressive --prune=now`
- Without cleanup, sensitive data may still be recoverable locally.

## 6. Secrets Are Still Considered Exposed
- Even after rewrite, treat leaked credentials as compromised.
- Rotate keys, tokens, passwords, and certificates.
- Update incident and audit records if required by policy.

## 7. Tags and Non-Default Branches
- Ensure all branches and tags are rewritten and pushed.
- Verify hidden refs are not carrying old objects.
- Check both local and remote refs before declaring success.

## 8. Forks and External Clones Keep Old History
- Rewriting your main remote does not rewrite users' forks or old clones.
- Old history may continue to exist outside your control.
- Coordinate with fork owners if full ecosystem cleanup is required.

## 9. Large Repositories and Runtime
- Rewrites can take time and heavy I/O on large repos.
- Run in a clean environment and avoid interrupting the process.
- Prefer running from a fresh clone for predictable results.

## 10. LFS Objects Need Separate Handling
- If sensitive content is in Git LFS, handle LFS cleanup explicitly.
- History rewrite alone may not remove all LFS objects from storage.
- Validate LFS object state on both source and destination remotes.

## 11. Validate Before and After
- Before rewrite: identify where the target file appears.
- After rewrite: verify target file no longer appears in history.
- After push: validate on remote and in a fresh clone.

## 12. Useful Verification Commands
- `git log --all -- path/to/file`
- `git rev-list --objects --all | findstr /I "filename"`
- `git fsck --full`

## Recommended Safe Sequence
1. Announce freeze window to team.
2. Take backup/snapshot.
3. Rewrite with `git filter-repo`.
4. Verify file/object removal locally.
5. Expire reflog and run garbage collection.
6. Force push rewritten history to origin.
7. Re-verify from a fresh clone.
8. Instruct team to reclone or hard reset.
9. Rotate any exposed secrets.

## Quick Decision Rule
- If data is truly sensitive, do both:
  - rewrite history
  - rotate credentials immediately

History rewrite reduces exposure in Git history, but credential rotation addresses the security risk itself.
