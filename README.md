# ansibledocker
Deploying docker with apache using ansible playbook from controller to target machine with custom subnet 172.168.10.0/30 subnet.

Steps : 

1.Create your target and controller machine(We have created on ec2-ubuntu machine in us-east-1)
2.install ansible on controller machine -

  sudo apt update
  sudo apt install ansible -y
  
3.Create a project directory or you can clone a git repo 

mkdir ansible-project

cd ansible-project

Create a hosts file using 

sudo touch hosts

use nano hosts 

and add your target EC2 instance information. -

[target]
target_ec2_public_ip_or_dns ansible_ssh_private_key_file=/path/to/your/key.pem ansible_user=ec2-user

4. Check your connection to the target machine -
  ansible -i hosts -m ping all

7. install docker in your target machine if not installed -

   
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce

9. Start and Enable Docker Service if it is not started

    
   sudo systemctl start docker
   sudo systemctl enable docker

11. check your service name (in our case it was docker.service)


systemctl list-unit-files | grep docker

make change in ansible playbook accordingly

9.play your ansible book on controller machine using - 

sudo  ansible-playbook -i hosts your_playbook.yml


10. . Docker Installed and Running
The target machine should have Docker installed and the Docker service should be active and enabled to start on boot. You can verify this by logging into the target machine and running:


docker --version

This command should return the version of Docker that is installed. Additionally, you can check the status of the Docker service with:


sudo systemctl status docker

This command should show that the Docker service is active and running.

 Docker Network Created
 
The playbook specifies the creation of a Docker network with a custom subnet (172.168.10.0/30). To verify this, you can list all Docker networks on the target machine with:


docker network ls

And for more detailed information about the network, including its subnet, use:


docker network inspect apache_network

Replace apache_network with the name of your network if you've used a different one. This command should show the network details including the specified subnet.


The playbook includes a task to run an Apache server within a Docker container and exposes port 80 to the host. To check that the container is running, use:

docker ps

This command lists all running containers. You should see your Apache container in the list. Look for the httpd image and note the container ID or name.

For more detailed inspection of the container, including its network settings, use:

docker inspect container_id_or_name

This provides a JSON output with extensive details about the container, including its IP address within the Docker network you created.


Finally, you should be able to access the Apache server from a web browser or using a tool like curl by pointing it to the EC2 instance's public IP or DNS name:

curl http://ec2_instance_public_ip_or_dns

You should receive the default Apache HTTP server welcome page or the content you've configured to serve.




