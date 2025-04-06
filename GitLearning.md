# Commands
git init //init
git ls-tree -r HEAD --name-only // show all files tree
git log // recent commits 
git status // staged files not commited and status
git push -u origin master // push commited local branch to online repo (origin is online repo name and master is branch name)
git branch // current branch
git commit -m "message for commit" // commit staged files
git add . // add all files to stage
git remote -v // online repo hooked status
git branch -M master // M for move branch or create if doesnt exist and set current
git remote add origin https://github.com/talal-jpg/OBS_GameplayAbilitySystemRpg.git // hook online repo and set origin as its name
 git rm --cached -r .\topDownRpg\  // clear submodule cache