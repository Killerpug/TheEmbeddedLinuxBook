## Command Repo

Repo tool have a set of commands, using the below command after repo tool install you will see complete list of Repo commands

```
repo help
```
Example output 
```
The complete list of recognized repo commands is:
  +abandon        Permanently abandon a development branch
  +branch         View current topic branches
  +branches       View current topic branches
  +checkout       Checkout a branch for development
  +cherry-pick    Cherry-pick a change.
  +diff           Show changes between commit and working tree
  +diffmanifests  Manifest diff utility
  +download       Download and checkout a change
  +forall         Run a shell command in each project
  +gitc-delete    Delete a GITC Client.
  +gitc-init      Initialize a GITC Client.
  +grep           Print lines matching a pattern
  +help           Display detailed help on a command
  +info           Get info on the manifest branch, current branch or unmerged branches
  +init           Initialize a repo client checkout in the current directory
  +list           List projects and their associated directories
  +manifest       Manifest inspection utility
  +overview       Display overview of unmerged project branches
  +prune          Prune (delete) already merged topics
  +rebase         Rebase local branches on upstream branch
  +selfupdate     Update repo to the latest version
  +smartsync      Update working tree to the latest known good revision
  +stage          Stage file(s) for commit
  +start          Start a new branch for development
  +status         Show the working tree status
  +sync           Update working tree to the latest revision
  +upload         Upload changes for code review
  +version        Display the version of repo
See 'repo help <command>' for more information on a specific command.
```


## Use Repo commands


Time to create our own AGL repository 

```repo init  -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo```

Now we will see the content of this repo, so enter in manifest folder

```
cd manifests
```

