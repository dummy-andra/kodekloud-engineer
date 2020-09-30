# Linux Banner



During the monthly compliance meeting, it was pointed out that several servers in the Stratos DC do not have a valid banner. The security team has provided serveral approved templates which should be applied to the servers to maintain compliance. These will be displayed to the user upon a successful login


Update the message of the day on all application and db servers for Nautilus. Make use of the approved template located at `/tmp/nautilus_banner` on jump host

### Solution:

`on jump host`

```
jump_thor~# scp /tmp/nautilus_banner tony@stapp01:/home/tony/
		            	scp /tmp/nautilus_banner steve@stapp02:/home/steve/
			    		scp /tmp/nautilus_banner banner@stapp03:/home/banner/
			    		scp /tmp/nautilus_banner peter@stdb01:/home/peter/



#give sudo permission to user on /etc/motd
 sudo chown -R tony:tony /etc/motd

 #move   the banner to /etc/motd   
 sudo cat nautilus_banner >> /etc/motd     
```

 



NOTE:
=====

If you can `scp` onto DB Server  log in to it and `sudo yum install openssh-clients -y`OR FOR UBUNTU `apt-get install openssh-client`  - `to install SCP`	

