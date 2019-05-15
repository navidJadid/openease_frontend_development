# openease_webserver_development

### Table of Contents
1. [Background and purpose of this repository](#1-background-and-purpose-of-this-repository)
2. [Setting up and cloning the repository correctly](#2-setting-up-and-cloning-the-repository-correctly)
3. [Working with the repository correctly](#3-working-with-the-repository-correctly)
4. [Building and running the whole thing](#4-building-and-running-the-whole-thing)
5. [Development procedure](#5-development-procedure)

##### 1. Background and purpose of this repository
As of May 2019 the openEASE project has launched with a new project architecture which splits it into several sub-repositories. Each of the sub-repositories is a docker-module which is deployed to docker-hub and then build as a whole with docker-compose in the root repository. This kind of architecture will hopefully increase the project's maintainability and ease the development of new features for the project, both for the researchers at the University of Bremen but also externals. Yet, as a consequence of this change, some parts of the system cannot be run or tested standalone anymore. This repository aims to setup a development environment for the front-end and webserver. It contains all the submodules and instructions needed to run and develop the mentioned components.

##### 2. Setting up and cloning the repository correctly
This repository utilizes git-submodules which allows us to include and develop the dependencies of this project, without having to include their files directly into this git-repository. When cloning this project correctly (which we will explain in a minute) the submodules are cloned from their own repositories. Now when changes are made to the submodules they are only pushed to the repository of the respective module and not to this repository.

If this repository is cloned normally, then the submodules will not be cloned together with the main repository. There will be subfolders, but those will be empty. To load the repository with all submodules, either of the following two approaches will work:

1. If the repository is normally cloned, the sub directories will be included but empty as mentioned. To clone the sub-repositories run the following two commands in the root of this repository:

    ```
    git submodule init
    git submodule update
    ```

2. Alternatively add `--recurse-submodules` when cloning this repository, which should look like this:

    ```
    git clone --recurse-submodules https://github.com/navidJadid/openease_webserver_development.git
    ```

    This clones the main repository and then does this recursively for all sub-repositories.


##### 3. Working with the repository correctly
Pushing and pulling changes from the main repository works as usual. So let's see how to do the same for the submodules:

**Side note**: As of version 2019.1 on, most or all JetBrains IDEs support git-submodules and the following actions natively from the IDE (if you want to avoid the command line).

1. For updating **all** the submodules from the main repository, run this command in the root directory of the repository:

    ```
    git submodule update --remote
    ```

    This however assumes that you want to update from the master branch of the respective repositories. If you want to update from a different branch or only update specific modules, see the git-submodules documentation[¹]. The link in the footnote also covers **how to avoid overwriting local changes** when updating the submodules. This will likely happen, because on default the submodules will be be run in detached HEAD state by git. Basically the solution involves checking out a specific branch for each submodule, but please refer to the mentioned documentation for a more in-depth explanation.

2. For committing changes in projects with submodules it is usually best to first commit the changes in the submodules. This is because there may be changes in the main project which depend on the changes in the submodules, so they need.

    ```
    git push --recurse-submodules=on-demand
    ```

    This will check all the submodules for changes and try to push changes them first, but if that fails, the main push will be aborted as well.

    Again, if you want to manually push for each submodule and check for each push from the main repository if there are changes in the submodules before pushing, see the documentation linked in [¹].

[¹]: We strongly recommend reading through the [git-submodules documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules) once, as this project's documentation cannot touch on all the problems that might occur when using git-submodules. Among other things, it covers how to set the branches to update from for the submodules in `.gitmodules`, how to display changes of the submodules when calling `git diff` in the main repository, how to avoid overwriting local changes when updating submodules and how to merge changes in the submodules. 

##### 4. Building and running the whole thing
**TODO: DOCKER STUFF**

##### 5. Development procedure

