### Instructions

Follow the following instructions upon logging into the servers to run Jupyter Lab and RStudio projects properly.

# Create and enter a "projects" directory
Starting from your home "~" directory, (if you are not in home run ```cd ~```), run
```
mkdir projects
cd projects
```

# Clone this repository into "projects"

In the projects directory, run
```
git clone https://github.com/jpatsenker/klugerlab_servers_docker_setups.git
```

# Copy relevant docker setup folder to projects directory and enter

Still in the projects directory, run
```
cp -R klugerlab_servers_docker_setups/jupyter_docker_setup_cpu .
cd jupyter_docker_setup_cpu
```
to access the jupyter cpu image builder, or
```
cp -R klugerlab_servers_docker_setups/jupyter_docker_setup_gpu .
cd jupyter_docker_setup_gpu
```
to access the jupyter gpu image builder.

# Fill the ```.env``` file

First, obtain the following 4 pieces of info:

1. your username (run ```whoami``` to ensure)
2. your UID (run ```id -u```)
3. your GID (run ```id -g```)
4. a port that will be yours on the server. Choose a 4 digit number other than 8888 (collisions are unlikely).

Enter the ```.env``` file by running ```vim .env```. It should look as follows:

```
USERNAME=$ADD_USERNAME_HERE
UID=$ADD_UID_HERE
GID=$ADD_GID_HERE
JUPYTER_PORT=$CHOOSE_A_PORT
```

Press "i" to enter insert mode and using arrow-keys move the cursor and replace the "$" variables with the information you gathered.

Exit insert mode by pressing "esc". Save and quit vim by typing in ":wq".

# Edit the "requirements.txt" file

Check the requirements.txt file similarly by running ```vim requirements.txt```. Use insert mode to add any new lines with desired python packages. These will be installed via pip. Remove any unnecessary packages. The default set of packages should contain most of the useful numerical, statistics,and ML/DL libraries.

# Build and run the docker image

Still in the setup directory, run

```
docker compose up --build -d
```

This should build a docker image, and run your container in detatched mode.

# Attatch via an ssh tunnel on your local machine

Choose a 4 digit port for your $LOCAL_PORT (this can be 8888 for simplicity, if you are only running one instance of this setup on one server). In a new shell on your local device, run

```
ssh -N -f -L localhost:$LOCAL_PORT:localhost:$JUPYTER_PORT $USERNAME@vonneumann.med.yale.edu
```

# Open the browser and enter the lab

Open up your browser and write "localhost:$LOCAL_PORT" into the address bar, and you should see the JupyterLab interface open up!


