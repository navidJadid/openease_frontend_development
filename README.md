# openease_webserver_development

## Table of Contents
1. [Background and purpose of this repository](#1-background-and-purpose-of-this-repository)
2. [Setting up and cloning the repository correctly](#2-setting-up-and-cloning-the-repository-correctly)
3. [Working with the repository correctly](#3-working-with-the-repository-correctly)
4. [Building and running the whole thing](#4-building-and-running-the-whole-thing)
5. [Development procedure](#5-development-procedure)
6. [CI & CD](#6-ci-and-cd)

### 1. Background and purpose of this repository
As of May 2019 the openEASE project has launched with a new project architecture which splits it into several sub-repositories. Each of the sub-repositories is a docker-module which is deployed to docker-hub and then build as a whole with docker-compose in the root repository. This kind of architecture will hopefully increase the project's maintainability and ease the development of new features for the project, both for the researchers at the University of Bremen but also externals. Yet, as a consequence of this change, some parts of the system cannot be run or tested standalone anymore. This repository aims to setup a development environment for the front-end and webserver. It contains all the submodules and instructions needed to run and develop the mentioned components.

<sub>**Side Note:** We are aware that some of the procedures needed to work on this project are a bit 'annoying' or 'unhandy' (to say the least). We are constantly looking for ways to improve the setup procedure and the development flow, hence if you have suggestions consider writing us an email or editing this documentation and submitting a pull request.</sub>

### 2. Setting up and cloning the repository correctly
This repository utilizes git-submodules which allows us to include and develop the dependencies of this project, without having to include their files directly into this git-repository. When cloning this project correctly (which we will explain in a minute) the submodules are cloned from their own repositories. Now when changes are made to the submodules they are only pushed to the repository of the respective module and not to this repository.

If this repository is cloned normally, then the submodules will not be cloned together with the main repository. There will be subfolders, but those will be empty. Now depending on whether you have write access to the repositories in use here (meaning this repository and all the repositories of the submodules), the workflow is slightly different:

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

### 3. Working with the repository correctly
Pushing and pulling changes from the main repository works as usual. So let's see how to do the same for the submodules:

**IMPORTANT!!!** First of all, go into the directory of each of the submodules in a terminal and execute to the following command:

```
git checkout master
```

You could also pick any branch of your choice. But be aware that without this step pushing changes might not work, at least it did not for us. Also when working with multiple people on the submodules, there might be problems when committing. When working alone on your fork of the project there should not be any problems though. We are still looking into those matters and will update the project and documentation as soon as we have found solutions.

<sub>**Side note**: As of version 2019.1 on, most or all JetBrains IDEs (and maybe other IDEs too) support git-submodules and many (but not all) of the following actions natively from the IDE (if you want to avoid the command line).
This project is not sponsored by JetBrains, we just want to mention this for user convenience.</sub>

1. **Updating Submodules**  
    For updating **all** the submodules from the main repository, run this command in the root directory of the repository:

    ```
    git submodule update --remote
    ```
    
    Due to our previous setup this will update from the master branch of the respective repositories. If you want to update from a different branch or only update specific modules, see the git-submodules documentation[¹]. The link in the footnote also covers how to avoid overwriting local changes when updating the submodules. This will likely happen, because on default the submodules will be be run in detached HEAD state by git (if the `checkout branch` is not set). Basically the solution involves checking out a specific branch for each submodule, but please refer to the mentioned documentation for a more in-depth explanation.

2. **Committing and pushing changes**  
    For committing changes in projects with submodules it is usually best to first commit the changes in the submodules. This is because there may be changes in the main project which depend on the changes in the submodules and if those are not pushed prior the main repository will reference an old state of the submodule. Luckily git provides the following command: 

    ```
    git push --recurse-submodules=on-demand
    ```

    This will check all the submodules for changes and try to push changes them first, but if that fails, the main push will be aborted as well.

    Again, if you want to manually push for each submodule and check for each push from the main repository if there are changes in the submodules before pushing, see the documentation linked in [¹].

<sub>[¹]: We strongly recommend reading through the [git-submodules documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules) once, as this project's documentation cannot touch on all the problems that might occur when using git-submodules. Among other things, it covers how to set the branches to update from for the submodules in `.gitmodules`, how to display changes of the submodules when calling `git diff` in the main repository, how to avoid overwriting local changes when updating submodules and how to merge changes in the submodules.</sub>

### 4. Building and running the whole thing
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

<sub>**Side Note:** For the following it is not possible to use the integrated Docker-functionalities of JetBrain IDEs (at least on Linux). This is due to the IDE not loading the environment variables which we define in `.bashrc`. Thus the `docker-compose` build will throw an error, because it cannot the access those environment variables.
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

Now if you open a web browser of your choice and go to `localhost:5000` the web-interface should open and you can register and login. Just remember that if the `postgres-volume` is removed, the login data will be removed with it and it will be necessary to register again.

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

### 5. Development procedure
There are different approaches, depending on what part of the project you want to change. It should be possible to change most files while the project is running in `docker-compose` and see results with a refresh of the page in the web browser. This works because most container `volumes` are set up as `bind-mounts` in `docker`. As a result please take notice that moving files out of the specified locations might cause `docker` to not find them anymore. Additionally for certain files, additional steps need to be taken (unfortunately) e.g. sometimes containers need to be rebuilt to see changes. Let's now see which approach to use for what part of the project:

- **UI, i.e. HTML, CSS, images, icons and logos**:  
    - **HTML**  
    HTML (template-)files can be found in `openease_falsk/webrob/templates`. Saved changes will show upon a page refresh in the web browser.
    
        Be aware that if you change the `EASE_DEBUG` variable in the `docker-compose.yml` to `false` the changes might not show up anymore on refresh. This is because changing the variable will cause the server to not run in debug mode anymore which leads to caching or something alike from the [`flask`-framework](http://flask.pocoo.org/). Actually we are not sure exactly why, so if you do, let us now.

    - **CSS**  
    CSS code can be found in the `openease_css`-submodule. Saved changes should appear with a refresh of the page in the web browser with clearing the cache. This can be performed by pressing `ctrl` + `shift` + `r`. A normal page refresh might not work, because web browser tend to cache images and css-files (among other files) for performance.
    It is also possible to disable caching in the developer tools of most current web browsers. [This link](https://stackoverflow.com/questions/5690269/disabling-chrome-cache-for-website-development) shows an example for Chrome, but please refer to the documentation of the browser which you are using. In the case of Chrome the developer tools need to remain open for this setting to work.

    - **images, icons and logos**  
    These files can be found in `openease_falsk/webrob/static/<respective folder>`. Like the CSS code, these files can be changed or replaced and upon a refresh with clearing the cache changes should appear.

- **JavaScript**:  
Depending on which files you would like to work on, the approach is a little different:
    - **JavaScript-files which are directly linked in the HTML-files**:  
    These JavaScript-files basically can be edited like the HTML- and CSS-files, because the `volume` is a `bind-mount` as well. Save the file and refresh the page to see the changes take place.
    
    - **JavaScript-files which are modules for openease.js**:
    Even though these files are located inside the bind mount, they are not directly referenced by the HTML-pages and are instead used as modules for the mentioned `openease.js`. Therefore this will not directly translate into changes like with the CSS, because those are not the files which are referenced by the web page. Instead an `openease.js` is built when the image is created via `npm`-scripts. To rebuild the file while the containers are running, we first have to jump into the container which contains the `index.js` and the `package.json` which should be inside the `openease` container (unless changes were made to the `docker-compose.yml`). To do that, open a (separate) terminal and execute the following command:
    
        ```
        docker exec -it openease bash
        ```
        
        Next we have to go to the directory `/tmp/npm`, which can be done with:
        
        ```
        cd /tmp/npm
        ```
        
        To rebuild the mentioned file just run:
        
        ```
        npm run build
        ```
        
        Lastly we have to move the built file to the location of the old file:
        
        ```
        mv /tmp/npm/openease*.js /opt/webapp/webrob/static/    
        ```
        
        After a refresh of the web page, the new file should be loaded. Note that `index.js` cannot be changed without rebuilding the container and volume, because the file is not located inside the `bind-mounts`. If you are already in the `/tmp/npm`-folder and would like to execute both commands in one line, you can just link the commands with `&&`:
        
        ```
        npm run build && mv /tmp/npm/openease*.js /opt/webapp/webrob/static/    
        ```
        
        As of right now, this is still untested, as we could not find any modules in this project which are directly referenced by `openease.js`. So if you run into issues, please make sure to let us now either via e-mail or by submitting an issue.

- **Python**:  
Working with these files is a bit cumbersome for now. Basically on each change the container containing the webserver has to be restarted. This can simply be done by:

    ```
    docker restart openease
    ```
    
    This might take ~10-20 seconds, depending on your machine. We are working to find a better solution, like restarting the python-script or something alike. If you have suggestions, please feel free to let us know via e-mail or pull-request.

- **Postgres-DB**:  
If code for the `postgres`-container is changed, the `postgres`-container has to be rebuilt.

### 6. CI and CD
Coming soon...
