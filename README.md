# Data Science at Scale Toolbox

The data-science-at-scale-toolbox repository makes it easy to create a Docker
container for use during the [Data Science at Scale Specialization](https://www.coursera.org/specializations/data-science).

For ease of use a script named 'container.sh' can be used to manage the entire 
lifecycle and interaction with the Docker container. Git, Python, R, and SQLite
are installed as part of the image along with the Python package oauth2.
Additionally, other Python packages and R packages can be added after the 
container is created.

There are two ways to create a Docker container for use during the Data Science
at Scale Specialization either by fetching the pre-built  [image](https://hub.docker.com/r/gdhorne/data-science-at-scale-toolbox/) from the Docker website
or from scratch. In both scenarios the [data-science-at-scale-toolbox](https://github.com/gdhorne/data-science-at-scale-toolbox) repository
available on GitHub will be beneficial.

## Create a Docker Container from a Pre-built Image Available at Docker Hub 

Download and install the Docker software for Apple Mac OS X, GNU/Linux or 
Microsoft Windows following the  [instructions](http://docs.docker.com/linux/started/) at the [Docker](https://www.docker.com) website.

Retrieve the data-science-at-scale-toolbox repository and the 
data-science-at-scale-toolbox image to create a container, and optionally 
map a host filesystem share for storage and/or integrate with the host
computer's graphics capability if available.

	$ git clone https://github.com/gdhorne/data-science-at-scale-toolbox
	$ ./container.sh create toolbox gdhorne/data-science-at-scale-toolbox \
							/home/me/datascience
	$ ./container.sh status

Disable integration with the host computer's graphics capability by removing  
the following to the 'create' stanza in the container.sh script file. 
Currently, this only works with \*nix operating systems. Graphs/plots created with R can 
be written directly to a file instead if host-integration is not possible.

	--volume=/tmp/.X11-unix:/tmp/.X11-unix \
	--env=DISPLAY=unix$DISPLAY \

The pre-built image includes host-integration with the graphics system.

## Create a Docker Container from a Locally Pre-built Image or from Scratch

Download and install the Docker software for Apple Mac OS X, GNU/Linux or 
Microsoft Windows following the [instructions](http://docs.docker.com/linux/started/) at the [Docker](https://www.docker.com) website.

Retrieve the data-science-at-scale-toolbox repository and the 
data-science-at-scale-toolbox image to create a container, and optionally  
map a host filesystem share for storage as shown.

	$ git clone https://github.com/gdhorne/data-science-at-scale-toolbox
	$ ./container.sh create toolbox data-science-at-scale-toolbox /home/me/datascience
	$ ./container.sh status

Enable integration with the host computer's graphics capability by adding the 
following to the 'create' stanza in the container.sh script file. Currently, 
this only works with \*nix operating systems. Graphs/plots created with R can 
be written directly to a file instead if host-integration is not possible.
 
	--volume=/tmp/.X11-unix:/tmp/.X11-unix \
	--env=DISPLAY=unix$DISPLAY \

## Applications

After creating the container these applications are accessible within a web 
browser.

	Git:		Accessible via WeTTY and via RStudio integration

	Python:		Accessible via WeTTY

	R:			Accessible via WeTTY

	SQLite:		Accessible via WeTTY

	WeTTY:		http://127.0.0.1:8000

				UserID: dst
				Password: science

				To enable the terminal/console management utility 
				type 'screen' and press ENTER.

Alternatively, the data-science-at-scale-toolbox image provides a traditional 
command line interface, without WeTTY, to some applications such as Git, 
Python, R, SQLite, and vim. For convenience the terminal/console management 
utility 'screen' has been installed and starts automatically.

	$ ./container.sh attach toolbox

Press ENTER if the container's shell prompt does not appear. To exit the 
container and leave it running press CTRL+P, CTRL+Q; this is the preferred 
method. To exit the container and stop it type 'exit'.

## Container Management

The container.sh script makes managing the Docker container straight-forward 
via a user-friendly, simplified interface to the Docker container management 
functions covering the entire lifecycle. 

|Action|Command|
|------------------|----------------------------------------|
|Create container|container.sh create \<container\> \<image\> [\<host\_file\_share\>]|
|Access container|container.sh attach \<container\>|
|Pause container|container.sh pause \<container\>|
|Unpause container|container.sh unpause \<container\>|
|Start container|container.sh start \<container\>|
|Stop container|container.sh stop \<container\>|
|Delete container|container.sh kill \<container\>|
|Display container status|container.sh status [\<container\>]|

Throughout these examples the container is named 'toolbox', the image is
named 'data-science-at-scale-toolbox', and the host file system share is
'/home/me/datascience'. 

Example 1: Create a container

	$ ./container.sh create toolbox data-science-at-scale-toolbox /home/me/datascience

If the fourth argument is omitted the container cannot write to the host 
file system. When you delete the container any files that you created will 
be lost unless you commited them to a remote Git repository.

Example 2: Check the status of a specific container or all containers

    $ ./container.sh status toolbox
    $ ./container.sh status

The current status of all containers is reported.

Example 3: Access a running container at the command line without WeTTY

    $ ./container.sh attach toolbox

Press ENTER if the container's shell prompt does not appear. To exit the 
container and leave it running press CTRL+P, CTRL+Q; this is the preferred 
method. To exit the container and stop it type 'exit'.

Example 4: Pause a running container and unpause a container

    $ ./container.sh pause toolbox
    $ ./container.sh status toolbox
    $ ./container.sh unpause toolbox
    $ ./container.sh status toolbox

Processes running within a container can be paused and will resume when unpaused.

Example 5: Stop a running container and start a container

    $ ./container.sh stop toolbox
    $ ./container.sh status toolbox
    $ ./container.sh start toolbox
    $ ./container.sh status toolbox

Any running process is shutdown potentially loosing any unsaved work.

Example 6: Delete an existing container (non-recoverable)

	$ ./container.sh kill toolbox

A container can be deleted if it is in either started or stopped state. A 
paused container cannot be deleted. This is a non-recoverable action.

Example 7: Error handling

    $ ./container.sh pause toolbox
    $ ./container.sh stop roolbox
    $ ./container.sh attach toolbox

The container.sh script detects attempts to perform an invalid command or 
otherwise attempt an action not possible in the current context of the 
named container. Try the preceding sequence of commands and notice the 
output to the console.

