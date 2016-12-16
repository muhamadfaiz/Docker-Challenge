#Setting up Git to sync configuration files

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

#Install and setting up Docker

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

##Install MySQL within Docker

```
# docker run --name mysql-ghost -v /tmp/mysql/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD="password" -d -p 3306:3306 mariadb
```
1. Above command will do;
  1. Download `mariadb` Docker image if it is not available locally
  2. Name our new container with `mysql-ghost`
  3. Mount `/var/lib/mysql` to `/tmp/mysql` so that content will be accessible outside of the container
  4. Setting up root password to `password`
  5. Run in detached mode.
  6. Map MySQL container port 3306 to local port 3306.
