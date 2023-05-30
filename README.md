# bds_rstudio_template
This is a template Open OnDemand (OOD) app for using BDS's RStudio Singularity containers. 

# Description
This template will help users create an [OOD](https://rc.uab.edu) app to use RStudio from a singularity container directly in a browser. Originally, RStudio would need to be started from the container in an HPC Desktop app which caused some interface issues. This should solve or mitigate those issues.

# Setup

## Enable Dev Sandbox Apps

In order to add this as an app on OOD, you will need to enable sandbox apps. You can enable sandbox apps with the following steps:

1. Open a terminal on Cheaha and running the following command: `mkdir $USER_DATA/ondemand/dev`
2. In OOD, click `Help > Restart Web Server` in the top right. This should cause a `</> Develop` dropdown menu to appear
3. Click `</> Develop > My Sandbox Apps (Development)`. This will open a new screen
4. Click the `New App` button. 
5. As of now, the only option is to clone an existing app from a Git repo. Click `Clone Existing App`
6. On the next screen, enter the following information:
    - `Directory Name`: the directory name the repo will be saved to. Example: `bds_rstudio_singularity`. The full path to the app would be `$USER_DATA/ondemand/dev/bds_rstudio_singularity` in this case. 
    - `Git remote`: `https://github.com/mdefende/bds_rstudio_template.git` or the path to a personal fork
    - If you'd like to create a totally separate git repo for this project, you can click the `Create a new Git Project from this?` checkbox. If you're operating off a fork, DO NOT click this.
    - Click `Submit`

## Edit Paths

For the app to work correctly for individual users, some paths in the `template/script.sh.erb` file need to be changed. Navigate to the app directory in your `$USER_DATA/ondemand/dev` folder. Make the following changes:

1. Set the path after `export RSTUDIO_SERVER_IMAGE=` to be the path to the SIF file you're wanting to run. Following the [BDS instrucitons](https://u-bds.github.io/training_guides/intro_to_docker_rstudio_part3.html), it would be set to "${USER_SCRATCH}/HPC_Container_session/rstudio_ggplot2_3.6.3.sif"
2. Optionally, you can edit the singularity bind paths. By default, these are set to be `/data` and `/scratch` which will allow Singularity to access any folder you have permissions for on Cheaha. This can be changed to restrict access if desired

### Other optional edits

- To change the name or description of the app, change the `name` or `description` entries in the `manifest.yml` file
- To change the resource options when requesting a job, change the options in the `form.yml` file
    - Note: Drastically changing the options in the form may necessitate changes in the `submit.yml.erb` file which defines the sbatch submission.
- By default, the password for the RStudio session is set to `NBI`. This can be changed in `template/before.sh.erb` on the line `export RSTUDIO_PASSWORD`

# Running the App

To run the app, refresh your [My Interactive Sessions](https://rc.uab.edu/pun/sys/dashboard/batch_connect/sessions) page on OOD. There should be a list of Sandbox Apps highlighted in red below the normal apps. Your BDS RStudio app (or if you changed the name) should show up there. Click the app name, select your resources like a normal HPC Desktop job, and start the job.

You will need to provide a username and password to get into the RStudio session. The username is just your Cheaha account username while the password is `NBI`. RStudio should open to the main screen at this point

# Contributing

While this app, by the barest of definitions, runs, it could be improved. If you have suggestions for improvements, you can file an issue on the github page. If you would like to contribute via code, pelase create a fork in your own github, create a new branch to make and test changes, and then submit a pull request to the main repo. If you would like to create your own fork and use it as the base where OOD pulls from, that's perfectly fine as well.