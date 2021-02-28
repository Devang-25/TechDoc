# Minikube Installation

1. __Create new user to launch minikube__
```Shell
1. useradd minikube-user
2. passwd minikube-user
3. vi /etc/sudoers # Append [minikube-user  ALL=(ALL)  ALL] under [root  ALL=(ALL)  ALL] to authorize sudo permission to minikube-user
```

2. __Install virtual box__
```Shell
1. su -
2. curl http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo -o /etc/yum.repos.d/virtualbox.repo
3. yum -y update
4. rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
5. yum install binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers-`uname -r` kernel-devel-`uname -r` dkms
6. yum install VirtualBox-6.1
7. usermod -a -G vboxusers minikube-user
8. exit
9. VBoxManage --version
```

3. __Install kubectl__
```Shell
1. curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
2. chmod +x ./kubectl
3. sudo mv ./kubectl /usr/local/bin/kubectl
4. kubectl version --client
```

4. __Install minikube__
```Shell
1. grep -E --color 'vmx|svm' /proc/cpuinfo # This command will verify the status of vmx or svm, make sure the output isn't empty after executing this command
2. curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube
3. sudo install minikube /usr/local/bin/
4. minikube version
```