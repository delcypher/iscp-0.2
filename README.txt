README - iscp

iscp is an attempt to make an interactive version of scp with a few useful features such as bandwidth limiting, file/directory resume, queued downloading. It is a bash shell script and uses rsync to do transfers.

INSTALL
1. You just need to make sure you set execute permission on the script. This can be done using the following command:

chmod u+x iscp

2. Place the script wherever you want it and rename it to whatever you want. I prefer iscp.

BUGS
Please note iscp 0.2l and lower that use SSH master connections have a bug due to the way that the connection is setup. Because the '-o ControlPersist=TIME' option wasn't used the master SSH connection was kept open by running an infinite loop (bash -c while true; do sleep 10m; done;). These infinite loops don't seem to be dying very quickly so it is advised you kill them and use the lastest version of iscp which has fixed this issue.

* Additional bug.


To kill the infinite loops on the remote machine you can run the following command.
pkill -9 -f 'while true; do sleep 10m; done'

USAGE

There are 3 ways to set iscp's paramters

1. Using command line arguments. Run ''iscp --help'' to see the options
2. Using a configuration file and running ''iscp --config CONFIG_FILE''. Read ''iscp --help'' to learn how to make a configuration file.
3. Changing iscp's default variable values (I don't recommend doing this, you may accidently change a parameter you shouldn't change).


iscp currently establishes a master SSH connection everytime it starts. This prevents repeated password requests if using password authentication, however it does leave a slight security hole because anyone who has read access to your master SSH connection socket has access to your remote machine! 

The default location of the socket is in the '/tmp/' directory. This can be changed by modifying the ISCP_SSH_CONTROL_PATH variable in iscp.

If you want to use ssh-keys then setup ssh-agent and pass the option --no-ssh-master when you use iscp.

Once a SSH connection is established a ls and du command are executed on the remote system in the directory that was specified (using one of the 3 ways previously mentioned). The contents of the directory are shown to the user in the following format:

[number] (size) filename

or 

[number] (size) directory/ 

number - Is the number associated with that directory of file that you specify if you wish to download it to your local system.

size - Is the file/directory size as determined using the du --si command.


The user is then asked to enter a space seperated list of what to download specified using the number associated with that file/directory. The files/directories will be downloaded in the order specified. Alternatively the user can type 'q' to quit iscp.

If the target directory contains a file/directory that is of the same name as a file/directory to be downloaded then iscp will ask the user if he/she wishes to download (removes the existing file/directory in the target directory), resume or skip (skips to the next file/directory to download in the list the user specified). Note if the --resume option is specified iscp will not ask the user and will always choose to resume.

The files/directories will be downloaded to the directory specified on the local system.


Enjoy.
