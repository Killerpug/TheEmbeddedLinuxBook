# Getting started
## Setting up a repository
There are many ways to install git for windows, and many GUIs... but the most basic is to download git directly from Git website: https://git-scm.com/download/win. There is a first-time-only setup(git config) that allows to store your custom configuration variables and profile(user, email) for all repositories that you work on. So don't forget to run the setup commands on the cheat sheet at bottom. Now lets create or clone a repository:

- **git init** → This takes you current directory and turns it into a git repository by creating a new
subdirectory named .git that contains all the necessary repository files for git.
- **git clone <url>** → Gets a copy of an existing Git repository. Git receives a full copy of nearly
all data that the server has.

## Recording changes in a repository
Lets play!!! Try to add or modify a couple of files and store those changes in your local repository. Below is a
list of commands that will be useful:

- **git status** → determine which files are in which state(remember de lifecycle of file status)

    - -s or --short, get simplified output of the state of files
- **git add** → is a multipurpose command — you use it to begin tracking new files, to stage
files, and to do other things like marking merge-conflicted files as resolved. It may be helpfulto think of it more as “add precisely this content to the next commit”.
    - -A stages all changes stages new files and modifications, without deletions
    - -u stages modifications and deletions, without new files
- **git commit** → saves the changes made(staged files) into the local repository.
    - -m inline commit message
    - -a commit includes all modified files, letting you skip the git add
    - --amend redo that commit, make the additional changes you forgot, stage them, and commit again
- **git diff** → shows you the exact lines changed (added and removed), but not yet staged.
    - --staged or --cached. This command compares your staged changes to your last commit
- **git log** → View commit history: lists the commits made in that repository from most recent to
oldest

