# openease_webserver_development

<!--ts-->
   * [openease_webserver_development](#openease_webserver_development)
         * [Table of Contents](#table-of-contents)
               * [1. Background and purpose of this repository](#1-background-and-purpose-of-this-repository)
               * [2. Setting up and cloning the repository correctly](#2-setting-up-and-cloning-the-repository-correctly)
               * [3. Building and running the whole thing](#3-building-and-running-the-whole-thing)
               * [4. Development procedure](#4-development-procedure)

<!-- Added by: navid, at: Mi 15. Mai 13:11:53 CEST 2019 -->

<!--te-->

(Table of contents created with this cool [tool][1])

### Table of Contents
1. Background and purpose of this repository
2. Setting up the repository for correctly
3. Building and running the whole thing
4. Development procedure

##### 1. Background and purpose of this repository
As of May 2019 the openEASE project has launched with a new project architecture which splits it into several sub-repositories. Each of the sub-repositories is a docker-module which is deployed to docker-hub and then build as a whole with docker-compose in the root repository. This kind of architecture will hopefully increase the project's maintainability and ease the development of new features for the project, both for the researchers at the University of Bremen but also externals. Yet, as a consequence of this change, some parts of the system cannot be run or tested standalone anymore. This repository aims to setup a development environment for the front-end and webserver. It contains all the submodules and instructions needed to run and develop the mentioned components.

##### 2. Setting up and cloning the repository correctly
This repository utilizes git-submodules which allows us to include and develop the dependencies of this project, without having to include their files directly into this git-repository. When cloning this project correctly (which we will explain in a minute) the submodules are cloned from their own repositories. Now when changes are made to the submodules they are only pushed to the repository of the respective module and not to this repository.

If this repository is cloned normally, then the submodules will not be cloned together with the main repository. There will be subfolders, but those will be empty. To load the repository with all submodules, either of the following two approaches will work:

1. If the repository is normally cloned, the sub directories will be included but empty as mentioned. To clone the sub-repositories you can then run the following two commands in the root of this repository:

    `git submodule init`
    `git submodule update`

2. Alternatively you can add `--recurse-submodules` when cloning this repository, which should look like this:

    `git clone --recurse-submodules https://github.com/navidJadid/openease_webserver_development.git`

    This clones the main repository and then does this recursively for all sub-repositories. We recommend this approach for its simplicity.

**TODO: pushing and pulling from repositories**

Check the following [documentation][0] for a more in depth explanation of git-submodules.

##### 3. Building and running the whole thing
**TODO: DOCKER STUFF**

##### 4. Development procedure

[0]: [https://git-scm.com/book/en/v2/Git-Tools-Submodules]
[1]: [https://github.com/ekalinin/github-markdown-toc#usage]
