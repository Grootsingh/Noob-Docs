### Git

## How to check your name as author in git bash (terminal)?

> for checking the author name:
> git command: git config user.name

> for updating the author name:
> git command: git config --global use.name "nameString"

> for updating the author email:
> git command: git config --global use.email emailText

> for updating default BranchName:
> git command: git config -- global init.defaultBranch "Name"

Note: use "" to add a string that has more then one word. otherwise you can write the string without any "" dubble quation.

## common git bash commands (terminal)

1. single Dot (.)

means currect directory in which we are standing.

2. dubble Dot (..)

mean go back one dir. (. currect + . prev)directory

3. Tab key => tab will help you automatically fill the full name if your provide something.

4. start .

this will open your currect folder directory (UI). you can also specify the path name realtive to the currect path. Ex: start Download => it will open the Download folder if any in the currect directory.

Note: open . for MacOS

5. ls

ls refer to list Directory. it will show all the Folder + file in the currect location.

ls is shortcut for (ls .)you can specify the path name realtive to the currect path.

Ex: ls Pictures

it will allow you to peak inside Pictures folder if any in the currect directory.

Note: ls allow you to peak inside into other directory from the currect directory. it doesn't change your current location in the directory. so if you want to access a file which is inside Picture folder (Ex:photo12.png) then your command will be (ls Pictures/photo12.png) you have to define the relative path from the currect path to to open that file. still your currect dir location is the same you have not changed your directory location.

> check hidden files with -a flag

-a refer to all hidden or non hidden you can see all the file from the currect path. ex:(ls -a)

6. dir

dir refer to directory. ls and dir both are same

dir is shortcut for (dir .)you can specify the path name realtive to the currect path

7. clear

clear the prev command line from the git bash terminal.

8. pwd

pwd refer to print working directory it is the currect location address or currectly woring directory address that your stainding in. it like URL but for file system.

9. cd

cd refer to change directory. you can provide location address (absolute or relative path) to change the currect directory path. you can move forward and backword in dir.

> cd ..

mean you can change dir and go back one dir. (. currect + . prev)directory

10. touch

- touch is used to create a file. ex: (touch MyNotes.txt)
- you can create multiple files too ex: (touch MyNote.txt MyOtherNotes.txt)
- you can create a file in another directory too ex:(touch desktop/Myfolder/Mynotes.txt)

11. mkdir

- mkdir refer to make Directory it will create empity folder. ex:(mkdir Myfolder)
- you can create multiple folders too ex: (mkdir MyFolder MyOtherFolder)
- you can create a folder in another directory too for that you need (-p) means parent folder: which allow you to add as many folder in nested order as you like. ex: (mkdir -p desktop/Myfolder/MyNewFolder/LatestFolder)

13. rm

rm refer to remove and you can use it to remove any file which is in the currect directory. ex:(rm notes.txt)

Note: when you remove file with rm it will permanantly delete the file. it will not go in the recyclebin folder.

you can remove multiple file from currect dir too ex(rm notes.txt javascript.js)

if you want to remove folder which is non-empity then use rm -r

> rm -r

remove recuresivly: it will remove the currect folder with all the files and folder which are inside currect folder recursivly.

> rm -rf

remove recurecivly and forcefully

14. mv

mv refer to move. which is used for both moving file or folder to different path and to rename file or folder name.

> mv for moving file or folder from one place to another

```
mv source_file destination
```

- source_file: Specifies the file you want to move.
- destination: Specifies the target directory where you want to move the file.

ex: (mv file.txt destination_folder/)

> mv for rename file or folder name

```
mv currentName updatedName
```

you can rename any file and folder for that you need to provide currectFileName and changeToFilename. ex:(mv Mynotes.txt Notes.txt)

15. code .

this will open VS code with currect folder directory.

16. q

q is used to quit and go back to previous page.

17. insert key + control key

for coping text in bash terminal

18. insert key + shift key

for pasteing text

### what is git and repository

> Repository

A repository (repo) is like a centralized storage place that holds all your project files and their complete history of changes. It's where all the information about how your project has evolved over time is stored.

- repo is a dairy.

> Git

Git, on the other hand, is the software tool that helps you manage that repository. It provides you with the tools and commands to create, update, and organize your repository. Git allows you to commit changes, create branches, merge changes from different sources, and more. It's like the toolbox you use to work with the contents of the repository.

### How to create a repository

for each new Project you need to create a new repo. by initalizing it.

in local Machine: first select a folder where you want to create a repo and initialize it.

```
// command for initalizating git
git init
```

you can also check the status of your git repo by git status

```
// command for checking git status
git status
```

Note: only create one folder/repo per project
Note: repo is not a folder. it is a wrapper on the folder. all the nested folder are refer to same repo.

### what is commit (Change)

- commit is entire on your repo dairy. in repo diary you note everything that has happens/changed (task) in your entire day. before bed you Note down what you have done in the entire day.

commit is a snapshort of changes that you have made on a repo. which contain a title/message (which describe what changes you have made) + change (add something, delete something,update something) to the repo.

A commit is a snapshot of the changes you've made in a repository. It captures the modifications you've introduced, whether it's adding new content, deleting existing content, or updating existing content. Each commit is associated with a title or commit message that describes the purpose or nature of the changes made in that particular commit.

So, to summarize, a commit is a record of changes you've made to the repository, including a description of those changes, and it helps maintain a clear and organized history of the project's development.

## Phases of Commit?

uptill now we have created a repo by creating a folder and intializing the git.

now in the same folder.

> Phase 1: Working on Files (Working Tree)

The working tree is the set of files and directories in your repository that you are currently working on or editing. These are the files you see and modify in your code editor or file explorer.

- create or edit files in repo Directory.

```
// i have created these 2 files
File 1: Typescript-patterns.md
File 2: Typescript-generics.md
```

add new files or folder. do some work on them. at this time if you see git status. git will recognise that repo folder contain some files. but there files are not being tracked by git. git will place all these file in Untracked files section.

```
// git status
> Untracked Files
Typescript-patterns.md
Typescript-generics.md
```

> Phase 2: Getting Ready to Show Changes (Staging Area).

Before you commit changes in Git, you have the option to stage (select) specific changes to be included in the next commit. The staging area is like a temporary holding area where you group together all the changes you want to commit.

- Select files you want to commit

Now, you need to tell the Git which files you wanted to be tracked by git. you will select there untracked files. by a git command

```
// command for selecting files to commit

// one file
git add fileName

//add multiple files too
git add fileName1 fileName2

// short hand for staging all the untracked file all at once.
git add .

// Our Example
git add Typescript-patterns.md Typescript-generics.md
```

git add . means add all the files which has been changed and being recognise or infer by git . becouse as we have seen earlier that git can infer that new file has been added (but are untracked), exisiting files has been motifled. etc you can select all the files which are being infered by the git using (.)

```
// Git Status
> Changes to be commited.
Typescript-patterns.md
Typescript-generics.md
```

you can unstage a file that you have exicedently staged also.

```
// git command for unstaging
git rm --cached fileName
```

now git know you want to track these two files. but still git is not tracking these file.

> Pahse 3: Taking a Snapshot (Commit):

When you're confident with your selected file in the staging area, it's like taking a photo of that files code. This is your commit. It captures the exact content of your files at that moment..

- commit all the selected files

now you are ready to commit all the files to the repo.

every time you commit your selected file. git will ask you for a little message/description think of it as title. which will help you or others to under what has been changed or updated.

```
// command for commiting
git commit

```

this is the basic way for commiting when you run this command it will open your VS code and there you need to type the message. after typing the message then click on the check mark to complete the process. people generally use this basic command when they need to give a large description in message to explain what they did insted of one line message.

```
// shothand
git commit -m "message"

// Ex:
git commit -m "New Typescipt pattern and generics Docs has been added"
```

shorthand for comming is generally is the prefered way of comminting with giving a short one liner explaination of what you did.

```
// more advance commit command which do both add (stage the file) and commit the file in single step.
git commit -am "message"

```

> Modifies the contents or message of the most recent commit.

after commiting, you realize that you have forgoten to add some files from the working stage or you have some type-error in the commit message and want to update the previous commit. you can use --amend flag to do so.

```
// fix prev commit mistakes
git commit --amend
```

Note: you can only make changes to the most recent commit you've made. you cann't fix commit before that.

- if you forgot to add one file from the working stage into the last commit. then you need to stage that file first. and then do the amendment

```
git add fileName
git commit --amend
```

- if you miss-spell the commit message. you can directly do

```
git commit --amend
```

git commit --amend will work like normal git command it will open the vs code and allow you to update previous commit message and then click on the check mark to complete the process and exit it.

```
// Git Status
nothing to commit,
working tree clean
```

> Phase 4: Checking for Cleanliness (Clean Working Tree)

Just before you take the snapshot (commit), you want to make sure you don't have any random/unwanted file lying around that you didn't incorporate into the stage area. This is your working tree being clean. It means there's no extra stuff left – everything you wanted to save from staging area has either added to your repo(committed) or discarded from the selected phase.

- all work is done. No work in progress left from the stage area.

### check who has commited in this repo

you can check log/recod of commit which tells you who has commited to the repo author name, time of commit and message (description of what has changed/ what you have done in the commit).

```
// git commit recod check command
git log
```

git log give you everything related to commit. who commited the commit,when he commited it. and the commit message

if you are only intrested in the summary of the commit message. then you can use --oneline flag with the git log it will give you first line of message of all the commits which is helpful. you don't need to see all the other things that you are not interested in.

```
// for loging one line description of message only
git log --oneline
```

### .gitignore file

A .gitignore file is a special text file used in a Git repository to specify files and directories that should be ignored by Git when tracking changes and committing code. This is particularly useful for excluding files that are generated automatically by the development environment, build tools, or operating system, as well as files that contain sensitive information.

Here's how it works:

1. . Excluding Unwanted Files: In a Git repository, you might have files that are created during the development process but don't belong in version control. These could include temporary files, file generated by Operating system like MacOS generate .DS_Store file.

> MacOS .DS_Store file

.DS_Store is a file that's automatically created by macOS Finder to store custom attributes of a folder, such as the position of icons, view settings, and other metadata. These files are used by the macOS operating system to maintain the appearance and arrangement of files and folders within a directory.

In the context of Git repositories, .DS_Store files are often seen as "noise" or unnecessary clutter because they contain information that's specific to the local machine and isn't relevant to the version-controlled content of a project.

Note: .DS_Store is generated by MacOS only. but it's a good practice to ignore it even if you have different OS.

2. Creating a .gitignore File: To prevent Git from tracking these files, you create a file named .gitignore in the root directory of your repository. You can use a text editor to define patterns for the files or directories you want to ignore.

```
.gitignore
```

3. Defining Patterns: Each line in the .gitignore file represents a pattern for matching files or directories to be ignored. The patterns can be simple file names, extensions, wildcards, or more complex rules. For example, you can ignore all .log files or an entire directory named build.

Note: each line in the .gitignore file represent a pattern.

> Some common patterns

- Ignore Specific Files by Extension:

```
*.log
*.tmp
*.swp
```

star represent any one word and .extension represent file type. so ignore all the files that has this extension.

- Ignore Entire Directories:

```
build/
node_modules/
.idea/
```

to distinguice folders from files you need to add / at the end of each folder name. by doing so you are ignoring the entire folder.

- Ignore Files Matching a Specific Name:

```
debug.log
secret.key
credentials.json
```

- Ignore Files in Specific Paths:

```
path/to/ignore/file.txt
path/to/ignore/
```

- Ignore Files Matching Wildcards

```
config-*.json
backup-*.zip
```

ignore any file that start with config-[anyword].json format.

- Ignore Files in All Subdirectories:

```
**/logs/
**/temp/
```

double start represent any directory and it's supdirectory. meaning ignore logs folder in any subdirectory you find.

- Don't-Ignore Files and Directories Using Negation (!):

```
*.log
!important.log
```

means ignore all the files that ends with .log extension but don't ignore important.log file. (!) overwrite ignorence rule and has precedence over ignorence.

4. Committing .gitignore: After creating or modifying the .gitignore file, you commit and push it to the repository. This ensures that everyone working on the project knows which files and directories to exclude from version control.

### Branch and HEAD

> Default Main Branch:
> When you create a new repository, Git usually sets up a default branch (often called "Master" in Git or "Main" in GitHub). This branch serves as the initial timeline of your project. It's where your project starts, and you build upon it as you make commits/changes.

> Main Branch as the Original Timeline:
> The main branch is often seen as the primary Timeline or the central line of development. It represents the stable, production-ready version of your project. Commits on this branch are usually considered well-tested and reliable.

> Head Pointer:
> The "HEAD" is a pointer in Git which indicates your current position in the timeline. It specifies both which branch you're on and which commit you're looking at. HEAD helps you navigate through different versions (TimeLine) of your project.

Head is a Pointer which indicate two things:

- it tell us on which Branch/TimeLine we are currently working on.
- and it also tell us current commit/changes you have made.

Head represnt Present Time in the Timeline. where are we at Presnet.

> Branches as Timelines:
> Each branch represents an independent TimeLine or independent line of development, allowing for parallel work on different features or fixes. These branches can evolve independently until you decide to merge them back together.

> Creating New Timelines (Branches):
> Creating a new branch is like creating a new parallel timeline. You can experiment, develop new features, or make changes without affecting the main timeline. This isolation is a powerful feature of Git that enables collaboration and experimentation.

you can create a new Parallel timeline that origins from the original Timeline.

> Merging Timelines:
> Merging branches is like bringing separate timelines back together. When you merge one branch into another, you combine their changes, making it easy to integrate new features or bug fixes into the main timeline.

you can also merge two different Timeline which will join the changes you have made in both the timelines.

its like you can added new History in the Original TimeLine. which will change the trejectory of the originalTimeLine.

- Head indicate the last entry/commit you have made on your repo diary. the latest entry/commit.

```
// you can have as many parallel timeline you want

Main TimeLine
-----------------------------------------------------
                    \
                     -------------------------------
                        Parallel TimeLine
```

```
// head represt latest entry on the repo.

                                     HEAD
                                       ↓
                                     Master
                                       ↓
........................................
    commit1     commit2   commit3   commit4

```

---

Stupid: Anime analogy
In dragonball's Future Timeline when Goku get's sick due to a "Heart Virus" and died. everything go into caos then Bulma decided to send Trunk in History Timeline and give the Medicine so that Goku. can be saved and later Goku can save the world.

then Future Trunk came back into current Timeline where Goku is Still alive and has not catch the "Heart Virus". He give him the medicine and save the goku's life which endup Fixing the future of the current Timeline.

but Future Trunks parallel Timeline has not be fixed becouse they both are in different timeline.

If they knows how to merge both parallel timeline then boths Timeline will beocome one at some point and Trunks Future can also be saved.

---

## create Branchs (Parallel TimeLines)

git branch will log all the branchs (Parallel TimeLines) that exisit at current Moment.

```
// to show all the branch name exist in the repo
git branch

// to show all the branches name + last commit oneliner of that branch (Head)
git branch -v
```

> create new Branch

```
git branch branchName
```

with this command you can create a parallel timeline. which origian from the original timeline last entry/Head.

you have created new TimeLine but how to switch to the newTimeline.

> switch to new branch

```
// old way to switch from A to branchName
git checkout branchName

// newer way to switch from A to branchName
git switch branchName

// shothand for creating a new branch and switching to it in single step
//old way
git checkout -b branchName
//new way
git switch -c branchName

```

now your head will indicate that you are on branchName timeline and you can add new commit from here. which will not affect the original timeline.

in new branch you can add new files or delete old files. update 100 files. these will not change anything on other branches. this will stay seprate in that perticuler branch.

> conflict while switching between branches

case1: let's say you currently are on Branch-A and created a new branch (Branch-B) and started working on it (Branch-B). you have just updated Branch-A file by either adding new line in same files. or deleted some lines from the previous commit.

now if you switch bach to branch-A without commiting the work you did in branch-B it will create a conflict. becouse on same file we have different code in two different branch. you should either commit the work you did in branch-B before switching to branch-A or (stash them before switching) (i don't know yet)

case2 : let's say you currently are on Branch-A and created a new branch (Branch-B) and started working on it. you have added new asset like added new files and worked on those new files. you have done nothing in the previously exsisting files which you get from branch-A commit.

now if you switch back to Branch-A it will not create a conflict and those new file that you work on will be carried with you into branch-A as untracked File. they will follow you. the reason there was no conflict becouse branch-A don't have those files. there is no common file that has been change to create conflict.

## delete branch

```
// -d will delete the branch . it only delete those branches which are fully merged.
git branch -d branchName

// -D will force delete the branch . it will delete the branch irrespective of branch status.
git branch -D branchName
```

Note: you can't delete a branch while currectly being at that branch. it means you can't delete the branch you are currently on.

## remane branch

```
// rename the current Head branch
// -m for move

//rename a branchName to NewbranchName if any-other branchName does not have the same name
git branch -m NewbranchName

//rename a branchName to NewbranchName Forcefully and if any-other branchName have the same name then it will overWrite the branches which endup removeing that another-branch.
git branch -M NewbranchName
```

Note: you can only be able to rename the branch name that you are currently on.

### merge

```
git merge branchName
```

Note: you can only merge a parell branch to the current branch you are on (Head).

ex: to merge a branch-A to branch-B. you have two options either merge branch-B to branch-A (Head) or branch-A to branch-B (head).

# case 1: fast-forward merge

```
// before mergeing

Master-branch
---------------------------------------
       commit-M1      commit-M2     commit-M3
                                       |
                                       ↓
                                       --------------------------------
                                                commit-P1           commit-P2

                                        parallel-branch

=================================================================================
// after merging
// parallel-branch commit gets copied to master branch.


Master-branch
--------------------------------------------------------------------------
       commit-M1      commit-M2     commitM3    commit-P1           commit-P2
                                       |
                                       ↓
                                       ----------------------------------
                                                commit-P1           commit-P2

                                        parallel-branch


Note: parallel-branch still exisit.
```

> when fast-forward merge happens?

✅ if parallel-branch has the complete commit history of Master-branch + it's new commit history.

✅ parallel branch has imerge from the master branch so it contain the complete history till the parallel-branch was created + its own commits [M1,M2,M3,P1,P2]

❌ Master-branch has not added any new commit after the parallel-branch has been added/created

> what happens when we merge?
> first we need to decide whome to merge with whome.
> as general thum of rule. master branch is concider the main branch so we will merge parallel-branch in the marster-branch.

```
// first you need to stand/be on master branch your head need to be on master-branch
git switch master

// then merge
git merge parallel
```

now there is no conflict between master and the parallel branch becouse parallel-branch has all the code that exsisited on the master branch. both has the same files but there is no code conflict

becouse it's not like master files code-line number 5 has different code in compare to parallel files code-line number 5. what happend is new lines has been aaded in the parallel file code. with the master previous file code. as we already said that parallel commit has complete commit history of master file + its own new commits [M1,M2,M3,P1,P2].

so there was no conflict as it was easy to merge both files. it like moving the pointer

conflict happens when there is different code at same-line in file which is shared between two different branches.

if you added new file in parallel branch then there is no conflict its easy to added that new file to master branch beocuse there is no conflict . there is no common ground/file to create a conflcit.

what happens is parallel branch all new commits [P1,P2] will be added to the master branch.

Note: after merge parallel branch still exist it has not been removed.

> no to fast forward

when we do fast forward merge. it will merge the data to the current commit without creating new commit. if you want to have a new commit which indicate that at this point a merge happend and what to have a history of a new commit to go back to. you can use --no-ff

```
git merge --no-ff <BranchName>
```

# case 2: merge conflcit (non-overlaping code)

```
// before mergeing

Master-branch
--------------------------------------------------------
       commit-M1      commit-M2     commit-M3     commit-M4
                                       |
                                       ↓
                                       --------------------------------
                                                commit-P1           commit-P2

                                        parallel-branch
===============================================================================
// after mergeing
// new commit-M5 has been added to master-branch

Master-branch
--------------------------------------------------------------------------
       commit-M1      commit-M2     commit-M3     commit-M4         commit-M5
                                       |                              ↑
                                       ↓                              |
                                       --------------------------------
                                                commit-P1           commit-P2

                                        parallel-branch


```

> what kind of conflict?

parallel-branch commit history [M1,M2,M3,P1,P2] but Master-branch has added new commit-M4 after creating a new branch. and parallel-branch has no recod of commit-M4.

but in commit-M4 we have not touched the files that are shared with the parallel-branch. a new file has been created. we added some data and made a commit-M4

> Now what will happen?

there will be No overlapping code confict. but still parallel-branch has no idea about new commit-M4. when you merge parallel-branch to master-branch then git will create a new Merge-commit and ask you to give a description/message to describe the change like normal commit. and merge both the branches.

# case 3: merge conflcit (overlaping code)

```
// before mergeing

Master-branch
--------------------------------------------------------
       commit-M1      commit-M2     commit-M3     commit-M4
                                       |
                                       ↓
                                       --------------------------------
                                                commit-P1           commit-P2

                                        parallel-branch
===============================================================================
// after mergeing
// new commit-M5 has been added to master-branch

Master-branch
--------------------------------------------------------------------------
       commit-M1      commit-M2     commit-M3     commit-M4         commit-M5
                                       |                              ↑
                                       ↓                              |
                                       --------------------------------
                                                commit-P1           commit-P2

                                        parallel-branch

```

> what kind of conflict?

parallel-branch commit history [M1,M2,M3,P1,P2] but Master-branch has added new commit-M4 after creating a new branch. and parallel-branch has no recod of commit-M4.

in commit-M4 we have added new data in the same file that has been shared with the parallel-branch.

if we try to merge parallel-branch to master-branch then the shared/common file will have different code at same lines. it's like master files code-line number 5 has different code in compare to parallel files code-line number 5. it will create a confict.

parallel-branch has different code at shared file in same line in compare to master-branch becouse parallel branch does not has the code for M4 commit.

> what will happen when we merge?

now if you merge parallel-branch to master branch then you will see a code-conflict message on the bash terminal. and then your VS-code will open.

In Vs-Code you will see

```
// unconfliced data this is the common data between both the parallel and master branch so there is no conflict here.

Unconflict-Code...

............................................................................
// Option buttons availiable
1. Accept Current Changes
2. Accept Incoming Changes
3. Accept Both Changes
4. Compare Changes

//<<<<<<< HEAD branch to which you are merging to
// the code of master-branch (Head) that is overlaping with the parallel-branch

conflcit-Code... from (Head-branch Side)

=======
// the code of Parallel-branch that is overlaping with the Head-branch

conflcit-Code... from (Parallel-branch Side)

>>>>>>> ParallelBranch the branch you are merging of
............................................................................
```

> VS code give us options to what you want to do with overlapping code?

// Option buttons availiable

1. Accept Current Changes : accept only Head-branch conflicting code
2. Accept Incoming Changes : accept only parallel-branch conflicting code
3. Accept Both Changes: accept both-branch conflicting code
4. Compare Changes: check side by side and then deicde.

Note: Un-conflict code will be added these are options for conflicted code that is overlapping. different code at same line.

after you decide manually which conflict-code to keep. you can save the changes and after that git will automatically created a new Merge-Commit and ask you to give a message. after that the merge is complete.

Merge-commit has two parent pointer pointing to it. up untill now every commit had only one parent commit.

this time master-branch has added a new commit. on the other hand in fast-forward maerging it doesn't create a new commit. it just added/copy the parallel-brach commit to the master-branch.

### Diff

Git Diff shows the differences between different versions of files in your Git repository. It provides a clear view of what has changed between two points in your commit history, whether that's between different commits, branches, or even between the working directory and the most recent commit.

The output of git diff provides a line-by-line breakdown of changes, indicating additions, deletions, and modifications. It helps you understand what exactly has been altered between the two points you're comparing.

Remember that git diff shows differences, but it doesn't alter your repository in any way. It's a read-only command to help you understand changes and review your code before deciding whether to commit or perform other actions.

before we dive deep lets revice some terminology:
Commit has 3 phases:

- Working : when you add or edit code
- Stageing : when you select files from wokring Directory for commiting
- Commit : actually commiting by taking a snapshort

Diff can be used to check the difference between all 3 point in time.

1. Last Commit vs current Working Directory

```
// Compare difference between last commit code vs currently changed code from working tree only.
git diff
```

2. Last Commit vs current staging Phase

```
// Compare difference between last commit code vs only with currently staged file code
git diff --staged
git diff --cached
---------------------
// you can nerrow down the comparision. it quite possible that you have staged multiple files. it will
// compare prev version of that file (last commit) vs staged version of that file.
git diff --staged fileName
```

3. Compare Last commit vs All the changes till now includes Both Working and staging phase and commit too

```
git diff head
---------------------
// you can nerrow down the comparison
git diff head filename
```

4. Compare two different Commit

```
// all the changes that happens since from commitHashCode-1 to commitHashCode-2.
git diff commitHashCode-1 commitHashCode-2
git diff commitHashCode-1..commitHashCode-2
----------------------------------------------------
// you can nerrow it down
git diff commitHashCode-1 commitHashCode-2 fileName
```

.. (double dot) represent a range A..B means All the commits that are reachable from A to B.but Not B to A.
...(Tripal dit) represent range A...B but all the commit that are reachable from A to b And B to A also.

4. 1. you can compare two different Commit with ~ too

```
git diff Head Head~1
```

~ symbol refer to tilda. what it represent is it work has back button. Head means current last commit and Head~1 means one before the last commit. so you can compare two commit this way also.

Note: ~ is use to refrence previous commit to the current specified Branch. it can only go back to the first commit of that specific branch.

4. 2. you can compare two different Commit with @{n} too

```
git diff Head Head@(1)
```

@(n) is also used to refrence previous commit but it is not restrected to the specified current branch. @(n) is used to get back in the ref-log history (git log). it can be on any branch upto first commit in the repo. isn't it cool.

5. Compare two different Branches

```
// all the changes that has happens since from branchName-A to branchName-B
git diff branchName-A branchName-B
git diff branchName-A..branchName-B
----------------------------------------
// you can newrrow it down
git diff branchName-A branchName-B fileName
```

> let understand how compare code looks like and understand each component.

```
Last Commit
File Name: ranbow.txt

Line-0 red
Line-1 orange
Line-2 yellow
Line-3 green
Line-4 blue
Line-5 purple
-------------------------------------
Working Area
File Name: ranbow.txt

Line-0 red
Line-1 orange
Line-2 yellow
Line-3 green
Line-4 blue
Line-5 blue
Line-6 indigo
Line-7 violet
```

```
git diff
-------------------------------------
Git Diff Output

Line-1 diff --git a/rainbow.txt b/rainbow.txt

// we are comparing rainbow.txt file but "a" represent previous commit version and "b" represent working area version. a and b are just created for make it easy to understand which file we are talking about.

Line-2 index 72d1d5a..f2c8117 100644

// this is just some meta data Hashcode generated for every phase Not importent for our purpose.
// index means staging phase

Line-3 --- a/rainbow.txt
Line-4 +++ b/rainbow.txt

// Line-3 and Line-4 tell us that a/rainbow.txt which is previous commit version is represented by --- Symbol and b/rainbow.txt which is working area version is represented by +++ Symbol.


Line-5 @@ -3,4 +3,5 @@ orange

// Line-5 can be divided into two part first Half = -3,4

// which say - symbol meaning we are talking about a/rainbow.txt file. and code has been taken from that file we have taken code from Line-3 upto next 4 Lines means 3,4,5,6 Lines has been taken.

// Line-5 second Half represnet = +3,5

// which say + symbol meaning we are talking about b/rainbow.txt file. and code has been taken from that file. we have taken code from Line-3 upto next 5 lines means 3,4,5,6,7 Lines has been taken.

Line-6  yellow
Line-7  green
Line-8  blue
Line-9  -purple
Line-10 +indigo
Line-11 +violet

// Line-6 to 11 are the code they are also color coded means if lines are of

// Black color means these line has not been changed between different phases. in general Diff output always show some black code from starting of the code and some black code at the end so you can have a general understanding where the code has changed.

// Red color means these lines has been removed in the second file (in b/rainbow.txt purple has been removed. which was present in the a/rainbow.txt file)

// Green color means these lines has been added in the second file (in b/rainbow.txt indigo and violet has been added. which was not present in the a/rainbow.txt file)

```

### Git stash

git stash is a Git command that allows you to temporarily save changes in your working directory and staging area directory that are not yet ready to be committed. It's a useful tool for situations where you want to switch to a different branch or perform some other task without committing your current changes.

When you run git stash, Git creates a stash/stack that stores all of your changes in working dir and staging area (also known as index area) in a separate area (Hidden), effectively "saving" them for later use. This allows you to empity your currently working and staging area so that return to a clean working directory without the uncommitted changes. Later, you can apply or restore those changes back to your working directory when you're ready to continue working on them.

The term "stash" reflects the concept of safely storing your changes away so that you can retrieve them later when you're ready to continue working on them. It's a way to maintain a clean working directory while still keeping track of your ongoing work without committing incomplete or experimental changes.

> How to save changes in the stash?

```
// stash the changes without descrption
git stash
git stash save
// stash the changes with descrption
git stash save message
```

when you run git stash it will save all of your work in a store and emipty your current working and staging are so that you can switch to other branches ro do something else without worring.

if you don't use stash, and has some work in working and staging area then git will not allow you to switch to other branchs. it will say either stash your work or commit it.

you can stash changes in one branch and un-stash them anywhere in the repo. in other branches too. you can use stash as copy paste mechanism too.

> How to add multiple stash in stash store?

```
git stash
git stash save message
```

you can use these command as many times you want to add stash into the stash store.

> how to check how many stash we have added or there is no stash?

```
git stash list
```

it will show you the recod of all the stash you have added in the stash store.

> How to restore the changes to re-use them again?

```
// restore the stash in the current branch and remove the stash from the stash store
git stash pop

// pop + specificity
git stash pop stash@{id}

// restore the stash in the current branch and  don't remove the stash from the stash store

// you can use apply if you don't want to remove changes from the stash and use those changes at
// multiple places.
git stash apply

// appy + speficity
git stash apply stash@{id}
```

> How to remove stash from the stash store?

```
// remove the most recently added stash from the stash store + add that removed stash on the current branch
git stash pop

// pop + specificity
git stash pop stash@{id}

// remove the most recently added stash from the stash store.
git stash drop

// remove the a specific stash from the stash store.
git stash drop stash@{id}

// remove all the stash from the stash store.
git stash clear
```

> if you add a new file that is untracted in the stash ?

```
git stash -u
```

git stash only stash those files which are tracked. if you add completly new file which is not being tracked or not bean commited even once then. then you need to use git stash -u to tell the stash that i want to stash all the files in this branch tracked or untracked both.

### Time traval Go back to any previos commit in branch.

> before that we need to understand HEAD pointer a little better?

so, HEAD pointer points to the latest commit added to the "Branch". this statement is Wrong.

Head Pointer "generally" Points to the "Branch Name". and Branch Name points to the latest commit of the branch.

Head doesn't point to the commit directly (generally).

```
// General setup
                                      HEAD
                                        ↓
                                      Master
                                        ↓
-----------------------------------------
       commmit-1     commit-2        commit-3

```

> Time Travel (going back to any previos commit of a specific branch)

```
// go back to a specific commits
git checkout CommitHeashCode
git checkout HEAD~Number
```

when you go back in commit History using these commands you Head which is suppose to be attached to the branchName get's detached and refer to the specificed commit from the commit history.

```
git checkout Head~2
..............................................................

           HEAD                        Master
            ↓                            ↓
-----------------------------------------
       commmit-1     commit-2        commit-3

```

your git will show you Head detached state message to inform you that the head has been detached from the BranchName.

Now if you check your files. all the files will be revertback to commit-1 state.

you will observe few More things:

- git log --oneline will show you the commit history upto the Head. (Not all the commit upto branchName pointer master)

don't worry all the changes you have made upto master (commit-2 & commit-3) still exist. they are just not visible becouse git log show commit history upto Head.

if you want to go back to branchname pointer (master) simply use switch

```
git switch BranchName

// shorthand for switching to the previously checked out branch.
git switch -
-------------------------
Ex: git switch master
```

> things to know about Detached state.

detached state is Observer state (Readonly). you can go back in time but to check not to modify the content you had in prev commits.

if you want to modify the data. you can create a newBranch from commit-1 which carry all the commit-1 data on new branch and you can modify the data as much you want there. which will keep all the changes that you do in with commit-1 in parallel branch. which does not affect the orginal master timeine branch.

if you don't create a new branch and add a commit on the original branch then it will do nothing all the commit changes will be descarded.

### undo changes in working area or staged area.

if you have worked on a file (working area) and later decided to not want it to commit it. you want those changes to be descarded/removed.

> restore back to last commited state (only for working area changes).

```
git checkout head fileName
git restore fileName

// for all the files
git checkout head
git restore .

// shothand
git checkout -- fileName
```

git checkout Head will restore back the fileName file to it's previosly commited state. which in result remove all the changes you have made in working area.

git restore has defult souce-value set to Head so you can directly tell the fileName and it will bringback that file to its previos commit state.

> restore back to specify commited state.

```
git checkout Head~Number fileName
git checkout CommitHashCode fileName

git restore --souce  Head~Number fileName
git restore --souce CommitHashCode fileName
```

you can decard the changes you have made in working area and restore back to any specific commit for that perticuler file.

> unstage files to working stage.(only for working area changes)

```
git reset fileName
git restore --staged fileName

//unstage all the files
git reset
git restore --staged .
```

If you have accidentally added a file to your staging area with git add and you don't wish to include it in the next commit, you can use git restore --staged to remove it from staging area to working area.

### undo the commit in commit history but keep the changes made in file in that commit.

> undo the commit from the commit history and uncommit the changes and place them in staging area.

```
git reset commitHashCode
git reset Head~number
```

reset will keep the changes that you made in the Commit and those changes are un-done (un-commited) and are placed in staging area but reset still undo the commits in the commit history.

reset is useful when you have accidently added a commit in the wrong branch. you can remove/discard the commit from the commit history and still keep the changes you have made in those commits in the staging area and create a new branch and commit those changes there.

> undo the commit from the commit history and uncommit the changes and place them in working area.

```
git reset --mixed commitHashCode
```

This command undoes the commit from the commit history.
The changes from the undone commit are placed in the working directory, not in the staging area.
The staging area is also cleared, meaning the changes from the undone commit are "unstaged" and moved to the working directory.

> undo the commit from the commit history and uncommit the changes and descard them.

```
git reset --hard commitHashCode
```

This command undoes the commit from the commit history.
The changes from the undone commit are placed in the working directory, and descarded.

this command forcefully reverts your entire repository to the state of the specified commit, including removing any subsequent commits and changes in both the working and staging area. making clean working tree.

### undo the changes made in file by a commit but keeping the commit in the commit history + adding a new commit to know that this commited is un-commited.

```
git revert commitHashCode
git revert Head~Number
```

revert remove all the changes that are commited in specified commit but still keeping the commit in the commit history recod. + git will create a new commit which indicate revert + message to know why you reverted that commit.

The git revert command is used to create a new commit that undoes (descard/removed) the changes introduced by a commit. It's a way to "reverse" the effects of a specific commit while preserving a complete history of changes in your repository.

Here's how git revert works:

Creating a Revert Commit:
When you run git revert <commit>, Git analyzes the changes introduced by the specified commit and creates a new commit that undoes those changes.

New Commit Created:
The result of the git revert command is a new commit with a message indicating that it's a revert of the original commit. This commit effectively cancels out the changes from the original commit.

Commit History:
The commit history now includes the original commit that introduced the changes and the new revert commit that undid those changes. This provides a clear record of the change, its undoing, and why it was undone.

The git revert command is often used when you want to correct a mistake in a commit or if you want to remove specific changes that were made in the past without altering the overall history of your repository. It's a safe way to undo changes while maintaining a clean and coherent history.

#### GitHub

### What is GitHub?

- GitHub is a Cloud Server that allow you to store/host your Repository in the Cloud.

Github is a hosting platform for git repositories. You can put your own Git repos on Github and access them from anywhere and share them with people around the world. Beyond hosting repos, Github also provides additional collaboration features that are not native to Git (but are super useful). Basically, Github helps people share and collaborate on repos.

some common features of GitHub:

- Repository Hosting: GitHub allows you to create repositories to store and organize your code. Each repository represents a project and contains the project's files, history, and documentation.

- Version Control: GitHub is built on top of Git, which is a distributed version control system. This allows developers to track changes to their code over time, collaborate with others, and manage different versions of their software.

- Collaboration: GitHub provides tools for collaboration, such as pull requests, issues, and discussions. Developers can work together on code, review each other's contributions, and discuss changes before merging them into the main codebase.

- Pull Requests: Developers can propose changes to a repository by creating a pull request. This allows for code review, discussions, and testing before changes are merged into the main codebase.

- Issues and Bug Tracking: GitHub's issue tracking system helps teams manage and track bugs, feature requests, and other tasks related to the development process.

- Wikis and Documentation: Repositories can include wikis and documentation to provide information about the project, its features, and how to use it.

- GitHub Actions: This feature allows you to automate workflows and tasks related to your codebase, such as running tests, building and deploying software, and more.

- Community and Social Features: GitHub fosters a sense of community among developers. Users can follow repositories, star projects they find interesting, and contribute to open-source projects.

- Visibility Options: Repositories on GitHub can be public or private, depending on whether you want to share your code openly or keep it restricted to collaborators.

### how to clone a repo from GitHub to you Local machine.

you use git clone when you want a local copy of a repo which is Hosted/Stored on a remote server like GitHub.

```
git clone GitHubRepositoryURL
```

git clone is a git command not a Github command. you can clone a repo from any cloud server that Store/Host Repository on there plateForm like GitHub, GitLab. you don't need an account on github to clone a repo from GitHub.

you simply go to the plateform look for a repo you want to clone then copy the HTTPS URL of that Repo

```
// in github
Browser URL is not the same as Repo URL.
you will find Repo URL in Code Button> Clone> HTTPS Section.

------------------------------------------------------------
// in your Local Machine
git clone GitHubRepositoryURL
```

Git will retrieve all the files associated with the repository and will copy them to your local machine. In addition, Git will automatically initializes a new repository on your machine for you and give you full access to the Git history of the cloned project.

Note: Make sure you are not inside of a repo when you clone!

# Deep Dive into Git Clone (When happens when you clone a repo)

```
// GitHub
Main-branch
                                       main
                                        ↓
-----------------------------------------
       commit-M1      commit-M2     commit-M3

=================================================================
// git clone (after cloneing the repo to local machine)

// Git
Main-branch
                                       Head
                                        ↓
                                       main
                                        ↓
-----------------------------------------
       commit-M1      commit-M2     commit-M3
                                        ↑
                                   origin/main
```

when we clone a GitHub repo we get repo + commit history of that repo + <remote>/<branch> pointer.

> let's talk about <remote>/<branch> pointer first.

we have one extra pointer origin/main. this pointer refer to the last commit on the GitHub. when you clone a repo from the gitHub at that time both git main and origin/main will be pointing to the same commit. this indicate that gitHub repo is uptodate to the changes in your git local repo.

as you start adding new commit to the git main branch on every commit you will recive a message "your branch is ahead of 'origin/main' by 1 commit".

```
// after adding new commit
// Git
Main-branch
                                                       Head
                                                        ↓
                                                       main
                                                        ↓
---------------------------------------------------------
       commit-M1      commit-M2     commit-M3      commit-M4
                                        ↑
                                   origin/main

```

you can timeTravel back too to check what was the last commit on the Github repo.

```
git checkout <remotename>/<branchName>
Ex: git checkout origin/main
```

this will ofcouse will detached your head. you can obsercer and switch back to main again to do your work.

as you can see gitHub does not automatically it's repo in reference to git (local machine) so you need to manually tell github by using git push <remote> <branchName>

> let's talk about repo + commit history

```
// GitHub repo
                                     main
                                       ↓
-----------------------------------------
       commit-M1      commit-M2     commit-M3
                                       |
                                       ↓
                                       --------------------------------
                                                commit-P1           commit-P2
                                                                       ↑
                                                               Parallel-Branch

=====================================================================================
// Git repo after cloning in your local machine

                                       Head
                                        ↓
                                       main
                                        ↓
-----------------------------------------
       commit-M1      commit-M2     commit-M3
                                        ↑
                                   origin/main

```

Once you've cloned a repository from github, we get all the files and commit history for the project upto github last commit. as you can notice we have cloned a github repo which has more then one branch [main,parallel-branch]. but when i cloned it and open in my local machine. i can only see the main branch.

when you do git branch -v to checkout all the branches that are availiable to access. you will only see one main branch only.

> what's going on here ?

When you clone a Git repository to your local machine, you do have access to all the branches present in the remote repository. However, by default, Git only checks out the default branch (often named "main") after cloning. + create upstream realtionship with the default branch too.

This default behavior helps prevent your local working directory from becoming cluttered with branches you might not need immediately.

if you are cloneing a repo from github then atleast you want to access the main branch so git checks out the default branch for you by default. and leave the other branches for the user to decide if they want to access them in there working directory or not.

you can check all the remote branches

```
// shows all the remote/gitHub branches availiable in your local machine.
git branch -r

----------------------------------------------------
// from our above example
origin/head -> origin/main
origin/parallel-branch
```

you will find it contain all the branchs that are are supposed to be there both the branchs [main,parallel-branch].

you can timetravel too to these branches if you want.

> how to access remote branchs in your local machine

```
git switch <remoteBranchName>
ex: git switch parallel-branch
```

with get switch command you can easily add remote branches in your local working environment. + switch also create upstream realationship (this is switch command feature) by defualt.

now when you check all the branches that are availiable to access in your environment. you will find parallel-branch too.

by the way switch is generally used for switching between branches so if you have a branch in your local machine then you can access it. and this is was actually happening. you have remote branch avaliable in your local machine so it allow you to access it. the difference is that remote branches were not visiable before but as switch to that branch git realize that oh you want to access this branch then let it be avaliable for local environment. and now you can find that branch to be availiable in git branch -v.

### create an acount on GitHub

after creating account using UserName, Email, Password.

it is recommanded to give the same Email as Git config user.Email this help gitHub to who you are when you push code from git to github. it helps to show your Profile Pictures when you push your code.

You need to Setup SSH Key for easy communication between Git and GitHub when ever you push your code. other wise GitHub will ask you for Login Email and password every time you push your code.

SettingUp SSH key save us time from Not Authenticate Every time we push your code. after SSH key has been setup Once. Github can automatically authenticate who you are without you telling it. save Time when you are working.

you have to setUp SSH for Every Machine Once.

---

Extra: What is SSH Key

An SSH key (Secure Shell key) is a pair of cryptographic keys used for secure authentication and communication in a networked environment. SSH keys are commonly used in the context of secure remote access to servers, version control systems (like Git), and other secure communication protocols.

SSH keys work based on public-key cryptography. The pair of keys consists of:

Public Key: This key is intended to be shared freely. It's used to encrypt data or verify digital signatures. The public key is usually stored on the remote servers or services you want to access.

Private Key: This key must be kept secret and secure. It's used to decrypt data or create digital signatures. The private key should never be shared or exposed to others.

When you want to authenticate yourself to a server or service using SSH keys, the process typically goes like this:

1. You generate a pair of SSH keys on your local machine.

2. You provide your public key to the server or service you want to access. This is often done by adding the public key to your user account on the remote server.

3. When you attempt to connect or authenticate, the remote server sends you a challenge that can only be decrypted using your private key.

4. Your local machine decrypts the challenge using the private key and sends the decrypted response back to the server.

5. If the decrypted response matches what the server expected, you are granted access.

Using SSH keys has several advantages over traditional password-based authentication:

- Security: The private key is not transmitted over the network, reducing the risk of interception.

- Authentication without Passwords: You don't need to remember passwords; you only need to have your private key available.

- Two-Factor Authentication (2FA): Using SSH keys can be part of a two-factor authentication setup, where the possession of the private key is one factor, and the passphrase protecting the private key is the second factor.

- Automation: SSH keys are often used in automated processes or scripts, allowing for secure authentication without human interaction.

It's important to manage your SSH keys carefully. Protect your private key with a strong passphrase and don't share it with anyone. Also, be cautious when adding your public key to remote servers, as this is what allows you access. If someone gains access to your private key, they can impersonate you and potentially gain unauthorized access to systems.

---

# How to Generate SSH for GitHub

You can read about it on GitHub Website too.

Every Operating System has there version of command to generate SSH key so Do follow that. i have windows so i will procide with that.

1. Check for Exisisting SSH key if any.

```
// Open your Bash Terminal and type this code
ls -al ~/.ssh
```

this will look for .ssh directory in your computer if any. and show your all the generated SSH key.

otherwise it will say No Directory found.

you can generate new key too even if there where already exisiting SSH key.

2. generate SSH key

```
// Open your Bash Terminal and type this code
ssh-keygen -t ed25519 -C "your_email@example.com"
```

then git will ask you for a file location where to place your ssh key file which contain generated key. Provide a new Location or simply Press enter which will take the default location.

then git will ask for Enter the passphrase: type any text you want.
then again repeat the passPhrase: type the same text again.
your Private key will be encripted with the passPhrase to protect your private key.

now shh key has been generate and you can check it too.

Note: SSH key is not just a single key actually there are two keys that are generated by this command.

1. Public key : used to encript key (give freely to others)
2. Private key : used for decript the key (keep it hidden)

Now when you connect to your GitHub account you will give a Public key to GitHub. when gitHub try to authenticate your account . it will send you a challenge that is encripted by the Public key you have given to the Github. which means that challenge can only be accessed by Your Private key. Your local machine decrypts the challenge using the private key and sends the decrypted response back to the server. If the decrypted response matches what the server expected, you are granted access.

Now we have setup a way to authenticate our mechine with the GitHub. but still everyTime you acess the GitHub with your machine gitHub will ask you for the private key.

We manage to create a authentication system. but we need to automate this so we don't have to manually type private key every time we try to access GitHub.

For that you need to setup SSH Agent in your machine that will automatically take care the task for providing Private key every time you access the GitHub.

# SSH Agent

SSH Agent is a program that help us to automate the process of providing private key to the remote server. when ever remote server ask for it.

as log as SSH agent is running in the background we don't need to worry about providng the private key manually.

Once you've started the SSH Agent and added your private key to it, it will continue to manage your decrypted private keys for the duration of your session. as long as your Bash terminal is running it will stay active in the background. once you close the terminal. the SSH Agent will be terminated along with it.

```
step:1 Run the SSH Agent
eval "$(ssh-agent -s)"

step:2 Provide the Private key to SSH Agent
ssh-add ~/.ssh/id_ed25519

step:2.1 it will ask for your PassPhrase. Provide that and SSH Agent will got the Private Key.
```

you have succesfully provided the private key to your SSH Agent.

Now next step is to provide the Public key to the GitHub.

---

Extra: What is SSH Agent?

SSH Agent (also known as ssh-agent) is a program that helps manage and store private SSH keys used for authentication when connecting to remote servers or services over SSH (Secure Shell). It acts as a secure repository for your private keys and helps streamline the process of authenticating to remote systems without having to repeatedly enter your passphrase.

Here's how SSH Agent works:

1. Private Key Encryption: When you create an SSH key pair, your private key is encrypted with a passphrase. This passphrase adds an extra layer of security to your private key.

2. Passphrase Entry: When you attempt to use your private key to authenticate with a remote server, you're typically prompted to enter the passphrase for your private key. This can become cumbersome if you're connecting to multiple servers frequently.

3. SSH Agent: SSH Agent serves as a "keyring" for your encrypted private keys. When you start the SSH Agent, you enter your passphrase once, and the agent holds the decrypted private keys in memory for as long as the agent is running.

4. Automatic Key Usage: Once the SSH Agent is running, whenever you attempt to connect to a remote server that requires the private key, the agent automatically provides the decrypted private key without requiring you to re-enter the passphrase. This streamlines the authentication process.

Benefits of using SSH Agent:

- Convenience: You only need to enter your passphrase once when you start the agent, rather than entering it every time you connect to a remote server.
- Reduced Exposure: The private keys remain encrypted on disk and are only decrypted in memory while the agent is running. This reduces the chances of your private key being compromised.
- Automation: SSH Agent is useful when automating tasks that involve SSH authentication, as it enables scripts and tools to use the agent to authenticate without user interaction.
- Multiple Keys: SSH Agent can manage multiple private keys, so you can use different keys for different servers or services.

To start an SSH Agent and add your private key to it, you can use the following commands in a terminal:

```
// bash
eval $(ssh-agent -s)
ssh-add /path/to/your/private/key
```

Remember that while SSH Agent makes key management and authentication more convenient, it's important to secure your passphrase and ensure that you're using strong passphrase protection for your private keys.

---

# adding Public key to the GitHub Account

```
// copy public key in the clipboard
clip < ~/.ssh/id_ed25519.pub
```

then open your GitHub account and Go to Settings > SSH and GPG keys click on add a new SSH key paste your key into key section add a title to know which device this key is used from.

we are Done here we have succesfully connected our laptop git bash to GitHub remote account. so when ever we push code to the remote account we don't need to authenticate ourself.

### How to add Repo on GitHub?

there are two ways you can add your Repo.

1. How to add existing Repo which you have in your Local Machine which already has (commit History).

> step:1 Create a new repo on Github.

> step:2 Tell your Local Repo about Newly created GitHub repo (with Remotes).

now we need to tell Git Repository that i want to connect my git repo to a remote Repo which lives on cloud. you can connect as many remote repo as you want so that you can push your repo at multiple place.

> Remotes

Remote here refer to a remote address for a Repo which lives in the Cloud. which is hosted on a remote server. you need to add that address to your Git Repository to tell when ever i push my git repo i want you to refer to this remote repo.

Remotes are used to facilitate collaboration and sharing of code between different developers and repositories.

A Git remote typically includes the following information:

URL: The URL of the remote repository. This is the address that Git uses to communicate with the remote server.

Name: A short, easy-to-remember name that you assign to the remote repository URL. so that you don't need to type out the entire Remote Repo URL every time you push your code. Common names include "origin" (by default), "upstream," "fork," etc.

1. checkout a list of all the remotes address.

```
git remote -v
```

2. get a detailed info about a specific Remote

```
git remote show <remote-name>
```

2. how to add a new remote with the given name for easy reference and URL to your repository.

```
git remote add <name> <url>
```

you will find the remote repo URL from Github when you create a new repo on github it will give you a repo URL which you can use to conntect your Git repo to GitHub repo.

3. how to renames an existing remote.

```
git remote rename <old-name> <new-name>
```

4. how to remove a remote from a git repository

```
git remote remove <remote-name>:
```

Now your Git repo is sucessfully connected to a remote server repo.

> step:3 Push up your changes from your local machine to Github repo.

before you push your repo on gitHub. it is recommanded by GitHub to change your master branch name to main.

```
// change branch name
git branch -m NewName
```

---

Extra: Why Git and GitHub changed there original branch name to main from master.

The computer industry's use of the terms master and slave caught everyone's attention in the summer of 2020. Amid the many protests and the growing social unrest, these harmful and antiquated terms were no longer considered appropriate.

"Both Conservancy and the Git project are aware that the initial branch name, 'master,' is offensive to some people and we empathize with those hurt by the use of that term," said the Software Freedom Conservancy.

GitHub took action based on the Conservancy's suggestion and moved away from the term master when a Git repository is initialized, "We support and encourage projects to switch to branch names that are meaningful and inclusive, and we'll be adding features to Git to make it even easier to use a different default for new projects." As a result, GitHub renamed the master branch to main branch.

later in Git version 2.28 (released 27th July 2020) Git also realise new command so that you can configure the default branch name in git so you don't have to change git original branch name every time you create a new repo.

```
git config -- global init.defaultBranch "main"
```

---

> Now how to push changes to Github repo. after configuring the remote repo.

```
git push <remoteName> <branchName>
```

git Push is a git command Not a github specific command.

git Pushes your local Branch (with commit history of that branch) to the specified branch on the remote repository.

when you specify git push <remoteName> <branchName> this way. branchName that you push from local and the branchName that is created on the remote repo are same BranchName. same branchName is used from the local git repo as the BranchName that is created on the remote server.

Note: it is not require to be on the same branch to push it to the remote repo. you can be on any branch.

you can tell a different BranchName for the remote repo if you want but it is not recommanded generally.

```
git push <remoteName> <localBranchName>:<remoteBranchName>
```

when you do git commit in your local repo. it does not push the changes to the remote repo automatically. you have to push it manually to the remote repo.

To be fair that's a good protectection layer. becouse you don't want to push your every commit to the remote repo. it will take alot of time to work on your repo becouse pushing/uploading commit to the remote server take time (depends on your internet speed).

if you are pushing a LocalBranch that doesn't exist on the remote repo (pushing for the first Time )then remote repo will create a branch with the same name as LocalBranchName. and store the repo contents and commit history.

but when you push the second time the same branch it will update the remote repo branch. it will not replace it. which is good becouse every time you added new commit. remote repo doesn't need to download the complete branch every time you push your branch. it can download only the new changes. and we are good to go.

> Now Setup upstream relationship between git repo and the github repo.

you can set up a tracking relationship between your local branch and the corresponding branch on the remote repository. This is often referred to as "setting upstream."

what does it means with tracking relationship?

generally when you push changes to the remote address you need to specify every time the remote address name and the branch you want to push to the remote address.

```
git push <remoteName> <branchName>
```

but when we setup a upstream realtionship. Git Local branch keep tracks of remote repo address name and the branchname. so you don't need to specify remote and branchname everytime you push or pull to the remote repo.

```
// after setting upstream realtion
git push
```

how to setup upstream realtaionship?

```
// seting up upstream for main branch
git push -u <remoteName> <branchName>
ex: git push -u origin main

// when you switch to a branch it will create a upstream realtionship automatically
git switch <branchName>
```

there are two ways to setup upstream relation.

1. git push -u

Note: you only need to create a upstream realtionship with main branch manually when you are working on a local repo which later need to send to the github. you don't need to setup upstream realtionship if you have a completly new project which you have created by cloneing a project from the github first before workign on the project in your local machine. becouse when you clone upstream realtion will be automatically be created for the main branch.

The -u flag (or --set-upstream) establishes a link between the local branch and the remote branch, creating a tracking relationship. This means that in the future, you can simply use git push without specifying the remote and branch names, and Git will know where to push your changes.

you just need to setup upstream relationship onces.

generally you need to create a upstream relationship for the main branch with git push -u.

for other branches you can use git switch <branchName> which will automatically create upstream relationship for us.

> how GitHub know which upstream relationship to pick when you have multiple branches?

GitHub will know which upstream relationship to use based on the branch you are currently on.

In your scenario:

```
If you are on the main branch and you run git push, Git will use the upstream relationship you set using git push -u origin main.

If you switch to the parallelBranch and run git push, Git will use the upstream relationship set using git push -u origin parallelBranch.
```

This behavior allows you to work on different branches concurrently and push changes to their respective upstream branches without explicitly specifying the remote and branch every time you push.

2. How to add New Repo from scratch. to Git and Github both.

If you haven't begun work on your local repo, you can...

> Step:1 create a new Repo on GitHub

copy the repo URL address

> Step:2 Clone that Github repo To your Local Machine Git

```
git clone repoURL
```

this command will automatically create a new repo in your currently standing Directory by running git init automatically for you. it will also setup your remote address automaically for you by the default remote name origin. which you can change it later if you want.

```
// change remote name
git remote rename <old-name> <new-name>
```

everything is setup automatically for you.

> Step:3 Push up your changes to Github.

simply do some work and push that code to github

```
git push <remote> <branch>
```

and you are good to go.

### Sync your Local repo (git) with Github remote repo.

```
// GitHub

Main-branch
                        main
                          ↓
---------------------------
       commit-M1      commit-M2

=========================================================
// after cloning
// git

Main-branch
                        main
                          ↓
---------------------------
       commit-M1      commit-M2
                          ↑
                     origin/main

```

this is the setup you have cloned the gitHub repo and started working on the repo in your local machine.

```
Main-branch
                                    main
                                     ↓
--------------------------------------
       commit-M1      commit-M2    commit-M3
                          ↑
                     origin/main

```

> when local repo get ahead to github repo (sync github repo with local repo)

now you in your local machine your main is 1 commit ahead to the origin/main. you can sync github repo with the local repo with git push. and now github repo is sync with the local repo.

but what if there are multiple people working on the same github repo. and this time your local repo is behind some commit that has been added on the github by other developer?

> when local repo is behind some commit to the GitHub (sync local repo with github repo)

there are two ways to sync your local repo with github repo.

1. fetching
2. pull

> fetching (Download newly added commits but Don't merge them,place them in remote-traking-branch separatly)

```
// Before
// GitHub
                        main
                          ↓
---------------------------
       commit-M1      commit-M2
==========================================
//Git
                                    main
                                     ↓
--------------------------------------
       commit-M1      commit-M2    commit-M3
                          ↑
                     origin/main
```

now you are ahread one commit.

```
// after
// GitHub
                                                                      main
                                                                      ↓
-----------------------------------------------------------------------
       commit-M1      commit-M2    commit-N1       commit-N2      commit-N3

```

now a new developer comes in and added 3 more commit to the github repo [N1,N2,N3].

your local repo has no idea about the newly added commits.

> how to download new changes?

Note: your local repo don't give you any message about your local repo is one commit behind to the github repo. you have to manually check it out. your local repo only give you message regarding your current possition in reference to last intereaction with the github repo.

```
// download all the changes from all Branchs from the github.
git fetch <remote>

// if you are fetching from the origin and has only one remote then you can also use the shorthand
git fetch


// download all the changes from a specific branch
git fetch <remote> <branchName>
```

```
// before
//Git
                                    main
                                     ↓
--------------------------------------
       commit-M1      commit-M2    commit-M3
                          ↑
                     origin/main
=========================================================
// after fetching
// git
                                    main
                                     ↓
--------------------------------------
       commit-M1      commit-M2    commit-M3
                           \
                            \
                             -----------------------------------------
                                   commit-N1       commit-N2      commit-N3
                                                                      ↑
                                                                 origin/main
```

fetching download all the new commits that has been added in your github repo. then add them into remote-branch from the point of last intereaction with the github repo. place all of the new commit on that remote-branch. this allow us to review newly added commits first before deciding to merge them into our curretly working directory. so that there will be no conflict. with the work we are doing currently.

when you first clone a repo. your main-branch and your remote-tracking-branch both are same. and boths pointer points to the same last commit. but when you fetch. the remote-branch diverge from the same path and movie into new direction. and add all the new commit on them. this is becouse remote branch refelect the github repo. and your local repo is reflected by main. both has different code. so not to create conflict and giving you with an opportunity to review the changes fetched from the remote repository before incorporating them into your work. This helps prevent unexpected conflicts or unwanted changes from being merged directly.

> pulling (Download newly added commits and merge them to our current working directory)

```
// downloading a specific branch from the remote
git pull <remote> <branch>
```

Note: it's importent to be on the right branch when you run the command of git pull becouse it will merge the new changes to the currently standing branch.

```
// short hand for pull;
git pull
```

it will pull the same branchNamed repo from the remote that you are curently standing on and merge the newly updated commits. this shorthand work due to upstream relationship

pull = fetch + merge

```
// Before
// GitHub
                        main
                          ↓
---------------------------
       commit-M1      commit-M2
==========================================
//Git
                                    main
                                     ↓
--------------------------------------
       commit-M1      commit-M2    commit-M3
                          ↑
                     origin/main
```

now you are ahread one commit.

```
// after a newdeveloper add new commits to GitHub
// GitHub
                                                                      main
                                                                      ↓
-----------------------------------------------------------------------
       commit-M1      commit-M2    commit-N1       commit-N2      commit-N3

```

```
// before
//Git
                                    main
                                     ↓
--------------------------------------
       commit-M1      commit-M2    commit-M3
                          ↑
                     origin/main
=========================================================
// after pulling
// step: 1 fetching
// git
                                    main
                                     ↓
--------------------------------------
       commit-M1      commit-M2    commit-M3
                           \
                            \
                             -----------------------------------------
                                   commit-N1       commit-N2      commit-N3
                                                                      ↑
                                                                 origin/main
================================================================================
// after pulling
// ste: 2 merge
// git

// git
--------------------------
       commit-M1      commit-M2
                           \                                                    main
                            \                                                     ↓
                             --------------------------------------------------------
                                   commit-N1       commit-N2      commit-N3    commit-MN
                                                                                  ↑
                                                                             origin/main
```

pulling combine feching and merging into single command.

pulling download all the new commits from the gitHub and then imidiataly add those change by merging into our current working directory. which will create a conflict. and you can reslove the merge depend on the conflict you are having.

recommended tip: when you are done with your work. before you push your code to github. it is better to pull (download and merge new commits) so that there will be no conflict on the github repo.

### github README file

if you create a README file in your repo and place that file in the root of your project. github will automatically use that file to show read me content. in your repo.

ReadME file are usefull for people who visit your repo for the first time. it has detailed instructions about what your project repo is? how to use it? how to contribute to it and so on.

READMEs are Markdown files, ending with the .md extension. Markdown is a convenient syntax to generate formatted text. It's easy to pick up!

Note: when you create readme file in your root. make it capitalcase README.md

### MarkDown Syntex

you can create a new file with .md extension your VS-code editor will recoganise it as markdown file. you can see the markdown file changes by ctrl k + v . this will open a preview window for you where you can see what the end result looks like of a markdown file. when it is converted into normal HTML.

Note: you can use normal HTML too to create markdown files too but the syntex for HTML you have to deal with too much boilerplate code.

so there is a simpler sintex you can use to create your markDown files. which later automatically converted into HTML by markdown file reader.

````
Element Markdown                          Syntax

Heading                                   # H1
                                          ## H2
                                          ### H3

Bold                                      **bold text**

Italic                                    *italicized text*

Blockquote                                > blockquote

Ordered List                              1. First item
                                          2. Second item
                                          3. Third item

Unordered List                            - First item
                                          - Second item
                                          - Third item

Code                                      `code`

Horizontal Rule                           ---

Link                                      [title](https://www.example.com)

Image                                     ![alt text](image.jpg)


Extended Syntax

These elements extend the basic syntax by adding additional features. Not all
Markdown applications support these elements.

Element                                   Markdown Syntax

Table                                     | Syntax | Description |
                                          | ------ | ----------- |
                                          | Header | Title |
                                          | Paragraph | Text |

Fenced Code Block                         ```
                                          {
                                          "firstName": "John",
                                          "lastName": "Smith",
                                          "age": 25
                                          }
                                          ```

Fenced Code Block                         ```<tell the name of language the code is written in
       with color coded lines (syntex highlighting)    Ex: js,jsx,ts>
                                          {
                                          "firstName": "John",
                                          "lastName": "Smith",
                                          "age": 25
                                          }
                                          ```

Footnote                                  Here's a sentence with a footnote. [^1]
                                          [^1]: This is the footnote.

Heading                                   ID ### My Great Heading {#custom-id}

Definition                                List term
                                          : definition

Strikethrough                             ∼∼The world is flat.∼∼

Task List                                 - [x] Write the press release
                                          - [ ] Update the website
                                          - [ ] Contact the media
````

you can visit: https://markdown-it.github.io/ for better markdown syntex understanding.

markdown file Automatically convert a URL to a link and it's quite annoying sometimes. you don't want the URL to be a link you just want it to be a string type.

you can solve this issue by adding <span><span> tags

```
 https://<span></span>markdown-it.github.io/
```

this will automatically break the flow of url and don't let it automatically register as link.

you can also add empity space with &nbsp;

### gitHub gist

Github Gists are a simple way to share code snippets and useful fragments of code with others. gist are like mini-repo. it contain the code snippet (as repo) and it's commit history. Gists are much easier to create, generally used to collect good piece of code that you like and want to keep them as refrence, or want to discuss a piece of code with other collaburator to fix a problem.

GitHub Gist is a feature of GitHub that allows users to share and collaborate on small code snippets, text, or other pieces of content. Gists are like mini-repositories where you can store and share code snippets, notes, configurations, and more. Each Gist has its own URL, making it easy to share with others.

> Key features of GitHub Gist include:

- Code Sharing: Gists are commonly used to share code snippets or small pieces of code. You can create Gists for various programming languages and technologies.

- Version Control: Each Gist has version history, allowing you to track changes over time. You can view the revisions made to a Gist and revert to previous versions.

Collaboration: You can invite other GitHub users to collaborate on a Gist by allowing them to edit or comment on it. This makes it useful for collaborative coding or reviewing code together.

- Embedding: Gists can be easily embedded into websites, blog posts, or documentation. This allows you to showcase code examples in an interactive and readable format.

- Public and Private Gists: Gists can be public or private. Public Gists are visible to everyone and can be found by search engines, while private Gists are only visible to you and users you explicitly share them with.

- Star and Fork: You can "star" Gists that you find interesting or useful. You can also "fork" Gists, creating your own copy that you can modify and work on separately.

- Categories and Tags: You can categorize Gists using tags to make them easier to discover and organize.

GitHub Gist provides a simple way to share and collaborate on code snippets without the need for a full-fledged repository. It's especially useful for quickly sharing code examples, troubleshooting issues, and collaborating on small projects or concepts

### GitHub Pages

GitHub Pages is a web hosting service provided by GitHub that allows you to publish and share web content directly from your GitHub repository. It's a powerful and convenient way to create static websites, blogs, documentation, and more using your GitHub repositories as the source for your content.

Key features of GitHub Pages include:

- Static Website Hosting: GitHub Pages hosts static websites, which means you can publish HTML, CSS, JavaScript, images, and other web assets. It's suitable for websites that don't require server-side processing.

- Automatic Deployment: GitHub Pages automatically builds and deploys your website whenever you push changes to the repository. This eliminates the need for manual uploading or syncing of files.

- Custom Domains: You can use a custom domain name (e.g., www.example.com) for your GitHub Pages website. This is useful for branding and making your site accessible through a memorable URL.

- Project Sites and User/Organization Sites: GitHub Pages can be set up at both the user or organization level (username.github.io) and at the project repository level (username.github.io/repository-name). This allows you to host websites related to your projects or personal/organizational pages.

- Free Hosting: GitHub Pages hosting is free, making it a cost-effective solution for hosting personal websites, project documentation, portfolio sites, and more.

> Getting started with GitHub Pages is straightforward:

1. Create a new repository or navigate to an existing repository on GitHub.
2. go to setting > Pages
3. select your source : where you have to select a branch which is used as a folder which contain all the nesesory files for a web page.(at least index.html is required)
4. press save and you are Done.

you can also provide your custom domain name if you want.

GitHub Pages is a great way to showcase your work, create Personal Portfolio, create documentation, share projects, and build a web presence using the tools and repositories you're already familiar with on GitHub.

### collaburative workflows??

there are common patterns which people use to work in teams.

1. centralized Workflow

Everyone Works On Main Branch. and there is only one branch.

it's simple but coz alot of overhead for all the collaborator developers.

here are some common problems:

- **Merge Conflicts:** When multiple developers are working on the same branch, pushing changes simultaneously can lead to merge conflicts. This occurs when the changes made by one developer overlap with the changes made by another developer. Resolving these conflicts can be time-consuming and disrupt the development process.

- **Frequent Merge Conflicts:** the continuous merging of changes can lead to frequent and unnecessary merge conflicts, affecting developers' productivity and causing frustration.

- **Code Quality and Stability:** If a developer pushes incomplete or unstable code to the shared branch, it can lead to issues for everyone else working on the project. It can potentially break the codebase and cause delays in development.

- **Risk of Data Loss:** In a centralized workflow, there's a risk that accidental or malicious actions can lead to data loss. If someone deletes the main branch or other important code, it can result in significant setbacks and loss of work.

- **Lack of Isolation:** Working on the same branch can limit developers' ability to work independently and experiment with new features without affecting the main codebase.

2. Feature branching

In feature branching developers create separate branches for each new feature or task they are working on.

here are common features of feature branching:

- **Feature Branching:**

  - Developers create separate branches for each new feature or task they're working on.
  - These feature branches are isolated from the main codebase and allow developers to work on their changes independently.

- **Main Branch as Production:**

  - The main branch, often referred to as the `master` or `develop` branch, represents the production-ready codebase.
  - Feature branches are created based on this main branch.

- **Merging and Review:**

  - Once a developer completes work on their feature branch, the changes are reviewed before they're integrated into the main branch.
  - This review process ensures that the code meets quality standards and doesn't introduce errors.

- **Code Conflict Prevention:**

  - Since developers work on separate feature branches, the risk of immediate code conflicts is minimized.
  - Developers can freely make changes within their own branches without affecting others' work.

- **Controlled Integration:**

  - Instead of every developer having direct permission to merge code into the main branch, a trusted individual or team can oversee the integration process.
  - This control helps maintain the stability and quality of the main branch.

- **Risk Mitigation:**
  - By reviewing and testing changes in feature branches, the risk of introducing bugs or breaking the main codebase is reduced.
  - Feature branches allow for thorough testing without affecting the main code.

### What is Forking??

In GitHub, a fork refers to the action of creating a personal copy of another user's repository (or a public repository) in your own GitHub account. This copy is completely independent of the original repository, allowing you to make changes, experiment, and contribute to the codebase without directly affecting the original repository.

when you fork a repo, entire repo get's copied to your own account with all the files and all the commit history. there is no upstream realtionship. you get a copy upto the last commit, till that time. No future commits will be updated automaitcally in your copied repo. you have to manually pull or fetch new changes from the original-repo to your copied-repo.

Here's how the process of forking works on GitHub:

1. **Navigate to the Repository:** Go to the GitHub page of the repository you want to fork.

2. **Fork the Repository:** On the top right corner of the repository page, there's a "Fork" button. Clicking this button initiates the process of creating a copy of the repository in your GitHub account.

3. **Choose the Destination:** When you click the "Fork" button, GitHub will prompt you to choose where you want to fork the repository. You can select your personal user account or any organizations you are part of.

4. **Forking Process:** GitHub will create a complete copy of the repository, including all its files, branches, commits, and history, under your account. You'll be automatically redirected to the forked repository's page.

5. **Work in Your Fork:** You can now make changes, create branches, add commits, and push them to your forked repository. This is a safe space to experiment without affecting the original repository or its contributors.

you generally make a new branch to try out your own new fetatures. adn later commit and push it to your own copy-repo.

6. **Keeping Up-to-Date:** Over time, as the original repository gets updated, you might want to incorporate those changes into your fork. This can be done by adding the original repository as a remote and pulling in changes. This ensures that your fork remains in sync with the original repository.

when you fork a repo. you are making a copy of the original repo. but as time passes its quite possible that the original repo get's updated as time passes. but those changes are not reflected in your copied-repo. becouse there is no connection no link between your copied-repo and original-repo. once you copied it now it's in your own independent environment to experiment. you can do what ever you want.

so, to be uptodate with the newly added commits to the original-repo. you can added that origina-repo in your local-machine as remote generally named as upstream. becouse you are using it for pulling new changes in your copied repo (in your local machine). you can't push changes becouse you don't have permision to push in the original repo.

7. **Contributing Back:** If you've made changes or improvements in your fork that you think could benefit the original repository, you can submit a pull request. This is essentially a request to the original repository's maintainers to consider merging your changes into their codebase.

Forking is a key feature of GitHub and other Git-based version control systems. It promotes collaboration, open-source contributions, and allows developers to work on projects without requiring direct access to the original repository. It's often used to contribute to open-source projects, experiment with code, and create personalized versions of existing projects.

### What is Pull Request (PR)

- it is a request to the owner of the repository to merge your branch to the original repo.

when we use git pull origin. we fetch + merge changes to the working directory but in our local machine. in pull request we want the owner of the repository to fetch our branch and merge our branch to there working directory.

A pull request (often abbreviated as PR) is a feature commonly used in GitHub to propose and manage changes to a codebase or repository that we do not have access to. It serves as a way for developers to collaborate, review, and discuss code changes before they are merged into the main codebase.

Here's how the process typically works:

1. Forking: A developer who wants to contribute to a repository first creates a personal copy (fork) of the repository. This allows them to work on changes without directly affecting the original repository.

2. Creating a Branch: The developer creates a new branch in their forked repository. This branch is dedicated to the specific changes they intend to make. This helps isolate their work from the main codebase.

3. Making Changes: The developer makes the necessary code changes, additions, or deletions in the branch. make a new branch and add new changes.

4. Committing Changes: As the developer works on the changes, they commit their changes to the branch. Each commit represents a logical step in the development process.

5. Pushing to Fork: After the developer is satisfied with the changes, they push the branch with its commits to their forked repository on the version control platform.

commit changes and push them into your copied repo. there you will see a message that say now your copied-repo is some commit ahead to the original-repo.

6. Creating a Pull Request: Once the branch is pushed to the forked repository, the developer initiates a pull request from that branch to the original repository. The pull request serves as a request to merge the changes into the original repository's main branch.

pull request will open a new window which has similar to github commit interface. it will as you where you want to push the changes to (main branch) and what you want to push (feature branch). ask you for a message and a description for pull request. and submit.

7. Code Review: Other developers and collaborators can review the code changes, leave comments, suggest improvements, and ask questions within the context of the pull request. This collaborative process helps ensure code quality, readability, and correctness.

8. Discussion and Iteration: The developer who initiated the pull request can respond to comments, make additional commits to address feedback, and refine their changes based on the feedback received during the review process.

9. Merging: Once the changes have been reviewed, approved, and any necessary adjustments have been made, a repository maintainer or someone with the necessary permissions can merge the pull request. This incorporates the proposed changes into the main branch of the original repository.

if you are the owner/maintainer of the original repo and want to merge the changes you can merge the changes on github but it is recommanded to open the request in local machine in vs-code so that if merge conflict arises you can handle it. you can click on view on command line instruction.

it will show you some basic command to open this branch and merge it.

Ex:

```
git fetch origin // download all new changes with all the branchs
git switch my-new-feature  // go to the branch
git merge main  //fix conflicts! // merge that branch with main
git push origin main // update the github repo
```

pull request will be automatically updated to merged and you can close the request.

10. Closing the Pull Request: After the changes are merged, the pull request is typically closed. Depending on the platform and workflow, it might be marked as merged or closed without merging.

Pull requests are a fundamental aspect of collaborative software development, enabling teams to maintain a controlled and organized approach to integrating changes, fostering collaboration, and ensuring the quality of the codebase.

## How to contribute to opensource project.

1. FORK THE PROJECT // copy the original repo in your github account
2. CLONE THE FORK // clone it to your local machine
3. ADD UPSTREAM REMOTE // set a upstream remote original repo
4. DO SOME WORK // do work on your copied repo
5. PUSH TO ORIGIN // push the updated code to your copied repo
6. OPEN PR // raise a PR to original repo

### Rebase

Rebase hsa two features:

1. as an alternative to merge (Read Rebase vs merge)
2. as history Cleanup tool (Read Intereactive Rebase)

## Rebase vs merge

let's understand what's the problem with simple merge.

let's say there is a team of 10 developer working on same project. you are one of them and working on a feature branch. as development procedes other developer complete there work on there branch and there branches will be merged into the main branch. the process of merging branchs to the main branch happens maltiple times in a day depending on the size of the team.

so you are working on feature branch and endup needing a feature of someother developer. to complete your feature developement. as other developers completes there branches work there work will be merged to the main branch and you will later merge "main" branch into your "feature" branch so that you can take advantage of other developer work features and incorporate that into your feature-branch.

as you are working on a feature that constantly needs other developer work to complete your feature you will do a lot of merges (merging main branch into your feature branch).

adding merges again and again will clutter your commit history makes it muddier (dirty).

```
// it will looks like this
// commit-1: "created feature file"
// commit-2: "added navbar"
// commit-3: "merged main-to-feature"
// commit-4: "added hero"
// commit-5: "added aside"
// commit-6: "merged main-to-feature"
// commit-7: "added section-1"
// commit-8: "merged main-to-feature"
```

it's hard to look in your commit history and tell what work have you done with these merged commit disturbing the flow of commits.

```
                                          Head
                                           ↓
                                          Main
                                           ↓
--------------------------------------------
      commit-M1        commit-M2       commit-M3
                            \
                             \
                              -----------------------
                                    commit-F1   commit-F2
                                                    ↑
                                             Feature-Branch
=============================================================================================
// git merge
                                          Head
                                           ↓
                                          Main
                                           ↓
-----------------------------------------------
      commit-M1        commit-M2       commit-M3
                            \                    \
                             \                    \
                              \                    \
                                ----------------------
                            commit-F1   commit-F2    commit-FM
                                                        ↑
                                                 Feature-Branch


```

> git rebase

git rebase solve this commit history clutter problem by "Re-writing the commit history" when you rebase. it removes all of the commit histroy of that branch (the branch you are merging into Ex:feature branch). and "reset-base" of that branch and re-write all of the commit histroy from the new-base.

```
                                          Head
                                           ↓
                                          Main
                                           ↓
--------------------------------------------
      commit-M1        commit-M2       commit-M3
                            \
                           ↑ \
                           ↑   -----------------------
                           ↑         commit-F1   commit-F2
                           ↑                         ↑
                          BASE                  Feature-Branch
```

current base (the stating point where the feature-branch starts to diverge from the main branch). is at commit-M2. rebase shift the base of (feature-branch) to the Tip of the main branch (which endup adding all the changes of the main branch to the feature branch) and re-commit all of the commit that you have added in your feature branch. there re-commit are completly new commit but done automatically.

```
git rebase
                                          Head
                                           ↓
                                          Main
                                           ↓
--------------------------------------------
      commit-M1        commit-M2       commit-M3
                                           \
                                          ↑ \
                                          ↑  -----------------------
                                          ↑        commit-F1   commit-F2
                                          ↑                        ↑
                                        BASE                 Feature-Branch
```

this is modify the original commit history. but made your dirty commit history much clearner.

```
// it will looks like this
// commit-1: "created feature file"
// commit-2: "added navbar"
// commit-3: "added hero"
// commit-4: "added aside"
// commit-5: "added section-1"
```

you can use git merge to merge all of the changes again and again. and at end of the day when you decide i have done enough work you can run git rebase "BranchName" and it will re-write the history of your feature branch. or you can use rebase instend of merge on every merge you want to do. both works fine.

rebase is a great tool. it helps to group together all of your feature-branch commit in the commit history by re-writing the history and removing all the merge-clutter.

Note: there will be no merge commit in the commit history of the feature branch. it will feel like you have done fast forward merge.

> lets understand the syntex of rebase for merging.

```
git switch branch  // first switch to the branch you want to merge into (fetaure-branch)
git rebase "mergeToBranchName" // tell which branch you want to merge (main-branch)
```

> when merge conflcit arises

at the time of merge-conflict you will see an error meessage "merge conflict in XYZ file". at this point in time your rebase has paused it's mergeing process. you are in the middle of merging. half of your merge is done but later there was an conflict. you can fix the conflict and continue the merging process or you can abort the merging completly and go back to how it was before merging.

to keep mergin after fixing the conflict and adding the changes into staging area. and then continue merging.

```
git rebase --continue
```

or abort the merging and get back to where we were before we start mergeing.

```
git rebase --abort
```

> Some Golden Rules to use rebase properly.

Rebase is useful but can be dangerous at times. becouse it has the potential to re-write the commit history. it is recommended to never ever use rebase on the main branch (don't merge any feature-branch to the main-branch with rebase) becouse it will remove all the muddie-commits (cluter) of the main branch and all the developer which had cloned it earlier will have the muddie-commit history version of main branch and now main don't have any cluter. it will create problem when other developer try to merge there feature to the main-uncluter branch. becouse both have different commit history for the main branch.

Note: eventhough it is possible to fix this problem but it's really dificult task becouse the history has been tempered.

it is only safe to use rebase on feature-branchs only.becouse all other developer don't have any history of your feature-branch. so it is safe. you are working on the feature branch locally. in seprate branch.

## Intereactive Rebase

```
git rebase -i <commitHash>
git rebase -i Head~Number
```

As we all know Rebase has the capability to re-write the history of the currently standing branch. rebase intereactive mode allow us to modify the commit history of the currently standing branch and then re-write the history. this help us to clean up our commit history. Ex: combining two commits into one commit, fixing typo by re-wrtiting the commit messsage of a commit, removing a commit from the commit history and many more.

you can go back to any commit in the commit history and modify any number of commit from the commit history. rebase will start re-writing from the last commit that you have modified up to the tip of the branch. all of those commit will be removed and re-commited, completly new commits will be added
automatically with new commit hesh.

# Purpose of Interactive Rebase

The primary purpose of interactive rebase is to enable commit history cleanup, which includes tasks like:

- Combining multiple commits into one.
- Editing commit messages.
- Removing unwanted commits.
- Rearranging the order of commits.

# how intereactive mode works?

```
git rebase -i <commitHash>
git rebase -i Head~Number
```

when you run this command. you choice of editor will open up. which contain commit history upto the point you have defined by giving commitHash code.

```
Method    Commit-Hash       Commit-Message

pick      f7f3f6d           Change my name a bit         ↓    Oldest commit
pick      310154e           Update README                ↓
pick      a5f4a0d           Add cat-file                 ↓    Newest commit
```

you can decide which commits you want to modify and what kind of modification you want to do by chaning the method.

pick means keep this commit. there are many type of modification methods to choose from. you will find a detailed description of all modifer type blow in that same file. simply change the method and save the file. and close it. which ever modification method you have choosen that will be take place just after you close the file. rebase was waiting for you to tell what you want to modify. depending on the type of modification rebase will take action accordingly.

Note: which ever oldest commit you have decided to modify. all of the commits that come after that commit will be re-written. you can define multiple modifer all at once too.

# type of modification we can do?

there are ton of modification methods avalilable in rebase but here are some most commonly used methods:

- pick - use the commit. keep the commit has it was. (default)
- reword - use the commit, but edit the commit message.
- edit - use commit, but stop for amending. you can edit code.
- fixup - use to combine a commit into it's previous commit all of the changes that you have made in the new commit will be added into previous commit and that commit will be removed.
- drop - remove commit.

# Workflow

1. Specify the modifications you want for each commit in the editor.
2. Save and close the editor.
3. rebase applies the specified changes sequentially, effectively rewriting the history.

# How to use rebase properly

you should not use rebase in main branch. which means don't use rebase when other developer of the teams has the shared history. changing the history might create un-nessesery merge conflict.

only use rebase on local machine on a feature branch before shareing the histroy with other. use it to cleanup your work before you decide to share your work with other developer.

### Tags

Git tag allow us to mark/add a Note/label/tag a specific commit in the commit history of the repo. generally tags are used for marking or highlighting an importent point in commit history like versions release of the project.(Ex: v8.0.2)

there are two type of tags:

1. lightweight tags : tags that contain only a label or name.
2. annotated tags : tag label + more details like meta data (like author name , email), tag description message like a comment.

# tag sintex and it's useage.

> To view list of all the tags in your repo.

```
git tag               // l is implicit here
git tag -l
```

> To filter some tags from the list

```
git tags -l "pattern"
Ex: git tags -l "*Tag*"
```

you can use any wild card in the pattern to filter out your tags. in this example i have use * which refer to any number of character. so *tag\* means any number of charactor before and after the tag word.

> To checkout/visit a perticuler taged commit from the commit histroy

```
git checkout <tag>
```

you can visit a tag with checkout by giving it a tag name. it will detach the head and you can visit that specific commit from the commit history. just like git commit <commitHashCode> or git commit <Head~Number>.

> To check the different between two different Tags commit

```
git diff <Tag1> <Tag2>
```

> create Tags

> > add tag to the currently standing commit

1. lightWeight tag

```
git tag <tagName>
Ex: git tag v17.0.1
```

2. annotated tags

```
git tag -a <tagName>
```

this will add a label + openUp your VS_Code where you can type any type of additional info you want like a description or additonal meta data if you like.

> > add tag to older commit from the commit histroy.

```
git tag <tagName> <commitHash>         // for lightweight
git tag -a <tagName> <commitHash>      // for anotated
```

> > Force Move a tag.

```
git tag -f <tagName>
```

this will remove the tag from the original tagged commit and move it to the specified tagName commit.

> to see detailed description of a taged commit

```
git show <tagName>
```

give a detailed description of the tager. like who tagged it (author), email of the tager, date and time of the tagging, annotated taged message if any etc.

> delete a tag

```
git tag -d <tagname>
```

# pushing tags

when you push your changes to your remote repository. tags are not included by deafult in your pushed changes. you have to manually tell to push your tags seprataly

> push all the tags (push only newly added tag)

```
git push <repoName> <branchName> --tags
git push --tags                               // with upstream realtionship
```

it will push all the tags from the local machine to the remote repo. if remote repo has some of the tags you are sending then it will only send the missing tags that the repo doesn't have.

so on consecutive push only the newly added tags will be pushed.

> push a specific tag (push only one specific tag)

```
git push <repoName> <tagname>
```

it will only push a specific tag to the remote repo. it will not push your commit changes only the tag

### How to restore a file data that you have excidently deleted but you have the commitHash of that ?

- to print and check the content that git have stored at a perticuler hash

```
git cat-file -p <commitHash>
```

- to retrive that content from the git and store it into a file.

```
git cat-file -p <commitHash> > fileName.extention
Ex: git cat-file -p <commitHash> > Index.html
```

### reflog

when we use git log it shows the commit history of a repository. it shows all the commits that has oocered while working on this repository.

```
commit c4e4e62f735ef23452367e6fa27f5678298a19e3
Author: John Doe <john@example.com>
Date:   Tue Aug 10 12:30:00 2023 +0200

    Add feature X functionality

commit 7f75e902e3bc091c20d918f41c8d9826ca65d6e9
Author: Alice Johnson <alice@example.com>
Date:   Sun Aug 08 09:15:00 2023 +0200

    Update documentation for new API changes

commit d6f6b3f0a89324475e1c5e8d19b1d92a74f201aa
Author: John Doe <john@example.com>
Date:   Thu Aug 05 16:00:00 2023 +0200

    Initial project setup: Add README and LICENSE

```

on the other hand git reflog command provides a comprehensive record of all the moves/update your HEAD Pointer go through while working in the repo. It maintains a reference log of Head Pointer which includes changes to branches (switching between branches) and tags, commit amendments, rebase operations, and other reference updates. Essentially, every type of update you Head Pointer go through within the repository is recorded in the reflog.

git reflog keeps a recod of every update your Head pointer go through in the repo. so in simple terms every time you do anything and your head updates. reflog recod that change.

```
commithash    refpoint:  update/action you have performed

a1b2c3d       HEAD@{0}: commit: Fixed a critical bug
e4f5g6h       HEAD@{1}: rebase finished: updating HEAD
i7j8k9l       HEAD@{2}: pull origin main: Fast-forward
m0n1o2p       HEAD@{3}: commit: Added new feature
```

> Note:

Limitations! Git only keeps reflogs of your local activity. They are not shared with collaborators and not pushed to github nor you pull other people reflog its totally a local thing. reflog are tracked by git in your local machine and it is only for local reference. Reflogs also expires after sometime. Git cleans out old entries after around 90 days, though this can be configured.

> reflog syntex:

```
git reflog                    // show head is implisit by default
git reflog show Head          // you can define (branch, refpoint) inplace of head
ex: git reflog show main
ex: git reflog show HEAD@{3}
```

> reflog refrence point

```
name@{qualifer}

// name can be any branch or simple head
// qualifer can be Number which act has back button
// qualifer can also be Time refernce
```

Reflog References We can access specific git refs is name@{qualifier}. We can use this syntax to access specific ref pointers and can pass them to other commands including checkout, reset, and merge.

> qualifer as Timed References

Every entry in the reference logs has a timestamp associated with it. We can filter reflogs entries by time/date by using time qualifiers like:

```
1.day.ago
3.minutes.ago
yesterday
Fri, 12 Feb 2021 14:06:21 -0800
```

```
Ex: git reflog show manin@{yesterday}
```

> use reflog to recover code that you have lost

you can recover lost code when you screw-up your code. screw-up like use the rebase which rewrite the commit history (commit history has been tempered with but reflog is still un-touched), or you have deleted at a perticuler commit while useing intereactive rebase, or you have removed some file during a perticuler commit.

you can recover all that by simply checking reflog and pick a commithash point in the history of reflog. commithash you want to go back to and fix anything that you have screwup. you can undo at that perticuler commit with git reset --hard <commithash> it will reset the commit histroy as if that recoved commit is the current head + recover the file code too.

```
git reset --hard <commitHash>
git reset --hard name@{qualifer}
```
