# openease_webserver_development

### Table of Contents
1. Background and purpose of this repository
2. Setting up and cloning the repository correctly
3. Working with the repository correctly
4. Building and running the whole thing
5. Development procedure

##### 1. Background and purpose of this repository
As of May 2019 the openEASE project has launched with a new project architecture which splits it into several sub-repositories. Each of the sub-repositories is a docker-module which is deployed to docker-hub and then build as a whole with docker-compose in the root repository. This kind of architecture will hopefully increase the project's maintainability and ease the development of new features for the project, both for the researchers at the University of Bremen but also externals. Yet, as a consequence of this change, some parts of the system cannot be run or tested standalone anymore. This repository aims to setup a development environment for the front-end and webserver. It contains all the submodules and instructions needed to run and develop the mentioned components.

##### 2. Setting up and cloning the repository correctly
This repository utilizes git-submodules which allows us to include and develop the dependencies of this project, without having to include their files directly into this git-repository. When cloning this project correctly (which we will explain in a minute) the submodules are cloned from their own repositories. Now when changes are made to the submodules they are only pushed to the repository of the respective module and not to this repository.

If this repository is cloned normally, then the submodules will not be cloned together with the main repository. There will be subfolders, but those will be empty. To load the repository with all submodules, either of the following two approaches will work:

1. If the repository is normally cloned, the sub directories will be included but empty as mentioned. To clone the sub-repositories you can then run the following two commands in the root of this repository:

    ```
    git submodule init
    git submodule update
    ```

2. Alternatively you can add `--recurse-submodules` when cloning this repository, which should look like this:

    `git clone --recurse-submodules https://github.com/navidJadid/openease_webserver_development.git`

    This clones the main repository and then does this recursively for all sub-repositories.
    
We recommend the second approach for its simplicity.


##### 3. Working with the repository correctly
For updating all the submodules from the main repository, you can run this command in the root directory of the repository:

    `git submodule update --remote`

This however assumes that you want to update from the master branch of the respective repositories. If you want to update from a different branch or only update specific modules, see the git-submodules documentation[¹]. The link in the footnote also covers **how to avoid overwriting local changes** when updating the submodules. This will likely happen, because on default the submodules will be be run in detached HEAD state by git. Basically the solution involves checking out a specific branch for each submodule, but please refer to the mentioned documentation for a more in-depth explanation.



[¹]: Again, we strongly recommend reading through the [git-submodules documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules) once, as this project's documentation cannot touch on all the problems you will come across when using git-submodules. Among other things, it covers how to set the branches to update from for the submodules in `.gitmodules`, how to display changes of the submodules when calling `git diff` in the main repository and how to avoid overwriting your changes when updating submodules. 

**TODO: pushing and pulling from repositories**



##### 4. Building and running the whole thing
**TODO: DOCKER STUFF**

##### 5. Development procedure

