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

If this repository is cloned normally, then the submodules will not be cloned together with the main repository. There will be subfolders, but those will be empty. Now depending on whether you have write access to the repositories in use here (meaning this repository and all the repositories of the sub-modules), the workflow is slightly different:

- **If you have write access to all the repositories in use** or just want to run the project and not commit changes:  
    To load the repository with all submodules either of the following two approaches will work:

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

- **If you do not have write access to all the repositories in use**:  
    First you will need to fork (on [GitHub](https://www.GitHub.com)) and normally clone the main repository. This will clone the empty directories of the submodules (because we did not tell git yet to clone the repositories of the submodules; we will see why in a second). Now fork but do not clone (yet) the repositories of each of the submodules that you want to develop. 
    
    (Alternatively you can clone the main repository with all the submodules (as explained in the section for developers with write access) and then fork with with the command line.)
    
    Next go into the `.gitmodules`-file of the main repository and change the url for the repositories which you forked. That should look something like this:
    
    ```
    ...
    [submodule "openease_flask"]
	 path = openease_flask
	 url = <url of your forked repository>
    ...
    ```
    
    Lastly, to clone the sub-repositories run the following two commands in the root of the main repository:

     ```
     git submodule init
     git submodule update
     ```
     
     As implied above, you do not need to fork all the submodules if you do not want to develop them. In that case those will just have a downstream from our repositories, but remember: since you do not have write access you will not have upstream for those respective repositories.

##### 3. Working with the repository correctly
Pushing and pulling changes from the main repository works as usual. So let's see how to do the same for the submodules:

<sub>**Side note**: As of version 2019.1 on, most or all JetBrains IDEs (and maybe other IDEs too) support git-submodules and the following actions natively from the IDE (if you want to avoid the command line). This project is not sponsored by JetBrains, we just want to mention this for user convenience.</sub>

1. For updating **all** the submodules from the main repository, run this command in the root directory of the repository:

    ```
    git submodule update --remote
    ```

    This however assumes that you want to update from the master branch of the respective repositories. If you want to update from a different branch or only update specific modules, see the git-submodules documentation[¹]. The link in the footnote also covers **how to avoid overwriting local changes** when updating the submodules. This will likely happen, because on default the submodules will be be run in detached HEAD state by git. Basically the solution involves checking out a specific branch for each submodule, but please refer to the mentioned documentation for a more in-depth explanation.

2. For committing changes in projects with submodules it is usually best to first commit the changes in the submodules. This is because there may be changes in the main project which depend on the changes in the submodules and if those are not pushed prior the main repository will reference an old state of the submodule. Luckily git provides the following command: 

    ```
    git push --recurse-submodules=on-demand
    ```

    This will check all the submodules for changes and try to push changes them first, but if that fails, the main push will be aborted as well.

    Again, if you want to manually push for each submodule and check for each push from the main repository if there are changes in the submodules before pushing, see the documentation linked in [¹].

<sub>[¹]: We strongly recommend reading through the [git-submodules documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules) once, as this project's documentation cannot touch on all the problems that might occur when using git-submodules. Among other things, it covers how to set the branches to update from for the submodules in `.gitmodules`, how to display changes of the submodules when calling `git diff` in the main repository, how to avoid overwriting local changes when updating submodules and how to merge changes in the submodules.</sub>

##### 4. Building and running the whole thing
This project requires the use of:
1. `docker` ([docker distributions and installation guides for different OS](https://docs.docker.com/install/))
2. `docker-compose` ([installation guide](https://docs.docker.com/compose/install/)).

First it is necessary to create an 'episodes'-folder on your machine and set an environment variable pointing to it. This is needed, because some module in the open-EASE project requires such a folder to exist, otherwise the build will not complete. Again, we know this an inconvenience and we are working to have the project work without this necessity.

For now please create a directory called `docker` somewhere on your machine. Inside that folder create another directory called `episodes`. Next you need to add a system environment variable `OPENEASE_EPISODE_DATA` which contains the path to the `episodes`-directory.

On Linux, open the `.bashrc`-file (which is usually located in `/etc/.bashrc`) and add the following lines to the end:

```
export OPENEASE_EPISODE_DATA="< system path to the mentioned directory >/docker/episodes/"
```

as well as the following line:

```
export OPENEASE_WEBSERVER_DEV="< system path to root of this repository >"
```

<sub>**Side Note:** See [this link](https://www.rc.fas.harvard.edu/resources/documentation/editing-your-bashrc/) on how to edit the `.bashrc`-file with the `nano`-editor or any other editor of your choice.</sub>

Now either open a new terminal or if you had one opened already, execute the following command to reload the `.bashrc`:

```
source ~/.bashrc
```

As of right now we have not yet tested whether this works on Windows as well. We assume simply adding the environment variables in your system settings should do the job. If there are problems, please consider creating an issue or contacting us.

<sub>**Side Note:** Though not needed in any way for the development of the web server, if you want to download episodic memory data, you can do so as shown [here](http://www.knowrob.org/doc/docker#setting_up_experiment_logs). But be wary that around 40GB of disk space is needed for all episodic memory data.</sub>

<sub>**Side Note:** For the following it is not possible to use the integrated Docker-functionalities of JetBrain IDEs (at least on Linux). This is due to the IDE not loading the environment variables which we define in `.bashrc`. Thus the `docker-compose` build will throw an error, because he cannot the access those environment variables.
 If you want to use the JetBrains IDE Docker-functionalities, the work around is to start the IDE from the command line which will cause it to have all the environment variables defined in `.bashrc`. For convenience you can setup an alias for the IDE.</sub>

Now to build and start the project (in detached mode mind you) run the following command in the root directory of the project:

```
docker-compose up -d
``` 

On consecutive executions the same command will just start up the containers of the images (given that the images are not deleted). Unfortunately as of right now, the build might take a while (~5-20 min, depending on your machine and internet connection), as some of the required images are quite large. We are working to reduce their sizes and the build time, but for now it sadly is what it is.

After the build was successful, use the following command to see which containers are currently running:

```
docker-compose ps
```

To stop and remove all the container execute the following command:

```
docker-compose down
```

Alternatively run the `start` or `stop` command to start or stop the containers without removing them. For more information on `docker-compose` please refer to the source linked in [²].

Docker will not rebuild images automatically when code is changed. It will only reconfigure (which is not the same as build) the image if the `dockerfile` or the `docker-compose` were changed. To rebuild existing images, execute either of the following two commands:

```
docker-compose build
```

or

```
docker-compose up -d --build
```

<sub>[²] For more information on `docker-compose` check the [documentation](https://docs.docker.com/compose/overview/).</sub>

##### 5. Development procedure
Depending on what part of the project you want to change.