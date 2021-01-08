![LittleBrother-Logo](doc/icon-baby-panda-128x128.png)
![LittleBrother-Logo](doc/docker-logo-128x128.png)

# Docker Support for `LittleBrother`

## Overview

There is a [docker image](https://hub.docker.com/repository/docker/marcusrickert/little-brother-slave) 
available to run a LittleBrother slave process. The easiest way to use it start as follows:
 
*   Download the [docker-compose.yml](docker/docker-compose.yml) file from GitHub and store in a local directory.
    Alternatively, clone this repository by issuing 
    
        git clone https://github.com/marcus67/docker-little-brother.git    

*   Copy the [template.env](docker/template.env) and save it as `.env` into the same directory.

*   Read the following short sections to choose the right version of the image and acknowledge the security aspects.

*   Continue with Section *Running the Slave*.

## Tag Naming Conventions

The tag names of the images are derived from the pattern `BRANCH-REVISION` where `BRANCH` is either `master` or
`release` and `REVISION` represents the number as listed in [changes](CHANGES.md). The special tag `latest` is used
for the most up-to-date release revision.

## Security Details

*   In order for LittleBrother to be able to kill processes, the configuration contains the option `pid: host`. 
This makes sure that the container sees ALL processes including those running *outside* the container.

*   For the same reason the container has to be run in a privileged mode (using `privileged:true`). Ideally,
this should be replaced by activating a set of Linux capabilities. 
For details see [this issue](https://github.com/marcus67/docker-little-brother/issues/1).

*   In order to be able to "speak" (that is play sound files), the process inside the container has to access the sound
device of the host. This is made possible by mounting the file `/etc/asound.conf` and the device `/dev/snd` 
into the container. **Note:** This requirement is obsolete since notifications should be issued by 
[LittleBrotherTaskbar](https://github.com/marcus67/little_brother_taskbar) now and no longer by the slave process.



## Running the Container

### Running the Regular Slave

*   Edit the environment variables in the `.env` file. See the comments in the file.
*   Open a shell in the directory containing `docker-compose.yml`.
*   Start the container by issuing in the shell

        docker-compose up -d slave 

### Running the Legacy Slave

The legacy slave can be used to enable notifications and audio playback using the application. However,
this is not recommended. Please, consider installing 
[LittleBrotherTaskbar](https://github.com/marcus67/little_brother_taskbar) instead.

The steps are the same as with the regular slave (see above) except for issuing

        docker-compose up -d legacy-slave 
