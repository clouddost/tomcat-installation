# Installing Tomcat using Binary Method on Amazon Linux 2

## Installing Java 11 and Open JDK 11 on Amazon Linux 2:

```t
sudo amazon-linux-extras install java-openjdk11
```
## Confirm the Java is installed or not using following command:

```t
java -version
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
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.27/bin/apache-tomcat-10.0.27.tar.gz
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

```t
vim tomcat-uses.xml
```

- To deploy a .war file, there are different ways to deploy .war files
* GUI Method
* CLI Method
- To deploy our .war file using GUI Method, what is the role? So that tomcat defined few roles in tomcat-users.xml

