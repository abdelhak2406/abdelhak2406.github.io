---
title: "Mange users in linux"
date: 2024-01-07
tags: ["linux"]
---

## General

- `adduser user_name`: add a new user
- `useradd` : another type of commend!
	- [ ] What's the difference between the two?
- `cat /etc/passwd` : display the list of the users
- `deluser user_name` : delete a user
- `passwd user_name`: change passworld for the user
- Every file created by a user is automatically owned by it's primary group
- That can be files that are owned by secondary groups too!
- `addgroup group_name` : create a new group
- `usermod -a -G group_name user_name` : add user to a group! (we can do `-aG` directly)
- `groups username` : check who are the groups that a user belongs to 
- `gpasswd -d username group_name` : delete user from a group
- `delgroup groupname` : delete a group

### Groups

A collection of user that have the same privileges
- users have a "primary" and a "secondary" group, by default when we create a user it's gonna greate a group with the same name as the user, and add the user into  it! and this is the "primary group"
- We have 1 primary group and up to 15 secondary groups
- `cat /etc/group`
- All permissions for groups are inside a sudoers file 
	- [ ] find out where this file is!
- to modify this file we do
	- `visudo`
	- we just follow what the comments say and to add permission for a user to add some command, we need to put the full path!

---- 

## LinuxTv way 
- `sudo useradd username` : to create a new user!
	- if we check /etc/passwd  
	- in general IDs from 1000 and above are user related and below are system related
- `sudo useradd -m username`: create a user and create home directory for him
- `sudo useradd -r username`: create a system user
	- system user is a user that generally runs stuff in the background ect!

- `/etc/default/useradd` : contains the defulat for useradd command
	- if you wanna change the default alter this file!

- `sudo userdel username`: delete a user
	- this doesnt imply that the home directory of the user gets deleted!
- `sudo userdel -r username` : delete user AND the HOME directory

- `passwd username` : change the password of the user!


- Who are the users that are in the system?
	- check the /home directory , but it's not effective sometimes it's not!
	- better check the  `/etc/passwd` file!
	- each user has it's own line
	- each line is kindof like this : 
		- **username:x:UserID:GroupID: UserInformationField:home_directory:shell_of_user**
			 - x : essencially means that the password is hashed!(it's a cary over from the old days)
			 - UserInformationField: genrally contains the first and last time, it's OPTIONAL
			 - shell_of_user: the shell that's gonna startup!
	- `/etc/shadow` : file that contains the hashed password and other information
		- at the enc ::howmanydays:howmany_days_is_required:how many days the user will be prompted!
	- [ ] continue explaining this!

### Group managment

https://linuxhandbook.com/group-management/
- Every file is owned by a user and a group
- `groups` check what groups the current user are a part of 
- `groups username`  groups that the user belong to.
- `sudo groupadd group_name`: add group
- `sudo groupdel group_name`: delete group
- `usermod -aG group_name user_name`: add user to group
- `usermode -g group_name user_name`: change the user's primary group!
	- DONT DO IT, its just too much work and no one does it
- `gpasswd -a username group_name` : add  user to group (another way)
- `/etc/groups`: 
	- similar to `/etc/passwd` but for groups there is several 
	- groupName:group_password:GID:user_members
		- group_password: should apparently not be used because it's not secure somehow
- he went into /etc/ssh/sshd_config : and added users that are allowed to enter thourgh ssh!
	- he add a line to demonstrate a usercase of using groups

- there are 2 types of group
	- primary group and secondary group
	- to check the primary group of the user 
		- go the toe passwd file and grab the GID 
		- check the /etc/group and search for the GID
- What's the difference between primary and secondary group?
	- NOTHING!







## SKEL directory

In Linux and other Unix-like operating systems, "skel" is short for "skeleton". It is used to refer to the directory `/etc/skel`, which contains a set of default files and directories that are copied to a new user's home directory when the user is created.

The purpose of the `skel` directory is to provide a set of default settings, files, and directories for new users, so that they start with a basic working environment. The contents of the `skel` directory are typically customized by the system administrator to match the needs and preferences of the organization or system.

Some of the files and directories that may be included in the `/etc/skel` directory are:

-   `.bashrc` - a configuration file for the Bash shell
-   `.bash_profile` - a script that runs when the user logs in
-   `.profile` - a shell initialization file that is read by various shells
-   `.config/` - a directory for user-specific configuration files
-   `.ssh/` - a directory for SSH configuration files and keys
-   `.vimrc` - a configuration file for the Vim text editor

When a new user is created, the contents of the `skel` directory are copied to the user's home directory and become the basis for the user's initial environment.



## useradd VS adduser

Both `useradd` and `adduser` are commands used to create new users in Linux and other Unix-like operating systems, but they have some differences in their default behavior and options.

`useradd` is a low-level command that is typically used by system administrators to create new users from the command line. It requires a number of options and arguments to specify the user's details, such as username, user ID (UID), home directory, shell, and so on. By default, `useradd` does not create a home directory, set a password, or create any default configuration files for the new user. The administrator needs to manually specify these options or run additional commands to set them up.

On the other hand, `adduser` is a higher-level command that provides a more user-friendly interface for creating new users. It prompts the administrator for the user's details, such as name, password, home directory, and so on, and automatically creates a home directory, sets a default shell, and creates some default configuration files for the new user. `adduser` is designed to make the process of creating new users easier and more intuitive for non-expert users or for use cases where the administrator wants to create a user quickly with reasonable defaults.

In general, `useradd` is more suitable for use cases where the administrator wants complete control over the user's details and configuration, and is comfortable with specifying all the options manually. `adduser`, on the other hand, is more suitable for use cases where the administrator wants a simpler, more intuitive interface for creating new users, and is willing to rely on the default settings and options provided by the command.

## ect/sudoers and visudo

`visudo` command is used to edit the `/etc/sudoers` file, which contains the configuration settings for the `sudo` command. The `visudo` command is designed to prevent accidental or malicious errors in the sudoers file by checking the syntax and preventing simultaneous edits by multiple users.

The `visudo` command opens the sudoers file in a text editor, typically `vi` or `vim`, and allows the administrator to make changes to the file. When the administrator saves and exits the editor, `visudo` checks the syntax of the sudoers file and alerts the user if any errors are found.

The `sudoers` file is located in the `/etc/` directory, and is usually only editable by the root user or by users with sudo privileges. Therefore, the full path to the `sudoers` file is `/etc/sudoers`.

It is important to note that the `sudoers` file should only be edited using the `visudo` command, as making manual edits to the file can easily result in syntax errors or security vulnerabilities that could compromise the system.

