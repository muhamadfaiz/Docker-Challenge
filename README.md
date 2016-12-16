#Scenario 1 - Creation of a Blog

##Objective
Demonstrate basic understanding of Docker containers and images

##Mission
  * To have a blogging platform running for low traffic blog hosting
  * As a Blog Owner, I want to have a Ghost blogging platform (https://ghost.org/) running as Docker container with MySQL database backend (instead of default embedded SQLite database)

##Installation Procedures
###Setting up Git to sync configuration files

1. Install git
  ```
  # yum update -y
  # yum install git -y
  ```
  
2. Create config directory to store configuration files  
  ```
  # mkdir /root/configs
  ```

3. Use ~/configs
  ```
  # cd ~/configs
  # git init
  ```

4. Clone repo
  ```
  # git clone https://github.com/muhamadfaiz/dockerchallenge169419.git
  ```

###Install and setting up Docker

1. Add the yum repo
  ```
  # tee /etc/yum.repos.d/docker.repo <<-'EOF'
  [dockerrepo]
  name=Docker Repository
  baseurl=https://yum.dockerproject.org/repo/main/centos/7/
  enabled=1
  gpgcheck=1
  gpgkey=https://yum.dockerproject.org/gpg
  EOF
  ```

2. Install Docker
  ```
  # yum install docker-engine -y
  ```

3. Start Docker
  ```
  # service docker start
  ```

####Setting up MySQL container within Docker

1. Below command will do;

  ```
  # docker run --name mysql-ghost -v /tmp/mysql/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD="password" -d mariadb 
  ```
  
  1. Download `mariadb` Docker image if it is not available locally
  2. Name our new container with `mysql-ghost`
  3. Mount `/var/lib/mysql` to `/tmp/mysql` so that content will be accessible outside of the container
  4. Setting up root password to `password`
  5. Run in detached mode.

2. Login to MySQL
  ```
  # docker exec -i -t mysql-ghost /bin/bash
  # mysql -uroot -ppassword
  ```
  
3. Create the database, `ghost_blog` and grant all rights to the `ghost_user`.
  ```
  MariaDB [(none)]> create database ghost_blog;
  Query OK, 1 row affected (0.00 sec)

  MariaDB [(none)]> create user 'ghost_user'@'%' identified by 'ghost_password';
  Query OK, 0 rows affected (0.00 sec)

  MariaDB [(none)]> grant all privileges on *.* to 'ghost_user'@'%';
  Query OK, 0 rows affected (0.00 sec)

  MariaDB [(none)]> flush privileges;
  Query OK, 0 rows affected (0.00 sec)
  ```
  
####Setting up Ghost container within Docker

1. This is the same as we did above the only difference is now we are linking two containers to each other using the
`--link` parameter (`mysql-ghost`: "alias"). 
  ```
  # docker run --name ghost_blog -p 80:2368 -v /tmp/ghost/:/var/lib/ghost -d --link mysql-ghost:mariadb ghost
  ```

2. Load `config.js` from Github.
  ```
  # cp ~/configs/dockerchallenge169419/ghost/config.js /tmp/ghost/
  ```

3. Restart Ghost container
  ```
  # docker restart ghost_blog
  ```

##Acceptance Criteria

*	Candidate must be able to demonstrate the running containers.
*	There should be at least 2 running containers; Ghost blogging platform and MySQL database container (Tip: Use readily available official images from Docker Hub)

![img](http://i.imgur.com/wEGL6MV.png)

●	If the container is destroyed, the Ghost blog data is preserved if a new container is spun up
●	The Ghost blog app from its container must persist data into the MySQL database container.
●	Only HTTP connection is exposed to port 80 on the host from Ghost container.
●	MySQL database container should not expose any ports to the host.

