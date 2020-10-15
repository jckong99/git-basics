# Git Basics

A Git tutorial intended to introduce UMD Game Developers Club members to version control and the general Git workflow.

Written with Windows, Git Bash, and GitHub in mind, but many of the fundamentals are transferrable to Mac and Linux devices as well as other Git-using services like GitLab.

Git is available for download here: https://git-scm.com/downloads


## Motivation: Why Use Git?

- You can take snapshots of your work as you make progress (*commits*)
- You can protect functional iterations of project by keeping new additions separate (*branches*)
- **You can collaborate with others on code in a controlled, organized manner** (*remote repositories*, *fetch/merge/pull*)


## Global Configuration

Set local values for recording you as the author of commits and other changes.

- Username:

```
git config --global user.name "Your Name"
```

- Email:

```
git config --global user.email "name@example.com"
```


## Getting Started with a Local Repository

### Repository Initialization

Projects are tracked in *repositories*---databases of references to files you care about. To initialize your current directory as a Git repository, use

```
git init
```

Note the creation of a `.git` file. If you can't see it, make sure you do one of the following, depending on how you're viewing your files:

- Set hidden items to be visible in File Explorer
- Use the `/a` option for `dir` in cmd
- Use the `-a` option for `ls` in Git Bash

If you don't already have a project you'd like to use with Git, and/or you'd like to create a new one, specify a name after the `init` command:

```
git init repo
```

This will create a new folder named "repo", initialized with a `.git` file.


### Staging and Committing

After major iterations of adding, deleting, or modifying files---really, anytime you just want to save your progress---it's time to *commit* your changes, or store a snapshot of the current state of your project.

You can always check which files have changed since the last commit, as well as existing commits and which files are tracked, with

```
git status
```

Before committing can be done, you must specify which files' changes you'd like to include in the snapshot. This can be done with

```
git add path/to/your/file
```

This process of `add`ing files in preparation for a commit is called *staging*. Typing `git status` at this point should show what files are currently staged for committing.

To finally perform the commit itself, we type

```
git commit
```

Every commit needs a concise yet descriptive message to go alongside it, which is why `git commit` opens the text editor you associated with Git. Add a message and exit.

Alternatively, you can include the message at the same time as the command and bypass opening a text editor with

```
git commit -m "This is my commit message"
```


### Using a Remote Repository

Everything is currently saved locally. If you want to save changes to some remote host (e.g., GitHub), use

```
git remote add remote-name url
```

which will often resemble something like this:

```
git remote add origin https://github.com/user/repo.git
```

From this point onward, you can sync the local and remote versions of your repository with `pull` and `push`.

```
git push
```

fetches changes from the remote repository and attempts to merge them with your local version---i.e., copy local to remote.

```
git pull
```

attempts to merge your local repository with the remote version---i.e., copy remote to local.

By not specifying anything after `push` and `pull`, we're using the default remote name of "origin".


## Getting Started with a Remote Repository

If there's a repository that already exists on some remote host you want, you can make a local copy with

```
git clone url
```

which, when using HTTPS (as opposed to SSH), will often resemble something like this:

```
git clone https://github.com/user/repo.git
```

From this point onward, more or less all operations work the same as for a local repository.


## Working with Branches

All repositories start with a single definitive version of their projects: a *branch* called `master`.

`master` ideally represents the current most functional, stable build of your project.

If you want to contribute to a project and, say, add a new feature, it is safest to create a new branch off of `master` (or another branch), make your additions, and then *merge* your changes back into the original branch.

The easiest way to make a new branch and then immediately switch to it is to use

```
git switch -c new-branch-name
```

This is equivalent to typing

```
git branch new-branch-name
```

to create the branch, followed by

```
git switch new-branch-name
```

to switch to it.

If you want to push your new branch's changes to a remote repository, you will need to specify the *upstream* branch---i.e., the destination of your push, which is not automatically set when a new branch is made. You can do this when pushing with

```
git push --set-upstream remote-name branch-name
```

The new branch should then be visible on the remote host. When work on that branch is complete, and the new feature is ready to be added, it's time to create a *pull request*---a formal request for the modifications in your branch to be pulled by another. Though this can be achieved locally with `git request-pull`, I recommend using the tools provided by GitHub, or whatever your remote host is. Once the changes are pulled in and the branches are merged together, you should be free to delete your working branch.


## Excluding Extraneous Files with `.gitignore`

A game includes plenty of files that aren't code: images, audio, etc. Binaries like these and their metadata files are typically not the kinds of things you want to track with Git, especially as projects grow larger. You can specify what files you'd like Git to ignore by including a file in your repository called `.gitignore`.

Each line you write in `.gitignore` represents a pattern, and all files that match said pattern is excluded from tracking. As an example, here is GitHub's sample `.gitignore` for Unity projects:

```
# This .gitignore file should be placed at the root of your Unity project directory
#
# Get latest from https://github.com/github/gitignore/blob/master/Unity.gitignore
#
/[Ll]ibrary/
/[Tt]emp/
/[Oo]bj/
/[Bb]uild/
/[Bb]uilds/
/[Ll]ogs/
/[Uu]ser[Ss]ettings/

# MemoryCaptures can get excessive in size.
# They also could contain extremely sensitive data
/[Mm]emoryCaptures/

# Asset meta data should only be ignored when the corresponding asset is also ignored
!/[Aa]ssets/**/*.meta

# Uncomment this line if you wish to ignore the asset store tools plugin
# /[Aa]ssets/AssetStoreTools*

# Autogenerated Jetbrains Rider plugin
/[Aa]ssets/Plugins/Editor/JetBrains*

# Visual Studio cache directory
.vs/

# Gradle cache directory
.gradle/

# Autogenerated VS/MD/Consulo solution and project files
ExportedObj/
.consulo/
*.csproj
*.unityproj
*.sln
*.suo
*.tmp
*.user
*.userprefs
*.pidb
*.booproj
*.svd
*.pdb
*.mdb
*.opendb
*.VC.db

# Unity3D generated meta files
*.pidb.meta
*.pdb.meta
*.mdb.meta

# Unity3D generated file on crash reports
sysinfo.txt

# Builds
*.apk
*.aab
*.unitypackage

# Crashlytics generated file
crashlytics-build.properties

# Packed Addressables
/[Aa]ssets/[Aa]ddressable[Aa]ssets[Dd]ata/*/*.bin*

# Temporary auto-generated Android Assets
/[Aa]ssets/[Ss]treamingAssets/aa.meta
/[Aa]ssets/[Ss]treamingAssets/aa/*
```

Check out https://git-scm.com/docs/gitignore for an official explanation of pattern formats.
