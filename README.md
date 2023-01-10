# Installing Tomcat using Binary Method on Amazon Linux 2

## Installing Java 11 and Open JDK 11 on Amazon Linux 2:

```t
sudo amazon-linux-extras install java-openjdk11 -y
```
## Confirm the Java is installed or not using following command:

```t
java -version
```
## Install Maven

```t
sudo yum install maven -y
```
## Install Git

```t
sudo yum install git-all -y
```

## Now, Download and Install Apache Tomcat:

```t
https://tomcat.apache.org/
```

- Goto to download section and click on whichever version you want to install such as Tomcat 8 / Tomcat 9 / Tomcat 10

- For this demonastartion I am downloading Apache Tomcat 10.

- Navigate to downlaod section on the page of version you selected > Binary Distributions > Core >  Now, right click and copy the URL "tar.gz" file

## Goto to your Amaozn Linux 2 terminal 

```t
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.70/bin/apache-tomcat-9.0.70.tar.gz
```

## Extract the downloaded file into /opt/ directory

```t
sudo tar -xvzf apache-tomcat-10.0.27.tar.gz -C /opt/
```

## Navigate to /opt/ directory

```t
cd /opt/
```

## Now, create the new "tomcat" user
- We need to provide permission of "/opt/apache-tomcat-10.0.27" directory to newly created "tomcat" user.

```t
sudo useradd tomcat
```

```t
sudo passwd tomcat
```

## First check who is the owner of the "/opt/apache-tomcat-10.0.27/" directory

```t
ls -ld /opt/apache-tomcat-10.0.27
```

## Change the permission to "tomcat" user using:

```t
sudo chown -R tomcat:tomcat /opt/apache-tomcat-10.0.27/
```
## Switch to tomcat user

```t
whoami
```

## Main configuration files

```t
cd /opt/apache-tomcat-10.0.27/conf/
```

## We need to remember:
- We need to create an application user when we are deploying a war file, using which user we are going to use. So that information is present inside tomcat-user.xml file
- We never distrub original files, so make it a backup

```t
cp tomcat-users.xml tomcat.users.xml.backup
```

- To deploy a .war file, there are different ways to deploy .war files
  * GUI Method
  * CLI Method

- To deploy our .war file using GUI Method, what is the role? So that tomcat defined few roles in tomcat-users.xml
  * Manager-gui - allow access to the HTML GUI.
  * Manager-script - allow access to the HTTP API.
  * Manager-jmx - allows access to JMX proxy.
  * Manager-status - allows access to the status pages only.


```t
vim tomcat-users.xml
```

```t
<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users xmlns="http://tomcat.apache.org/xml"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
    version="1.0">
  <role rolename="manager-gui"/>
  <user username="tomcatmanager" password="tomcatmanager" roles="manager-gui"/>
</tomcat-users>
```
## Goto to /opt/apache-tomcat-10.0.27/bin and start tomcat

```t
cd /opt/apache-tomcat-10.0.27/bin
```

```t
./startup.sh
```
- You can also check if tomcat is started or not
```t
sudo service status tomcat
```
OR
```t
sudo systemctl status tomcat
```
OR
```t
ps -ef | grep -i tomcat
```

- If tomcat started then you can visit http://your-public-ip:8080/

![image](https://user-images.githubusercontent.com/111498842/211397401-7c2a4a21-41e5-4752-b8e8-acb5d7f814de.png)

- Click on "Manager App" you will get follwoing 403 error

![image](https://user-images.githubusercontent.com/111498842/211398639-515675d1-5e98-43eb-ad33-071f2036025e.png)

- All allowed localhost from whatever ip-address you wanted to allowed people to access your tomcat web application manager using GUI method, we required to create a file here in /catalina/localhost

```t
cd /opt/apache-tomcat-10.0.27/conf/Catalina/localhost
```

```t
vim manager.xml
```

- Add following content to manager.xml file

```t
<Context privileged="true" antiResourceLocking="false"
         docBase="${catalina.catalina.valves.RemoteAddrValve" allow="^.*$" />
  <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
</Context>
```

- Now you can go to Manager App and enter the manager-gui username and password this time it will not through an error

![image](https://user-images.githubusercontent.com/111498842/211401818-c77d788d-c598-48af-b9f5-d9738f8d17b4.png)



