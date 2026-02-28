Jenkins & Docker & Terraform  Installation:
#  Jenkins Installation
# Update and install Java 17 or 21 (Required for Jenkins)
sudo dnf update -y
sudo dnf install java-17-amazon-corretto -y

# Add Jenkins Repository
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# Install and Start Jenkins
sudo dnf install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins

#  Docker & Terraform Installation
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum install yum-utils awscli unzip maven git tree docker terraform -y
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl enable jenkins

-------------------------------------

# tomcatTomcat 7.0.57, 8091, "admin/admin" 

-----------------------------------------
build steps:

mvn clean package

sudo chmod 666 /var/run/docker.sock

docker rmi projectimage:1.0.0 -f

docker rm projectcontainer --force

docker build -t projectimage:1.0.0 .

docker run -d --name projectcontainer -p 8091:8080 projectimage:1.0.0

docker commit projectcontainer rajusw804/testproject:$version

docker login --username ${dockerhubusername} --password ${dockerhubpassword}

docker push rajusw804/testproject:$version

----------------------------------------------------------------
Jenkins Master-Agent Integration:
**Master Side:**
ssh-keygen -t rsa -b 4096

cd /var/lib/jenkins
mkdir -p ~/.ssh
chmod 700 ~/.ssh

sudo su -s /bin/bash jenkins
ssh-keygen -t rsa -b 4096

Master Jenkins user id_rsa.pub  copy to Agents jenkins user authorized_keys

**Agent side:**
sudo dnf update -y
sudo dnf install java-17-amazon-corretto -y

sudo mkdir -p /home/ec2-user/jenkins
sudo chown ec2-user:ec2-user /home/ec2-user/jenkins

ssh-keygen -t rsa -b 4096

sudo useradd -m -d /home/jenkins -s /bin/bash jenkins

sudo chown -R jenkins:jenkins /home/jenkins/.ssh
sudo chmod 700 /home/jenkins/.ssh
sudo chmod 600 /home/jenkins/.ssh/authorized_keys


Master Jenkins UI:
Settings -> Nodes -> click on New node
Launch method  : Launch agents via SSH

Host Agent privateIP: 172.31.16.16
Credentials
jenkins (masterkey) : id_rsa (jenkins_user)
Host Key Verification Strategy     : Non verifying Verification Strategy
