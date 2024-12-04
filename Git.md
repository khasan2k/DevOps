# GIT (GLOBAL INFORMATION TRACKER)
![Screenshot of a comment on a GitHub issue showing an image, added in the Markdown, of an Octocat smiling and raising a tentacle.](https://private-user-images.githubusercontent.com/100408810/392234080-40092a9a-3207-49f1-ad2a-d74e3241a1e3.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyOTMyMTEsIm5iZiI6MTczMzI5MjkxMSwicGF0aCI6Ii8xMDA0MDg4MTAvMzkyMjM0MDgwLTQwMDkyYTlhLTMyMDctNDlmMS1hZDJhLWQ3NGUzMjQxYTFlMy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjA0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwNFQwNjE1MTFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03ZjAzM2ZkMWIwMjY4OTBhMmE5OGFhMTYyMDM1MmY0NDhkNjZjY2MzMzgxYTc2NGM0ZGZjNTYxOTRjZjBlY2U0JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.pgmMM1zxW3PPr5JU-UZ9I0YHP4oxqjryrxYwT8UqKZ8)   
GIT Workflow Diagram

**GIT:** GLOBAL INFORMATION TRACKER  
**VCS:** VERSION CONTROL SYSTEM

#### It will keep the code separately for each version.

v-1	: 100 lines --- > store (repo-1)   
v-2	: 200 lines --- > store (repo-2)   
v-3	: 300 lines --- > store (repo-3)   

**REPO:** It is a folder where we store our code.   
**index.html:** it is a basic file for every application

v1 --- > index.html   
v2 --- > index.html   
v3 --- > index.html   

### INTRO:
- Git is used to track the files.   
- It will maintain multiple versions of the same file.   
- It is platform-independent.   
- It is free and open-source.   
- They can handle larger projects efficiently.   
- It is 3rd generation of vcs.   
- It is written on c programming   
- It came on the year 2005   

**CVCS:** CENTRALIZED VERSION CONTROL SYSTEM   
**SVN:** It can store code on a single repo.   

**DVCS:** DISTRIBUTED VERSION CONTROL SYSTEM   
**EX: GIT:** it can store code on Multiple repos.   

**ROLLBACK:** Going back to the previous version of the application.   

### STAGES:   
**WORKING DIRECTORY:** Where we write our source code.   
**STAGING AREA:** We track files here.   
**REPOSITORY:** Where we store tracked source code   

### WORKING WITH GIT:   
#### Create an ec2 server:   
```
mkdir swiggy
cd swiggy
yum install git -y //[yum=pkg manager, install=action, git=pkg name -y=yes]
```
**To check version 		:** `git -v`   
**To install. git (local repo) 	:** `git init`   
**To create file			:** `vim index.html` (content is opt)   
**To check status			:** `git status`   
**To track file			:** `git add index.html`   
**To check status			:** `git status`   
**To store file			:** `git commit -m "commit-1" index.html`   

### Create a file -- > add -- > commit    

**To show commits		:** `git log`   
**To show last 2 commits	:** `git log -2`   
**To show commits in single line:** `git log -2 –oneline`  
**Show you the difference between your committed file and the current staged file.				:** `git diff`  

=================================================   
### Use config to add your name and email:   
> $git config --global user.name “nameHere”    
$git config --global user.email “emailHere”   

**NOTE:** this user and email will be replicated to new commits only.  

**Git show:** Used to show the files which are attached to commits.   
```
git log --oneline   
git show commit_id
```

### BRANCHES:   
:sparkles: It is an individual line of development.   
:sparkles: Developers write the code on branches.   
:sparkles: Initially breaches we create on git.   
:sparkles: After write source code on git, we push to GitHub.   
:sparkles: Default branch is Master. 

**Note:** When we do an initial commit the only default branch will be created.   

Dev -- > Git (Movie’s branch) -- > code -- > GitHub   

### COMMANDS:   
**git branch				:** to list the branches   
**git branch movies		:** to create the branch movie   
**git checkout movies		:** to switch blw the branches   
**git checkout -b dth		:** to create and switch dth at same time   
**git branch -m old new		:** to rename a branch   
**git branch -D recharge		:** to delete a branch   

**NOTE:** To recover the delete branch   

### PROCESS:
:sparkles: git branch movies   
:sparkles: git checkout movies   
:sparkles: touch movies{1..5}   
:sparkles: git add movies*   
:sparkles: git commit -m "dev-1" movies*   

#### NOW PUSH THE CODE TO GITHUB:   
create a repo
git remote add origin https://github.com/nayakdebasish091/paytm.git   

**PUSH:** to send files form git to GitHub   
**local:** .git & remote: paytm.git   
git push origin movies   
username:   
password:   

### TOKEN GENERATION: 
account -- > settings -- >developer settings -- > PAT -- > Classic -- > Generate new token -- > classic -- > name: abcd -- > select 6 options -- > generate    

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

**Note:** it will be visible only once   

create branch --> switch to branch --> files -- > add -- > commit -- > push   


**GIT PULL:** It will get the branch from GitHub to git   

git pull origin recharge   
git checkout recharge   

**GIT IGNORE:** It will not track the files which we want.   
touch java{1..5}   
vim .gitignore   
j* -- > :wq   
git status   

you can’t see the files now    

**GIT RESTORE:** To untrack the tracked file   
touch raham   
git status   
git add   
git status   
git restore --staged raham   
git status   

### GET BACK THE DELETED FILE:
Note: we can restore only tracked files.   
git reset 		 drops the changes staged into the index, but the working copy is left intact    
git reset –-hard 	 drops all the changes in the index and in the working copy   
=================================================
### MERGE:   
Adding the files between one branch to another branch   
git merge branch_name   

### REBASE:   
Adding the files between one branch to another branch   
git rebase branch_name   
 MERGE VS REBASE:   
MERGE	REBASE   
Merge will show files.	Rebase will not show files.   
Merge will not show branches.	Rebase will show branches.   
Merge will show entire history.	Rebase will not.   

**STASH:** to hide the files which are not committed.   
**Note:** file need to be tracked but not committed   

touch file2   
git stash   
git stash apply   
git stash list   
git stash clear   
git stash pop (clears last stash)   

==================================================================

### MERGE CONFLICT:   
When we try to merge 2 branches with same file and Different content Then conflict will raise.   
So, we need to resolve the conflict manually   

### CHERRY-PICK:    
We can merge only specific files from one branch to another with the help of commit_id   
git cherry-pick commit_id   

### SHOW:   
To show the commits for a file   
git show commit_id   

git log --patch   
git log --stat   

**REVERT:** To undo the merging   
git revert branch_name   
git revert   
git revert branch-1   

### INTERVIEW QUESTIONS:   
what is git & why to use?   
Explain stages of git?   
Diff blw CVCS & DVCS?   
How to check commits in git?   
Explain branches?   
What is .gitingore ?   
Diff blw git pull vs git push?   
Diff blw git pull vs git fetch?   
Diff blw git merge vs git rebases?    
Diff blw git merge vs git cherry pick?   
Diff blw git clone vs git pull?   
Diff blw git clone vs git fork?   
Diff blw git revert vs git restore?
merge conflict & how to resolve?
what is git stash?



