New tools have been installed on the app server in `Stratos Datacenter`. Some of these tools can only be managed from the graphical user interface. Therefore, there are requirements for these app servers.





On all App servers in `Stratos Datacenter` change the default runlevel so that they can boot in `GUI (graphical user interface)` by default.



| **Runlevel** | **Target Units**                    | **Description**                           |
| ------------ | ----------------------------------- | ----------------------------------------- |
| **0**        | runlevel0.target, poweroff.target   | Shut down and power off the system.       |
| **1**        | runlevel1.target, rescue.target     | Set up a rescue shell.                    |
| **2**        | runlevel2.target, multi-user.target | Set up a non-graphical multi-user system. |
| **3**        | runlevel3.target, multi-user.target | Set up a non-graphical multi-user system. |
| **4**        | runlevel4.target, multi-user.target | Set up a non-graphical multi-user system. |
| **5**        | runlevel5.target, graphical.target  | Set up a graphical multi-user system.     |
| **6**        | runlevel6.target, reboot.target     | Shut down and reboot the system.          |

## View and change default target (runlevel)

For systemd, the concept of runlevels is replaced by the term "targets". For those that are familiar with the init runlevels, there is a "mapping" between the init runlevels and systemd targets:

```
   ┌─────────┬───────────────────┐
   │Runlevel │ Target            │
   ├─────────┼───────────────────┤
   │0        │ poweroff.target   │
   ├─────────┼───────────────────┤
   │1        │ rescue.target     │
   ├─────────┼───────────────────┤
   │2, 3, 4  │ multi-user.target │
   ├─────────┼───────────────────┤
   │5        │ graphical.target  │
   ├─────────┼───────────────────┤
   │6        │ reboot.target     │
   └─────────┴───────────────────┘
```

2 common targets are the most common ones:

- multi-user.target: analogous to runlevel 3, Text mode
- graphical.target: analogous to runlevel 5, GUI mode with X server

![img](https://www.systutorials.com/wp/files/2015/12/Systemd-components-630x354.png)



# DEBIAN/CENTOS

### Get the default

If the system running on Non-GUI Mode, “systemctl get-default” command will return “multi-user.target” :

```
# systemctl get-default
multi-user.target
```



Run the below command to list of all currently loaded and available target.

```
# systemctl list-units -t target
```



 The following target are required to be loaded :

```
graphical.target     loaded active active Graphical Interface
multi-user.target    loaded active active Multi-User System
```



### Change the Default Runlevel

##  Changing and Viewing the default runlevel

The default run level is defined in the file '/etc/inittab'. You can view it as follows.

```
# grep ^id /etc/inittab
id:5:initdefault:
```

Change default runlevel to Non-GUI (text-based) 

```
# systemctl set-default multi-user.target
```

```sudo systemctl enable multi-user.target```

Change default runlevel to GNOME Desktop

```
# systemctl set-default graphical.target
```



```
sudo systemctl enable graphical.target
```

systemctl get-default

systemctl set-default graphical.target

sudo systemctl enable graphical.target



sudo systemctl start runlevel5.target



UBUNTU



To see the current (and previous) runlevel:

```
runlevel
```

To switch runlevels:

```
sudo init $runlevel
```

For example, to reboot:

```
sudo init 6
```

The init you are reading about was replaced by [upstart](http://upstart.ubuntu.com/) starting with Edgy Eft 6.10; and, one of the programs provided by upstart is its own implementation of init. [Here are the docs](https://help.ubuntu.com/10.04/) for 10.04.

To change the default runlevel, use your favorite text editor on /etc/init/rc-sysinit.conf...

```
sudo vim /etc/init/rc-sysinit.conf
```

Change this line to whichever runlevel you want...

```
env DEFAULT_RUNLEVEL=2
```

Then, at each boot, upstart will use that runlevel.