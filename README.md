**jssh** is a wrapper utility for the Unix tool `ssh`. It slightly simplifies the login process of ssh and adds some extra facility. It uses per domain configuration files to store the login related information like IP or domain name, username, working directory, port number etc.. and uses this info to run the actual ssh command with all user-passed options and arguments. It doesn't store password; passwords are prompted or managed the usual way. With this script you wont't need to remember the credentials for your domains anymore.

#Install:
It's a single script, give it execution permission and copy it in a bin folder that is in the system PATH environment variable.
```bash
chmod 755 ./jssh
sudo cp jssh /usr/bin
```

Or without installing, you can run it from whatever directory you may wish, with full path or going to that directory.

#Usage:
jssh [native options] config_file_name ssh_options_or_arguments

**Example:**

```bash
jssh example.com
jssh example.com 'ls -a'
jssh -cdw example.com 'ls -a' 
```


**Native options:**

* **-c**  : Creates a configuration file.
* **-cdw**: Adds a command `cd WorkDir` at the beginning of other commands, where *WorkDir* is defined as in the configuration file. If *WorkDir* is not defined, jssh outputs error message and exits.

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
Port=21098
User=my_username
WorkDir=$HOME/public_html
```

**ssh options/arguments:**

All arguments of `jssh` after the configuration file are forwarded to ssh.

#Notes:

1. Always use the domain name as the configuration file name. It keeps things simpler.
2. It is a must to have the configuration file name (without `.conf`) same as the domain name if you use [jletse](https://github.com/neurobin/jletse) to automate the acme challenge completion for Let's Encrypt free SSL certificate.
