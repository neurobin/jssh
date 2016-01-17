**jssh** is a wrapper utility for the Unix tool `ssh`. It slightly simplifies the login process of ssh and adds some extra facility. It uses per domain configuration files to store the login related information like IP or domain name, username, working directory, port number etc.. and uses this info to run the actual ssh command with all user-passed options and arguments. It doesn't store password; passwords are prompted or managed the usual way. With this script you wont't need to remember the credentials for your domains anymore.

#Install:
It's a single script, give it execution permission and copy it in a bin directory that is in the system PATH environment variable.
```bash
chmod 755 ./jssh
sudo cp jssh /usr/bin
```

Or without installing, you can run it from whatever directory you may wish, with full path or going to that directory.

#Usage:
```sh
jssh [native options] config_file_name ssh_options_or_arguments
```

**Example:**

```bash
jssh example.com
jssh example.com 'ls -a'
jssh -cdw example.com 'ls -a' 
```


**Native options:**

* **-aw   :** Add another config file appended 'www.' at the beginning of the filename during creation. Must be passed **before** `-c` to have its' effect.
* **-c    :** Creates a configuration file.
* **-cl   :** Creates a link to an existing config file.
* **-cdw  :** Adds a command `cd WorkDir` at the beginning of other commands, where *WorkDir* is defined as in the configuration file. If *WorkDir* is not defined, jssh outputs error message and exits.
* **-v    :** Shows version information.
* **-h    :** Shows jssh help.

**Configuration file:**

The configuration file is necessary to run the `jssh` command. It usually takes username, domain/IP and port from the configuration file and runs a `ssh` command like:
```sh
ssh -p port username@domain other_ssh_options_or_commands_here
```

A typical configuration file would look like this:

```bash

############################# jssh config file ################################
# Property names are case insensitive
# if path doesn't have / at the beginning
# it will be taken relative to /home/user (~)
# '#' doesn't mean a comment. Everything not in the format: 
# Property=value are comments.
# You don't need to delete anything inside this block.
# You can comment out a line safely with a '#' at start, even though it
# has no special meaning in this config file.
###############################################################################

Host=example.com
Port=22
User=neurobin
WorkDir=$HOME/public_html
```

> Configuration files are stored in `$HOME/.neurobin/jssh`

An example of creating a configuration file with `jssh -c`:
```sh
$ jssh -c

    Give an easy and memorable name to your config file, something like example.com.
    Later you will call jssh with this name to login to remote.
    
Enter config file name (without .conf): 
example.com
Overwrite (y/n)?: 
y
Enter domain/IP: 
example.com
Enter port number: 
22
Enter username: 
neurobin

    If a working directory is specified, jssh -cdw example.com
    will run cd WorkDir just after logging in.
    
Enter working directory: 
$HOME/public_html

    Saved as: /home/jahid/.neurobin/jssh/example.com.conf
    You will call jssh for this configuration like this:
    jssh example.com other_ssh_options_or_args
    'jssh example.com' is the native jssh part. All other arguments
    will be forwarded to ssh command. Any jssh native option
    must be passed before example.com.
    You can edit the config file to make further changes.

```

**ssh options/arguments:**

All arguments of `jssh` after the configuration file are forwarded to `ssh`.

#Notes:

1. Always use the domain name as the configuration file name. It keeps things simpler.
2. If the domain name is complex or you want an easier name to invoke the configuration, create a link (with `jssh -cl`) to the domain-named config file and use that easier name to invoke jssh with (`jssh easier-name ...`).
3. It is a must to have the configuration file name (without `.conf`) same as the domain name if you use [lcget](https://github.com/neurobin/lcget) to automate the acme challenge completion for Let's Encrypt free SSL certificate.
