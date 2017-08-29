## apt-get-common_issues
Following are some common errors that may arise while running `$ apt update`.

It is very easy to upgrade and manage software and packages with systems based on Debian or Ubuntu using `$ apt update` or `$ apt-get update`. But sometimes it can be rather painful.

### Solving an expired key (KEYEXPIRED) with apt

* Full list of known keys can be viewed using:
```
$ apt-key list
```

* List only *expired* repository keys:
```
$ apt-key list | grep "expired"
```
* Update the gpg key. Run:
```
$ sudo apt-key adv --keyserver keys.gnupg.net --recv-keys <KEY>
```
* Update
After renewing the expired key, `apt update` can be run again an you can install any available upgrades
```
$ sudo apt-get update && apt-get upgrade
```

### issues with $PATH env variable
Installing some programs might require you to add the directory to your system's `$PATH` variable.  
You can change $PATH using `export` command.
Example: 
```
$ export PATH=newPATH 
```
However, this will completely replace `$PATH` with `newPATH`.  
Items can be added (+=) to an already populated `$PATH` variable using a `:` (colon-separated paths).  
Example:
```
$ export PATH=$PATH:newPATH
```
newPath can be concatenated at the end preceded by `$PATH:` like above, or it can be added to the beginning of the path.
Example:
```
$ export PATH=newPATH:$PATH
```
Doing this, the terminal will first search for the program at newPATH first.

One of the consequences of having an inaccurate `$PATH` variable is that the shell will not be able to find and execute programs without a full path.

Also, setting paths in `$PATH` on terminal is temporary. It needs to be set everytime you start a shell. In order to eliminate this, the usual way would be to edit the `.bashrc` file. This gets invoked everytime a new shell session is started, hence `newPath` will be available in `$PATH` everytime a terminal is opened.  
Example:
* Open .bashrc in gedit (sudo, if not su)
```
$ sudo gedit ~/.bashrc
```
Add `export PATH=newPATH:$PATH` or `export PATH=$PATH:newPATH` to this file. Save and exit.
* Now reload .bashrc in order for it to work with the new configuration.
```
$ source ~/.bashrc
```
