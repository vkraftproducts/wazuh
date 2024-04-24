This documentation outlines the steps to set up Wazuh on a Microsoft Azure VM using Docker. Wazuh is an open-source security monitoring platform that can be used for threat detection, integrity monitoring, and incident response.

	v Prerequisites

		ü Microsoft Azure account
		ü Access to a Microsoft Azure virtual machine (VM)
		ü SSH access using a .pem file

	1. Connect to the Azure VM
	
	Use SSH to connect to your Azure VM using the provided .pem file.

		a. Using CMD bash:
			
			ssh -i your-key.pem username@98.70.107.191
			
		Replace `your-key.pem` with the path to your .pem file and `username` with your VM username.
		
		b. Using MobaXterm:
		
			1. Open MobaXterm.
		
			2. Click on the "Session" button in the upper-left corner to open the session management window.
		
			3. In the session management window, click on the "SSH" button to open the SSH session settings.
		
			4. Fill in the following details:

				□    Remote host: `98.70.107.191` (the IP address of your Azure VM)
				□    Specify your username in the "Username" field.
				□    Under the "Advanced SSH settings" tab, click on the "Use private key" checkbox.
				□    Click on the file icon next to the "Private key" field and navigate to the location of your `.pem` file.
				□    Select your `.pem` file and click "Open".
		
			5. Once you've filled in the details, click the "OK" button to save the session settings.
		
			6. Now, double-click on the session you just configured in the session management window to initiate the SSH connection.
		
			7. MobaXterm will use the specified `.pem` file to authenticate the connection to your Azure VM, and you'll be logged in to your VM via SSH.
			
	1. Install Docker

		a. Increase `max_map_count` on your Docker host:
	
			sysctl -w vm.max_map_count=262144

		b. Update the `vm.max_map_count` setting in `/etc/sysctl.conf` to set this value permanently. To verify after rebooting, run `sysctl vm.max_map_count`.

		c. Run the Docker installation script:
	
			curl -sSL https://get.docker.com/ | sh

		d. Start the Docker service:
	
			systemctl start docker

		v Note: If you would like to use Docker as a non-root user, add your user to the docker group:

			usermod -aG docker your-user
			
		Log out and log back in for this to take effect.

	3. Install Docker Compose

	The Wazuh Docker deployment requires Docker Compose 1.29 or later.

		a. Download the Docker Compose binary:

			curl -L "https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

		b. Grant execution permissions:

			chmod +x /usr/local/bin/docker-compose

		c. Test the installation:

			docker-compose --version

		Output:
		
			Docker Compose version v2.12.2

		v Note: If the command `docker-compose` fails after installation, create a symbolic link to `/usr/bin` or any other directory in your path:
		
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose![image](https://github.com/vkraftproducts/wazuh/assets/108314975/42ddedab-2957-4054-a217-3fa4c0c1520d)
