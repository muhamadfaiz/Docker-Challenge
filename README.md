#Setting up Git to sync configuration files

1.Install git
```
# yum update -y
# yum install git -y
```
2.Create config directory to store configuration files 

```
# mkdir /root/configs
```

3.Use ~/configs
```
# cd ~/configs
# git init
```

4.Clone repo
```
# git clone https://github.com/muhamadfaiz/dockerchallenge169419.git
```

#Install and setting up Docker

1.Add the yum repo
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

2.Install Docker
```
# yum install docker-engine -y
```

3.Start Docker
```
# service docker start
```
