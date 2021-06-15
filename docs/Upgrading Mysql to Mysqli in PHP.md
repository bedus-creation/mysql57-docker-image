# Inplace upgrade Mysql 5.7 to Mysql 8

**Environment:** My local machine is running on ubuntu 20.4 LTS, and my laravel application is running on docker environment which has **Mysql server of version 5.7**. 

**Task/Problem:** We just upgraded our laravel application Laravel 5.4 to Laravel 8, and we also interested to upgrade our database server from Mysql 5.7 to Mysql 5.8, So our task is to upgrade that docker Mysql 5.7 server to  Mysql 8.

**Options:** There might be two options to do this task: 

* **Logical Upgrade:** Backup the data and installed Mysql 8 on the server and import the data
* **Inplace Upgrade:** which does upgrade Mysql server by replacing old Mysql binaries with Mysql 8 binaries.



## Inplace Upgrade Process

**Step 0: Preparation**

First we want to validate whether our data stored in current database (Mysql 5.7 in docker) is compatible with Mysql 8. So to check compatibility, **Mysql shell 8.0** comes with update checker.

Here I created a separate docker container that exists only Mysql 5.7, as in this projects. To start container

```bash
docker-compose up --build -d
```

 After running the container login the container as

```bash
docker exec -it mysql57_database_1 bash
```

Note: *mysql57_database_1* is the container name, it might change for you so check container name while building container.

Before start any thing perform updates and install wget as

```
apt update && apt upgrade
apt install wget lsb-release
```

Now next step is to install Mysql apt config

**Step 1: Install Mysql APT Config**

Go https://dev.mysql.com/downloads/repo/apt/ and download Mysql APT config download link

![](/home/ellite/code/jobins/mysql57_01/docs/images/Screenshot from 2021-02-19 09-08-12.png)

Example of Link: https://dev.mysql.com/get/mysql-apt-config_0.8.16-1_all.deb

use ```wget``` to download the file as

```bash
https://dev.mysql.com/get/mysql-apt-config_0.8.16-1_all.deb
```

and Install the update checker as 

```bash
dpkg -i mysql-apt-config_0.8.16-1_all.deb
```

Here, after installing Mysql apt perform update as 

```
apt update
```

  

**Step 2: Install Mysql-shell**  

```
apt install mysql-shell
```

Mysql-shell 8.0 provides upgrade checker utility, run Mysql shell;

```bash
mysqlsh
```

Which enters Mysql Utill shell

```bash
util.checkForServerUpgrade("root@localhost:3306");
```

It ask for user name and password for the **docker Mysql server** (I have stopped Mysql server on local machine), the above user ```root@localhost``` for docker Mysql 5.7 server.



**Step 3: Upgrade Mysql server to Mysql 8**

```bash
sudo apt install mysql-server
```

Here, we haven't change any configuration files so both version of Mysql uses same data directory to store and read data, and no require to explicitly load Mysql from different directory. 



**At Last**

Here, although this process is done in docker, it's not the way how docker updates Mysql, as the installing new server on docker will only remain till the docker volume is destroyed. Instead it is presented here as the simulation as considering docker as real server. 