# Repo

Repo is a tool built on top of Git, Repo helps manage many Git repositories, does the uploads to revision control systems, and automates parts of the development workflow. Repo is not meant to replace Git, only to make it easier to work with Git. The repo command is an executable Python script that you can put anywhere in your path.


## Repo Project

Repo use an artifactory to crate a repo project defining the folder structure and the different git repositiries and it specific version used by this project, this artifactory is called repo manifest 

![Manifest](./diagram/manifest.drawio.png#center)

A repo manifest describes the structure of a repo client; that is the directories that are visible and where they should be obtained from with git.
The basic structure of a manifest is a bare Git repository holding a single default.xml XML file in the top level directory.
Manifests are inherently version controlled, since they are kept within a Git repository. Updates to manifests are automatically obtained by clients during repo sync.

[See more details](https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.md)
