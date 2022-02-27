# GIT

git: is source control technology

- versioning
- history
- backup-archive
- isolation of changes


### Resources:
- https://www.atlassian.com/git/tutorials
- https://git-scm.com/docs


### Source control:
    Centralized
    Decentralized or Distributed

### Git Repository
    .git
        Working directory | Staging Area | Repository .Git  
                            .txt                .txt


**First:**

**.gitignore:** file = in order to ignore files which you want
`git version` 

**to git repo**:
git remote add origin URL
git push 
git pull


**Commits:**
Master Branch | Different Branch
    C1, C2          C1, C2, C3, C4

To **start**: `git init`

for tracking add: `git add "filename" or git add .` 
`git commit -m "your messages"` (dont forget to add what are your messages)

**Who are you?**
`git config --global user.email "abckg@x.com"`

`git config --global user.name "abckg"`

# Fundamental
`git log ` (show your history)

`git checkout "unique commit code" ` (go this commit)

`git checkout master (go master branch)`
`git branch (show your branch -a -r )`

` git branch "test" ` (create test branch)
` git checkout test `(switch to test branch)
` git checkout -b poc ` (create and switch to poc)


**Merge**:
merge master and test -> `git merge test`

`last commit called "**HEAD**"`
`git switch poc (it is like checkout = switch )`
`git switch -c poc2`

`git merge <new-branch-name>`
`git merge <new-branch-name> --allow-unrelated-histories` (if it occurs error "Fatal: refusing to merge unrelated histories")
change branch name:` git branch -m oldname newname`

**undoing unstaged**:
`git rm "filename"` (remove files on branch)
`git restore "filename"` (clean)
`git clean -dn` (untrack)
`git clean -df` (delete)
`git checkout "filename"` (undoing)

**undoing staged:**
`git reset "filename" ()`
`git checkout "filename`
or
`git restore --staged "filename"`
`git checkout "filename"`

**deleting commits:**

`git reset --soft HEAD~1` (reset 1 commit)
`git commit -m "deleted"`
`git reset HEAD~1` (reset 1 commit, default)
`git reset --hard HEAD~1` (reset 1 commit, default)

**deleting Branch**
`git branch -D "branchname"`


# Summary:

- git --version
- git init
- git status
- git log
- git ls-files
- git add filename
- git commit -m "message"
- git checkout commitid
- git branch branchname or git switch branchname
- git checkout branchname
- git checkout -b branchname or git switch -c branchname
- git merge otherbranch
- git rm filename
- git checkout (--) . git restore filename .
- git clean -df (delete untrack)
- git reset filename and git checkout -- filename / git restore --staged filename
- git reset HEAD~1
- git reset --soft HEAD~1
- git reset --hard HEAD~1
- git branch -D branchname (delete branch)
- git update
- git update 

- git stash
- git stash apply 1

- git stash list
- git stash clear

**Bringing lost data backup**
`git reset --hard HEAD~1 `(last commit no longer it's common way)
`git reflog `
`git reset --hard ce6106a` (go to ce6106a you can add ce6106a) 
`git checkout -b feature`

#Merge types:
    Fast forward
    Non fast forward   
        recursive
        octopus
        ours
        subtree
master m1 m2      ? (fast forward merge)
feature m1 m2 f1 f2 

- git switch feature
- git switch master
- git merge feature (you can see fast-forward)
- git merge --squash feature (without commit, merged to main (feature). Add commit later!)

**recursive**:

`git merge --no-ff feature` (recursive)

**rebase**:
    be careful doing at cloud
    git rebase master

**Merge vs Rebase vs Cherry-pick**
|  merge non-fast forward--->create merge commit--->new commit |
| rebase---->change single parent--->new commit ID |
| cherry pic---->add spesific commit to head---->copies commit with New ID |


git cherry-pick "hash" (master'a yeni id ile o değişikliği eklemiş oluyor.)
git tag 1.0
git tag -d 1.0

**Summary:**
- git stash: temporary storage for unstaged and uncommitted changes
- git reflog: log of all project changes including deleted commits
- git merge: commit and fast forward merge
- git rebase: change the base of commits in another branch
- git cherry-pick: coppy commit including the changes made only in this commit as Head to other Branch

https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging#_advanced_merging
https://git-scm.com/docs/merge-strategies


**Remote tracking:**
    remote file-> git branch -a or git branch -r
    git push origin <newbrunch> (added new brunch and push but you can see remote tracking file with git branch -r or -a )

If you add branch on repo(cloud) you cant see on your local, so
-     git ls-remote (see files)
-     git fetch origin  (for fetching)
-     then -a (all branches)
-     git pull origin (pulling)


remote branch---->"git fetch" remote tracking ------> git merge local tracking then "git push" update repo.

**Local tracking:**
    git branch --track rometebranchnamered origin/remotebranchname 
    git switch remotebranchname
    git push (eğer sadece lokali değiştirirsen böyle yazman yeterli bu değişiklik localden remote'e yazıylıyor.)
    git brunch -vv önemli: git branch -r önemli

### Summary of commands:
	
git remote (show remote servers )
git branch -a (all branch)
git branch -r (remote)
git remote show origin (detailed)
git branch -vv (local tracking)
git branch --track branchname origin/branchname

git clone:
git clone "link"
then git init
git branch --track origin/feature remotes/origin feature

git push -u origin feature-upstream (short cut to connect with track remote)
git branch --delete --remotes origin/feature (remote branch delete) -> git push origin --delete feature

# Summary:
                 git                             github
    Repository   local                           remote                     git remote add origin URL
    Branch       Local tracking                  remote                     git branch --track branchname origin/branchname
                 Remote Tracking                                            git pull/push origin Branch
Github: you can clone public repositories from github but you need personel token to pull. Contrubitor or collabrator

part of organizations:
    *role
     You can add external people to your project with ROLES and IAM. (def:read)

teams -> IAM You can divide teams that you can manage different branches and projects.

clone: Remote to local
	
fork: fork is using for contribitor.fork and pull request
	
copy with fork (github feature) after development, you can pull request to owner.
	
issues: in a nutshell ticket
	
project: management to project with kanban, agile strategies
	


