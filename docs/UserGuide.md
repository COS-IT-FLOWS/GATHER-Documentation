# Working with GIT and GITHUB

## Introduction

[Git](https://git-scm.com) is a version control tool that we use to:

* track changes over time in the Gather source code,
* facilitate parallel development between multiple system developers and research projects,
* encourage peer code review as well as issue and bug tracking

[Github](https://github.com/) is a web-based [Git](https://git-scm.com) repository hosting service. We use [Github](https://github.com/) for:

* collaboration and discussion on Gather development issues,
* code peer review,
* bug and issue tracking, and
* distribution of official and bleeding edge releases of the model source code.

### Gather for Non-Developers

In general, if you are only going to be using the system and not working directly on the source code, you can just download the working system from Gather github page. <!-- Additional archived tags and releases (including all previous versions) are available here.-->

### Gather for Developers

If you plan on contributing to developing the system or would like a structured way to incorporate updates to the Gather source code, we encourage you to use Git. The following sections are designed to get you started using git and working with the Gather source code repository.

### GIT Resources

If you are not familiar with Git yet, we encourage you to get acquainted with it before you start working with the Gather source code. A few minutes of learning about GIT will go along way in helping you manage your code development.

There are a number of good GIT learning resources that will provide a basic introduction to the version control system.

* <http://git-scm.com/about>
* <https://help.github.com/>

## Getting the Code

To obtain the Gather source code repository, follow the steps in this tutorial:
<https://help.github.com/articles/fork-a-repo>

### Step 0: A few definitions

Using Git involves a few different copies of the archive:

1. **Truth Repo**: this is the master copy of the archive, maintained on the GitHub server by the Gather Administrator.

2. **Your Fork of the Repo**: this is your version of the archive, maintained on the GitHub server by you. This is generally not where you edit code; it is more of a transfer point between your clone (see below) and the truth repo. Git keeps track of the differences between your fork and the truth repo. To move code changes from your fork to the truth repo, you must make a "pull request" for the Gather administrator, who will do the actual pull.

3. **Your Clone(s) of your Fork**: a clone is a copy of your fork, stored on your local machine, where you can edit and test the code. You can have one or more clones. Each clone is a snapshot of a particular version of the code in your fork (e.g., you select which branches, and which versions of the code on those branches, to copy to your clone). You can change which version of the code each of your clones points to at any time. Your clone is where you edit files and commit changes. You can then push code changes from your clone back up to your fork.

### Step 1: Fork the repo

If you do not have a github account, create one. It's free for public repositories. On the [COS-IT-FLOWS/Gather](https://github.com/COS-IT-FLOWS/Gather) page, click "Fork" in the upper right hand corner.

### Step 2: Clone your fork

You've successfully forked the Gather repository, but so far it only exists on GitHub. To be able to work on the project, you will need to clone it to your local machine.

Run the following code on your local machine:

        git clone <https://github.com/username/Gather.git>

This clones your fork of the repository into the current directory. The clone will live in a directory called "Gather".

### Step 3: Configure remotes

When a repository is cloned, it has a default remote called `origin` that points to **your** fork on GitHub, not the original repository it was forked from. To keep track of the original repository, you need to add another remote. You can name this anything you want, but the name `upstream` is descriptive and an informal convention.

Change the active directory in the prompt to the newly cloned "Gather" directory:

        cd Gather

Assign the original repository to a remote tracking branch called "upstream":

        git remote add --tracking upstream <https://github.com/COS-IT-FLOWS/Gather.git>

### Step 4. Sync up your clone with the truth repo

#### 4.a. Fetch information from the truth repo

Before starting to edit the code, pull in any new changes to the truth repo that have been made by other people since you first created the clone.

        git fetch upstream

If you have already made changes to the code, this command by itself will not overwrite your files. For updates from the truth repo to show up in your files, you must do a **merge**.

#### 4.b. Merge changes

Determine which branches you will need to work with. At the very least, this will include the master branch. If you are working on a hotfix or a feature branch that already exists, you will need this branch as well; the Gather administrator has likely given you the name of the appropriate branch to use. Alternatively, you may want to create a new branch (e.g., if you are the first person to work on a new feature or bug fix).

For each branch, merge any changes from the truth repo into your local version:

        git checkout branchname

        git merge upstream/branchname

where branchname = name of the branch

### Working with the code

#### Making changes

1. **Select a branch**

    Change your active branch to the desired branch:

        git checkout branchname

    where "branchname" is the name of the branch.

2. **Make changes**

    You can edit the code using any editor or development environment you prefer. You can also create new files, and move, rename, or delete existing files. You will not be able to push these changes to your fork until you **commit** them.

It is a good idea to **compile and test** your changes on your local machine before you commit them. This avoids extra commits to fix typos, etc.

At any point during the process of changing the code, you can pull in any changes that other people have made via the fetch/merge procedure described above.

#### Committing changes

Before committing your changes, remove any extraneous files that have been created during compiling and testing (e.g., *.o files, executables, .depends files, etc.). An easy way to do this is to type `make clean` in Gather's "src" subdirectory.

1. **Register your changes for commit**

    To register all edited and new files:

    `git add *`

    To register the changes to (or creation of) a specific file:

    `git add filename`

    To register moving or renaming any files:

    `git mv oldpath/oldfilename newpath/newfilename`

    To register the deletion of a file:

    `git rm filename`

2. **Commit the changes**

    `git commit`

    This will bring up a commit log in your default editor. The list of files whose changes will be committed (i.e. were registered via "git add" etc) is shown in the header at the top of the file. If you disagree with this list, exit the editor and do "git add" etc as necessary to correct the list, and then try "git commit" again. If satisfied with the list of changed files, add a description of the set of changes (including a brief description of the problem that motivated the changes). Save and exit.

3. **Correcting code syntax issues prior to creating a PR**

    After completing your code commits, you also need to check that your code is compliant with Gather style conventions for indentation and spacing. Please make sure you have uncrustify installed (version 0.64 or later). Once you have installed uncrustify, you should navigate to the ...Gather/tools/code_format directory in the Gather repo and run uncrustify, which you can do by running the script in that directory, ./run_uncrustify.bash. A simple git diff --name-only will list all files that uncrustify has updated the syntax for. If any files come up that you have edited, you should commit those syntax edits.

#### Pushing commits to your fork

After committing your changes, you should push them to your fork (which has the alias `origin`) stored on GitHub:

        git push origin branchname

where "branchname" is the name of the branch where you made the commits.

#### Making a pull request

To make your changes visible other users/developers, your changes must be incorporated into the truth repo. To do this, you must create a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) on GitHub.

The Gather administrator and other developers will examine your pull request and decide if/how they want to incorporate your changes into the code.

## Github Issues

We use Github Issues to track bugs and model development in the Gather source code. When issuing a Github Issue for a bug, it is import to include the following information:

* Version of Gather that you are using (e.g. Gather.4.2.b)
* Operating system
* A description of relevant system configuration
* A summary of the bug or error message you are getting

When proposing a new feature, be specific about the functionality you are would like to see in Gather. Remember that Gather is an open source model with no official support for development, so be prepared to contribute to the implementation of your feature request. Also note that features that are only of interest to you are unlikely to be implemented in the main source code repository (although you are of course free to modify the code in any way you see fit).

## Git Workflow

In order for us to leverage Git to its full potential, we have implemented a Git-oriented workflow. This requires developers to adhere to a few rules regarding branch names and merge requests. A full description of the workflow we use can be found [here](./Workflow.md).
