

# Create a Linux User with non-interactive shell

`adduser username -s /sbin/nologin`

**Interactive:** As the term implies: Interactive means that the commands are run with user-interaction from keyboard. E.g. the shell can prompt the user to enter input.

**Non-interactive:** the shell is probably run from an automated process so it can't assume if can request input or that someone will see the output. E.g Maybe it is best to write output to a log-file.

**Login:** Means that the shell is run as part of the login of the user to the system. Typically used to do any configuration that a user needs/wants to establish his work-environment.

**Non-login:** Any other shell run by the user after logging on, or which is run by any automated process which is not coupled to a logged in user.



- When gnome terminal or any terminal emulator is opened, `a non-login, interactive shell` is executed.
- `An interactive, login shell` is executed however, when the user logs into a computer using the command line or via ssh.
- The user, when he logs in via GUI (.i.e graphically), is executing a completely different thing depending on the graphical environment and the system. It is the graphical shell in this case that deals with the login.
- When a shell script is run, it is executed in `a non-login, non-interactive shell.`



## The differences

### Logging

Depending on how the session is launched, the bash shell reads several configuration files. One difference between distinct sessions is whether the shell is being invoked as `non-login` or as `a login` session.

#### 1 – **Login**

If the user first logs in into a terminal session or via SSH, his shell session will be considered as a login shell. In other words, a login session is a shell session that starts by authenticating or identifying the user.

To tell that you are in a login shell, type in the command :

echo $0

![img](https://net2.com/wp-content/uploads/2019/11/word-image-32.png)



This shows that you are not in a login sell. If the result comes up as -bash, then you are in a login shell. Here is an excerpt from man page :



![img](https://net2.com/wp-content/uploads/2019/11/word-image-33.png)



In Bash, you can also use [shopt login_shell](https://www.gnu.org/software/bash/manual/html_node/The-Shopt-Builtin.html) as shown below:

`shopt login_shell`

![img](https://net2.com/wp-content/uploads/2019/11/word-image-34.png)

For instance in the terminal preferences (Ubuntu for instance), there is the option that enables to run a command as a login shell :

![img](https://net2.com/wp-content/uploads/2019/11/word-image-119.jpeg)

If you tick the option shown in the snapshot above, it means that every time you open up a new terminal window, it is going to run as a login shell.
This kind of session reads configuration data from the file /etc/profile first. Afterwards, It will search for the first login shell config file in the home directory of the user in order to get configuration details that are specific to the user.

It reads the first file among ~/.bash_profile, ~/.bash_login, and ~/.profile and does not proceed any further.

#### 2 – **Non-login**

When the user who is working on this authenticated session starts a new shell session, a **non-login** shell session is launched. Notice here that no prior authentication is required when the child session is started.

A non-login shell will read the file /etc/bash.bashrc and then the user specific file ~/.bashrc to create its environment.



### Interactivity

When a shell session is not attached to a terminal, it is called **non-interactive**. When however a shell session is coupled to a terminal, it is an **interactive** session.

The environment variable BASH_ENV is read by Non-interactive shells which read the file specified in order to create the new environment.

## Which shell is being used ?

In order to find which shell you are using, you can execute the echo command below :

echo “$ECHO”

![img](https://net2.com/wp-content/uploads/2019/11/word-image-35.png)

This however does not tell which shell is running at the moment ($SHELL is the [default login shell](http://tldp.org/LDP/abs/html/internalvariables.html#SHELLVARREF)) but it rather shows the shell for the current user.

To find out exactly which shell you are using, run the ps command with -p {pid} switch as follows.

`ps -p $$`

![img](https://net2.com/wp-content/uploads/2019/11/word-image-36.png)



This will select the processes whose ID numbers show up in {pid}. The $ returns the PID or process identification number of the current process which happen to be your shell. Invoking a ps command on $ will yield the desired result.



## How many shells are installed?

Pathnames of valid login shells can be found in the /etc/shells file. The cat command below will display all installed shells on your Linux :

`cat /etc/shells`

![img](https://net2.com/wp-content/uploads/2019/11/word-image-120.jpeg)



## Conclusion

A script that is executed from the terminal is run in a non-login, non-interactive shell session whereas a session that is started with SSH for instance is an interactive login shell session.
A script that runs in its own non-interactive sub-shell that runs to execute another script and then closes immediately afterwards represents a Non-interactive non-login shell.
It is not common to encounter a non-interactive login shell, for instance, launch :

`echo command | ssh server`

When ssh is executed without a command, it starts a login shell. If the standard input (stdin) of the ssh is not a terminal(tty), it starts a non-interactive shell. This has some consequences on which files are accessed or read to initialize the shell session.

