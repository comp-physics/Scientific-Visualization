# Interactive Visualization on Phoenix
This tutorial outlines how to perform interactive remove visualization on GATechs Phoenix cluster.
Using remote visualization allows for the visualization of much larger simulations interactively, without the need for Paraview's batch mode

## Step 1 - Setting up your Environment
Begin by downloading the .zip file here (https://gatech.box.com/s/1diud4lgequvuie5fg2ac6hcvoelndu5).
This file contains two things

- A bash script to automate the job submission process and provide instructions for remote connection.

- A prebuilt Paraview 5.11 binary

Place the file `paceParview.zip` in you scratch direction on Phoenix and unzip is using `unzip paceParaview.zip`.
Enter the new directory `paceParaview` and run `tar -xvf ParaView-5.11.0-egl-MPI-Linux-Python3.9-x86_64.tar.gz` to decompress the compiled binary.
Now that you have the binary on Phoenix, you'll need to download Paraview 5.11 on your local machine.
Paraview binaries can be downloaded here (https://www.paraview.org/download/).
Make sure to select `v5.11` from the version drop down bar and install a `5.11.0` version of Paraview.

## Step 2 - Customizing the Bash Script
While all of the options for the bash script could be passed as command line arguments, it saves time to hard code certain options that are unlikely to change.
The following is a list of required and suggested updates to make to `pace-paraview-server`.

- (Optional) Update line 4 to customize the job name that will show up in the scheduler

- (Required) Update line 6 to point towards the location of you Paraview bin directory

- (Optional) Update line 51 to reflect the default account you'll use to run Paraview jobs

- (Optional) Update line 52 to reflect the default wall time requested for your job

## Step 3 - Running pace-paraview-server
Before running `pace-paraview-server` for the first time, you'll need to update its permissions by running `chmod u+x pace-paraview-server` in your command line.
Once this has been done, you can run `./pace-paraview-server` with the following options:

- `--account` specifies the charge account to use for the job.
If you updated line 51 of `pace-paraview-server` to reflect a default account, this option is optional, otherwise it is required.

- `--nodes` specifies the number of nodes to request (default 1)

- `--mem` specifies the memory per node to request (default is to request all memory)

- `--gres` specifies the GPU resources to request.
These instructions have only been verified to work with `--gres gpu:V100:2`.

- `--time` specifies the time limit for your job.
This option is optional and defaults to whatever is set on line 52 of `pace-paraview-server`.

Once you run `./pace-paraview-server <options>`, it'll take a bit to start up. In the meantime, you'll see the below message:

```
Submitted batch job <job #>
Waiting for ParaView server to start. This may take several minutes  ...
```

When it's done initializing, you should see a dialogue with some recommended next steps numbered 1-4. Below is a slightly altered version of that dialogue:


1) Create the appropriate port forwarding for your local ParaView session to connect with.
* On your local machine, run the following from a terminal where `nodeIdentifier` is the remote node running the ParaView server process (given in the output of the batch script) and `paceSystemIdentifier` is the name of the PACE system (however this is configured with your `.ssh/config`). This terminal session must not be killed for the duration of your ParaView session as it's maintaining the port forwarding.
    * `ssh -L 8722:<nodeIdentifier>:53723 <paceSystemIdentifier>`

2) Once you have `Paraview5.11.0` open on your machine, select `file -> Connect..` to open the remote connection dialogue box.
* If you've already set up the pace connection, simply double-click the existing configuration.
* If you have not yet set up the pace connection, click `Add Server`.
This will bring up a new dialogue box where you can specify a configuration name and set the `Port` to 8722.
Once this is done, click `configure` and then `save` on the next dialogue box.
