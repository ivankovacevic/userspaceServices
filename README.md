userspaceServices
=================

Give non privileged, normal users, on a Linux machine a way to start their desired processes during system boot and/or shutdown. Every user(UID >= 1000) including the root user get three new hidden directories in their home directory(automatically created on first run):

**.startUp**

**.shutDown**

**.status**

Executable files in those directories get executed on corresponding system events. Files found in **.startUp** directory are executed in the background(asynchronously) unless they have a **.foreground** extension which overrides that default behaviour. That is useful for example if some user has multiple services/scripts which depend on each other. 

Similary, files found in **.shutDown** and **.status** directories are executed in foreground(synchronously) by default, unless they have a **.background** extension. 

Special **.status** directory is there to provide the root user with a method to call custom status reporting from users who implemented that(optional). Root user would call: **service userspaceServices status** and that would basically execute all files in **.status** directories one by one. The processes are started as user processes, not as roots, so security is preserved. An example of status process could simply be a script which calls **ps** to display running processes of that user.

## Installation ##

This is a Linux System V init daemon compatible init script(tested on Debian Wheezy 7.4.0).
- Place the **userspaceServices** script in **/etc/init.d/**. 
- Make it executable with **chmod +x userspaceServices** 
- Run **update-rc.d userspaceServices defaults** so that symbolic links in all /etc/rc?.d directories get configured automatically.
