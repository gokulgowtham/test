PIPELINE :


Creation of jenkins server :

search >> ec2 >> instances
region : ohio

 ami : CentOS 7 (x86_64) - with Updates HVM
instance type : t2.micro

security grup : all traffic

after creation :

https://mobaxterm.mobatek.net/download.html

connect to instance by using ssh .....

--
JENKINS SERVER CREATION :

--https://www.jenkins.io/download/

--select centos ..


sudo -i
 hostnamectl set-hostname jenkins
 bash
yum update -y
yum install wget -y


sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install fontconfig java-11-openjdk
yum install jenkins -y


--making jenkins as root priv. :

vi /usr/lib/systemd/system/jenkins.service

##
$JENKINS_HOME, $JENKINS_LOG, and (if you have already run Jenkins)
$JENKINS_WEBROOT.
User=root Group=root
##

systemctl daemon-reload

systemctl status jenkins

systemctl restart jenkins

cat /var/lib/jenkins/secrets/initialAdminPassword

-----


Tomcat server creation :

same instance creation again with same sg and key , reselect that

get inside tomcat instance using ssh

change hostname to tomcat

 sudo -i
hostnamectl set-hostname tomcat

cd /opt/

-- https://tomcat.apache.org/download-90.cgi

yum update -y

yum install wget -y

wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.65/bin/apache-tomcat-9.0.65.tar.gz

tar -xvzf /opt/apache-tomcat-9.0.65.tar.gz

yum install java -y

cd bin/

./startup.sh

-- browse ur tomcat server on chrome


try to click on manager app ..

find / -name context.xml
    7  vi /opt/apache-tomcat-9.0.65/webapps/host-manager/META-INF/context.xml
       
       vi /opt/apache-tomcat-9.0.65/webapps/manager/META-INF/context.xml
##

<!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->

##

    8  vi /opt/apache-tomcat-9.0.65/webapps/manager/META-INF/context.xml
  
   10  cd bin/
   11  ls
   12  ./shutdown.sh
   13  ./startup.sh


cd conf >>

---Update users information in the tomcat-users.xml file goto tomcat home directory and Add below users to conf/tomcat-users.xml file


#  <role rolename="admin-gui,manager-gui,manager-script,manager-jmx,manager-status,admin-gui"/>
  <user username="tomcat" password="s3cret" roles="admin-gui,manager-gui,manager-script"/>  #


----Restart serivce and try to login to tomcat application from the browser. This time it should be Successful


