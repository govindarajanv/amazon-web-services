Two factor authentication in AWS (Setting up both password and key based authentication)
========================================================================================


Assume a node named 'bastion-host'and another node named 'webserver'

1	Create the user with the below steps if he/she does not exist
	-	mkdir -p /home/<new-user>/.ssh
	-	touch /home/<new-user>/.ssh/authorized_keys
	-	useradd -d /home/<new-user> <new-user>
	-	usermod -aG sudo <new-user>
	-	chown -R <new-user>:<new-user> /home/<new-user>/
	-	chown root:root /home/<new-user>
	-	chmod 700 /home/<new-user>/.ssh
	-	chmod 644 /home/<new-user>/.ssh/authorized_keys
	-	passwd <new-user>
2	create ssh keys using ssh-keygen in bastion-host
3	copy those keys into the desired node, here the desired node is 'bastion-host' using ssh-copy-id <user>@<web server ip>
4	verify in the web server .ssh/authorized_keys if your new public key is copied into that directory
5	Go to webserver, modify /etc/ssh/sshd_config with the following changes
	-	PasswordAuthentication yes
	-	AuthenticationMethods publickey,password
6	Restart the sshd server using 'service sshd restart'
